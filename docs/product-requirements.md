# Product requirements

Version 0.1 · Status: **MVP defined; not yet user-validated** · Chain: Stellar (Soroban), testnet only

Each requirement is marked ✅ implemented, 🔜 planned or 📋 not started.

## Users

| Role | Needs to | Notes |
|---|---|---|
| **Reporter** (community water committee or monitor) | Report a fault quickly; see that it's being handled | May have low connectivity and a basic phone; may need assisted reporting |
| **Coordinator** (WASH coordinator or asset owner) | Triage faults; assign technician and verifier; close or cancel jobs | Holds the admin role in the contract |
| **Technician** | See assigned work; submit repair evidence; resubmit after rework | Must not verify their own work |
| **Verifier** | Assess submitted repairs; approve or return with reasons | Should be independent of the technician |
| **Funder / programme partner** | Reconstruct what was done and verified, for reconciliation | Read-only |

All MVP users are fictional, and all accounts are testnet accounts.

## MVP workflow

1. **Register.** The coordinator registers a water point with a random opaque ID and a fingerprint of its off-chain record.
2. **Report.** An authorised reporter opens a job for an active water point, with a fingerprint of the fault report.
3. **Assign.** The coordinator assigns a technician and a *different* verifier. Reassignment is allowed until evidence is submitted.
4. **Submit.** Only the assigned technician submits a fingerprint of the repair evidence.
5. **Verify.** Only the assigned verifier approves the repair or returns it for rework, with a reason fingerprint.
6. **Rework.** The technician resubmits. Earlier submissions and rejections stay in the history.
7. **Close.** The coordinator closes the job only after approval. A mistaken or duplicate report can be **cancelled** instead, but not while evidence awaits verification.
8. **Inspect.** Anyone can read a job's status and full audit history.

## Functional requirements

| ID | Requirement | Status |
|---|---|---|
| F1 | Only the coordinator registers water points, manages reporters, assigns, closes and cancels | ✅ |
| F2 | Reports must reference a registered, active water point, with at most one open job per point | ✅ |
| F3 | Technician and verifier must be different accounts | ✅ |
| F4 | Only the assigned technician submits; only the assigned verifier decides | ✅ |
| F5 | A job closes only from `Verified`; `Closed` and `Cancelled` are final | ✅ |
| F6 | Every transition appends to an audit history; nothing is erased | ✅ |
| F7 | Every state change emits an event with no personal data | ✅ |
| F8 | Duplicate, out-of-order and unauthorised actions are rejected with specific errors | ✅ |
| F9 | Web app shows status, roles, timestamps and audit history in plain language | ✅ |
| F10 | Web app explains contract errors in plain language | ✅ |
| F11 | Users sign actions with their own wallet on testnet | ✅ (frontend `v0.2.0`) |
| F12 | Evidence files are fingerprinted client-side and stored off-chain under access control | 🔜 (client side) · 📋 (store) |
| F13 | Optional stricter role separation (e.g. verifier ≠ reporter) | 🔜 |
| F14 | Coordinator role can be handed over safely | 🔜 |

## Non-functional requirements

| ID | Requirement | Status |
|---|---|---|
| N1 | **Privacy:** no names, phone numbers, photos, exact locations or raw reports on-chain | ✅ by design |
| N2 | **Fingerprints are salted** so low-entropy records can't be guessed from their hash | ✅ (frontend) |
| N3 | **Mobile-first:** usable at 390px wide | ✅ |
| N4 | **Low bandwidth:** default app bundle avoids blockchain libraries | ✅ (~100 KB gzipped) |
| N5 | **Accessibility:** keyboard navigation, visible focus, screen-reader labels, reduced motion | ✅ baseline · 📋 audit |
| N6 | **Honest status:** demo and testnet data are always labelled as such | ✅ |
| N7 | **Reproducible:** documented build and test commands; CI on every PR | ✅ |
| N8 | **Offline-first capture** with later sync | 📋 future |

## Explicit exclusions for v0

- Real payments, escrow, stablecoins or automatic disbursement
- Mainnet deployment
- Maintenance marketplace, bidding, vendor scoring or credit history
- AI fault detection, predictive maintenance or satellite analysis
- Public storage of photos, names, phone numbers, GPS coordinates, household information or raw documents
- Claims that the blockchain alone verifies physical repairs or water quality
- Replacing existing public WASH systems

## Success criteria

**Software (met for M0)**
- All permitted transitions pass automated tests; unauthorised, duplicate, premature and out-of-order actions are rejected
- The verifier can't be the technician; a job can't close before verification
- Rework leaves a traceable history
- The demo runs entirely on synthetic data and testnet

**Discovery (M2, not started)**
- 8–10 structured conversations across the user groups above
- A documented current-state workflow and an inventory of systems and data owners
- At least one repeated bottleneck confirmed by several interviewees or records
- A clear reason why a shared ledger adds value beyond a database, or a decision that it doesn't

**Pilot metrics (only if a pilot happens; not claimed until measured)**
- Median time from report to assignment, and from report to restoration
- Share of repairs independently verified
- Share of jobs with complete records
- Repeat-failure rate within an agreed period
- Time and cost to reconcile records across actors

## Decision gates

| Gate | Question | Consequence if "no" |
|---|---|---|
| A (problem fit) | Do stakeholders confirm a meaningful gap? | Stop or re-scope |
| B (blockchain fit) | Does more than one party need shared, tamper-evident state? | Use a conventional database |
| C (data protection) | Can we avoid exposing sensitive or re-identifiable data? | Redesign before going further |
| D (operational fit) | Is it clear who assigns, verifies, corrects and resolves disputes? | Don't pilot |
| E (pilot readiness) | Are security, legal and operational reviews and a responsible partner in place? | No real funds or production use |
