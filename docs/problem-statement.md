# Problem statement

> **This is a hypothesis, not an established finding.** It describes what WaterUptime wants to test. It does not claim that existing WASH systems lack these functions everywhere.

## Context

Community water points (handpumps, boreholes, standpipes) break down. Restoring service usually involves several actors:

- a **community water committee or monitor**, who notices and reports the fault;
- a **WASH coordinator or asset owner** (often LGA staff or an implementing organisation), who triages and assigns the work;
- a **technician or local service provider**, who carries out the repair;
- someone who **confirms service is restored**, ideally independent of the technician;
- a **funder or programme partner**, who pays for maintenance and needs to reconcile records.

The starting geography is Nigeria. It will be narrowed to one locality after discovery.

## Hypothesis

Where several organisations or community actors must coordinate a repair, the records for each step (fault report, assignment, evidence of work, independent verification and payment) may be held separately. That fragmentation may make it hard to establish:

- **what** was repaired, and whether it was the reported fault;
- **who** confirmed the result, and whether they were independent;
- **when** service was actually restored;
- whether a repeat failure is a new fault or a repair that never worked.

If so, the consequences could include slower restoration, repeat visits, disputes between actors, weak accountability for maintenance funds, and little reliable data on how long communities go without water.

## Why consider a shared ledger?

A shared ledger is only worth its cost if **more than one party needs to rely on the same record**, and no single party should be able to quietly rewrite it. For example, a funder relying on a verifier's sign-off of a contractor's work. WaterUptime tests whether a minimal on-chain workflow can provide:

- one shared timeline from report to verified close-out;
- enforced separation between who does the work and who checks it;
- a tamper-evident audit history, including rejected attempts.

If a conventional database run by one trusted organisation would do the job, **that is the better answer**, and the project should say so (see [Gate B](../ROADMAP.md#m3-fit-and-design--gates-b-c-d)).

## What a ledger cannot solve

- It cannot prove that a photo is genuine or that a repair meets safety standards.
- It cannot prove water quality.
- It cannot fix weak governance, missing budgets, collusion between roles, or a lack of spare parts.
- It adds friction (accounts, keys, connectivity) that may outweigh its benefits for some users.

## What we don't know yet

1. How faults are reported and recorded today, and by whom, in the target locality.
2. Which existing systems (for example WASHIMS or functionality-tracking tools) already cover these steps, and whether they can be integrated.
3. Where delays, disputes or missing records actually occur, and how often.
4. Whether role separation (technician ≠ verifier) is workable in low-resource communities.
5. Who would own and operate a shared workflow, and who would pay for it.

These questions drive the [user research plan](user-research.md).

## Outcome we're aiming for

For a single reported fault, an authorised reviewer can reconstruct the lifecycle from report to verified close-out: who did each step, when, and what evidence reference they submitted. They can do this without relying on one person's private spreadsheet as the only record.
