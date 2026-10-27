# Roadmap

WaterUptime moves forward through **decision gates**, not dates. Each milestone ends with a question that has to be answered honestly before the next one starts. Stopping or changing direction is an acceptable outcome.

Status key: ✅ done · 🚧 in progress · 🔜 next · 📋 later · ⛔ blocked on a gate

## M0: Technical demonstrator ✅

Show the full fault-to-closure workflow with synthetic data on testnet.

- ✅ Soroban contract: registry, reporter permissions, lifecycle with rework and cancellation, role separation, audit history, events ([contract](https://github.com/WaterUptime/WaterUptime-SmartContract))
- ✅ Unit tests for positive, negative, authorisation and event cases; CI
- ✅ Shared testnet deployment and a reproducible demo script
- ✅ Web app with an interactive demo and a read-only testnet view ([frontend](https://github.com/WaterUptime/WaterUptime-Frontend))
- ✅ Project hub, organisation profile, contribution and security policies

## M1: Hardening and contributor wave 🚧

Make the demonstrator robust enough for others to build on. Issue sizes, dependencies and the cycle plan are in the [Drips Wave backlog](docs/drips-wave-backlog.md).

**Contract**
- 🔜 Configurable role-separation policy ([#2](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/2))
- 🔜 Two-step admin handover ([#3](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/3))
- 🔜 Per-water-point job index ([#4](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/4))
- 🔜 Exact event-payload tests ([#5](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/5))
- 🔜 TTL extension for audit records ([#6](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/6))
- 🔜 Data-minimisation checklist ([#7](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/7))
- 🔜 TypeScript bindings in CI ([#8](https://github.com/WaterUptime/WaterUptime-SmartContract/issues/8))

**Frontend**
- 🔜 Wallet signing on testnet ([#2](https://github.com/WaterUptime/WaterUptime-Frontend/issues/2))
- 🔜 Evidence attachments with client-side fingerprints ([#3](https://github.com/WaterUptime/WaterUptime-Frontend/issues/3))
- 🔜 Deep links on static hosts ([#4](https://github.com/WaterUptime/WaterUptime-Frontend/issues/4))

**Release hygiene**
- ✅ `v0.1.0` tagged in both repositories and recorded in the [compatibility table](README.md#compatibility)

**Exit:** a reviewer can run the whole workflow on testnet from the web app with their own wallet.

## M2: Discovery ⛔ Gate A (problem fit)

Find out whether the problem is real, and where. → [User research plan](docs/user-research.md)

- 8–10 structured conversations across water committees, technicians, LGA WASH staff and implementers
- A documented current-state workflow and an inventory of existing systems (including WASHIMS, where used)
- At least one specific, repeated bottleneck confirmed by several sources

**Gate A:** do stakeholders confirm a meaningful gap? If not, stop or re-scope.

## M3: Fit and design ⛔ Gates B, C, D

- **Gate B (blockchain fit):** does more than one party need shared, tamper-evident state or rule enforcement? If not, recommend a conventional database and archive the ledger approach.
- **Gate C (data protection):** can the workflow avoid exposing sensitive or re-identifiable data? Off-chain evidence store design with access control.
- **Gate D (operational fit):** who has authority to assign, verify, correct errors and handle disputes?
- Table-top scenario tests with willing stakeholders; revise the workflow based on findings.

## M4: Pilot readiness ⛔ Gate E

Only with an accountable implementation partner and explicit approvals:

- Independent contract and security review
- Data-protection, legal and operational review
- Governance, dispute and incident procedures
- Pilot design with baseline and success metrics, plus a rollback and exit plan

**Gate E:** no real funds or production use until every item above is in place.

## Not planned

Mainnet deployment, real payments or escrow, tokens or NFTs, AI fault detection, and replacing public-sector WASH systems are out of scope until the gates above say otherwise. See [product requirements](docs/product-requirements.md#explicit-exclusions-for-v0).
