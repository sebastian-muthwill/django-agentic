---
name: project-initialization
description: Initialize a new Django project by orchestrating django-software-architect, which delegates bootstrap execution to the django-project-initialization agent and applies plugin and project-level standards.
license: MIT
metadata:
  author: Sebastian Muthwill
  version: "0.2.0"
tools: ['task', 'ask_user']
---

# Project Initialization

Use this skill only for Django project initialization. It orchestrates the full bootstrap
sequence via `django-software-architect`, which delegates to the appropriate subagents.

Use this skill to re-initialize an existing project only if the task is explicitly to recreate bootstrap scaffolding. In this case, check the existing project against the baseline and project-level standards, and apply any missing scaffolding or configuration.

## Required Behavior

1. Invoke `django-software-architect` as the entry point — it will delegate bootstrap
   execution to `django-project-initialization`.
2. Before starting, check for `django-agentic.extras.md` in the project root.
   - If present: load it and pass its contents as project-specific overrides to the architect.
   - If absent: Inform the user about the possibility and offer to copy the template so the user can add extras; else proceed using the plugin baseline from `references/django-standards.md` only.
3. Ensure the following bootstrap rules are applied:
   - Create a Python virtual environment first.
   - Create `.env` file and use it for environment variables (e.g., `DJANGO_SECRET_KEY`, `DATABASE_URL`, etc.).
   - Create `requirements.txt` before installing any packages; install only inside the venv.
   - Scaffold the project using the default packages and folder structure from `references/django-standards.md` unless overridden by `django-agentic.extras.md`.
   - Create a `core` app for shared templates, styling, and basic functionality like a home view, navigation, footer, login and logout flows and pages.
   - Create a `settings/` package with `basic.py`, `dev.py`, and `prod.py` — `dev.py` and `prod.py` inherit from `basic.py`.
   - Configure settings to use `.env` for environment variables and secrets which are automatically loaded by `django-environ`.
   - Configure SQLite for local development unless `django-agentic.extras.md` specifies a different database.
   - Configure authentication as specified in `django-agentic.extras.md`; fall back to Django's default session auth if not specified.
   - Initiate a superuser account with credentials provided by the user or guide the user in the summary how to create one after the bootstrap is complete.
   - Finish with a runnable project where `manage.py runserver` works.

## Available agents in this plugin

| Agent | Role |
|---|---|
| `django-software-architect` | Primary orchestrator — always the entry point |
| `django-project-initialization` | Bootstrap subagent — scaffolds the project |
| `django-developer` | Backend feature implementation after bootstrap |
| `django-frontend-developer` | Templates, Bootstrap 5, HTMX |
| `django-tester` | Write, run, and fix tests (~60% coverage) |
| `django-debug` | Diagnose bugs, report findings and fix suggestions |

## In Scope

1. New greenfield Django project setup.
2. Re-initialization when the task is explicitly to recreate bootstrap scaffolding.

## Out of Scope

1. Feature implementation after bootstrap (models, APIs, business logic, auth flows, admin customization).
2. Refactoring or extending existing project functionality unrelated to initialization.
3. General debugging tasks not directly blocking initial project startup.
4. Deployment/infrastructure work beyond base project bootstrap configuration.
