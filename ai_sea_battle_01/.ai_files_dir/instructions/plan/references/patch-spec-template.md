# <patch ID>: <title>

## Goal and requirements

<Observable result. Requirement IDs AND the full applicable requirement text. This specification must stand alone without architecture.md or other patch specifications.>

## Dependencies and execution

- Prerequisites: <patch IDs or none; concrete contracts available after prerequisites>
- Recommended model: Luna medium
- Effort rationale: <only if recommending Luna high>
- Required skills: <names and what each is needed for>

## Scope and constraints

- Allowed changes: <specific responsibilities>
- Architecture and code-design constraints: <all applicable constraints, copied here>
- Security/data/history/compatibility constraints: <where applicable>
- Out of scope: <explicit prohibitions>

## Verified code context

<Relevant existing types, interfaces, signatures, conventions, and data flow. Identify actual project-relative paths and symbols. Clearly identify new files as new. Do not refer the implementer to architecture.md.>

## Required changes

1. `<project-relative path>` — `<symbol or new component>`: <concrete required change, contract and behavior, including relevant failure cases>.
2. <Next change. Include necessary call sites, documentation and configuration when in scope.>

## Validation

<Minimal necessary focused checks with concrete cases, test locations, executable commands, and expected outcomes. Identify tests to reuse before requesting new ones. Include mandatory project build gates; use tools/cargo-limited.cmd for every Cargo task on Windows.>

## Completion checklist

- [ ] Required behavior and constraints implemented without unrelated changes.
- [ ] Required tests implemented where specified and all mandatory checks passed.
- [ ] Diff self-reviewed against this specification and applicable project instructions.
- [ ] Only after all gates pass: mark this patch [x] in the execution checklist.

## Stop conditions

If requirements conflict, an interface is unavailable, scope must expand, or a required check cannot pass/run, leave the patch unchecked. Record evidence and the clarification needed in the execution checklist's `## Interruptions` section, then stop. Do not read architecture.md or rewrite the task contract.

<!-- For correction patches: omit a dedicated new-tests section unless essential to cover a substantial otherwise-uncovered correctness/security risk. Explain that necessity if new tests are requested. Existing relevant checks and required project validation remain mandatory. -->
