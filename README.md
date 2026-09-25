# django-agentic (GitHub Copilot skill)

This repository contains a GitHub Copilot skill for Django development, including project initialization, feature implementation, frontend work, debugging, and QA. The entry point is the root [SKILL.md](SKILL.md), which defines the `django-agentic` skill and its workflow contracts.

> **Note:** This project is intentionally structured as a skill, not a legacy Copilot plugin. Use the skill through GitHub Copilot chat or the appropriate local skill integration for your environment.

## Using the skill

Add this repository to your Copilot skill setup in the way your environment supports local/custom skills, or keep it in a workspace that Copilot can access. Once available, invoke it in chat with:

```text
/django-agentic
```

You can also start a task directly, for example:

```text
/django-agentic Initialize a new Django project in the current directory with SQLite for development and PostgreSQL for production.
```

The skill will walk through the bootstrap questionnaire, scaffold the project, and continue with implementation, debugging, and validation.

## Project-specific overrides

This skill ships with the general baseline in [references/django-standards.md](references/django-standards.md). For project-specific rules such as auth strategy, deployment target, or extra packages, copy the template in [assets/django-agentic.extras.template.md](assets/django-agentic.extras.template.md) to your project root and save it as `django-agentic.extras.md`:

```shell
cp /path/to/django-agentic/assets/django-agentic.extras.template.md /path/to/your-project/django-agentic.extras.md
```

The skill will automatically load that file and treat it as an override over the baseline rules. Project-level settings always take precedence.

## Example workflows

Initialize a project:

```text
/django-agentic Create a new Django project named "blogsite" with a custom user model, Bootstrap 5, and HTMX support.
```

Implement a feature:

```text
/django-agentic Add a blog app with a title, content, and publication date. Show the latest posts on the home page and keep the UI clean with Bootstrap styling.
```

Debug an issue:

```text
/django-agentic The home page throws a template error after adding a new model. Diagnose the root cause and fix it with a regression test.
```

## Repository layout

- [SKILL.md](SKILL.md): skill entry point and orchestration rules
- [references/django-standards.md](references/django-standards.md): baseline Django rules
- [references/role-playbooks.md](references/role-playbooks.md): role contracts for workers
- [assets/django-agentic.extras.template.md](assets/django-agentic.extras.template.md): optional project-level override template

If you run into issues, open a GitHub issue with the exact failure and the Copilot environment you're using.
