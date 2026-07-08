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

