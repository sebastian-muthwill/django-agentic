# django-agentic Plugin (GitHub Copilot)

This folder is initialized as a GitHub Copilot plugin workspace for the `django-agentic` plugin.

## Structure

- `plugin.json` — required plugin manifest
- `skills/` — Django development skills
- `agents/` — custom agents
- `references/` — reusable technical references and standards

## Included

- Skill: `skills/project-initialization/SKILL.md`
- Skill: `skills/feature-development/SKILL.md`
- Skill: `skills/debug-fix-issue/SKILL.md`
- Agents:
  - `agents/django-software-architect.agent.md` — primary orchestrator
  - `agents/django-project-initialization.agent.md` — subagent: new project bootstrap
  - `agents/django-developer.agent.md` — subagent: feature implementation
  - `agents/django-frontend-developer.agent.md` — subagent: templates, Bootstrap 5, HTMX
  - `agents/django-tester.agent.md` — subagent: write, run, and fix tests (~60% coverage)
  - `agents/django-debug.agent.md` — subagent: diagnose bugs, report findings and fix suggestions
- References:
  - `references/django-reference-index.md`
  - `references/django-standards.md`

## Extending the plugin with project-specific standards

This plugin ships with general-purpose Django standards. For project-specific overrides
(authentication strategy, deployment target, extra packages, etc.) create a file named
`django-agentic.extras.md` in the **root of your Django project** (not this plugin folder).

All agents will automatically load it at runtime and apply it as an override over the
plugin baseline. Project-level rules always take precedence.

Use `django-agentic.extras.template.md` (included in this plugin) as a starting point:

```shell
cp <path-to-plugin>/django-agentic.extras.template.md <your-project>/django-agentic.extras.md
```

Then fill in only the sections relevant to your project and delete the rest.
