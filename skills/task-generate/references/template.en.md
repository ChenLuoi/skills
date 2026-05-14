# English Task Document Template

When generating the final document, use the structure below and remove all template guidance. Do not leave empty placeholders; write an explicit value such as "None", "Not started", or "Not run" when there is no content yet. Keep only one task format matching the current "Task-list maturity" and delete the unused task format section. Select the appropriate document mode for the task; do not force ordinary implementation, debugging, testing, or operations work into a redesign format.

```md
# <Subsystem Or Task Name> Task Document And Implementation Plan

Generated on: YYYY-MM-DD
Last updated: YYYY-MM-DD
Evidence baseline: YYYY-MM-DD
Branch or commit: <branch / commit / Not recorded>
Inspected scope: <code, docs, config, commands, or user input inspected for this plan>
Stale-plan triggers: <specific files, modules, interfaces, configs, docs, requirements, runtime states, or user decisions that require this document to be rechecked when they change; do not write vague triggers such as "when code changes">

Document mode: <resolved document mode>
Mode selection reason: <why this mode fits the current task>
Document stage: <Draft / Under review / Plan accepted / Tasks refined / In execution / Completion audit / Complete>
Task-list maturity: <Coarse / Needs refinement / Executable / Execution-maintained>
User confirmation status: <Pending review / Confirmed / Needs confirmation: specific question>
Current status: <resolved status>
Overall progress: 0%
Task size summary: S 0 / M 0 / L 0
Context risk: <one resolved value: Low / Medium / High>
Next task: <stage-based value: "user document review" for Draft/Under review; "task-list refinement" for Plan accepted; first executable task for Tasks refined/In execution; "completion audit" for Completion audit; "None" for Complete; blocking item when blocked>

Applicability and assumptions:

- <product, code, runtime, compatibility, data, security, operations, or boundary assumptions for this plan>

## Document Execution And Maintenance Contract

This document is a living implementation guide, not a one-time analysis memo. Future execution is expected to load only this document, not the skill or the earlier conversation that generated it. Future executors must follow these rules:

### General Maintenance Rules

1. When the user asks to complete one or more tasks from this document, implement the requested work instead of only restating the plan.
2. After implementation, update current status, overall progress, next task, affected task status, actual completion notes, verification records, and follow-up notes.
3. Partial completion must be marked as "Partially complete"; externally constrained work must be marked as "Blocked"; cancelled or intentionally abandoned work must be marked as "Won't do".
4. If implementation diverges from the original plan, update this document so it matches the landed code.
5. Verification records may only include commands, tests, or checks that actually ran; write "Not run" with the reason when verification did not run.
6. Task size is not a human time estimate. It is a rough estimate of code-change breadth, context consumption, and reasoning complexity.
7. Existing task numbers must not be reordered when new tasks are inserted. Append new tasks to the end. Keep cancelled tasks in place and mark them as "Won't do" instead of deleting them.
8. Overall progress is estimated from task status, task size, and completed acceptance criteria. It does not need to be exact, but it must be consistent with task states.
9. Initialize "Document Update History" when creating a new document. Later updates should record only content directly relevant to requirement confirmation, plan decisions, implementation progress, verification results, deviation handling, or user confirmation. Do not record process noise that does not help complete the task correctly.
10. If the code, docs, config, interfaces, or user requirements covered by the evidence baseline have clearly changed before execution, update the evidence, current-state analysis, and task list first, or mark affected tasks as "Blocked" with the reason.
11. If this document lacks background needed for execution, first fill the gap from current workspace evidence. If the gap still cannot be resolved, mark the task as blocked and record the user confirmation needed.
12. High context-risk tasks should be executed one at a time. Before context compaction or interruption, update actual completion, deviations, verification records, and the next task.

### Common Operation Rules

#### Specified Task Execution

1. When the user asks to complete a specific task, first check that task's status, blocking conditions, unblock conditions, earlier tasks, and current workspace evidence.
2. When task-list maturity is not "Executable" or "Execution-maintained", do not treat coarse tasks as final implementation tasks. Refine the task list first, or explain that refinement is required.
3. If an unresolved blocker exists, do not execute the task; first explain the blocker, the prerequisite task, or the user confirmation needed.
4. If the task is executable, after implementation update task status, actual completion, verification record, follow-up notes, current document status, overall progress, next task, and Document Update History.

#### Task Status Output

1. When the user asks for task status, list every task number, title, and status using this document plus current workspace evidence, and mark stale, inconsistent, or unverifiable statuses.
2. Status output must use this table shape so the user can directly choose the next task:

| Task | Title | Document status | Actual status judgment | Blockers | Suggested next action |
| --- | --- | --- | --- | --- | --- |
| 1 | <Task title> | <from this document> | <judged from workspace evidence> | <None / blocker reason> | <Executable / refine first / confirm first / complete> |

#### Completion Audit

1. When the user asks for a completion audit, first compare this document with the actual workspace state and list incomplete work, incorrect implementation, missing verification, plan deviations, stale document state, and optimization items.
2. Show the audit result to the user for confirmation before writing it into this document or appending new tasks.
3. After the user confirms a completion audit, append the confirmed decision to "Document Update History" and "Completion Audit Records", then append accepted follow-up work as new task numbers. Record only the key checked evidence scope, not process logs.
4. If all tasks are complete and no serious issues remain, do not invent follow-up tasks.

---

## Document Stage, Task-List Maturity, Status, Task Size, And Context Risk Scale

Document stage scale:

- Draft: Requirements, current-state analysis, target plan, and coarse tasks are ready for user review.
- Under review: The user is discussing, changing, or challenging the plan, and the document should keep being updated.
- Plan accepted: Requirements, scope, and target plan are confirmed enough to refine executable tasks.
- Tasks refined: The task list has been checked against current workspace evidence and is executable.
- In execution: At least one task has started implementation or verification.
- Completion audit: Tasks appear complete and are being checked against the document and actual workspace state.
- Complete: Implementation, verification, document updates, and confirmed audit follow-up are complete.

Task-list maturity scale:

- Coarse: High-level task grouping for plan review. Tasks must fit the actual workspace, but full implementation detail is not required.
- Needs refinement: The plan is mostly settled, but tasks still need implementation entry points, blockers, acceptance criteria, and verification requirements.
- Executable: Tasks are detailed enough for future execution with only this document and current workspace evidence.
- Execution-maintained: Tasks are being updated as real implementation, verification, deviations, and progress change.

Status scale:

- Not started: Work has not begun.
- In progress: Work has started but does not satisfy all acceptance criteria.
- Partially complete: A verifiable part is complete and explicit work remains.
- Blocked: Work cannot continue because required information, environment, permission, external dependency, or user confirmation is missing.
- Complete: Implementation, document updates, and required verification are complete, or the reason verification did not run is recorded.
- Won't do: The task was cancelled, replaced, or confirmed unnecessary; keep the task number and record the reason instead of deleting it.

Task size scale:

- S: Small task. Local change, usually touching a small number of files, docs, configs, scripts, pages, or one entry point; low context consumption and suitable for one pass.
- M: Medium task. One module or a few adjacent modules / docs / configs / workflows; requires related context plus tests or docs, but should fit in one session.
- L: Large task. Cross-module, cross-workflow, or core-path work; requires clear execution order and verification points, still executable as one task but with context pressure.

Context risk scale:

- Low: Local context is enough; low compaction or interruption risk.
- Medium: Requires understanding multiple files, modules, docs, or historical decisions; update progress during execution.
- High: Involves core paths, cross-module design, migration, debugging, or large evidence sets; split execution and update this document before interruption.

## 1. Task Background

Explain why this task exists, what problem it solves, why it should be handled now, and how it affects the product, engineering system, operations, or collaboration workflow. This section must make sense to an executor who did not read the earlier conversation.

## 2. Task Goals And Scope

- Final goal:
- Non-goals:
- Success criteria:
- Impact scope:
- Explicit boundaries:

## 3. User Requirements And Communication Decisions

- Original user goal:
- Confirmed decisions:
- Rejected alternatives:
- Exceptions that still require user confirmation:
- Requirement clarification record:
- Plan review record:

Record relevance principle:

- This document records only information that helps complete the user's request correctly and without deviation.
- Keep requirements, confirmed decisions, rejected alternatives, key evidence, implementation progress, verification results, deviation handling, and user confirmation.
- Do not record unrelated intermediate process, temporary thoughts, invalid attempts, repetitive discussion, or pure operation logs.

Ambiguous requirement criteria:

- The target subsystem, deliverable, or output path is unclear.
- The final goal, success criteria, non-goals, or impact scope would materially change the plan.
- The user only asks for "analysis" without clearly asking whether to write a task document.
- The request mixes planning, implementation, status output, completion audit, or other intents without a clear execution order.
- The request conflicts with workspace guidance, an existing task document, or confirmed decisions.
- Required external context, runtime environment, credentials, permissions, or product decisions are missing.
- Continuing would force the executor to invent requirements, compatibility rules, or user decisions.

Plan confirmation rule:

- Before document stage becomes "Plan accepted" or task-list maturity becomes "Executable", explicit user confirmation is required.
- Clear user language such as "confirm the plan", "refine this plan", or "generate the executable version" counts as confirmation when the accepted plan is clear.

## 4. Evidence List

| Evidence source | Observed fact | Relationship to this task |
| --- | --- | --- |
| `<path / doc / config / command / requirement source>` | <fact from code, tests, docs, config, command output, or explicit user requirement> | <why it matters for this task> |

## 5. Current State And Problem Analysis

### 5.1 Existing Foundation

- <cite real module paths or verified behavior>

### 5.2 Current Problems And Gaps

- <describe missing capabilities, entry points, state, observability, tests, or workflow breakpoints>

### 5.3 Constraints And Behavior To Preserve

- <describe compatibility, performance, security, data, user experience, operations, or team constraints>

### 5.4 Facts, Inferences, And Design Decisions

- Facts: <from code, tests, docs, commands, or explicit user requirements>
- Inferences: <reasonable conclusions that are not directly proven by code>
- Design decisions: <chosen direction and rejected alternatives>

## 6. Target Plan

### 6.1 Target Behavior Or Deliverable

- <describe the expected system, document, workflow, test, or operational state after completion>

### 6.2 Ownership And Entry Points

- <state which module, service, page, command, document, process, or role owns this behavior>

### 6.3 Data, State, Interfaces, And Configuration

- <describe data models, state lifecycle, APIs, configuration, files, scripts, or external dependencies as needed; write "No new requirement" when not applicable>

### 6.4 Observability, Debugging, And Operations

- <describe logs, metrics, audit records, events, traces, diagnostics, rollback, alerts, or manual procedures as needed; write "No new requirement" when not applicable>

### 6.5 User, Operator, Or Collaboration Guidance

- <describe usage, communication, operation, or handoff rules that future executors, users, administrators, operators, or collaborators need; write "No new requirement" when not applicable>

### 6.6 Plan Acceptance And Task Refinement Conditions

- Conditions to enter task refinement:
- Issues still under discussion:
- Content required before task-list maturity can become "Executable":

## 7. Implementation Strategy

- Implementation entry points:
- New or changed content:
- Preservation boundaries:
- Replacement or deprecation boundaries:
- External impact:
- Verification entry points:

For `redesign-plan`, explicitly state the old structures that must stop expanding and the new unique owner after replacement. For non-redesign work, do not invent replacement boundaries just to fill the template.

## 8. Default Strategy And Banned Approaches

- Default strategy:
- Default configuration or behavior:
- Banned approaches:
- Exceptions that need user confirmation:

Implementation documents should not leave unresolved optional paths. When a choice must remain open, state the trigger condition and decision owner.

## 9. Risks, Boundaries, And Verification Strategy

- Main risks:
- Blocking conditions:
- Regression risks:
- Required verification:
- Optional verification:
- Unverified scope and reason:

## 10. Final Self-Check Checklist

- Requirement coverage: every user goal, non-goal, constraint, and communication decision is reflected.
- Evidence coverage: core conclusions trace to code, docs, config, command output, or user requirements.
- Record relevance: document records directly support requirement validation, implementation correctness, verification, deviation handling, or user confirmation, with no unrelated process noise.
- Clarification handling: ambiguous requirements, blockers, and confirmation items are resolved or recorded in the right document stage.
- Stage consistency: document stage, task-list maturity, current status, overall progress, and next task agree.
- Plan consistency: target plan, implementation strategy, risks, verification, and task list do not contradict each other.
- Task maturity: coarse tasks are clearly for review; executable tasks have entry points, blockers, acceptance criteria, verification, and update requirements.
- Execution operations: specified task execution, task status output, and completion audit rules are written into this document.
- Completion audit: this document includes "Completion Audit Records" and requires user confirmation before appending follow-up tasks.
- Workspace constraints: project guidance, naming conventions, banned approaches, and verification requirements are reflected.
- Stale-plan triggers: concrete files, modules, APIs, configs, docs, runtime states, or user decisions are listed.
- Placeholder cleanup: the final document must not keep unresolved template placeholders or unused task formats.

## 11. Final Conclusion

Restate the final goal, execution path, expected system state after completion, current document stage, and whether the next action should be plan review, task refinement, task execution, status output, or completion audit.

## 12. Document Update History

| Date | Update type | Summary | Verification |
| --- | --- | --- | --- |
| YYYY-MM-DD | Created | Initial task document generated | Not run |

## 13. Completion Audit Records

| Date | Key checked evidence scope | Audit conclusion | User confirmation | Added tasks |
| --- | --- | --- | --- | --- |
| YYYY-MM-DD | Not performed | Not run | Not applicable | None |

## 14. Task List

Task list rules:

- When generating the final document, keep only the task format matching the current "Task-list maturity"; delete the unused task format section.
- When "Task-list maturity" is "Coarse" or "Needs refinement", use the coarse task format. Tasks only need to be workspace-grounded, reasonably ordered, goal-oriented, and explicit about known blockers and refinement requirements.
- When "Task-list maturity" is "Executable" or "Execution-maintained", use the executable task format. Every task must include complete implementation entry points, affected scope, expected result, blocking conditions, unblock conditions, banned approaches, execution requirements, acceptance criteria, actual completion, verification record, and follow-up notes.
- Executors must not reorder existing task numbers when inserting new work. Append new tasks at the end.

Current format: <Coarse task format / Executable task format>

The two formats below are template guidance. When generating the final document, delete the unused format heading and examples, and keep only the task list for the current format.

#### Coarse Task Format Template (use during Draft, Under Review, or Needs Refinement; delete this heading in the final document)

### 1. <Task Name>

- Status: <resolved status>
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <one resolved value: S / M / L>
- Context risk: <one resolved value: Low / Medium / High>
- Goal:
- Evidence or rationale:
- Expected result:
- Known blockers:
- Refinement requirements:
- Actual completion:
  1. Not started
- Verification record:
  1. Not run
- Follow-up notes:
  1. None

### 2. <Task Name>

- Status: Not started
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <one resolved value: S / M / L>
- Context risk: <one resolved value: Low / Medium / High>
- Goal:
- Evidence or rationale:
- Expected result:
- Known blockers:
- Refinement requirements:
- Actual completion:
  1. Not started
- Verification record:
  1. Not run
- Follow-up notes:
  1. None

#### Executable Task Format Template (use after task refinement or during execution maintenance; delete this heading in the final document)

### 1. <Task Name>

- Status: <resolved status>
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <one resolved value: S / M / L>
- Context risk: <one resolved value: Low / Medium / High>
- Goal:
- Implementation entry points:
- Affected scope:
- Expected result:
- Blocking conditions:
- Unblock conditions:
- Banned approaches:
  1. ...
- Execution requirements:
  1. ...
- Acceptance criteria:
  1. ...
- Required document updates after execution:
  1. Update current status, overall progress, next task, and Document Update History.
  2. Update this task's status, progress, actual completion, verification record, and follow-up notes.
- Actual completion:
  1. Not started
- Verification record:
  1. Not run
- Follow-up notes:
  1. None

### 2. <Task Name>

- Status: Not started
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <one resolved value: S / M / L>
- Context risk: <one resolved value: Low / Medium / High>
- Goal:
- Implementation entry points:
- Affected scope:
- Expected result:
- Blocking conditions:
- Unblock conditions:
- Banned approaches:
  1. ...
- Execution requirements:
  1. ...
- Acceptance criteria:
  1. ...
- Required document updates after execution:
  1. Update current status, overall progress, next task, and Document Update History.
  2. Update this task's status, progress, actual completion, verification record, and follow-up notes.
- Actual completion:
  1. Not started
- Verification record:
  1. Not run
- Follow-up notes:
  1. None
```
