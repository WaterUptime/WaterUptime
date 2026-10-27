# Implementation pathway

A staged route from prototype to a responsible, supervised pilot. **This is a plan, not a claim.** WaterUptime is not endorsed by, or part of, any UNDP, government or NGO programme. No institution should be described as a partner without a formal agreement.

The approach borrows from systems-based development practice: start from a concrete challenge, identify who owns the service, co-design with the people affected, and only scale what is shown to work. For example, UNDP's January 2026 publication *New Tech, New Partners: Transforming Development in the Digital Era* treats blockchain as one element of a wider system, alongside partner roles, institutional ownership, safeguards and interoperability. We use these as design principles only.

## Stage 1: Validate the development challenge

→ [User research plan](user-research.md) · **Gate A: problem fit**

Interview 8–10 people across water committees, technicians, LGA WASH staff and implementing organisations. Document the current workflow, existing tools, who can authorise changes, and where delays, disputes or missing records occur.

**Exit:** a repeated, specific bottleneck confirmed by several sources, or a decision to stop or re-scope.

## Stage 2: Map institutional ownership

Before proposing any pilot, identify:

- the asset owner and duty bearer;
- the official responsible for operations and maintenance;
- the body authorised to verify work;
- the organisation that controls the repair budget;
- the data controller, and who may access which data;
- procurement, safeguarding, complaints and incident-response processes.

A dashboard or smart contract cannot take over these responsibilities.

**Exit:** a clear map of who would own, operate and be accountable for the workflow (**Gate D**).

## Stage 3: Bounded testnet demonstrator ✅

Synthetic water points and accounts demonstrate the complete workflow: successful repair, rejected evidence and rework, unauthorised actions, attempted early closure, and cancellation. The documentation states exactly what the ledger records and what it cannot prove.

Status: **done** (see the [compatibility table](../README.md#compatibility) and [contract state machine](contract-state-machine.md)). No public funds, production records or personal data are handled.

## Stage 4: Compare against existing workflows

Run table-top scenario tests with willing stakeholders using the demonstrator:

- Walk through realistic cases: a straightforward repair, a disputed repair, a duplicate report, an unavailable technician.
- Compare time, steps and record quality with today's process.
- Ask whether they would use it, what would have to change, and who would run it.

**Exit (Gate B: blockchain fit):** if the shared ledger adds little over a conventional database, say so, and **revise or stop**.

## Stage 5: Design a supervised pilot with an implementation partner

Only with an identified partner and explicit approvals, define:

- pilot location and water points;
- operational and governance roles;
- the role of the existing public-sector system of record;
- data protection, consent and retention (**Gate C**);
- dispute, correction and incident handling;
- legal and procurement requirements;
- the maintenance budget and any payment process (still off-chain unless separately reviewed);
- baseline and success metrics;
- security review, key management, and a rollback and exit plan.

**Exit (Gate E: pilot readiness):** all of the above signed off. Until then: no real funds, no production use, no mainnet.

## Stage 6: Measure outcomes, then discuss wider use

Report results transparently, **including negative findings and limitations**. Possible metrics:

- time from report to assignment, and from report to restoration;
- share of repairs independently verified;
- completeness of records;
- repeat-failure rate;
- cost and time to reconcile records across actors.

Don't claim impact from a prototype or extrapolate beyond the data observed.

## Where we are

| Stage | Status |
|---|---|
| 1. Validate the challenge | 📋 Not started |
| 2. Map ownership | 📋 Not started |
| 3. Testnet demonstrator | ✅ Done |
| 4. Compare with existing workflows | ⛔ Needs stages 1–2 |
| 5. Pilot design | ⛔ Needs a partner and gates B–D |
| 6. Measure and report | ⛔ Needs a pilot |

Building the demonstrator before discovery was a deliberate choice: it gives stakeholders something concrete to react to. It is not evidence that the problem exists.
