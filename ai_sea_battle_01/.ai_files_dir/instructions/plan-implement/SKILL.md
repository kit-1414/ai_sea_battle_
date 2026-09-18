---
name: plan-implement
description: Implement sequential patches or one selected patch from an existing .ai/plans/NN_plan_name execution checklist, inspect Git state first without committing, validate against self-contained patch specifications, and record completion or interruptions without reading architecture.md.
disable-model-invocation: false
---

## Input and selection

Accept a plan directory name under `.ai/plans/`, optionally an execution filename (`plan.md` or `plan-fix_NN.md`) and a patch ID. An explicit path to that checklist is also accepted; derive its plan directory. Reject paths outside this workflow directory.

- Require an existing directory and original `plan.md`. Never create a missing plan directory or invent a plan.
- If no execution filename is given, select the original plan with pending patches, then correction plans in numeric order. If all are complete, report that and stop without a repository preflight.
- Entries annotated `Superseded by: plan-fix_NN.md#<patch ID>` are not pending work, despite remaining `[ ]`. Follow the replacement checklist when selecting work; never implement superseded specifications. A dependency on a superseded patch is satisfied only when its replacement chain is complete. Reject missing targets or cycles as interruptions.
- Read checklists for selection, status, and dependencies. If an unresolved interruption affects the selected work, stop and point to it; do not repeatedly attempt blocked work without resolution.
- For a specific patch, require completed prerequisites; do not implement prerequisites implicitly. Otherwise execute pending patches sequentially in listed order, stopping at the first blocker. Do not automatically start another checklist after completing the selected one.

## Information boundary

Use ONLY the selected patch specifications as task instructions, the checklists as execution metadata, applicable AGENTS.md rules, required skills, and relevant source/configuration/test files. Inspect actual code before editing.

MUST NOT read `architecture.md`, `review_NN.md`, or infer missing requirements from other planning documents. Do not read these indirectly through broad searches, tool output, delegation, or linked references. Scope searches accordingly. Do not copy architecture into context through another agent. An unavailable requirement or contradictory contract is a stop condition, not permission to guess.

## PREP: repository-state preflight

Execute once before code changes in EACH implementation invocation, not before every patch. The checklist PREP entry documents this preparation; an old checked entry does not waive checking the current working tree.

1. Locate the Git root and inspect staged, unstaged, deleted, and untracked nonignored files across the ENTIRE repository.
2. Preserve all existing work. Do not stage, commit, amend, reset, restore, revert, clean, or otherwise modify repository state as part of PREP.
3. Inspect for merge conflicts, likely credentials, private keys, sensitive exports, or another unsafe repository state without printing secret values. Stop and record an interruption only when that state makes the selected patch unsafe; an ordinary dirty worktree is not itself a blocker.
4. Record the clean/dirty outcome in PREP after the inspection succeeds. Mention pre-existing paths only as needed to avoid overwriting them. This checklist edit is expected to leave the working tree dirty.

Follow tool-specific Git/editor requirements and project instructions. This skill authorizes no staging or commits.

## Implement and self-check

For each selected patch:
1. Read its full specification and load the required skills.
2. Confirm prerequisites and actual interfaces match the specification. Implement only the permitted changes, respecting copied architecture/design constraints and project rules.
3. Add or adjust only the prescribed necessary tests. Run focused checks and every mandatory validation gate. If Rust code was changed, run the project's required `build --features dev-assets`; on Windows use `tools/cargo-limited.cmd` for ALL Cargo tasks. Bound command runtimes.
4. Inspect the diff against every requirement and completion criterion. Do not fix unrelated failures or silently broaden scope.
5. Mark ONLY this patch `[x]` in the selected execution checklist after all required checks pass. A failed, timed-out, or unavailable check is not a pass. Keep successful validation reporting brief in the final response, not in Interruptions.

## On interruption

Stop the invocation on ambiguity, contradictory requirements, unavailable interfaces, scope expansion, validation failure that cannot be fixed within scope, or unavailable mandatory validation. Preserve work; do not roll back user changes.

Leave the affected patch `[ ]`. Append a factual entry under `## Interruptions` in the selected checklist: timestamp, patch ID, unresolved state, issue, source or command evidence, attempted checks, and clarification needed. If using `plan-fix_NN.md`, also add a brief pointer under `## Interruptions` in original `plan.md` so the initial plan records the interruption. Do not include full patch contents or a success narrative.

Do not modify architecture, specifications, or acceptance criteria. Review resolves contradictions and may produce correction patches. End with the blocker and the plan directory to pass to the coordinator or `plan-review`.

## Resuming after user clarification

When a coordinating agent resumes the same implementation session with an explicit user answer to a recorded question, treat that answer as authoritative clarification for the selected patch. Append the answer and resolution state to the existing interruption, then continue only if it resolves the ambiguity without changing architecture, acceptance criteria, prerequisites, or permitted scope. If the answer requires any such change, keep the patch unchecked and stop for replanning or `plan-review`; do not rewrite specifications yourself.
