! Plugin will change to skill soon !
# django-agentic Plugin (GitHub Copilot)

This plugin provides a set of skills and agents for Django development, including project initialization, feature development, and debugging.

> **Note:** Currently tested with Github Copilot only! Give it a try and let me know if it works for you. Report an issue if you encounter any problems.

## Installing the plugin

### In GitHub Copilot CLI


1. Register the marketplace
    ```bash
    copilot plugin marketplace add sebastian-muthwill/django-agentic
    ```

2. Install the plugin
    ```bash
    copilot plugin install django-agentic@django-agentic
    ```

## Extending the plugin with project-specific standards

This plugin ships with general-purpose standards `references/django-standards.md`. 
For project-specific overrides
(authentication strategy, deployment target, extra packages, etc.) the agent should offer you to copy the template file `django-agentic.extras.template.md` to your project root so you can edit it. Otherwise copy it yourself:

```shell
cp <path-to-plugin>/django-agentic.extras.template.md <your-project>/django-agentic.extras.md
```

All agents will automatically load it at runtime and apply it as an override over the
plugin baseline. Project-level rules always take precedence.

## Using the plugin

Create a new folder `mkdir your-project-folder` and change into it `cd your-project-folder`. From within the folder run you GitHub Copilot CLI and invoke the `project-initialization` skill:

```bash
/django-agentic:project-initialization
```

The agent should ask you a few questions about your project and then scaffold a new Django project with the default folder structure, packages, and settings. It will also create a superuser account for you.

Start developing your Django project by invoking the `django-developer` skill for backend feature implementation or the `django-frontend-developer` skill for templates, Bootstrap 5, and HTMX. e.g.:

```bash
/django-agentic:django-developer Create a new app called "blog" where I can write blog articels that are shown on the home page. The blog should have a title, content, and a publication date. The home page should show a list of all blog articles with their title and publication date.
```

