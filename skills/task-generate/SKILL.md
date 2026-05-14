---
name: task-generate
description: Generate or revise standalone, self-explanatory, evidence-grounded task documents for planning, review, and task-list refinement. Use when the user explicitly asks to create, write, update, revise, or refine a task document or implementation plan. Do not use for executing tasks, reporting task status, or auditing completion of an existing task document unless the user explicitly invokes `$task-generate`. Use `task/*.md` only when the user or workspace guidance establishes that location. Inspect real workspace evidence before writing.
metadata:
  short-description: Generate evidence-grounded task docs
  version: "0.1.0"
---

# Task Generate

Use this skill to turn a workspace-specific request into a standalone task document that can guide review, implementation, progress management, and completion audits.

The generated document should still be useful after the conversation is gone. A future executor should be able to open it, understand the background and current workspace state, review the plan, pick the next unfinished task, identify blockers, implement work, report status, run verification, audit completion, and update progress in the same document.

This skill is not for lightweight memos or quick advice. When this skill triggers, default to producing or maintaining a complete, self-contained task document whose task-list maturity matches the current lifecycle stage, unless the user is explicitly asking only for review or discussion.

## Foundational Contract

This contract is the reason the skill exists. Preserve it when updating the skill or templates:

- The skill is a generation and review tool. It gathers evidence, resolves the plan, and writes or updates the task document.
- The final task document is the execution authority. Later implementation, progress updates, plan corrections, and document maintenance may happen with only that task document loaded, without this skill or the original conversation.
- Therefore, every rule needed during execution must be written into the final task document, not only into this skill.
- The final task document must include enough background, user requirements, communication decisions, evidence, assumptions, constraints, document stage, task-list maturity, task order, acceptance criteria, verification rules, progress rules, completion-audit rules, and update rules to stand alone.
- If a future skill change would make generated task documents depend on the skill, chat history, hidden context, or unresolved options, reject that change.
- Records in the task document exist only to help the user and future executors complete the requested work correctly and without deviation. Do not add audit trails, process notes, or intermediate records that do not help validate requirements, decisions, evidence, progress, verification, or user confirmation.

Keep rule ownership clear when updating this skill or its templates:

- `SKILL.md` owns generation behavior: intent classification, evidence gathering, mode selection, scope control, task splitting, and completion checks.
- The templates own final-document behavior: execution rules, progress maintenance, status scales, verification record rules, and update history that future executors need without loading this skill.
- Do not remove execution and maintenance rules from templates just to reduce repetition. Necessary repetition between this skill and templates is acceptable when it preserves the standalone document contract.
- If a rule appears both here and in a template, keep the meaning consistent. If the rule only affects generation, keep it here. If the rule must guide later execution, make sure it is present in the final document template.

## Execution Intent

Classify the user's intent before acting:

- Planning or document generation: inspect workspace evidence and write or update the task document. Do not change implementation artifacts unless the user also asks for implementation.
- Draft review and plan finalization: when the user reviews, challenges, or changes a generated task document, update the document's requirements, communication decisions, current-state analysis, plan, and coarse task list until the plan is settled. Do not prematurely over-detail the task list before the plan is accepted.
- Task-list refinement: after the plan is explicitly accepted, inspect the actual workspace evidence again and turn the coarse task list into fully executable tasks with concrete entry points, blockers, acceptance criteria, verification, and progress fields. A user request for refinement counts only when it clearly confirms the current plan or points to an already accepted plan.
- Executing tasks from a task document: implement the requested tasks first, then update the task document with real status, completion notes, verification, deviations, and next work.
- Progress-only update: do not change implementation artifacts. Update the task document from supplied evidence and workspace inspection; clearly mark unverifiable claims.
- Task status output: read the task document and relevant current workspace evidence, then list every task title and status so the user can directly choose what to execute next. Mark stale, inconsistent, or unverifiable statuses explicitly.
- Task completion audit: usually after all tasks appear complete, inspect the task document and actual workspace state, list missing work, incorrect completion, deviations, errors, and optimization items, then wait for user confirmation before writing the audit record or appending new tasks.
- Reviewing or improving a task document, template, or skill: when the user asks to analyze, review, discuss, evaluate, or suggest optimizations, default to reporting findings, tradeoffs, and proposed rule changes without editing files.
- Applying document, template, or skill changes: edit files only when the user explicitly asks to modify, apply, update, land, or directly change files, or after the user confirms a proposed change set. If the user asks for both analysis and modification, give a concise modification plan first, then make the edits.

For skill changes, protect the Foundational Contract above before all other cleanup goals. Never optimize this skill in a way that makes generated task documents depend on this skill, chat history, hidden context, or unresolved options.

If intent is ambiguous, choose the least destructive interpretation that satisfies the request and state the assumption briefly.

## Ambiguity And Confirmation Rules

Clarify the user's requirement before deep analysis or file output when any of these are true:

- the target subsystem, document subject, or expected deliverable is unclear
- the requested output path is missing and a file needs to be written
- the final goal, success criteria, non-goals, or impact boundary would materially change the plan
- the user asks for "analysis" but does not clearly ask for a document file
- the request mixes planning, implementation, status reporting, and audit in a way that changes what should happen first
- the request conflicts with workspace guidance or with an existing task document
- required external context, runtime environment, credentials, or product decisions are missing
- the answer would otherwise require inventing requirements, compatibility rules, or user decisions

When clarification is required, ask concise questions and wait for the user. Do not hide unresolved choices inside the generated document unless the document stage is explicitly `Draft` or `Under review` and the choices are recorded as confirmation items.

Before writing a new task document, confirm the destination path if the user has not already provided an exact path or workspace guidance has not established one. It is acceptable to finish analysis first, propose a path, and ask before creating the file.

Plan acceptance requires explicit user confirmation. Do not move a document to `Plan accepted`, `Tasks refined`, or `Executable` task-list maturity based only on silence or lack of objections. An explicit user request such as "confirm this plan", "finalize it", "refine the accepted plan", or "generate the executable version" counts as confirmation when the plan being accepted is clear.

## Template Selection

Use one bundled template when writing a new task document:

- Chinese request or Chinese output requested: read `references/template.zh.md`.
- English request or English output requested: read `references/template.en.md`.
- Bilingual output requested: read both templates and preserve equivalent sections in the requested bilingual format.

Read only the template needed for the current output language unless the user explicitly asks for multiple languages. The templates define the required skeleton. Adapt headings to the workspace area, subsystem, and document mode only when that makes the document clearer; preserve document stage, task-list maturity, status, progress, task, verification, maintenance, and evidence fields.

## Document Mode Selection

Choose the mode before writing. If the user does not name a mode, infer the narrowest mode that satisfies the request.

- `analysis-plan`: explain current state, gaps, options, and a task list. Use when the user mainly asks for analysis or best-practice guidance.
- `implementation-plan`: define the concrete implementation path, ownership boundaries, acceptance criteria, and verification. Use for most development task documents.
- `redesign-plan`: define replacement architecture and stop-extending boundaries. Use only when the user asks for redesign, replacement, architecture overhaul, or when evidence inspection shows the current shape should not be extended.
- `debug-plan`: define reproduction, evidence collection, hypotheses, fixes, and regression checks. Use for bug, replay, incident, diagnostic, or troubleshooting tasks.
- `migration-plan`: define old and new contracts, cutover, data handling, compatibility, rollback, and verification. Use for schema, API, configuration, provider, dependency, or storage migrations.
- `testing-plan`: define test scope, test layers, missing coverage, fixtures, regression matrix, and pass criteria. Use when the task is primarily about validation.
- `operations-plan`: define runtime procedures, observability, alerting, deployment, rollback, and operator workflows. Use for operational work.
- `progress-update`: update an existing task document after implementation work. Do the implementation first unless the user explicitly asks only to update the document.

Do not force every task into a redesign shape. Redesign-specific sections such as "legacy structures that must stop expanding" and "replacement boundaries" are required for `redesign-plan`, but should be concise or omitted for ordinary implementation, debug, testing, or operations plans when they do not apply.

Mode-specific minimums:

- `analysis-plan`: include current state, gaps, options considered, selected direction, and a sequenced task list.
- `implementation-plan`: include ownership boundaries, entry points, expected result, acceptance criteria, and required verification.
- `redesign-plan`: include replacement ownership, stop-extending boundaries, rejected alternatives, and migration or cleanup tasks when needed.
- `debug-plan`: include symptoms, reproduction path, evidence to collect, hypotheses, fix strategy, and regression checks.
- `migration-plan`: include old contract, new contract, compatibility rules, cutover, rollback, data handling, and verification.
- `testing-plan`: include test layers, fixtures, missing coverage, regression matrix, and pass/fail criteria.
- `operations-plan`: include runtime procedure, observability, alerting, rollback, manual operator workflow, and failure handling.
- `progress-update`: include actual completion, real verification, deviations from the plan, blockers, and next task.

## Document Lifecycle

Generate and maintain task documents through these lifecycle stages. Record the current stage and task-list maturity in the document itself so later executors do not need this skill to understand how mature the plan is.

Document stages:

- `Draft`: requirements, current-state analysis, target plan, and a coarse task list have been written for user review.
- `Under review`: the user is discussing, challenging, or revising the draft plan.
- `Plan accepted`: requirements, scope, and target plan are settled enough to refine executable tasks.
- `Tasks refined`: the task list has been checked against current workspace evidence and is executable.
- `In execution`: at least one task has started.
- `Completion audit`: tasks appear complete and the document is being checked against actual workspace state.
- `Complete`: implementation, required verification, document updates, and any accepted audit follow-up are complete.

Task-list maturity:

- `Coarse`: high-level task grouping for plan review. Tasks must be plausible, ordered, and evidence-aware, but they do not need full implementation detail.
- `Needs refinement`: the plan is mostly settled, but tasks still need workspace-grounded entry points, blockers, acceptance criteria, and verification detail before execution.
- `Executable`: tasks are sufficiently detailed for future execution with only the task document and current workspace evidence.
- `Execution-maintained`: tasks are being updated after real implementation, verification, deviations, and progress changes.

Lifecycle workflow:

1. Requirements understanding: identify the user's goal, target subsystem, expected output file, language, desired document mode, and whether the user expects review, execution, status output, or completion audit. If the requirement is ambiguous, broad, internally inconsistent, or missing a decision needed for useful analysis, ask the user for clarification before moving on.
2. Current-state analysis: read workspace guidance first, then inspect relevant code, docs, config, command output, process files, or user-provided evidence. Separate observed facts, inferences, and design decisions.
3. Clarification gate: after evidence inspection, decide whether requirements, blockers, conflicts, missing permissions, missing environment, or unclear product choices require user confirmation. If they do, present the blocker or decision questions and wait. If not, continue.
4. Draft document output: prepare task document content with background, goals, user requirements and communication decisions, evidence, current-state analysis, target plan, conclusion, and a coarse task list that fits the actual workspace. Write it only after a destination path is provided, established by workspace guidance, or confirmed by the user. Set document stage to `Draft` and task-list maturity to `Coarse` unless the user explicitly asked for fully executable tasks in one pass, the request itself clearly confirms requirements, scope, and target plan, and no review gate is needed.
5. Document discussion and plan finalization: when the user reviews the draft, update the document through discussion until requirements, scope, rejected alternatives, and target plan are settled. Keep the document update history and communication decisions current. Do not mark the plan accepted until the user explicitly confirms it.
6. Task-list refinement: after the plan is explicitly accepted, re-check relevant workspace evidence and refine the task list into executable tasks. Set task-list maturity to `Executable` only when tasks have concrete entry points, affected scope, blockers, acceptance criteria, required verification, and document-update requirements.
7. Final self-check: inspect the document using the Final Self-Check Rules below. Report issues to the user and update the document when confirmed or directly requested.
8. User final confirmation: after the user confirms the final plan, treat the task document as the execution authority for future implementation, status reporting, completion audits, and progress updates.

## Core Workflow

For document generation or refinement, follow the lifecycle above and this concrete sequence:

1. Identify the target subsystem, user goal, document mode, expected output file, output language, and whether the document will be used for later progress management.
2. Read workspace guidance first. Check `AGENTS.md`, `CLAUDE.md`, `README*`, `docs/`, and any files the user mentions. Do not assume a `task/` directory or existing task documents are present.
3. Select the output template from `references/` based on the requested language.
4. Inspect relevant evidence and code paths before designing. Use fast search and targeted file reads; do not rely on memory or generic architecture assumptions.
5. Separate facts, inferences, and design decisions. Keep facts tied to concrete files, tests, docs, commands, or observed behavior.
6. Stop for user clarification if evidence reveals blockers, unresolved product choices, conflicting requirements, or missing information that would materially change the plan.
7. Fix the task scope, non-goals, and evidence baseline before expanding the task list.
8. Design the target plan using the selected mode and the workspace's actual constraints.
9. Prepare document content with enough background, user requirements, communication decisions, current-state analysis, evidence, target plan, task list, and operating rules that it stands alone.
10. Write the prepared document to the requested path only after the path is provided or established. If no path is given and a task document is clearly needed, use workspace guidance when it defines a task-document location; otherwise propose a sensible Markdown file path in the current workspace and ask the user to confirm before creating the file. Do not infer that `task/` exists unless the user or workspace guidance establishes it.
11. If the user says the document should be self-contained or the sole implementation guide, absorb required context from explicitly relevant prior docs and resolve open options into fixed rules.

Inspect existing task documents only when the user names them, an existing document is the update target, or workspace guidance such as `AGENTS.md` explicitly says task documents are part of the project workflow. When inspecting them, read only documents that overlap with the current target based on path, title, or focused keyword search.

Do not skip evidence reading. For code tasks, inspect the relevant code. For non-code tasks, inspect the relevant docs, configs, command output, process files, or user-provided evidence. The document must be anchored in the actual workspace.

## Workspace Analysis Requirements

Inspect the parts that matter for the selected mode. Choose the relevant subset instead of reading the whole workspace blindly:

- source modules and ownership boundaries
- API/controller/CLI/UI entry points
- service, domain, and business logic
- database schema, migrations, persistence models, and data files
- runtime/cache/state/configuration structures
- logging, audit, metrics, tracing, alerting, and diagnostics
- UI pages, stores, routes, and API clients when user or operator experience is affected
- tests, fixtures, snapshots, and CI workflows that define current behavior
- user-specified or workspace-guidance-required existing task documents that overlap with the request
- deployment, operations, scripts, or infrastructure files when runtime behavior is affected
- docs, configuration, runbooks, process files, or other evidence sources when the target is not primarily code

For fragile runtime or architecture work, inspect the exact behavior owners first, such as:

- selectors, routers, schedulers, dispatchers, or lifecycle hooks
- builders that shape persisted records or user-facing DTOs
- execution paths that call external systems
- governance, quota, auth, billing, retry, fallback, transformation, persistence, or observability boundaries

When analyzing, explicitly separate:

- already implemented behavior
- partially implemented behavior
- missing behavior
- weak or obsolete structures that should be replaced rather than extended
- constraints that must be preserved

If the environment is not code-focused, or the task is mainly documentation, process, configuration, operations, or planning work, inspect the available evidence files and say what evidence sources were available. Do not invent code-specific modules when none exist.

## Evidence Rules

The evidence list is not decorative. Use it to make the task document auditable:

- Every core conclusion must trace to inspected evidence: code when relevant, existing docs, tests, command output, or an explicit user requirement.
- If evidence is missing, write "evidence not found" and name the search scope instead of presenting inference as fact.
- Inferences must be labeled as inferences and kept separate from observed facts.
- Design decisions must state the chosen direction and the rejected alternative when the alternative is plausible during implementation.
- Evidence should be concise. Prefer a few high-value paths and facts over a broad file inventory.
- Stale-plan triggers must be concrete. Name specific files, modules, APIs, configs, docs, requirements, runtime states, or user decisions that require rechecking; do not write vague triggers such as "when code changes".

## Scope Control

Keep the task document executable:

- If the user request is broad, define scope and non-goals before listing tasks.
- Split tasks by verifiable outcomes and durable ownership boundaries, not by file count.
- Avoid giant tasks that cannot be accepted independently.
- Avoid tiny tasks that only rename, reword, or touch one file unless they are part of a meaningful execution boundary.
- Preserve useful existing contracts for non-redesign modes unless evidence or user requirements justify replacement.
- During generation, use `XL` and `XXL` only as internal split signals. If a candidate task is `XL` or `XXL`, split it before writing the final task list. Final task documents must contain only executable `S`, `M`, or `L` task sizes.

## Output Contract

Every standalone task document must include:

- document mode: the selected mode and why it fits
- document stage and task-list maturity: whether the document is draft, under review, accepted, refined, executing, auditing, or complete, and whether tasks are coarse, need refinement, executable, or execution-maintained
- task background: why the work exists and what problem it solves
- applicability and assumptions: what context the plan assumes
- evidence baseline: generation or update date, inspected scope, and stale-plan triggers that require rechecking
- current status: overall state, progress, and next task
- execution and maintenance contract: how future executors should implement tasks and update progress
- task-document operations: how to execute a specified task, output task status, and perform a completion audit without loading this skill
- completion audit records: a dedicated section where confirmed completion checks and accepted follow-up decisions can be recorded
- document stage, task-list maturity, status, task-size, and context-risk scales: definitions that future executors can use without this skill
- user requirements and communication decisions: explicit goals, confirmed decisions, rejected alternatives, and conditions that require user confirmation
- task goal and scope: goals, non-goals, success criteria, and boundaries
- evidence list: concrete sources and observed facts that support the analysis
- current-state analysis: what the project already does and where it fails or falls short
- target plan: desired behavior, ownership, interfaces, data/state, operations, or validation plan as appropriate for the mode
- user, operator, or collaboration guidance: instructions future executors or users need after the change
- workspace-specific implementation guidance: concrete implementation entry points, affected scope, and replacement or preservation boundaries
- risk and verification strategy: what can break and how to verify it
- sequential task list with maturity-appropriate detail; use a coarse task format during draft review and a fully executable task format after task-list refinement

Use the selected template as the concrete skeleton. Do not write a task document that depends on the current conversation for core meaning.

## Progress Management Rules

Allowed top-level and task statuses:

- `Not started` / `未开始`
- `In progress` / `进行中`
- `Blocked` / `阻塞`
- `Partially complete` / `部分完成`
- `Complete` / `已完成`
- `Won't do` / `不再执行`

When creating a new task document:

- Set top-level status to not started unless work has already been completed or blocked.
- Set overall progress to `0%` unless inspected evidence or a user-specified or workspace-guidance-required existing task document proves partial completion.
- Set document stage to `Draft` and task-list maturity to `Coarse` unless the user explicitly requested an execution-ready document, the request itself clearly confirms requirements, scope, and target plan, and the plan has no unresolved review or clarification gate.
- Set next task according to lifecycle stage: user document review for `Draft` or `Under review`, task-list refinement for `Plan accepted`, the first executable task for `Tasks refined` or `In execution`, completion audit for `Completion audit`, `None` for `Complete`, or the blocking item when blocked.
- Add status, progress, actual completion, verification, and notes fields to every task. Coarse tasks should not include execution-only fields such as concrete implementation entry points or acceptance criteria until task-list refinement.

When updating an existing task document after implementation work, do the actual implementation first unless the user explicitly asks only for a document update. Then update:

- top-level current status, overall progress, and next task
- affected task status and progress
- actual completion notes
- verification commands and results that really ran
- deviations from the original plan
- remaining blockers or follow-up work

Partial completion and blocked work must be marked explicitly. If implementation diverges from the original plan, update the document to match the landed code instead of leaving stale instructions.

When executing a specified task from a task document:

- Read the task's status, blockers, unblock conditions, previous tasks, and relevant workspace evidence before editing implementation artifacts.
- If the task is blocked by an unfinished prerequisite, missing decision, missing environment, or stale plan, report the blocker to the user and update the document if requested or confirmed. Do not silently bypass the blocker.
- If the task is executable, implement it, run the required verification when possible, and update actual completion, verification records, progress, deviations, current status, next task, and document update history.

When producing a task status output:

- List every task number, title, and status.
- Compare the document with current workspace evidence when needed; mark stale, inconsistent, or unverifiable statuses explicitly.
- Include the next sensible task or blocker so the user can directly choose what to execute next.
- Prefer this status table shape: task number, task title, document status, actual status judgment, blockers, suggested next action.

When performing a completion audit:

- Inspect the task document and actual workspace state before concluding completion.
- List incomplete work, incorrect implementation, missing verification, deviations from the accepted plan, stale document state, errors, and optimization items.
- Show the audit result to the user first. Do not write the audit record or append follow-up tasks until the user confirms.
- After confirmation, append the audit decision to the document update history and the dedicated completion audit records section, then add any accepted follow-up work as new task numbers. If all tasks are complete and no serious issues remain, report that outcome without inventing follow-up tasks.

## Final Self-Check Rules

Before asking for final user confirmation or treating a task document as execution-ready, check all of these:

- requirement coverage: every explicit user goal, non-goal, constraint, and communication decision is reflected
- evidence coverage: core conclusions trace to inspected code, docs, config, commands, or user requirements
- record relevance: recorded history, decisions, audit notes, and progress notes directly support requirement validation, implementation correctness, verification, or user confirmation
- ambiguity handling: unclear requirements, blockers, and confirmation items are either resolved or recorded in the right document stage
- stage consistency: document stage, task-list maturity, current status, overall progress, and next task agree with each other
- plan consistency: target plan, implementation strategy, risks, verification, and task list do not contradict each other
- task maturity: coarse tasks are clearly marked for review, and executable tasks have concrete entry points, blockers, acceptance criteria, verification, and update requirements
- execution operations: specified-task execution, task status output, and completion audit rules are present in the document
- audit readiness: the document has a dedicated completion audit records section and rules for user confirmation before appending follow-up tasks
- workspace guidance: project guidance, naming conventions, forbidden approaches, and required verification are reflected
- stale-plan triggers: concrete files, modules, APIs, configs, docs, runtime states, or user decisions are named
- placeholder cleanup: no unresolved template placeholders or unused task formats remain in the generated document

## Design Stance

Choose design depth from the selected mode.

- For ordinary implementation, testing, debug, operations, and progress documents, preserve useful existing contracts unless evidence or user requirements justify replacement.
- For redesign documents, default to best-practice end-state design and explicitly state old structures that must stop expanding.
- For migration documents, state compatibility, cutover, rollback, and data handling rules instead of assuming a clean replacement.
- For debug documents, separate symptoms, evidence, hypotheses, fix plan, and regression checks.

Workspace-specific guidance has priority over this skill. If workspace guidance states product constraints, naming conventions, migration requirements, or forbidden features, reflect those constraints directly in the task document.

When the document will guide implementation directly, do not leave unresolved branches. Pick one final direction after evidence inspection unless the user explicitly asks for alternatives.

## Writing Requirements

The document should be concrete, workspace-specific, opinionated, implementation-oriented, and self-contained.

Do not write:

- generic architecture commentary
- a changelog of files
- a compatibility plan unless compatibility is requested, required, or selected by migration mode
- vague brainstorming without decisions

Prefer fixed rules over suggestion language when the document is meant to guide implementation. Draft and under-review documents may record unresolved confirmation questions explicitly, but plan-accepted, executable, and execution-maintained documents should replace open-ended wording such as "could", "maybe", "optional", "consider", "可以考虑", "建议", "可能", "可选", and "首版" with explicit decisions, constraints, or banned alternatives.

Follow the Evidence Rules above instead of repeating ungrounded claims in prose.

When discussing code, reference concrete module paths in prose or in the evidence table, for example:

- `server/src/proxy/core.rs`
- `front/src/pages/Example.vue`
- `src/services/request.ts`
- `migrations/...`
- `.github/workflows/...`

Explain why the current design is insufficient when recommending replacement.

Match the user's language. If the user writes in Chinese, write the document in Chinese unless they request otherwise.

## Task List Rules

Append the task list at the end of execution-oriented documents.

The task list must follow these rules:

1. Tasks are sequential and assume execution in order.
2. Tasks target the selected mode's final desired outcome, not transitional compromise states.
3. Draft and under-review documents should use the coarse task format: numeric identifier, status, progress, priority, difficulty, task size, context risk, goal, evidence or rationale, expected result, known blockers, refinement requirements, actual completion, verification, and notes.
4. Executable task lists must refine coarse tasks into reasonably sized, independently completable tasks using the executable task format.
5. Each executable task includes numeric identifier, status, progress, priority, difficulty, goal, implementation entry points, affected scope, expected result, banned approaches when relevant, execution requirements, acceptance criteria, actual completion, verification, and progress notes.
6. Number tasks in strict ascending order using `1.`, `2.`, `3.` style headings.
7. Make dependencies implicit through order; avoid nested dependency graphs unless necessary.
8. For fragile runtime behavior, name the concrete modules or functions to modify first when task-list maturity is `Executable`.
9. For redesign plans, each executable task must state where to start, what new structure owns the behavior afterward, which old implementation stops being extended, and what is forbidden.

Use the task template from the selected language reference.

## Task Splitting Guidance

Split work by durable execution boundaries, such as:

- discovery and evidence baseline
- goal, contract, or domain model definition
- schema, API, configuration, or data model design
- implementation owner or runtime path
- integration with existing entry points
- UI, CLI, docs, or operator workflow updates
- observability, diagnostics, and audit
- tests, fixtures, regression matrix, and CI
- migration, cutover, rollback, or cleanup
- removal or stop-extending boundaries for redesign work

Avoid superficial task boundaries such as:

- edit file A
- rename field B
- change label text

Small edits are acceptable only when they belong to a larger meaningful unit.

## Completion Checklist

Before finishing, verify:

- the selected document mode and mode selection reason are stated
- the document stage and task-list maturity are stated and match the current lifecycle point
- the execution intent is respected: planning, draft review, task-list refinement, implementation, progress-only update, status output, completion audit, or review
- the selected language template was used or intentionally adapted
- the target task document was actually written when file output was requested and a path was provided or established; otherwise path confirmation was requested before writing
- the design is based on inspected evidence, including code when relevant, not memory alone
- the document explains task background, current state, and target state without relying on conversation history
- the document includes user requirements, communication decisions, rejected alternatives, and any remaining confirmation triggers
- the document includes an evidence baseline and stale-plan triggers
- the evidence list references concrete sources and observed facts
- the final task document contains no unresolved template placeholders such as `<...>` and no template code fences
- new task documents initialize document update history; updates to existing documents append history entries instead of rewriting or deleting previous history
- top-level progress fields exist when the document will manage implementation progress
- document stage, task-list maturity, status, task-size, and context-risk definitions are present in the document, not only in this skill
- task-document operations for specified task execution, status output, and completion audit are present in the final document
- a dedicated completion audit records section is present in the final document
- final self-check rules are satisfied or unresolved issues are shown to the user
- records included in the document are relevant to completing the user's task correctly and are not process noise
- the document has goals, non-goals, analysis, target plan, risks, and verification strategy
- the task list is appended when appropriate and has the right maturity for the current stage
- each task has status, progress, actual completion, verification, and notes fields; executable tasks also have acceptance criteria and concrete entry points
- task ordering is executable in sequence once task-list maturity is `Executable`
- tasks optimize for the selected mode's final outcome, not accidental compatibility
- the document is self-contained for core concepts
- fragile tasks state implementation entry points, replacement or preservation boundaries, and banned alternatives
- plan-accepted, executable, and execution-maintained sections do not leave unresolved "maybe/optional/recommended" branches
