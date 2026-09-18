---
name: plan
description: Plan a task without implementing it. Create architecture.md, self-contained patch specifications, and an execution checklist under .ai/plans/NN_plan_name for subsequent plan-implement and independent plan-review runs.
disable-model-invocation: true
---

# Plan

Use Sol/Astra for planning. Model names and effort levels are recommendations, not an ability to switch the active model. Recommend a separate conversation for each workflow stage to preserve information boundaries.

## Responsibilities

1. Read the user's task, applicable AGENTS.md instructions, relevant source code, and applicable project skills. Inspect real paths, contracts, build/test commands, and existing patterns before specifying edits. Do not change application code or commit anything.
2. Clarify consequential ambiguities with the user. Record the original task faithfully, agreed clarifications, and any explicit assumptions. Do not silently expand scope.
3. Discover existing directories under `.ai/plans/`. Create the parent if absent. Allocate the next unused numeric prefix (maximum existing prefix + 1, starting at 01, padded to at least two digits), and a short descriptive snake_case name: `.ai/plans/NN_plan_name/`. Never overwrite another plan.
4. Create `architecture.md`, `plan.md`, and `patches/patch-NN-name.md`. Use [the patch template](references/patch-spec-template.md) and [the checklist template](references/plan-template.md). Replace template instructions with concrete verified content.
5. Check that every mandatory architecture requirement has implementation coverage and that each patch is independently understandable using only its specification, relevant code, project rules, and named skills.
6. Finish with the plan directory and a brief summary. Do not start implementation: it requires a separate user invocation.

## architecture.md

Include:
- Original request and agreed clarifications, preserving user intent.
- Observable intended behavior and numbered mandatory requirements (`REQ-001`, etc.).
- Architecture boundaries, dependency direction, data/contracts, compatibility and security constraints where relevant.
- Code-design constraints, existing patterns to reuse, and explicit non-goals.
- Acceptance criteria and the minimal necessary validation strategy.
- An initially empty clarification history for later review amendments.

This is the authoritative task contract for review. Distinguish requirements from optional ideas. Do not make patch completion an acceptance criterion.

## Patch decomposition

- Prefer small sequential patches suitable for `Luna medium`; use `Luna high` only with a concrete complexity rationale. Split unnecessarily large patches rather than relying on stronger models.
- Include concrete project-relative files and symbols, exact intended changes, relevant interfaces, dependencies, permitted scope, forbidden changes, and minimal necessary tests with executable commands and expected outcomes.
- Copy every applicable architecture constraint into each affected patch. Requirement IDs provide traceability but are not a substitute for the requirement text. The implementer MUST NOT need to read `architecture.md`.
- Include enough verified interface detail to avoid invention, but do not paste large source files or prescribe brittle line numbers.
- Name applicable skills. Project instructions always apply even when not copied into the patch.
- Specify deterministic, focused tests; avoid redundant coverage. Include required project build/validation gates. On Windows all Cargo commands use the project's wrapper.
- Order dependencies explicitly. A patch may depend on completed earlier patches, not undocumented reasoning from them.

## Checklist contract shared by all three skills

`plan.md` is a simple ordered checklist using [the checklist template](references/plan-template.md). Each patch entry has a relative link, `[ ]` or `[x]`, recommended model/effort, and prerequisite IDs if any. Do not embed implementation details here.

The first entry is a non-patch preparation step: checkpoint all repository changes before implementation, using the exact commit message `before NN_plan_name`. This step is executed by `plan-implement`, not by `plan`.

Reserve a `## Interruptions` section for factual blockers: patch ID, observed problem, evidence, attempted validation, and the clarification needed. This is the ONLY checklist content review may use as task evidence. Keep implementation success reports and patch summaries out of it.

Correction rounds use `plan-fix_NN.md` and `patches/fix-NN-patch-MM-name.md` in the SAME directory. They follow these templates and never replace previous rounds. Review reports use `review_NN.md` with their own monotonically increasing numbering.
