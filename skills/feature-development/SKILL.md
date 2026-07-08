---
name: feature-development
description: Entry point for new Django feature development. Clarifies requirements, then delegates implementation and subagent orchestration to django-software-architect.
license: MIT
metadata:
  author: Sebastian Muthwill
  version: "0.1.0"
tools: ['ask_user', 'task', 'view', 'glob']
---

# Feature Development

Use this skill as the entry point whenever a new feature needs to be implemented in an
existing Django project. It clarifies requirements before handing off to
`django-software-architect` for implementation and subagent orchestration.

## Required Behavior

### Step 1 — Understand the feature

1. Read the feature description provided by the user. If there is no description, ask the user to provide one.
2. Check for `django-agentic.extras.md` in the project root and load it if present —
   use it to inform clarifying questions and constraints.
3. Review the existing project structure (models, views, URLs, templates) relevant to
   the requested feature before asking questions.

### Step 2 — Clarify requirements

Ask the user targeted questions for anything that is ambiguous or missing. Cover:

- **Scope**: Which app(s) should the feature live in, or should a new app be created?
- **Data**: Are new models or fields required? Any relationships to existing models?
- **UI**: Is a frontend view required? List, detail, form, or a combination?
- **HTMX**: Should any part of the UI update in place without a full page reload? Default: yes.
- **Permissions**: Who can access this feature? Any role or group restrictions?
- **Authentication**: Any auth requirements beyond the project default?
- **API**: Is a REST API endpoint needed alongside or instead of a template-based view?
- **Acceptance criteria**: How does the user define "done" for this feature?

Only ask questions whose answers are not already clear from the feature description or
the project's existing code and `django-agentic.extras.md`. Do not ask for information
you can infer.

### Step 3 — Confirm and hand off

1. Summarize the feature scope and confirmed requirements back to the user.
2. Once the user confirms, delegate to `django-software-architect` with:
   - The full feature description.
   - All clarified requirements and acceptance criteria.
   - Relevant existing file paths identified during Step 1.
   - Contents of `django-agentic.extras.md` if present.
3. `django-software-architect` owns all further planning, subagent delegation, and
   implementation from this point forward.

## In Scope

1. New feature implementation in an existing, initialized Django project.
2. Extending an existing feature with new behavior (new fields, views, endpoints).

## Out of Scope

1. New project setup — use the `project-initialization` skill instead.
2. Bug fixing — describe the bug directly to `django-debug` via the architect.
3. Refactoring without new user-visible behavior.
