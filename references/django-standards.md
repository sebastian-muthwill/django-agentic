# Django Engineering Standards

## Baseline

- Keep business logic out of views where possible.
- Use migrations for every schema change.
- Protect secrets via environment variables.
- Enforce permission checks on every write endpoint.
- Prefer explicit serializer validation over implicit assumptions.
- Add tests via pytest for every class (except model classes) and non-trivial behavior change as well as fixed bug.


## Default requirements

- Add the following detailed requirements in the latest compatible versions
```python
# requirements.txt
django
pytest-django
azure-auth
django-tables
django-bootstrap-form
django-bootstrap-v5
django-tables2
python-dotenv
psycopg2-binary
django-filter
```

## Default folder structure

- Create the following folder structure for the Django project:

    app
    app/settings
    core
    core/services
    core/static
    core/templates
    core/test # contains all tests

    all other apps that are created should follow the same structure as core app


## Default packages

1. install the following packages as default
```
INSTALLED_APPS = [
    ...
    "bootstrapform",
    "bootstrap5",
    "django_tables2",
    "django_filters"]
```

## Default Authentication

- Use Django's built-in authentication system with custom user model. Create the following model in `core/models.py`:

    ```python 
    from django.contrib.auth.models import AbstractUser

    class User(AbstractUser):
        # Custom user model based on Django recommendation
        # https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project
        # This model can then be later extended 
        pass
    ```

- Create a superuser account during project initialization with credentials provided by the user or guide the user in the summary how to create one after the bootstrap is complete.

## Default Security concerns

- never store secrets in code or in the repository. Use environment variables and `.env` file for local development. Use `django-environ` to load environment variables from `.env` file.
- inform the user about the importance of keeping secrets safe and not committing them to the repository. Provide guidance on how to manage production secrets securely (e.g., using a secret manager or environment variables in the deployment environment).

## Default UI styling and template patterns

- Create a `base.html` template in `core/templates` that acts as a scaffold and lets other templates extend it. All other templates should extend this base template. Use the following structure for templates stick to design patterns and conventions for Bootstrap 5 and HTMX.:
- create a main navigation bar with a placeholder for a logo, title, and nav items that can be extended by other templates.
- create a right navigation profile item (dropdown) with placeholders for user authentication status (login/logout) as well as placeholder that can be extended by other templates.
- create a main content area that can be extended by other templates.
- create a footer with placeholders for copyright notice, links, and other information that can be extended by other templates.

```html
<html lang="en">
    
<head>
    {% block head-title %}<title>{{ title }}</title>{% endblock %}
    
    ... default imports for Bootstrap 5, HTMX, and other libraries ...

    {% block head-imports %}{% endblock %}
    
    {% block head-meta %}{% endblock %}

    {% block extrahead %}{% endblock %}

</head>
<body>
    ... navbar that items can be extended by other templates, add title, logo placeholder, nav items ...

    {% block body-content %}{% endblock %}

    ... footer that can be extended by other templates, add copyright notice, links, and placeholders for other information ...

    {% block body-scripts %}{% endblock %}
    
</body>
    ... default scripts ...
    {% block scripts %}{% endblock %}
</html>
```

## Default code quality and testing

- Use `ruff` for code quality checks. Run `ruff check . --fix` command to check for code quality issues and resolve any issues reported by `ruff` before committing code.
- Use `pytest` and `pytest-django` for testing. Create a `tests/` folder inside each app that contains all tests.
- Run tests with `pytest` command. Ensure that all tests pass before committing code.
