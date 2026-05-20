---
name: task-generate
description: Generate or revise standalone, self-explanatory, evidence-grounded task documents for planning, review, and task-list refinement. Use when the user explicitly asks to create, write, update, revise, or refine a task document or implementation plan. Do not use for executing tasks, reporting task status, or auditing completion of an existing task document unless the user explicitly invokes `$task-generate`. Use `task/*.md` only when the user or workspace guidance establishes that location. Inspect real workspace evidence before writing.
metadata:
  short-description: Generate evidence-grounded task docs
  version: "0.2.1"
---

# Task Generate

Use this skill to turn a workspace-specific request into either an intermediate planning document or a final standalone execution document.

The skill owns the analysis workflow: evidence gathering, draft generation, requirement clarification, plan review, task-list refinement, and judging whether the document has reached executable maturity. A task document owns execution only after this skill has driven it to the final executable state.

This skill is not for lightweight memos or quick advice. When this skill triggers, default to producing or maintaining a complete, self-contained task document whose task-list maturity matches the current lifecycle stage, unless the user is explicitly asking only for review or discussion.

## Foundational Contract

This contract is the reason the skill exists. Preserve it when updating the skill or templates:

- The skill is the analysis and generation authority. It gathers evidence, resolves requirements, handles plan clarification, performs draft review updates, refines tasks, and judges when a document is executable.
- Task documents have two distinct forms:
  - Intermediate documents are skill-assisted working artifacts. They record facts, open decisions, draft plans, and coarse or unrefined tasks. They are not execution authorities and must be marked clearly as non-executable.
  - Final executable documents are execution authorities. Later implementation, progress updates, plan corrections, completion audits, and document maintenance may happen with only that document loaded, without this skill or the original conversation.
- Intermediate documents must not pretend to be self-sufficient execution guides. If the plan is unclear, facts are incomplete, confirmation items remain, or tasks are not fully refined, the document only needs to mark itself as incomplete/non-executable and name what this skill must resolve next.
- Final executable documents must not contain this skill's generation knowledge. Do not include how to clarify requirements, how to decide whether refinement is complete, template-selection logic, or internal lifecycle workflow. Those are skill responsibilities. The final document should contain the settled facts, confirmed decisions, execution rules, task order, acceptance criteria, verification rules, progress rules, completion-audit rules, and update rules needed for execution.
- Final executable documents must contain deterministic scope decisions. Every candidate task, optimization, cleanup, migration, compatibility rule, and condition that affects execution must be explicitly marked as in scope, out of scope, a blocker with an objective unblock condition, or a pending user decision before finalization. Do not leave execution choices as optional or recommendation wording.
- If a future skill change would make a final executable document depend on the skill, chat history, hidden context, or unresolved options, reject that change.
- If a future skill change would make an intermediate document look executable before the skill has finalized the plan and tasks, reject that change.
- Records in the task document exist only to help the user and future executors complete the requested work correctly and without deviation. Do not add audit trails, process notes, or intermediate records that do not help validate requirements, decisions, evidence, progress, verification, or user confirmation.
- Standalone does not mean verbose. The final document should be a compact execution control surface: keep every rule needed for later execution, but consolidate repeated status definitions, maintenance rules, confirmation gates, and self-check rules into one coherent place instead of scattering similar prose across sections.

Keep rule ownership clear when updating this skill or its templates:

- `SKILL.md` owns generation behavior: intent classification, evidence gathering, mode selection, scope control, task splitting, and completion checks.
- The templates own both document shapes: a minimal non-executable intermediate shape, and a final executable shape with execution rules, progress maintenance, status scales, verification record rules, and update history that future executors need without loading this skill.
- Do not remove execution and maintenance rules from templates just to reduce repetition. Necessary repetition between this skill and templates is acceptable when it preserves the standalone document contract.
- If a rule appears both here and in a template, keep the meaning consistent. If the rule only affects generation, keep it here. If the rule must guide later execution, make sure it is present in the final document template.
- Do not put generation-only decision trees into task documents. Ambiguity-detection criteria, template-selection logic, refinement-completeness judgment, and internal lifecycle workflow belong in this skill. Intermediate documents should only say they are non-executable and list the unresolved items; final documents should record the resulting confirmed decisions, operating rules, and task-specific blockers.

## Decision Flow Summary

Use this routing table before selecting a template or editing files. It summarizes the detailed rules below without replacing them.

| User intent | Default action | Document shape | File edits |
| --- | --- | --- | --- |
| Create or revise a task document | Inspect workspace evidence, resolve ambiguity, then write or update the requested document after the path is known | Intermediate unless the one-pass executable gate is satisfied | Task document only |
| Review, challenge, or finalize a draft plan | Update the intermediate document until requirements, scope, rejected alternatives, and target plan are settled | Intermediate | Task document only |
| Refine tasks after explicit plan acceptance | Re-check pending decisions and current evidence before replacing coarse tasks with executable tasks | Executable only after all gates pass | Task document only |
| Execute a task from an executable document | Implement first, run required verification when possible, then update progress fields and records | Existing executable document | Implementation artifacts and task document |
| Progress-only update or status output | Inspect supplied evidence and current workspace state; report or update only verifiable status | Existing document | Task document only for progress updates |
| Completion audit | Inspect the document and workspace, report findings first, and wait for user confirmation before writing audit records or adding follow-up tasks | Existing executable document | No write until confirmation |
| Review or improve this skill, a template, or a task document | Report findings, tradeoffs, and proposed changes unless the user explicitly asks to modify files | Not applicable | No edits unless requested or confirmed |
| Apply confirmed changes to a task document, template, or skill | Make scoped edits after the change set is requested or confirmed | Existing target shape | Requested files only |

## Execution Intent

Apply these detailed intent rules after the routing summary and before acting:

- Planning or document generation: inspect workspace evidence and write or update the task document. If the plan or tasks are not final, produce an intermediate non-executable document. Do not change implementation artifacts unless the user also asks for implementation.
- Draft review and plan finalization: when the user reviews, challenges, or changes a generated task document, update the intermediate document's requirements, communication decisions, current-state analysis, plan, and coarse task list until the plan is settled. Do not prematurely over-detail the task list before the plan is accepted.
- Task-list refinement: after the plan is explicitly accepted and every user-confirmation or clarification item has been resolved, inspect the actual workspace evidence again and turn the intermediate document into a final executable document with concrete task entry points, blockers, acceptance criteria, verification, and progress fields. A user request for refinement counts only when it clearly confirms the current plan or points to an already accepted plan; it does not override unresolved confirmation questions.
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
- evidence exposes multiple plausible implementation directions, cleanup choices, compatibility choices, or task scopes and there is no workspace evidence or explicit user decision that justifies choosing one

When clarification is required, ask concise questions and wait for the user. Do not hide unresolved choices inside the generated document unless the document stage is explicitly `Draft` or `Under review` and the choices are recorded as confirmation items.

Do not present discretionary branches as executable work. A discretionary branch is any wording that leaves execution authority to decide later between alternatives, such as "could", "maybe", "optional", "recommended", "if desired", "if needed", "consider", "可以", "可考虑", "建议", "推荐", "可选", "可能", "若需要", or equivalent phrasing. During generation, resolve each branch into one of four states: included in the current plan, explicitly out of scope, blocked by an objective condition, or pending user confirmation. If you cannot resolve it from evidence and confirmed requirements, keep or create an intermediate non-executable document and list the choice as a pending decision.

Objective runtime or verification conditions are allowed when they do not create an implementation choice, for example "if verification fails, mark the task blocked and record the failing command." Product, architecture, cleanup, compatibility, or implementation alternatives are not objective conditions and must be decided before the document becomes executable.

Before writing a new task document, confirm the destination path if the user has not already provided an exact path or workspace guidance has not established one. It is acceptable to finish analysis first, propose a path, and ask before creating the file.

Plan acceptance requires explicit user confirmation. Do not move a document to `Plan accepted`, `Tasks refined`, or `Executable` task-list maturity based only on silence or lack of objections. An explicit user request such as "confirm this plan", "finalize it", "refine the accepted plan", or "generate the executable version" counts as confirmation when the plan being accepted is clear.

Task-list refinement has a separate confirmation gate. Before refining tasks or marking task-list maturity as `Executable`, scan the task document for unresolved user-confirmation exceptions, requirement clarification items, open product decisions, unresolved plan review questions, and blockers whose unblock condition is user input. If any exist, do not refine the task list. Present those items to the user in the response, keep the document stage at `Draft`, `Under review`, or `Plan accepted`, keep task-list maturity at `Coarse` or `Needs refinement`, set execution authority to non-executable, and wait for the user's decision.

When creating a new task document that otherwise looks eligible for one-pass executable refinement, apply the same gate. If evidence inspection reveals any unresolved confirmation or clarification item, write or maintain only the intermediate non-executable state, set the next task to user confirmation or task-list refinement after confirmation, and show the unresolved items to the user instead of silently producing executable tasks.

Pending user-decision reminder rule:

- Immediately after generating an initial draft or any intermediate non-executable document, list every pending user confirmation, clarification, open decision, or user-input blocker in the assistant response.
- While a task document still has unresolved user-confirmation, clarification, plan-review, evidence, or product-decision items, every subsequent response about that document must include a concise pending-decision list before suggesting refinement, execution, or other next steps.
- If the user asks for task-list refinement while any pending item remains, refuse the refinement for now, state that the document remains non-executable, and list the unresolved items that must be answered first.
- Each pending-decision list must be concrete enough for the user to answer: include item id or short title, the question or decision needed, why it blocks refinement or execution, known options when available, affected document sections or tasks, and the required user response.
- If the user asks for refinement and no pending item remains, say explicitly that there are no pending user decisions before starting task refinement.

## Template Selection

Choose the document shape before selecting a template:

- Intermediate shape: use while requirements, plan, evidence, user decisions, or task detail are still unresolved. Intermediate documents are non-executable.
- Executable shape: use only after the plan is accepted, all pending user decisions are resolved, evidence has been refreshed, and tasks have been refined enough for execution.

Read only the template matching both output language and document shape:

- Chinese intermediate: `references/template.intermediate.zh.md`
- Chinese executable: `references/template.executable.zh.md`
- English intermediate: `references/template.intermediate.en.md`
- English executable: `references/template.executable.en.md`

For bilingual output, read the matching shape in both languages and preserve equivalent sections.

The selected template defines the required skeleton. Adapt headings to the workspace area, subsystem, and document mode only when that makes the document clearer. Never mix intermediate-only sections into an executable document or executable-only operating rules into an intermediate document.

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

Calibrate complexity before selecting `redesign-plan` or locking in specific technology choices. Large files, duplicated logic, missing tests, or drift between docs and code are evidence of risk, but they do not automatically justify a redesign. Use `redesign-plan` only when the user asks for systemic redesign, replacement, architecture overhaul, or when inspected evidence shows an existing structure should stop being extended. Otherwise, prefer the narrowest mode that can still produce an executable, self-contained plan.

Do not turn an observed best-practice improvement into a fixed implementation choice unless it is supported by user confirmation, workspace guidance, current project direction, or concrete evidence. New dependencies, parser frameworks, interaction models, storage formats, platform-support decisions, cleanup/removal choices, and compatibility changes must be recorded as confirmed decisions, explicitly rejected alternatives, objective blockers, or pending confirmations before they appear in an executable task list.

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

Generate and maintain task documents through these lifecycle stages. Intermediate documents record stage and task-list maturity because the skill is still driving them. Final executable documents use compact execution-control fields instead of exposing the full draft/review lifecycle.

Document stages:

- `Draft`: an intermediate non-executable document has requirements, current-state analysis, target plan, and a coarse task list for user review.
- `Under review`: an intermediate non-executable document is being discussed, challenged, or revised.
- `Plan accepted`: an intermediate non-executable document has settled requirements, scope, and target plan, but still needs task-list refinement before execution.
- `Tasks refined`: the document has been finalized as an executable execution authority; the task list has been checked against current workspace evidence and is executable.
- `In execution`: at least one task has started.
- `Completion audit`: tasks appear complete and the document is being checked against actual workspace state.
- `Complete`: implementation, required verification, document updates, and any accepted audit follow-up are complete.

Task-list maturity:

- `Coarse`: non-executable high-level task grouping for plan review. Tasks must be plausible, ordered, and evidence-aware, but they do not need full implementation detail.
- `Needs refinement`: non-executable state where the plan is mostly settled, but tasks still need workspace-grounded entry points, blockers, acceptance criteria, and verification detail before execution.
- `Executable`: final executable state where tasks are sufficiently detailed for future execution with only the task document and current workspace evidence.
- `Execution-maintained`: tasks are being updated after real implementation, verification, deviations, and progress changes.

Lifecycle workflow:

1. Requirements understanding: identify the user's goal, target subsystem, expected output file, language, desired document mode, and whether the user expects review, execution, status output, or completion audit. If the requirement is ambiguous, broad, internally inconsistent, or missing a decision needed for useful analysis, ask the user for clarification before moving on.
2. Current-state analysis: read workspace guidance first, then inspect relevant code, docs, config, command output, process files, or user-provided evidence. Separate observed facts, inferences, and design decisions.
3. Clarification gate: after evidence inspection, decide whether requirements, blockers, conflicts, missing permissions, missing environment, or unclear product choices require user confirmation. If they do, present the blocker or decision questions and wait. If not, continue.
4. Draft document output: use the intermediate template to prepare a non-executable document with background, goals, user requirements and communication decisions, evidence, current-state analysis, target plan, conclusion, and a coarse task list that fits the actual workspace. Write it only after a destination path is provided, established by workspace guidance, or confirmed by the user. Set document stage to `Draft`, task-list maturity to `Coarse`, and execution authority to non-executable unless the user explicitly asked for fully executable tasks in one pass, the request itself clearly confirms requirements, scope, and target plan, no review gate is needed, and no confirmation or clarification item remains. If unresolved confirmation items exist, record them in the document and immediately present the pending-decision list to the user; do not proceed to executable tasks.
5. Document discussion and plan finalization: when the user reviews the draft, update the document through discussion until requirements, scope, rejected alternatives, and target plan are settled. Keep the document update history and communication decisions current. Do not mark the plan accepted until the user explicitly confirms it.
6. Task-list refinement: after the plan is explicitly accepted, first eliminate all unresolved user-confirmation exceptions, requirement clarification items, open product decisions, unresolved plan review questions, and user-input blockers. If any remain, refuse refinement for now, show the pending-decision list to the user, keep the document non-executable, and stop before task refinement. If none remain, state that no pending user decisions remain, re-check relevant workspace evidence, and refine the task list into executable tasks. Only after every task has concrete entry points, affected scope, blockers, acceptance criteria, required verification, and document-update requirements, run the Final Executable Cleanup Pass below and convert the document to the executable template.
   During refinement, also run a scope-decision pass: identify every candidate improvement, cleanup, migration, compatibility choice, removal, preservation rule, and test/verification expansion raised by the evidence or prior discussion. Assign each item to "included now", "explicitly not included", "objective blocker", or "pending user decision". Do not finalize the document while any item remains as "maybe", "optional", "recommended", or equivalent suggestion wording.
7. Final self-check: inspect the document using the Final Self-Check Rules below. Report issues to the user and update the document when confirmed or directly requested.
8. User final confirmation: after the user confirms the final executable document, treat that document as the execution authority for future implementation, status reporting, completion audits, and progress updates.

## Core Workflow

For document generation or refinement, follow the lifecycle above and this concrete sequence:

1. Identify the target subsystem, user goal, document mode, expected output file, output language, and whether the document will be used for later progress management.
2. Read workspace guidance first. Check `AGENTS.md`, `CLAUDE.md`, `README*`, `docs/`, and any files the user mentions. Do not assume a `task/` directory or existing task documents are present.
3. Select exactly one output template from `references/` based on the requested language and document shape.
4. Inspect relevant evidence and code paths before designing. Use fast search and targeted file reads; do not rely on memory or generic architecture assumptions.
5. Separate facts, inferences, and design decisions. Keep facts tied to concrete files, tests, docs, commands, or observed behavior.
6. Stop for user clarification if evidence reveals blockers, unresolved product choices, conflicting requirements, or missing information that would materially change the plan. When this happens during creation of a document that could otherwise be executable, write only an intermediate non-executable document and present the confirmation items to the user. Continue presenting the pending-decision list in every later response about the document until the items are resolved.
7. Fix the task scope, non-goals, and evidence baseline before expanding the task list.
8. Design the target plan using the selected mode and the workspace's actual constraints.
9. Prepare document content using the selected shape: intermediate documents may keep clarification and review context; executable documents keep only settled requirements, confirmed decisions, explicit in-scope and out-of-scope decisions, execution-relevant evidence, target plan, global operating rules, and refined tasks.
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
- Intermediate documents may record evidence gaps and exploration needed for the next skill-assisted step. Final executable documents must keep only execution-relevant evidence: facts that justify the target plan, task boundaries, risk controls, or required verification.
- Do not turn final evidence sections into command or search logs. Put actual verification command results in verification records; omit detailed exploration traces unless they directly change an implementation decision.
- Stale-plan triggers must be concrete. Name specific files, modules, APIs, configs, docs, requirements, runtime states, or user decisions that require rechecking; do not write vague triggers such as "when code changes".

## Scope Control

Keep the task document executable:

- If the user request is broad, define scope and non-goals before listing tasks.
- Split tasks by verifiable outcomes and durable ownership boundaries, not by file count.
- Avoid giant tasks that cannot be accepted independently.
- Avoid tiny tasks that only rename, reword, or touch one file unless they are part of a meaningful execution boundary.
- Preserve useful existing contracts for non-redesign modes unless evidence or user requirements justify replacement.
- During generation, use `XL` and `XXL` only as internal split signals. If a candidate task is `XL` or `XXL`, split it before writing the final task list. Final task documents must contain only executable `S`, `M`, or `L` task sizes.
- Resolve scope before splitting tasks. If a candidate item is not selected for the current plan, write it as explicitly out of scope or a rejected alternative, not as a future option inside a task.
- Prioritize real failing gates, confirmed requirements, release/deployment boundaries, and user-visible behavior risks before cosmetic cleanup or large structural moves.
- Judge task quality by execution coverage and control, not by task count. A strong final task list shows how the sequence reaches the final goal, which risks are controlled before high-risk changes, and how completion will be verified.
- Calibrate the task distribution before writing detailed tasks. A typical complex executable plan should consider these phases in order: baseline or behavior contract; P0 failing gates, user-visible risks, release/deployment boundaries, or current quality gates; shared foundations and ownership boundaries; core implementation or module changes; test matrix, regression, performance, or safety guards; documentation, release, and completion audit. Not every document needs every phase, but final executable documents should make skipped high-risk phases explainable through scope, non-goals, or evidence.
- Establish a baseline or guard task before high-risk refactors when current tests, outputs, schemas, release packages, or behavior contracts are weak. For low-risk local changes, do not inflate the plan with unnecessary baseline or redesign tasks.
- Split enabling foundations by independently verifiable ownership boundaries. For example, CLI/path helpers, time/range parsing, shared schemas, storage ownership, report formatting, and test matrix work should be separate tasks when they have different evidence, risk, or verification.
- Split large module refactors by behavior layers that can be verified independently, such as input parsing, domain model/schema, scanning or IO, accumulation/aggregation, reporting DTOs, formatting, and command glue.

## Output Contract

All task documents must include:

- execution authority: `Non-executable` for intermediate documents or `Executable` for final documents
- document mode and the reason it fits
- evidence baseline, inspected scope, and concrete stale-plan triggers
- current status, progress, next action, task background, goals, non-goals, success criteria, scope, evidence summary, current-state analysis, target plan, risk and verification strategy, and a sequential task list

Intermediate non-executable documents must also include:

- document stage and task-list maturity because the skill still owns lifecycle progression
- a prominent non-executable notice stating that no task may be executed from the document
- original user goal, requirement clarification record, plan review record, and pending user-decision details when they are needed for review
- unresolved confirmation, clarification, evidence, or refinement items with concrete questions, why each item blocks refinement or execution, options when known, affected scope, and required user response
- only coarse or needs-refinement tasks; do not include execution operations, completion-audit rules, full executable-task maintenance rules, or final-document self-check content

Final executable documents must also include:

- compact execution control fields: execution authority, current status, status note, overall progress, next task, evidence baseline, inspected scope, and stale-plan triggers
- execution and maintenance contract: how future executors implement tasks, output task status, perform completion audits, and update progress without loading this skill
- decision summary: confirmed requirements, implementation constraints, still-effective rejected alternatives, and `pending confirmations: none`; do not keep resolved confirmation tables or review timelines
- scope decisions: what is included in the current plan, what is explicitly not included, and which candidate alternatives were rejected; final documents must not leave unresolved optional branches
- compact evidence summary: high-value facts that justify the plan and task boundaries, plus task-level evidence references when useful
- global execution rules, global verification rules, and global banned approaches so task entries can inherit shared constraints
- completion audit records for confirmed completion checks and accepted follow-up decisions
- executable tasks with concrete entry points, affected scope, task-specific blockers, task-specific acceptance criteria, verification, actual completion, and progress notes

Use the selected template as the concrete skeleton. Do not write a final executable document that depends on the current conversation for core meaning. Intermediate documents may depend on this skill for further clarification and refinement, but must say so clearly and must not be executable. Keep final documents lean by grouping shared rules into the template's control and global-rules sections and by avoiding repeated copies of the same rule in each task.

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
- For intermediate documents, set document stage to `Draft`, task-list maturity to `Coarse`, execution authority to non-executable, and pending user-decision count to the number of unresolved confirmation, clarification, review, evidence, or product-decision items.
- For one-pass executable documents, use the executable template only when the user explicitly requested execution-ready output, the request itself clearly confirms requirements, scope, and target plan, and no unresolved review, clarification, or user-confirmation gate remains.
- If pending user-decision count is greater than zero, the next task must be user confirmation.
- Set next task according to lifecycle stage unless pending user decisions remain: user document review for `Draft` or `Under review`, task-list refinement for `Plan accepted`, the first executable task for `Tasks refined` or `In execution`, completion audit for `Completion audit`, `None` for `Complete`, the blocking item when blocked, or user confirmation when pending decisions remain.
- Add status, progress, actual completion, verification, and notes fields to every task. Coarse tasks should not include execution-only fields such as concrete implementation entry points or acceptance criteria until task-list refinement.
- Write the top-level current status using only the allowed status scale. Put explanatory prose in a separate status note or equivalent field in the document template.

When refining a task list, use this ordered update sequence. The first two passes protect the non-executable state; the last two passes finalize only after task bodies are complete:

1. Gate and evidence pass: confirm the plan has been accepted, every user-confirmation or clarification item is resolved, and relevant workspace evidence has been refreshed.
2. Task-body pass: fully replace or update the individual task entries first. During this pass, do not update the top-level document stage, task-list maturity, current status, next task, task size summary, final conclusion, or any equivalent field to claim executable/refined state. If an interruption happens here, the document must still read as not yet executable.
3. Final executable cleanup pass: convert the document to the executable template and keep only settled facts, confirmed decisions, execution-relevant evidence, global operating rules, refined tasks, verification rules, and maintenance records needed for future execution.
4. Finalization pass: only after every task entry is complete and the cleanup pass is done, update execution authority, top-level status, status note, overall progress, next task, task size summary, final conclusion, and document update history. Then run the Final Self-Check Rules.

If a tool, network, context, or editing failure interrupts task refinement before finalization, leave the document in `Plan accepted` / `Needs refinement` or an explicitly blocked/in-progress state, keep execution authority as non-executable, and record what remains. Never leave document-level metadata claiming `Tasks refined`, `Executable`, or execution authority while any task still uses a coarse or incomplete format.

## Final Executable Cleanup Pass

Run this pass whenever task-list refinement produces a final executable document. Remove generation-stage scaffolding without losing decisions that still affect execution.

Remove or compress from final executable documents: original request restatements that do not add constraints; requirement clarification and plan review timelines; resolved `CONF-*` tables; original blocking questions; skill-assisted next actions; refinement gates; template-selection notes; "current task format" labels; coarse-task instructions; final self-check checklists; draft/review lifecycle definitions; discretionary branch wording; and generation-phase update history unless collapsed into one execution-relevant baseline entry.

Keep or move useful content: confirmed requirements and settled decisions go into the decision summary; included and excluded scope decisions go into the target plan or decision summary; rejected alternatives remain only when they constrain implementation; user decisions that affect execution become implementation constraints, not dialogue history; evidence remains only when it supports the target plan, task boundaries, risk controls, or required verification; update history keeps execution progress, verification results, deviations, completion audits, and a compact "plan accepted and tasks refined" baseline when needed.

If any pending confirmation remains, do not create the final executable document. Keep the intermediate template and show the pending-decision list to the user.

## Execution-Maintenance Operations

Use these rules when the user asks to execute, update, report status, or audit an existing task document. They are runtime maintenance rules for already-created documents, not generation-stage cleanup rules.

When updating an existing task document after implementation work, do the actual implementation first unless the user explicitly asks only for a document update. Then update:

- top-level current status, overall progress, and next task
- top-level status note when the template includes one
- affected task status and progress
- actual completion notes
- verification commands and results that really ran
- deviations from the original plan
- remaining blockers or follow-up work

Partial completion and blocked work must be marked explicitly. If implementation diverges from the original plan, update the document to match the landed code instead of leaving stale instructions. After progress updates, scan document-wide prose for stale status phrases such as "next", "still needs", "pending", "not run", "not started", "needs discussion", "could", "maybe", "optional", "建议", "仍需", "待", "未运行", and "未开始". Keep only phrases that are still true; otherwise update current-state analysis, target plan, final conclusion, follow-up notes, and task-list summary to match reality.

When executing a specified task from a task document:

- Read the task's status, blockers, unblock conditions, previous tasks, and relevant workspace evidence before editing implementation artifacts.
- If the task is blocked by an unfinished prerequisite, missing decision, missing environment, or stale plan, report the blocker to the user and update the document if requested or confirmed. Do not silently bypass the blocker.
- If the task is executable, implement it, run the required verification when possible, and update actual completion, verification records, progress, deviations, current status, status note when present, next task, and document update history.

When producing a task status output:

- List every task number, title, and status.
- Compare the document with current workspace evidence when needed; mark stale, inconsistent, or unverifiable statuses explicitly.
- Include the next sensible task or blocker so the user can directly choose what to execute next.
- Prefer this status table shape: task number, task title, document status, actual status judgment, blockers, next action.

When performing a completion audit:

- Inspect the task document and actual workspace state before concluding completion.
- List incomplete work, incorrect implementation, missing verification, deviations from the accepted plan, stale document state, errors, and optimization items.
- Show the audit result to the user first. Do not write the audit record or append follow-up tasks until the user confirms.
- After confirmation, append the audit decision to the document update history and the dedicated completion audit records section, then add any accepted follow-up work as new task numbers. If all tasks are complete and no serious issues remain, report that outcome without inventing follow-up tasks.

## Final Self-Check Rules

Use this as the document readiness gate. Do not paste this checklist into generated task documents; final executable documents get their ongoing execution rules from the selected executable template.

Before asking for final user confirmation or treating a task document as execution-ready, check all of these:

- requirement coverage: every explicit user goal, non-goal, constraint, and communication decision is reflected
- evidence coverage: core conclusions trace to inspected code, docs, config, commands, or user requirements
- record relevance: recorded history, decisions, audit notes, and progress notes directly support requirement validation, implementation correctness, verification, or user confirmation
- ambiguity handling: unclear requirements, blockers, and confirmation items are either resolved or recorded in the right document stage
- stage consistency: document stage, task-list maturity, current status, overall progress, and next task agree with each other
- plan consistency: target plan, implementation strategy, risks, verification, and task list do not contradict each other
- task maturity: coarse tasks are clearly marked for review, and executable tasks have concrete entry points, blockers, acceptance criteria, verification, and update requirements
- execution authority consistency: intermediate documents are explicitly non-executable, and final documents are executable only after plan, facts, confirmations, and task refinement are complete
- confirmation gate: no unresolved confirmation exception, clarification item, open product decision, unresolved review question, or user-input blocker remains before task-list maturity becomes executable
- pending-decision reminder: initial draft responses and every subsequent response about a non-executable document list pending user decisions; refinement requests are refused while any pending decision remains
- refinement atomicity: document-level stage, maturity, status, next task, task format summary, final conclusion, and update history were finalized after all individual tasks were refined, not before
- execution operations: specified-task execution, task status output, and completion audit rules are present only in final executable documents
- generation-knowledge cleanup: final executable documents do not contain original-goal restatements, clarification timelines, plan review timelines, resolved confirmation tables, `CONF-*` details, skill-assisted next actions, refinement gates, "current task format" labels, coarse-task format instructions, or final self-check checklists
- audit readiness: final executable documents have a dedicated completion audit records section and rules for user confirmation before appending follow-up tasks
- workspace guidance: project guidance, naming conventions, forbidden approaches, and required verification are reflected
- stale-plan triggers: concrete files, modules, APIs, configs, docs, runtime states, or user decisions are named
- placeholder cleanup: no unresolved template placeholders or unused task formats remain in the generated document
- stale status cleanup: document-wide prose has been checked for stale next-step, pending-work, not-run, not-started, optional, or recommendation wording that contradicts the current stage or task state
- deterministic scope cleanup: final executable documents contain no discretionary implementation branches. Every candidate task, optimization, cleanup, migration, compatibility rule, and test expansion is either included, explicitly excluded, objectively blocked, or absent because evidence did not support it.
- task distribution quality: the final task sequence covers the required path from baseline or known starting state to final audit, prioritizes real failing gates and user-facing or release risks before cosmetic cleanup, and does not skip a high-risk phase without a scope or evidence reason.
- task boundary quality: each executable task has one primary outcome, can record actual completion and verification independently, and does not mix behavior change, broad refactor, performance optimization, cleanup, and documentation unless those are inseparable and the task boundary explains why.
- verification proportionality: global verification rules define task-type triggers, task-level checkpoints name the checks actually required for that task, and the final audit task or final checkpoint covers full verification or explains why it is not applicable.

## Design Stance

Choose design depth from the selected mode.

- For ordinary implementation, testing, debug, operations, and progress documents, preserve useful existing contracts unless evidence or user requirements justify replacement.
- For redesign documents, default to best-practice end-state design and explicitly state old structures that must stop expanding.
- For migration documents, state compatibility, cutover, rollback, and data handling rules instead of assuming a clean replacement.
- For debug documents, separate symptoms, evidence, hypotheses, fix plan, and regression checks.
- Learn reusable document structure from prior or example task documents, not fixed technical conclusions. Parser migrations, interaction libraries, pricing file formats, release target decisions, legacy cleanup, and similar concrete implementation choices must be justified by the current task's evidence or confirmed decisions, not copied from another document because it looked well structured.

Workspace-specific guidance has priority over this skill. If workspace guidance states product constraints, naming conventions, migration requirements, or forbidden features, reflect those constraints directly in the task document.

When the document will guide implementation directly, do not leave unresolved branches. Pick one final direction after evidence inspection unless the user explicitly asks for alternatives. If the user asks for alternatives, keep the document intermediate and non-executable until one alternative is selected.

## Writing Requirements

The document should be concrete, workspace-specific, opinionated, implementation-oriented, and self-contained.

Do not write:

- generic architecture commentary
- a changelog of files
- a compatibility plan unless compatibility is requested, required, or selected by migration mode
- vague brainstorming without decisions
- bulky restatements of the skill's generation workflow
- duplicated rule blocks when one compact operating rule or scale table covers the requirement

Prefer fixed rules over suggestion language when the document is meant to guide implementation. Draft and under-review documents may record unresolved confirmation questions explicitly, but executable and execution-maintained documents must replace open-ended wording such as "could", "maybe", "optional", "recommended", "consider", "if desired", "if needed", "可以", "可以考虑", "建议", "推荐", "可能", "可选", "若需要", and "首版" with explicit decisions, constraints, blockers, or banned alternatives. Final executable documents must not keep a "needs user confirmation" item, except the fixed statement that pending confirmations are none.

Prefer tables for document control, decision registers, evidence, verification, update history, and audit records. Prefer short bullets for operating rules and task constraints. Use prose for background, current-state analysis, and target-plan rationale where narrative actually improves understanding.

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
5. Each executable task includes numeric identifier, status, progress, priority, difficulty, goal, scope decision, implementation entry points, affected scope, expected result, task-specific blockers, acceptance criteria, actual completion, verification, and progress notes.
6. Number tasks in strict ascending order using `1.`, `2.`, `3.` style headings.
7. Make dependencies implicit through order; avoid nested dependency graphs unless necessary.
8. For fragile runtime behavior, name the concrete modules or functions to modify first when task-list maturity is `Executable`.
9. For redesign plans, each executable task must state where to start, what new structure owns the behavior afterward, which old implementation stops being extended, and what is forbidden.
10. For migration, cross-cutting, or dependency-order-sensitive work, each executable task must state whether the workspace is expected to compile or pass core checks after that task. If temporary red status is allowed, name the exact allowed failing checks, why they are acceptable, and which later task must restore them.
11. Put shared banned approaches, execution requirements, verification requirements, and document-update rules in global sections of the final executable document. In individual executable tasks, write "inherits global rules" plus only task-specific additions when no special rule is needed.
12. During task-list refinement, cross-check every implementation entry point, affected file, and verification command against the inspected evidence baseline. Existing files or commands must have been inspected; planned new files or commands must be explicitly marked as new.
13. Executable tasks must not contain alternative implementation branches. If a task mentions a candidate action, it must say whether that action is included in the task, excluded from the task, or blocked by a concrete prerequisite. If the choice needs user judgment, move it to pending confirmations and keep the document non-executable.
14. Final executable task lists should start with a compact task overview table before detailed task entries when there are more than three tasks. The overview should show task number, title, priority, size, risk, phase, and main verification so executors can see the whole path at once.
15. Use task phases as a planning aid, not as bureaucracy. Suggested phases are baseline, user/release boundary, shared foundation, core implementation, testing/verification, and documentation/audit. If a different set fits the selected mode better, use that set consistently.
16. Each executable task should have one primary outcome. Split a task when it combines independently verifiable behavior changes, structural refactors, performance optimization, cleanup, documentation, or release changes.
17. A task may combine multiple kinds of work only when they are inseparable for correctness or verification. When combining them, the task boundary decision must say why they belong together.
18. Each task must have a clear post-task workspace state: core checks pass, a temporary red state is explicitly allowed with named failing checks and a restoring task, or the task remains blocked.
19. Global verification rules should define which task types trigger which verification families, such as code, release/package, CLI/UI behavior, data safety, migration, performance, documentation, or operations. Task-level verification checkpoints should be specific instead of mechanically copying every global command.
20. Include a final audit task by default for multi-task executable documents. Very small plans may fold the audit into the last task only when that task's acceptance criteria explicitly covers final verification, stale-document cleanup, and completion status.
21. The task list must demonstrate final-goal coverage. Do not write a set of local improvements that lacks an explicit path to the user's requested end state and the verification that proves it.

Use the task template from the selected language and document-shape reference.

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

Use this as the end-of-turn checklist for this skill run. It intentionally overlaps the Final Self-Check Rules, but it also checks assistant behavior, file-output decisions, template use, and user-facing responses.

Before finishing, verify these gates:

- Intent and output: the execution intent is respected; the document mode and mode reason are stated; the selected language template was used or intentionally adapted; the file was written only when a path was provided or established, otherwise path confirmation was requested.
- Evidence and self-contained context: the design is based on inspected evidence; facts trace to concrete sources; goals, non-goals, requirements, decisions, rejected alternatives, evidence baseline, stale-plan triggers, current state, target plan, risks, and verification strategy are present; the document does not rely on chat history for core meaning.
- Shape and authority: execution authority matches the document state; intermediate documents expose stage and task-list maturity, stay non-executable, omit execution operations, and name the skill-assisted next action; final executable documents use compact execution-control fields, allowed statuses, progress fields, status/task-size/context-risk definitions, and `pending confirmations: none`.
- Pending decisions and refinement gates: pending user decisions are listed in responses while unresolved; refinement is refused while any pending item remains; task bodies are refined before document-level metadata claims executable or refined state.
- Final executable contract: final documents went through the Final Executable Cleanup Pass, contain no generation-stage scaffolding or unresolved placeholders, keep execution rules in a compact control section, include specified-task execution, status output, completion audit operations, and have a dedicated completion audit records section.
- Task quality: the task list is appended when appropriate, has the right maturity, is ordered for sequential execution, covers the final goal, avoids accidental compatibility work, and each task has status, progress, actual completion, verification, and notes; executable tasks also have concrete entry points, boundaries, blockers, acceptance criteria, verification checkpoints, and no unresolved include/delete/move/keep choices.
- Maintenance cleanup: update history is initialized or appended; records are relevant rather than process noise; final self-check issues are resolved or reported; stale status prose and optional/recommendation wording are removed from executable, execution-maintained, and completion-audit documents.
