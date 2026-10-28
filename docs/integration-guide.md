# Integration guide

How to read from the WaterUptime contract, verify fingerprints, follow events, and, eventually, connect existing WASH systems.

> **Testnet only.** Never point an integration at mainnet. The current contract ID is in the [compatibility table](../README.md#compatibility).

| Setting | Value |
|---|---|
| Network passphrase | `Test SDF Network ; September 2015` |
| RPC URL | `https://soroban-testnet.stellar.org` |
| Contract ID | see [compatibility table](../README.md#compatibility) |
| Interface reference | [contract-state-machine.md](contract-state-machine.md) · contract repo [`docs/CONTRACT_INTERFACE.md`](https://github.com/WaterUptime/WaterUptime-SmartContract/blob/main/docs/CONTRACT_INTERFACE.md) |

## Reading state

Read calls are **simulated**: free, with no wallet or signature needed.

### With the Stellar CLI

```bash
CONTRACT=CD5V5AJCWEP25CVVFQ3J5E54AFOB6JFYO4MIWBJSIPECMEM3PHZ6TCLX
stellar keys generate reader --network testnet --fund     # any funded key works as --source

stellar contract invoke --id $CONTRACT --source reader --network testnet --send=no -- job_count
stellar contract invoke --id $CONTRACT --source reader --network testnet --send=no -- get_job --job_id 1
stellar contract invoke --id $CONTRACT --source reader --network testnet --send=no -- get_job_history --job_id 1
```

### With JavaScript / TypeScript

`@stellar/stellar-sdk` can build a typed client from the contract's on-chain interface at runtime, with no code generation:

```ts
import { contract, Networks } from "@stellar/stellar-sdk";

const client = await contract.Client.from({
  contractId: "CD5V5AJCWEP25CVVFQ3J5E54AFOB6JFYO4MIWBJSIPECMEM3PHZ6TCLX",
  networkPassphrase: Networks.TESTNET,
  rpcUrl: "https://soroban-testnet.stellar.org",
});

const count = (await client.job_count()).result;            // bigint
const job = (await client.get_job({ job_id: 1n })).result;  // Result<MaintenanceJob>
if (job.isOk()) {
  const j = job.unwrap();
  console.log(j.status.tag);                // "Closed"
  const hex = Array.from(j.fault_reference, (b) => b.toString(16).padStart(2, "0")).join("");
  console.log(hex);
}
```

Decoding notes:

- `u64` values (`id`, timestamps, `job_count`) come back as `bigint`.
- Enums come back as `{ tag: "Closed" }`.
- `BytesN<32>` comes back as `Uint8Array`; hex-encode it for display.
- Functions that can fail return a `Result`; check `isOk()` before `unwrap()`.

The frontend's [`src/data/testnet.ts`](https://github.com/WaterUptime/WaterUptime-Frontend/blob/main/src/data/testnet.ts) is a complete working example.

To generate static TypeScript bindings instead:

```bash
stellar contract bindings typescript --network testnet \
  --contract-id CD5V5AJCWEP25CVVFQ3J5E54AFOB6JFYO4MIWBJSIPECMEM3PHZ6TCLX \
  --output-dir packages/water_uptime
```

## Following events

Every state change emits an event (see the [list](contract-state-machine.md#events)). To fetch recent events:

```bash
stellar events --network testnet \
  --id CD5V5AJCWEP25CVVFQ3J5E54AFOB6JFYO4MIWBJSIPECMEM3PHZ6TCLX \
  --start-ledger <LEDGER> --count 50 --output json
```

RPC nodes keep events for a **limited window** (days, not months). For long-term history, either:

- run an indexer that stores events as they arrive, or
- read `get_job_history`, which keeps a job's full audit trail on-chain for as long as the job's storage is alive.

## Fingerprints

WaterUptime anchors off-chain records with salted SHA-256 fingerprints. The frontend's scheme ([`src/domain/hash.ts`](https://github.com/WaterUptime/WaterUptime-Frontend/blob/main/src/domain/hash.ts)):

```text
salt        = 32 random bytes, hex-encoded (64 chars)
content     = JSON.stringify(record)          // UTF-8
fingerprint = SHA-256( UTF-8( salt + ":" + content ) )   // 32 bytes → BytesN<32>
```

To verify a record against the chain:

1. Fetch the record and its salt from the off-chain store.
2. Recompute the fingerprint exactly as above. The serialisation must be byte-identical, so store the exact `content` string.
3. Compare it with the on-chain reference (`fault_reference`, `repair_reference`, `verification_reference`, `reason_reference`, or an `AuditEntry.reference`).

A match proves the record is unchanged since it was anchored, not that it is true.

> **Planned:** when evidence attachments land ([frontend #3](https://github.com/WaterUptime/WaterUptime-Frontend/issues/3)), the record will include each file's SHA-256, so the fingerprint covers the files too. This section will be updated then.

## Writing

Writing requires signatures from the right role account. Today you can write with:

- the **Stellar CLI** (`stellar contract invoke ... --source <role-key>`),
- the contract repo's **demo script**, [`scripts/testnet-demo.sh`](https://github.com/WaterUptime/WaterUptime-SmartContract/blob/main/scripts/testnet-demo.sh), which deploys your own instance where you hold every role, or
- the **web app** (frontend `v0.2.0` and later) in Testnet mode with [Freighter](https://www.freighter.app/) set to Testnet; see its [wallet setup](https://github.com/WaterUptime/WaterUptime-Frontend#signing-with-a-wallet-testnet-only).

The shared instance's admin key is held by the maintainers, so a new account can only read there. Use your own deployment to try every role, and point the app at it with `VITE_CONTRACT_ID`.

## Integrating existing WASH systems

Systems such as WASHIMS or functionality-tracking tools may already record water points and faults. WaterUptime's position:

1. **They stay the system of record** where they already are. WaterUptime adds a shared verification trail; it does not replace them.
2. **Permission first.** No scraping, copying or assumed access to government or partner data. Any integration needs the data owner's written permission and a lawful basis for processing.
3. **Read-only to start.** Map existing water-point IDs to WaterUptime's random IDs off-chain. Never put the external ID on-chain if it could identify a location or community.
4. **Documented field mapping.** Each integration gets a mapping table (source field → WaterUptime field → on-chain or off-chain) reviewed against [security-and-privacy.md](security-and-privacy.md).
5. **After discovery.** Which systems to integrate is a question for [user research](user-research.md), not an assumption.

No integration with any external system exists today.
