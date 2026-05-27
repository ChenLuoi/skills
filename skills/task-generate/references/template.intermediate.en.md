# English Intermediate Task Document Template

Use this for plan clarification, evidence completion, user confirmation, and pre-refinement work. Remove all template guidance when generating a document. Do not leave empty placeholders; write "None", "Not started", or "Not run" when empty.

Intermediate documents must be non-executable. They may record the original goal, discussion process, pending decisions, and coarse tasks, but they must not include final execution operations, completion-audit rules, or executable task formats.

Intermediate documents may record undecided choices, but those choices must be centralized in the pending decision table or draft boundaries. Coarse tasks must not turn undecided choices into executable steps.

```md
# <Subsystem Or Task Name> Intermediate Task Document

## 0. Document Control

| Field | Value |
| --- | --- |
| Generated on | YYYY-MM-DD |
| Last updated | YYYY-MM-DD |
| Evidence baseline | YYYY-MM-DD |
| Branch or commit | <branch / commit / Not recorded> |
| Inspected scope | <code, docs, config, commands, or user input inspected> |
| Stale-plan triggers | <specific files, modules, interfaces, configs, docs, requirements, runtime states, or user decisions that require this document to be rechecked> |
| Document mode | <resolved document mode> |
| Mode selection reason | <why this mode fits> |
| Execution authority | Non-executable |
| Document stage | <Draft / Under review / Plan accepted> |
| Task-list maturity | <Coarse / Needs refinement> |
| Pending confirmation count | <0 / N> |
| Current status | <Not started / In progress / Blocked / Partially complete / Complete / Won't do> |
| Status note | <one-sentence explanation; write "None" when no extra note is needed> |
| Overall progress | 0% |
| Context risk | <Low / Medium / High> |
| Next task | <user confirmation / user document review / task-list refinement / blocking item> |

> This is a skill-assisted intermediate document, not an execution authority. When Execution authority is "Non-executable", no task may be executed from this document. While pending confirmations exist, every later response must show those items first and task refinement is forbidden.

## 1. Background, Goals, And Scope

- Original user goal:
- Background:
- Applicability and assumptions:
- Goal:
- Non-goals:
- Success criteria:
- Impact scope:
- Explicit boundaries:

## 2. Requirements, Confirmations, And Decisions

- Confirmed decisions:
- Rejected alternatives:
- Requirement clarification record:
- Plan review record:

Pending user-decision details (write "None" when empty):

| Item ID | Question or decision needed | Why it blocks | Known options or user input needed | Impact scope | Status |
| --- | --- | --- | --- | --- | --- |
| <CONF-1; write "None" when empty> | <question that needs user judgment or clarification> | <why refinement or execution cannot proceed without the answer> | <known options or "user input required"> | <affected section, task, or module> | <Needs confirmation / Resolved / Rejected / Not applicable> |

While any item status is "Needs confirmation", Next task must be "user confirmation", and task refinement or execution is forbidden. Before a final executable document is created, every "Needs confirmation" item must be resolved.

## 3. Evidence And Current State

| Evidence source | Observed fact | Relationship to this task |
| --- | --- | --- |
| `<path / doc / config / command / requirement source>` | <fact from code, tests, docs, config, command output, or explicit user requirement> | <why it matters> |

- Existing foundation:
- Current problems and gaps:
- Constraints and behavior to preserve:
- Facts:
- Inferences:
- Design decisions:
- Evidence still needed:

## 4. Draft Plan And Boundaries

- Target behavior or deliverable:
- Ownership and entry points:
- Data, state, interfaces, and configuration:
- Observability, debugging, and operations:
- User, operator, or collaboration guidance:
- Implementation strategy:
- Draft scope decisions:
- Scope choices awaiting confirmation:
- Preservation boundaries:
- Replacement or deprecation boundaries:
- Banned approaches:
- Task-refinement gate: <no unresolved confirmation / unresolved confirmation remains, list items>

## 5. Risks And Verification Concerns

- Main risks:
- Blocking conditions:
- Regression risks:
- Verification required later:
- Currently unverified scope and reason:

## 6. Next Skill-Assisted Action

- If pending confirmations exist: wait for user answers.
- If the plan is accepted but tasks remain coarse: refresh evidence and refine the task list.
- If evidence is missing: collect evidence and update this document.

## 7. Document Update History

| Date | Update type | Summary | Verification |
| --- | --- | --- | --- |
| YYYY-MM-DD | Created | Initial intermediate task document generated | Not run |

## 8. Stage Draft (Non-Executable)

Fill this only when the task is large, the user asks for staged review, or context risk is high. When a stage draft does not apply, replace the entire table with "None". This section is only for reviewing development order. Do not use executable checkpoint IDs such as C1 or C2, and do not include commands such as "complete through this stage".

| Stage | Stage goal | Expected coarse tasks covered | Review question |
| --- | --- | --- | --- |
| <stage name; write "None" when empty> | <state after this stage is done> | <related coarse task numbers or range> | <question that needs user confirmation or review; write "None" when empty> |

## 9. Coarse Task List

Tasks are only for plan review or later skill-assisted refinement. Do not execute them directly.

### 1. <Task Name>

- Status: <Not started / In progress / Blocked / Partially complete / Complete / Won't do>
- Progress: 0%
- Priority: P0 / P1 / P2
- Difficulty: Low / Medium / High
- Task size: <S / M / L>
- Context risk: <Low / Medium / High>
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
```
