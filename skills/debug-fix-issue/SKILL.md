---
name: debug-fix-issue
description: Entry point for bugs, errors, and broken features. Collects the issue and desired state from the user, then delegates investigation and fix orchestration to django-software-architect.
license: MIT
metadata:
  author: Sebastian Muthwill
  version: "0.1.0"
tools: ['ask_user', 'task', 'view', 'glob']
---

# Debug & Fix Issue

Use this skill as the entry point whenever something is broken, not working as expected,
or producing errors. It collects the issue and the desired state, then hands off to
`django-software-architect` who orchestrates `django-debug` for investigation and the
appropriate implementation agent for the fix.

## Required Behavior

### Step 1 — Collect the issue

1. Read the issue description provided by the user.
2. Check for `django-agentic.extras.md` in the project root and load it if present.
3. Review any referenced files, URLs, or views mentioned in the issue description.

### Step 2 — Establish the desired state

Ask the user for any information that is missing before handing off. At minimum confirm:

- **Current behavior**: What is actually happening? (error message, wrong output, broken UI)
- **Desired behavior**: What should happen instead? Ask explicitly if not provided — this
  is required before delegation.
- **Reproduction steps**: How can the issue be consistently reproduced?
- **Affected area**: Which URL, view, template, model, or API endpoint is involved?
- **Frequency**: Always broken, intermittent, or only under specific conditions?

Only ask for information that is not already clear from the issue description or the
existing project code. Do not ask for information you can infer.

### Step 3 — Confirm and hand off

1. Summarise the issue and the confirmed desired state back to the user.
2. Once the user confirms, delegate to `django-software-architect` with:
   - The full issue description.
   - Current vs desired behavior.
   - Reproduction steps.
   - Affected file paths or endpoints identified during Step 1.
   - Contents of `django-agentic.extras.md` if present.
3. `django-software-architect` will delegate to `django-debug` for root cause
   investigation, then route the fix to the correct implementation agent
   (`django-developer` or `django-frontend-developer`) and optionally trigger
   `django-tester` to validate the fix.

## In Scope

1. Runtime errors, exceptions, and 500 responses.
2. Broken or missing UI behavior (wrong rendering, HTMX not updating, form errors not shown).
3. Logic defects producing wrong data or unexpected query results.
4. Performance issues (slow pages, excessive queries).
5. UI/backend mismatches (context variables missing in template, wrong response type).

## Out of Scope

1. New feature development — use the `feature-development` skill instead.
2. New project setup — use the `project-initialization` skill instead.
3. Refactoring without a concrete broken behavior to fix.
