# GitHub Actions Workflow Hardening Checklist

Use this checklist before moving a workshop workflow into a real repository.

## Required Controls

- [ ] Workflow runs on `pull_request` before merge and on `push` to the default branch.
- [ ] `permissions:` is set explicitly; start with `contents: read` and add only required scopes.
- [ ] Third-party actions are reviewed and pinned to a full commit SHA before production use.
- [ ] Dependency install uses a lockfile (`pnpm install --frozen-lockfile`, `npm ci`, or equivalent).
- [ ] Secrets are read only from protected environments or repository/org secrets.
- [ ] Deploy jobs target protected environments with required reviewers.
- [ ] Concurrency prevents overlapping deploys to the same environment.
- [ ] Build, test, coverage, or package artifacts have useful retention periods.

## Release Gate

Before enabling this workflow as a required check, record:

| Evidence | Link or Value |
| -------- | ------------- |
| Passing run URL | |
| Required branch protection check | |
| Artifact name and retention | |
| Rollback or disable path | |

## Rollback Note

If the workflow blocks good PRs or deploys bad output, first disable the affected deployment job or environment, then revert the workflow change through a pull request. Keep the previous passing run URL in the incident note.
