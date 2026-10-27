# Contributing to the WaterUptime hub

This repository holds product, architecture and research documents. Code contributions go to the [contract](https://github.com/WaterUptime/WaterUptime-SmartContract) and [frontend](https://github.com/WaterUptime/WaterUptime-Frontend) repositories.

The organisation-wide [contributing guide](https://github.com/WaterUptime/.github/blob/main/CONTRIBUTING.md) and [Code of Conduct](https://github.com/WaterUptime/.github/blob/main/CODE_OF_CONDUCT.md) apply here too. This file adds hub-specific rules.

## Good contributions to the hub

- Corrections where a document no longer matches the code. Code is the source of truth; link the file and line you checked.
- Clearer explanations for non-technical readers: water committees, WASH staff, funders.
- Research method improvements (interview guide, consent, synthesis).
- New rows in the compatibility table, after you've run the versions together.

## Writing rules

- **Be honest about status.** Label things *implemented*, *experimental* or *planned*. Don't describe planned features as working.
- **No unverified claims.** Don't claim impact, partnerships, endorsements or affiliations (government, NGO, UNDP, Stellar Development Foundation or others).
- **No personal data.** Research notes must be anonymised before they reach this repository. Never commit interview recordings, names, phone numbers or exact locations.
- **Plain language.** Short sentences, and define terms such as *fingerprint* or *testnet* on first use.
- **Keep docs in sync.** If you change [docs/contract-state-machine.md](docs/contract-state-machine.md), check it against `contracts/water_uptime/src/` and the contract repo's `docs/CONTRACT_INTERFACE.md`.

## Updating the compatibility table

Add a row to the table in [README.md](README.md#compatibility) when:

- the contract interface changes (functions, types, errors or events);
- a new shared testnet contract is deployed; or
- a frontend release targets a different contract version.

Each row needs:

1. the frontend version (tag, or `main` @ short SHA);
2. the contract source version (tag or SHA);
3. the deployed testnet contract ID;
4. the date tested;
5. **what you actually checked**, for example "testnet mode loads jobs and history; wallet actions X, Y, Z succeed".

Don't add a row you haven't tested, and don't delete old rows.

## Pull requests

Docs-only PRs need a clear description and links to any code you checked. One reviewer approval is enough unless the change touches [security and privacy](docs/security-and-privacy.md), which needs a maintainer.
