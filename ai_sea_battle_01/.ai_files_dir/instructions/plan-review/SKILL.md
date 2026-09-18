---
name: plan-review
description: Independently compare source code with architecture.md in an existing .ai/plans/NN_plan_name directory, run all project tests, consider factual implementation interruptions, and create correction plans without reading implementation patch specifications.
disable-model-invocation: true
---

# Plan Review

Recommend Sol medium; Sol/Astra are suitable alternatives. Recommendations do not switch the active model. Use a fresh conversation without implementation patch context. If patch contents are already present, request a fresh conversation rather than claiming independent review.

## Input and information boundary

Accept the name of an existing `.ai/plans/NN_plan_name/` directory. Require that directory, `architecture.md`, and original `plan.md`; otherwise refuse to proceed. Do not create a missing plan directory.

- Read `architecture.md`, relevant repository source/configuration/tests, applicable AGENTS.md rules, and relevant project skills.
- MUST NOT read existing patch specifications, implementation reports, or checklist completion entries as review evidence. Do not obtain them through broad searches, diffs, history, other agents, or linked documents.
- Exception: extract ONLY the `## Interruptions` sections from `plan.md` and `plan-fix_NN.md`, including their resolution notes. Use a section-targeted extraction, not a whole-file read that exposes execution entries. These are factual questions/blockers, not proof of implementation correctness.
- Directory filenames may be listed for allocating round numbers. The generic templates in `../plan/references/` are allowed: they are not implementation specifications.
- If checklist editing is needed, use narrowly targeted edits or structured section processing without bringing unrelated checklist contents into the review context.

## Independent review

1. Map every mandatory architecture requirement to concrete source evidence. Check behavior, boundaries, design constraints, security, and explicit non-goals. Do not infer correctness from completed checkboxes or ask whether code matches a patch.
2. Investigate interruption evidence against source and the original task. Resolve implementation questions where the contract permits a clear answer.
3. Clarify `architecture.md` when necessary, preserving original intent, mandatory requirements, and the original request. Append a clarification history entry with reason and affected requirement IDs. Do not weaken requirements merely to accept existing code. If resolving a contradiction requires a product decision or scope change, ask the user and record review as blocked; do not fabricate an approved decision.
4. Run ALL project test suites, discovering their actual commands from repository configuration and project rules. Include relevant required features/integration suites, not just tests for touched files. Use required local wrappers (all Cargo tasks on Windows go through `tools/cargo-limited.cmd`) and bounded runtimes. Do not start external/destructive tests without required safe setup or authorization. Missing setup, timeout, or failure must be reported; never claim a full pass when suites were skipped.
5. Review does not modify application code or tests. Request NEW tests in correction specifications only as a last resort for a substantial otherwise-uncovered correctness/security risk. Running existing tests is always required. Do not add a test-coverage section or generic requests to expand coverage unless this necessity is demonstrated. Required validation commands are still included in correction patches.

## Findings and correction rounds

Allocate review report numbers as maximum existing `review_NN.md` number + 1, starting at 01. Always create a concise report containing verdict (`pass`, `changes required`, or `blocked`), requirement-to-source evidence, actionable findings with severity, interruption resolutions, and commands/results including failures and unavailable suites. Do not add speculative refactors, style preferences, or optional feature work.

If actionable discrepancies exist, allocate correction number as maximum existing `plan-fix_NN.md` number + 1, starting at 01. Review numbering and correction numbering are independent. Never overwrite earlier documents.

- For correction rounds 01 and 02, report actionable deviations from the agreed architecture, not optional improvements.
- If the candidate correction number is 03 or higher, make the ENTIRE review final-mode: only blockers and information-security findings can require changes or generate patches. Blockers are unmet mandatory acceptance requirements, significant correctness/data-loss failures, or inability to build/run required functionality. Do not reclassify cosmetic issues as blockers. There is no automatic iteration cap.
- In final-mode still inspect the full mandatory contract and run all tests. Report validation limitations honestly; unrelated preexisting failures are not an excuse to expand scope.
- If no in-scope actionable discrepancy exists, create only the review report, not an empty correction plan. If verification is incomplete, use a blocked verdict rather than an unconditional pass.

For each correction round create:
- `plan-fix_NN.md`, following [the checklist template](../plan/references/plan-template.md), including the repository-state PREP and ordered patch entries with model/effort (Luna medium by default).
- `patches/fix-NN-patch-MM-name.md`, following [the patch template](../plan/references/patch-spec-template.md).

Create these specifications from architecture and inspected current source, NEVER by reading old patches. Each must contain full applicable requirements, concrete paths/contracts/changes, constraints, prerequisites, necessary skills, acceptance criteria and minimal validation. Do not require the implementer to read architecture or review reports. Include a dedicated new-tests section ONLY when essential and explain why existing tests cannot cover the risk.

## Resolve interruptions without falsifying completion

Append a resolution to each addressed interruption, retaining its original evidence. Use state `resolved` only when a concrete answer or a correction patch supplies the resolution; otherwise leave it unresolved.

If a blocked original patch must be replaced rather than resumed, annotate that patch's checklist entry using ONLY its ID from the interruption and a targeted edit: keep `[ ]` and append `Superseded by: plan-fix_NN.md#<new patch ID>`. This is routing metadata, not a completion claim. Do not read its specification. Replacement patches must cover the remaining mandatory behavior determined independently from architecture and source. Never mark unfinished work `[x]` or silently remove it.

Finish with the verdict, review report path, and correction checklist path if created. Do not start implementation or commit changes.
