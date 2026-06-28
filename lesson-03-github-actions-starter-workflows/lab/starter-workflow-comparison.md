# Starter Workflow Comparison Exercise

Compare three workflow options before adopting one.

| Option | Best Fit | Risk | Required Changes | Decision |
| ------ | -------- | ---- | ---------------- | -------- |
| GitHub starter workflow | Fast first CI for one repo | Defaults may not match local commands | Runtime version, package manager, permissions, cache key | |
| Custom workflow | Repo-specific build/test/deploy path | Easy to overfit or duplicate patterns | Explicit triggers, actions, artifacts, environment gates | |
| Reusable workflow | Many repos need the same CI contract | Interface changes affect callers | `workflow_call` inputs, outputs, secrets, versioning | |

## Review Prompts

1. Which option gets a passing pull request check fastest?
2. Which option is easiest to audit for least privilege?
3. Which option gives the best rollback path if a shared change breaks multiple repos?
4. Which option should become the portfolio artifact for this lesson, and why?

## Validation Notes

Record one workflow run URL and one branch protection or required-check setting that proves the selected option is actually integrated.
