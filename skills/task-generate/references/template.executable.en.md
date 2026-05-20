# English Final Executable Task Document Template

Use this only after the plan is clear, facts are settled, every pending confirmation is resolved, and tasks are sufficiently refined. Remove all template guidance when generating a document. Do not leave empty placeholders; write "None", "Not started", or "Not run" when empty.

The final document is an execution control surface. Keep only facts, decisions, rules, tasks, and verification needed for execution. Final documents must not keep discretionary implementation branches; every candidate task, optimization, cleanup, compatibility rule, and verification expansion must already be classified as included in the current plan, explicitly out of scope, objectively blocked, or pending confirmation. If pending confirmations exist, do not use this template.

```md
# <Subsystem Or Task Name> Executable Task Document

## 0. Execution Control

| Field | Value |
| --- | --- |
| Generated on | YYYY-MM-DD |
| Last updated | YYYY-MM-DD |
| Evidence baseline | YYYY-MM-DD |
| Branch or commit | <branch / commit / Not recorded> |
| Core evidence scope | <key code, docs, config, commands, or user decisions supporting this execution plan; do not write a full exploration log> |
| Stale-plan triggers | <specific files, modules, interfaces, configs, docs, requirements, runtime states, or user decisions that require this document to be rechecked> |
| Document mode | <resolved document mode> |
| Mode selection reason | <why this mode fits> |
| Execution authority | Executable |
| Pending confirmations | None |
| Current status | <Not started / In progress / Blocked / Partially complete / Complete / Won't do> |
| Status note | <one-sentence explanation; write "None" when no extra note is needed> |
| Overall progress | 0% |
| Task size summary | S 0 / M 0 / L 0 |
| Context risk | <Low / Medium / High> |
| Next task | <first executable task / completion audit / None / blocking item> |

## 0.1 Status Scales

| Category | Scale |
| --- | --- |
| Status | Not started / In progress / Partially complete / Blocked / Complete / Won't do; "Current status" and task "Status" may only use these values, with prose in "Status note" or follow-up notes |
| Task size | S: local small change; M: one module or nearby modules; L: cross-module, core-path, or migration work |
| Context risk | Low: local context; Medium: multiple files or historical decisions; High: core path, cross-module, migration, debugging, or large evidence set |

## 0.2 Global Execution Rules

- This document is the living implementation guide. Future execution is expected to load only this document, not the skill or earlier conversation.
- Specified task execution: first check task status, prior tasks, blocking conditions, unblock conditions, and current workspace evidence. If a blocker remains, explain it instead of bypassing it.
- After a task is completed, update task status, actual completion, verification record, follow-up notes, current status, status note, overall progress, next task, and document update history.
- Task status output: when the user asks for status, use `Task / Title / Document status / Actual status judgment / Blockers / Next action`, using both this document and current workspace evidence, and mark stale, inconsistent, or unverifiable states.
- Completion audit: compare this document with workspace reality and list incomplete work, incorrect implementation, missing verification, plan deviations, stale document state, and optimization items. Show the result to the user first. Do not write audit records or append follow-up tasks until the user confirms.
- Do not reorder task numbers. Append new work at the end. Keep cancelled work and mark it "Won't do". If implementation diverges from the plan, update this document to match landed code.
- This document must not keep discretionary implementation branches. If execution reveals an undecided implementation, cleanup, compatibility, or verification choice, mark the affected task blocked and ask for a decision instead of executing it as discretionary work.

## 0.3 Global Verification Rules

- Verification records may only include commands, tests, or checks that actually ran; write "Not run" with the reason when they did not.
- If the evidence baseline changed materially, update evidence, the plan, and tasks first, or mark affected tasks as blocked.
- Trigger verification by task type: code tasks, release or package-boundary tasks, CLI/UI behavior tasks, data-safety tasks, migration tasks, performance tasks, documentation tasks, and operations tasks should each name the checks they require; write a reason when a verification family does not apply.
- Task-level verification may inherit this section; write task-specific verification only when it adds something special.

## 0.4 Global Banned Approaches

- Do not bypass unresolved blockers.
- Do not record verification as passed when it did not run.
- Do not delete or renumber existing tasks.
- Do not keep stale status, verification, or next-action wording.
- Do not keep discretionary or undecided wording in the target plan, task steps, acceptance criteria, or follow-up notes; objective failure handling and unblock conditions are allowed.

## 1. Background, Goals, And Scope

- Background:
- Applicability and assumptions:
- Final goal:
- Non-goals:
- Success criteria:
- Impact scope:
- Explicit boundaries:

## 2. Decision Summary

- Confirmed requirements:
- Confirmed implementation constraints:
- Included in the current plan:
- Explicitly out of scope:
- Rejected alternatives and still-effective reasons:
- Pending confirmations: None

## 3. Evidence Summary

| Evidence source | Key fact | Decision or task supported |
| --- | --- | --- |
| `<path / doc / config / command / user decision>` | <fact supporting the plan or task boundary; do not record full exploration process> | <decision, constraint, risk, or task number> |

- Facts:
- Inferences:
- Design decisions:

## 4. Target Plan And Boundaries

- Target behavior or deliverable:
- Ownership and entry points:
- Data, state, interfaces, and configuration:
- Observability, debugging, and operations:
- User, operator, or collaboration guidance:
- Implementation strategy:
- Scope decisions:
- Preservation boundaries:
- Replacement or deprecation boundaries:

## 5. Risks And Verification

- Main risks:
- Blocking conditions:
- Regression risks:
- Unverified scope and reason:

| Verification item | Command or method | Required | Current result |
| --- | --- | --- | --- |
| <verification item> | `<command or method>` | <Yes / No> | <Not run / Passed / Failed / Not applicable with reason> |

## 6. Final Conclusion

Restate the final goal, execution path, expected system state after completion, current execution status, and whether the next action should be task execution, status output, or completion audit. When updating this document, the conclusion must match Execution Control.

## 7. Maintenance Records

### 7.1 Document Update History

| Date | Update type | Summary | Verification |
| --- | --- | --- | --- |
| YYYY-MM-DD | Baseline | Plan accepted and tasks refined into executable list | Not run |

### 7.2 Completion Audit Records

| Date | Key checked evidence scope | Audit conclusion | User confirmation | Added tasks |
| --- | --- | --- | --- | --- |
| YYYY-MM-DD | Not performed | Not run | Not applicable | None |

## 8. Task List

| Task | Title | Priority | Size | Risk | Phase | Main verification |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | <Task name> | P0 / P1 / P2 | <S / M / L> | <Low / Medium / High> | <Baseline / User or release boundary / Shared foundation / Core implementation / Testing and verification / Documentation and audit / Other> | <key command or method> |

### 1. <Task Name>

- Status: <Not started / In progress / Blocked / Partially complete / Complete / Won't do>
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <S / M / L>
- Context risk: <Low / Medium / High>
- Goal:
- Task boundary decision:
  - Included:
  - Excluded:
  - Dependency or sequencing constraints:
- Implementation entry points:
- Affected scope:
- Evidence references:
- Expected result:
- Blocking conditions:
- Unblock conditions:
- Task-specific banned approaches: inherits global banned approaches; additional requirements:
  1. None
- Task-specific execution requirements: inherits global execution rules; additional requirements:
  1. None
- Acceptance criteria:
  1. ...
- Build and verification checkpoint: <checks that must pass after this task; classify temporary red status as "not allowed" or "allowed", and when allowed name the failing commands, reason, and later task that restores them>
- Post-execution document update requirement: inherits global execution rules; additional requirements:
  1. None
- Actual completion:
  1. Not started
- Verification record:
  1. Not run
- Follow-up notes:
  1. None
```
