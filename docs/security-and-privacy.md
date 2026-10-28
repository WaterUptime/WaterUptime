# Security and privacy

WaterUptime is an experimental prototype on Stellar **testnet**. It is not approved for real funds, production operations or personal data. This document explains what the system protects, what it doesn't, and the rules that keep it from becoming a privacy risk.

To report a vulnerability, see [SECURITY.md](../SECURITY.md).

## Principles

1. **Public by default, so minimal by design.** Everything on-chain, including events, is public and effectively permanent. If something must not be public forever, it doesn't go on-chain.
2. **Fingerprints, not data.** On-chain references are salted SHA-256 fingerprints of off-chain records.
3. **Accountable roles.** Every action is tied to the account that signed it, and the person who does the work is not the person who checks it.
4. **Honest claims.** The ledger records what an authorised account *submitted*; it does not prove what happened in the field.

## What may and may not go on-chain

| Allowed | Never allowed |
|---|---|
| Random 32-byte IDs (water points) | Names, phone numbers, ID numbers |
| Salted fingerprints of off-chain records | Photos, audio, documents, raw reports |
| Stellar account addresses of role holders | Exact household or GPS coordinates |
| Statuses, timestamps, job numbers | Health or socio-economic information about households |
| Boolean flags (e.g. active, approved) | Anything derived from personal data without salting |

Contract-specific field-by-field guidance and a PR checklist are being written in [contract issue #7](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/7).

## Why hashes need salt

A hash is not encryption. If a record is predictable (a village name, a phone number, "pump broken"), anyone can hash the guesses and compare. WaterUptime therefore fingerprints `salt + record`, where the salt is 32 random bytes stored **with the off-chain record**, never on-chain. See the [integration guide](integration-guide.md#fingerprints) for the exact scheme.

A fingerprint proves a record **hasn't changed since it was anchored**. It doesn't prove the record is true.

## Linkability

A Stellar address that acts repeatedly links all of its actions together. Anyone can see that one technician account did 40 repairs in one area. Mitigations to consider before any pilot:

- Role accounts per programme rather than per person, where accountability rules allow
- Keeping the address-to-person mapping off-chain under access control
- Never publishing the mapping, even in reports

## Threat model

| Threat | Example | Current mitigation | Gap / plan |
|---|---|---|---|
| Unauthorised action | Stranger reports or closes jobs | Signature checks; stored admin; per-job roles | — |
| Self-verification | Technician approves own work | `technician ≠ verifier` enforced | Same person with two accounts → governance; optional stricter policy ([contract #2](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/2)) |
| Collusion | Technician and verifier agree to fake a repair | None on-chain | Independent spot checks; community confirmation; rotation of verifiers |
| False evidence | Old or staged photo | Fingerprint prevents later edits | Evidence capture rules; EXIF stripping ([frontend #3](https://github.com/WaterUptime/WaterUptime-Frontend/issues/3)); human review |
| Coordinator key loss or compromise | Admin key stolen | — | Two-step handover ([contract #3](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/3)); key-management plan before any pilot |
| Wrong network | Signing on mainnet by mistake | Testnet config only; frontend `v0.2.0` refuses to sign unless Freighter's network passphrase is testnet, and re-checks it and the selected account at every signature | Manual check with the real Freighter extension ([frontend #2](https://github.com/WaterUptime/WaterUptime-Frontend/issues/2)) |
| Storage growth / DoS | Endless rework loop | History capped at 50 entries | In `v0.1.1` a job can get stuck open at the cap; fix in review ([contract #13](https://github.com/WaterUptime/WaterUptime-SmartContract/pull/13)) |
| Archived data | Finished jobs expire from live state about 120 days after their last change | Writes extend TTL; archived entries can be restored | Routine export, verify and extend ([below](#retention-of-the-audit-trail)); in-contract extension ([contract #6](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/6)) |
| Re-identification | Guessing a record from its hash | Salted fingerprints | Salt must stay off-chain and access-controlled |
| Misleading UI | App implies more than the ledger proves | Plain-language notices; demo and testnet labels | Review copy with users during discovery |

## Retention of the audit trail

The on-chain audit trail is **not permanent or unconditionally available**. In this prototype:

| Limit | What it means |
|---|---|
| **50 entries per job** | Endless rework is cut off. Once the fix in [contract #13](https://github.com/WaterUptime/WaterUptime-SmartContract/pull/13) is merged and deployed, a job always keeps room to be closed or cancelled. |
| **Storage TTL** | A job's records stay live for about 120 days after its last change. Free (simulated) reads don't extend them, and the network caps any extension at about 180 days. After that the records are archived: still restorable, but unreadable until someone pays to restore them. |
| **Events** | RPC nodes keep events for days, not months. |
| **Testnet resets** | A reset erases every deployment and record. |
| **Off-chain records** | Descriptions and evidence never go on-chain. Today they exist only in the browser that created them, so a fingerprint can be checked only while that record is kept. |

Retention procedure (proposed in [contract #13](https://github.com/WaterUptime/WaterUptime-SmartContract/pull/13), `docs/RETENTION.md` and `scripts/audit-export.sh`, run against the shared testnet instance on 2026-10-28):

- **Extend:** extend every record at least every 90 days. Any funded account can do this; it doesn't need to be the admin. All records of the shared instance currently last until about 2027-04-19.
- **Export:** export every job and its full history after jobs finish, and at least monthly. Keep each export with its SHA-256, off-chain.
- **Verify:** check an export against the chain, and again after restoring any archived record.

Restoring a genuinely archived record has been tested only in the contract's unit tests so far, not on testnet, because nothing there has archived yet. Who keeps doing this, and for how many years, is a governance question for any real deployment ([implementation pathway](implementation-pathway.md)).

## Corrections and mistakes

On-chain records can't be edited or deleted. Corrections are made by **adding** records:

- **Mistaken or duplicate report** → the coordinator cancels the job, with a reason fingerprint.
- **Bad repair** → the verifier returns it for rework; both attempts stay in the history.
- **Wrong assignment** → reassign before evidence is submitted.
- **Reporter misuse** → revoke the reporter; their existing jobs remain for audit.
- **Wrong data in an off-chain record** → correct it off-chain. The old fingerprint will no longer match; record why.

If personal data is ever put on-chain by mistake, it cannot be removed. Treat it as an incident: report it privately, stop further writes from that path, and document the response.

## Before any real-world use

Required by [Gate E](../ROADMAP.md#m4-pilot-readiness--gate-e):

- Independent contract and application security review
- Data-protection review against applicable law (for Nigeria, the Nigeria Data Protection Act 2023), done by qualified people. This document is not legal advice.
- A documented role, key-management and recovery model
- Consent, retention and access-control rules for the off-chain store
- Incident response and dispute procedures, with named owners
- An accountable implementation partner
