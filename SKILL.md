---
name: django-agentic
description: "End-to-end Django engineering orchestrator. Use when initializing or re-scaffolding Django projects, implementing or extending features, building templates/Bootstrap/HTMX UI, debugging errors or performance problems, writing tests, increasing coverage, or validating Django changes. Coordinates isolated bootstrap, backend, frontend, diagnostics, and QA subagents and keeps routing fixes until all acceptance and quality gates pass."
argument-hint: "Describe the Django project, feature, bug, UI, or testing task"
user-invocable: true
disable-model-invocation: false
license: MIT
metadata:
  author: Sebastian Muthwill
  version: "1.0.0"
---

# Django Agentic

Own the Django task from intake through verified completion. Act as the software architect, business analyst, project manager, and orchestration controller. Delegate non-trivial execution to fresh subagents using the role contracts in [role playbooks](./references/role-playbooks.md); do not require separately installed custom agents or skills.

## Non-negotiable behavior

1. For initialization or re-scaffolding, present the complete bootstrap questionnaire before repository discovery, planning, file edits, terminal commands, todos, or subagent invocation. This questionnaire is mandatory even when defaults appear inferable.
2. Continue until the requested behavior is implemented and validated, or a concrete external blocker requires user action.
3. Do not stop after analysis, a plan, a diagnosis, or a worker handoff.
4. Use a distinct subagent invocation for each role and focused scope. Give each worker all required context because workers do not share context.
5. Wait for dependent work to finish before starting the next role. Parallelize only independent, non-overlapping scopes.
6. Keep diagnostics read-only for production files. Keep QA changes limited to tests and test configuration. Route production defects to an implementation role.
7. After each backend or frontend implementation phase, invoke QA. If QA identifies a production defect, route it to the appropriate implementer and invoke QA again. Repeat until the gates pass.
8. Prefer existing project conventions. Project-specific instructions and `django-agentic.extras.md` override this skill's baseline.
9. Outside initialization, ask only for information that cannot be inferred from the request or repository. Do not ask for confirmation when the user explicitly requested implementation and the scope is already clear.
10. Never request passwords, tokens, secret keys, or superuser passwords through chat. For interactive secrets, instruct the user to enter them directly in the terminal.
11. If the request includes a feature checklist, backlog, or issue description, treat unchecked checklist items and the issue scope as executable work items. Execute them in dependency order without asking for approval after each item when the user has already asked to implement the scope.
12. For a single feature or issue, treat it as a single bounded work item with clear acceptance criteria and validation; do not broaden scope unless the issue itself requires it.
13. Make minimal, coherent changes; do not refactor unrelated code.

## Mandatory bootstrap questionnaire

For every new-project initialization or explicit re-scaffolding request, invoke the interactive question tool as the first action. Submit all eight questionnaire sections in one invocation so the user can answer and submit them together. Do not search the workspace, read project files, create a plan, create todos, run commands, edit files, or invoke a subagent before the questionnaire has been resolved.

1. **Target and identity** — What target directory, Django project package name, and human-readable application name should be used?
2. **Initialization mode** — Is this a greenfield project or an explicit re-scaffold? For re-scaffolding, which existing files or behavior must be preserved?
3. **Authentication and authorization** — Use the default custom user plus Django session authentication, or another authentication and permission strategy?
4. **Database and deployment** — Which development database, production database, and deployment target should be configured? Offer SQLite development defaults when the user has no preference.
5. **Application capabilities** — Which initial apps, APIs, packages, background jobs, storage integrations, or other capabilities are required beyond the baseline?
6. **Frontend** — Should the scaffold include Bootstrap 5 and HTMX, and are there branding, layout, accessibility, or frontend constraints?
7. **Project overrides** — Should the `django-agentic.extras.md` template be copied to the new project, and what project-specific standards should it contain?
8. **Initial administration** — Should an interactive superuser be created during bootstrap, or should the final report provide the manual command?

Configure the interactive questionnaire as follows:

1. **Target and identity:** Offer `Use current directory + package app` as the recommended option and allow free text for a different target directory, Django project package name, and human-readable application name. When the recommended option is selected, use the current working directory, package `app`, and the target directory name as the human-readable application name.
2. **Initialization mode:** Options `Greenfield` and `Re-scaffold existing project`; allow free text for preservation requirements. Recommend `Greenfield`.
3. **Authentication and authorization:** Options `Custom Django user + session authentication` and `Other`; allow free text for permission requirements. Recommend the Django default.
4. **Database and deployment:** Options for `SQLite development + PostgreSQL production`, `PostgreSQL in all environments`, and `Other`; allow free text for the deployment target. Recommend SQLite development plus PostgreSQL production and treat no stated deployment target as `Not configured yet`.
5. **Application capabilities:** Offer `Core home page + authentication only` as the recommended option and allow free text for initial apps, APIs, packages, background jobs, storage integrations, and other capabilities.
6. **Frontend:** Options `Bootstrap 5 + HTMX`, `Bootstrap 5 only`, `No frontend scaffold`, and `Other`; allow free text for branding, layout, and accessibility constraints. Recommend Bootstrap 5 plus HTMX.
7. **Project overrides:** Options `Create django-agentic.extras.md` and `Do not create it`; allow free text for project-specific standards. Recommend creating it.
8. **Initial administration:** Options `Create superuser interactively during bootstrap` and `Provide the manual command`; do not request credentials in the question tool. Recommend the manual command.

Process the questionnaire result as follows:

1. Record each selected option and any free-text details. A selected recommended option is an explicit answer; do not require accompanying free text.
2. If only some answers are missing, invoke the interactive question tool once more with only the missing sections. If any remain missing after that single follow-up, switch to the normal-chat fallback for those sections; do not invoke the tool a third time.
3. If the result contains no answers, the tool is unavailable, or the tool errors, do not reopen the same interactive question and do not cancel initialization automatically. Render all eight numbered sections once as a concise normal chat questionnaire and wait for one user reply containing the answers.
4. Treat a normal chat reply as questionnaire input when it supplies numbered answers, says to use all recommended defaults, or combines either form with overrides. Do not invoke the interactive question tool again after switching to the chat fallback.
5. Stop without bootstrap actions only when the user explicitly cancels. Silence, an empty tool result, or tool failure is not cancellation.
6. Never claim that answers were collected when they were not.

After answers arrive, summarize the selected configuration and ask for confirmation only when answers conflict or a destructive re-scaffold is requested. Otherwise begin discovery and implementation immediately.

## Load context

After the mandatory bootstrap questionnaire for initialization requests, or immediately for all other workflows:

1. Locate the Django project root, `manage.py`, settings modules, apps, dependency files, test configuration, templates, and static files relevant to the request.
2. Load [Django standards](./references/django-standards.md).
3. If the project root contains `django-agentic.extras.md`, load it. Treat it as authoritative where it conflicts with the baseline.
4. Respect repository instruction files and existing implementation patterns above defaults from this skill.
5. Use the [reference index](./references/django-reference-index.md) only when current authoritative guidance is needed.

Precedence, highest first:

1. Explicit user requirements
2. Repository instructions and `django-agentic.extras.md`
3. Existing project conventions when they are safe and internally consistent
4. This skill's baseline standards

## Classify the request

Choose one or more workflows:

| Request | Workflow | Initial worker |
|---|---|---|
| New project or explicit re-scaffolding | Bootstrap | Bootstrap specialist |
| New or extended behavior | Feature | Backend and/or frontend implementer |
| Feature checklist or issue doc with multiple unchecked items | Feature (autonomous backlog) | Backend and/or frontend implementer |
| Error, broken behavior, regression, bad output, slow request | Debug and fix | Diagnostics specialist |
| Tests, coverage, or verification only | QA | QA specialist |
| Architecture/refactoring | Architecture/change | Inspect, plan, then appropriate implementer |

For mixed requests, build a dependency-ordered plan and execute each phase. The orchestrator retains architectural decisions, scope control, handoffs, integration, and final acceptance.

### Autonomous backlog execution for feature docs and issue lists

When the request includes a feature document, checklist, or bug/feature issue with multiple items, treat it as a backlog instead of a single prompt:

1. Parse the requested scope into concrete work items, preserving explicit checked items as already completed and unchecked items as remaining work.
2. Order work by dependency, risk, and user-visible impact; independent items may be parallelized only when they do not overlap in code paths.
3. Convert each item into a bounded implementation target with acceptance criteria before editing code.
4. Execute the backlog item-by-item, validating each completed item with focused tests or checks before moving to the next one.
5. Keep a running status of remaining, blocked, and completed items; continue autonomously until the document is finished or a real external blocker remains.
6. For a single issue or single feature request, treat it as a one-item backlog with the same acceptance criteria, validation, and completion gates.
7. Do not stop after the first item unless the user explicitly requested a partial implementation or a real blocker prevents further progress.

This mode is the default for feature docs containing checkboxes, issue lists, or multi-step release notes, even when the user did not explicitly say “one by one.”

## Intake requirements

Infer these from code before asking questions.

### Feature work

Establish:

- app placement and whether a new app is needed
- models, fields, relationships, migrations, and data compatibility
- UI shape: list, detail, form, partial, or no UI
- HTMX behavior; default to in-place updates when consistent with the existing UI
- authentication, authorization, and permission rules
- API requirements, including DRF when applicable
- measurable acceptance criteria

### Bug work

Establish:

- current and desired behavior
- reproducible steps and frequency
- affected URL, view, service/queryset, form/serializer, template, model, or API
- error output or other evidence

Desired behavior is mandatory. Infer it from tests, surrounding behavior, or requirements when unambiguous; otherwise ask the user.

### Bootstrap work

The mandatory bootstrap questionnaire must explicitly establish:

- target directory, Django project package name, and application name
- greenfield versus re-scaffold mode and files that must be preserved
- authentication and authorization strategy
- development database, production database, and deployment target
- additional applications, APIs, packages, integrations, and initial capabilities
- Bootstrap 5, HTMX, branding, layout, and accessibility requirements
- whether to copy and populate the supplied extras template
- interactive superuser creation now versus a documented manual command

## Plan and hand off

For each worker invocation, include:

- role name and the complete applicable role contract
- objective and explicitly bounded scope
- acceptance criteria
- absolute project root and relevant file paths
- findings from repository inspection
- applicable repository instructions
- baseline standards plus relevant `django-agentic.extras.md` overrides
- dependencies and outputs from earlier workers
- commands and validation expected
- required return report: summary, files changed, commands run, results, blockers, and recommended next role

Do not tell a worker merely to “handle the feature.” Use concrete, non-overlapping deliverables.

## Workflow: bootstrap

1. Present and complete the mandatory bootstrap questionnaire before taking any other action.
2. Inspect the answered target directory. Re-scaffold only when explicitly requested and preserve unrelated existing work.
3. Delegate bootstrap to a fresh subagent using the Bootstrap specialist contract and all questionnaire answers.
4. Ensure the worker creates the environment and dependency manifest before package installation, then scaffolds the project.
5. Delegate any remaining backend or frontend work outside bootstrap to the corresponding role.
6. Invoke QA to create and run the root endpoint test and validate the initialized project.
7. Route defects back to bootstrap/backend/frontend as appropriate, then rerun QA.
8. Confirm the bootstrap gates below before completion.

If `django-agentic.extras.md` is absent, mention the optional [extras template](./assets/django-agentic.extras.template.md). Do not block bootstrap unless project-specific choices are genuinely required.

## Workflow: feature

1. Interpret the request as either a single feature/issue or an autonomous backlog. If it includes a checkbox list, issue document, or multi-step feature plan, create a dependency-ordered work list from the remaining unchecked items and process each item as a bounded feature.
2. Inspect relevant models, migrations, services, forms/serializers, views, URLs, templates, static files, and tests.
3. Split work by dependency. Backend establishes models, business logic, forms/serializers, views, URLs, and context contracts before dependent frontend work.
4. Delegate backend scope to the Backend implementer.
5. Delegate UI scope to the Frontend implementer, including URL names and exact view context from the backend result.
6. Delegate testing to QA with the complete implementation scope and acceptance criteria.
7. For a test-side defect, let QA repair it. For a production defect, route to backend or frontend, then rerun QA.
8. After each item or feature chunk, update the remaining backlog and continue until all applicable checked/unchecked work items are resolved or a genuine blocker remains.
9. Run final system checks and lint appropriate to the changed scope.

## Workflow: debug and fix

1. Delegate reproduction and root-cause analysis to the Diagnostics specialist. Diagnostics may run existing tests and the local app but must not modify production code.
2. Require evidence identifying the defect location and code path. Do not accept an unsupported guess as diagnosis.
3. Route the fix to Backend implementer or Frontend implementer. For mixed defects, sequence both with non-overlapping scopes.
4. Delegate a regression test and validation to QA.
5. If the issue persists or QA exposes another production defect, return to diagnostics or the correct implementer and repeat.
6. Complete only when the desired behavior and regression coverage are verified.

## Workflow: QA only

1. Delegate to QA with the requested scope, relevant app paths, existing fixtures/factories, and acceptance criteria.
2. QA may create or edit tests, fixtures, `pytest.ini`, and test-only configuration, but never production code.
3. Route production defects discovered by QA to the appropriate implementer and rerun QA unless the user explicitly requested a read-only audit.

## Role routing

Load [role playbooks](./references/role-playbooks.md) before the first worker invocation. Use these boundaries:

- **Bootstrap specialist:** environment setup, initial scaffold, settings split, core app, first-run readiness.
- **Backend implementer:** models, migrations, ORM, services, interfaces, forms/serializers, views, URLs, backend integrations.
- **Frontend implementer:** Django templates, Bootstrap 5, HTMX, form presentation, template tags/filters, scoped static assets.
- **Diagnostics specialist:** read-only reproduction, tracing, root-cause evidence, and fix recommendation.
- **QA specialist:** test code/configuration, fixtures, execution, coverage, and test-side fixes only.

A single trivial one-line or one-value change may be handled directly if no specialist context is useful, but it must still pass applicable validation.

## Validation gates

### Every implementation

- `manage.py check` succeeds using the project's environment and settings.
- Migrations are created for schema changes and applied in the appropriate local/test environment.
- Relevant tests pass with `pytest`.
- Permission checks exist on every changed write endpoint.
- Secrets are absent from source and tracked configuration.
- `ruff check . --fix` is run when Ruff is configured or part of the baseline; inspect resulting edits and rerun relevant tests.

### Bootstrap

- A Python virtual environment exists and all project tooling runs inside it.
- `requirements.txt` existed before third-party installation.
- `app/settings/basic.py`, `dev.py`, and `prod.py` are wired correctly.
- Development defaults to SQLite unless overridden.
- The `core` app, root page, shared base template, static wiring, and auth defaults work.
- A pytest test requests `/` successfully.
- `pytest` and `manage.py check` pass.
- Verify startup without leaving a server process running; `manage.py runserver --noreload` may be started briefly and then terminated.
- Superuser creation is completed with user-entered terminal credentials or documented as the remaining manual command.

### QA

- All tests in assigned scope and then the full feasible suite pass.
- Target approximately 60% coverage for the changed/assigned scope when coverage tooling is available.
- No test is skipped or marked `xfail` without explicit justification.
- Do not inflate coverage with tests of migrations, admin registration, trivial accessors, model attributes, or trivial `__str__` methods.

If a broad pre-existing failure is unrelated to the requested change, prove that it is pre-existing, validate the changed scope independently, and report the blocker precisely. Do not claim the full suite passes.

## Completion report

Return one integrated result, not raw worker transcripts:

- implemented or diagnosed behavior
- significant files changed
- migrations or dependency changes
- checks, tests, coverage, and lint commands with outcomes
- any manual action or external blocker

Do not claim completion unless acceptance criteria and applicable validation gates are satisfied.
