# Django Engineering Standards

These are fallback defaults. Explicit user requirements, repository instructions, `django-agentic.extras.md`, and safe existing project conventions take precedence.

## Engineering baseline

- Keep business logic out of views where practical; use app service modules.
- Use migrations for every schema change.
- Keep secrets in environment variables, never in source control.
- Enforce authentication and authorization on every endpoint. Exceptions must be explicit and documented (e.g. public URLs, home page, etc.).
- Use HTMX for dynamic content updates and partial page refreshes where appropriate (e.g. form validation, table updates, modal dialogs).
- Prefer explicit form and serializer validation over implicit assumptions.
- Add pytest coverage for non-trivial behavior and every fixed bug.
- Preserve backwards compatibility unless the task explicitly changes a contract.

## Greenfield dependencies

Create `requirements.txt` before installing dependencies. Select current mutually compatible versions of packages required by the scaffold:

- `django`
- `pytest-django`
- `django-environ`
- `python-dotenv`
- `django-tables`
- `django-bootstrap-form`
- `django-bootstrap-v5`
- `django-tables2`
- `django-filter`
- `psycopg2-binary`
- `ruff`

Do not install an unnecessary package merely because it appears in the baseline. If two packages overlap, retain the package required by existing code and avoid introducing duplicate functionality. Add coverage tooling when scoped coverage is required and it is not already available.

Expected installed app labels for applicable baseline packages include:

- `bootstrapform`
- `bootstrap5`
- `django_tables2`
- `django_filters`

Verify each label against the installed package and current package documentation rather than guessing.

## Greenfield structure

Use this baseline unless overridden:

- `manage.py`
- `app/`
- `app/settings/__init__.py`
- `app/settings/basic.py`
- `app/settings/dev.py`
- `app/settings/prod.py`
- `core/`
- `core/services/`
- `core/static/core/`
- `core/templates/`
- `core/test/`

Additional apps should follow the same service, static, template, and test organization when applicable. Existing projects keep their established `test/` or `tests/` convention.

## Settings and environments

- `basic.py` contains shared settings.
- `dev.py` imports shared settings and defaults to SQLite.
- `prod.py` imports shared settings and requires production-safe secret/database configuration.
- Load environment variables through `django-environ`.
- Keep `.env` ignored. A committed example file must contain placeholders only.
- Never silently use insecure development secrets in production.

## Authentication and authorization

Use Django session authentication unless overridden. For a new project, define the custom user model before initial migrations:

```python
from django.contrib.auth.models import AbstractUser


class User(AbstractUser):
    pass
```

Set `AUTH_USER_MODEL` accordingly. Create a superuser interactively with credentials entered directly by the user, or report the command as a manual follow-up. Never place credentials in source, logs, agent prompts, or chat.

## Templates and UI

Create `core/templates/base.html` for greenfield projects. Other full-page templates extend it. Include:

- HTML language and responsive metadata
- overridable title, head imports/meta, and extra-head blocks
- Bootstrap 5 and HTMX imports consistent with project dependency strategy
- navigation with logo/title and extensible navigation items
- profile/auth area with login/logout states
- main content block
- extensible footer
- body-script and final-script blocks

A compatible block baseline is:

```html
<html lang="en">
<head>
    {% block head-title %}<title>{{ title }}</title>{% endblock %}
    {% block head-imports %}{% endblock %}
    {% block head-meta %}{% endblock %}
    {% block extrahead %}{% endblock %}
</head>
<body>
    <!-- Extensible Bootstrap navigation -->
    {% block body-content %}{% endblock %}
    <!-- Extensible footer -->
    {% block body-scripts %}{% endblock %}
</body>
{% block scripts %}{% endblock %}
</html>
```

Follow existing block names in established projects rather than rewriting their base template. Use dedicated partials for HTMX fragment responses and preserve full-page behavior when a request is not from HTMX.

## Coding conventions

- Stick to python PEP8 and PEP257 conventions. Use `ruff` to check and fix style issues.
- Create a `/services/` module for business logic and data access. Keep views thin and focused on request/response handling. One Service per model is preferred, but multiple services per model are acceptable if the service grows too large or has distinct responsibilities.
- Use `/helpers/` for utility functions that are not tied to a specific model or service. Avoid placing business logic in helpers.
- Use `/interfaces/` for abstract base classes and interfaces. Avoid placing concrete implementations in interfaces.

## Security

- Never commit `.env`, credentials, private keys, tokens, or production connection strings.
- Validate and authorize writes server-side; UI visibility is not authorization.
- Preserve CSRF protection for form and HTMX writes.
- Validate redirects and user-controlled paths.
- Use parameterized ORM operations rather than raw SQL where possible.
- Configure production hosts, secure cookies, HTTPS, and secret management through deployment environment configuration.

## Testing and code quality

- Use `pytest` and `pytest-django`.
- Test non-trivial service logic, edge cases, validation, permissions, views/endpoints, and fixed bugs.
- Aim for approximately 60% coverage of changed scope without artificial tests of boilerplate.
- Do not skip or `xfail` tests without explicit justification.
- Run `ruff check . --fix`, inspect its changes, and rerun relevant tests.
- Run `manage.py check` after implementation.
- Run migrations after schema changes in the appropriate local/test environment.
