# django-agentic extras

Copy this file to the root of your Django project, rename it `django-agentic.extras.md`,
and fill in the sections relevant to your project. Agents will load this file at runtime
and apply it as an override over the plugin baseline (`references/django-standards.md`).
Remove any section you do not need — empty sections have no effect.

---

## Additional required packages

<!-- List any project-specific required packages agents must include. Examples:
- celery + redis for async tasks
- django-storages with Azure Blob Storage for media files
- channels for WebSocket support
-->


## Authentication

<!-- Describe the authentication strategy for this project. Examples:
- Use django-auth-adfs for Azure AD SSO. Login URL: /accounts/login/. All views require login by default via LOGIN_REQUIRED_MIDDLEWARE.
- Use django-allauth with email/password + Google OAuth.
- Use DRF SimpleJWT for API authentication. Web views use session auth.
-->

## Authorization

<!-- Describe the permission model. Examples:
- Use Django's built-in group-based permissions. Assign groups in the admin.
- Use django-guardian for object-level permissions.
-->

## Database

<!-- Override the default SQLite dev / PostgreSQL prod convention if needed. Examples:
- Use PostgreSQL in all environments (dev, staging, prod).
- Use MSSQL via mssql-django.
-->


## Folder structure overrides

<!-- Override or extend the default app folder structure if this project differs. -->

## Deployment target

<!-- Describe the production deployment environment so agents can generate correct configs. Examples:
- Azure App Service with Azure PostgreSQL Flexible Server
- Docker + Kubernetes, secrets via Azure Key Vault
- Heroku with Heroku Postgres
-->

## Additional standards

<!-- Any other project-specific rules, naming conventions, or constraints not covered above. -->
