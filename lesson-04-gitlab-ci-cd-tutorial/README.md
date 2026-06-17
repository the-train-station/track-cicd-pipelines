---
title: "GitLab CI/CD Tutorial"
type: lab
difficulty: beginner
tier: free
platform: "GitLab"
url: "https://docs.gitlab.com/ci/quick_start/"
tags: ["gitlab", "cicd", "tutorial", "pipelines"]
---

# GitLab CI/CD Tutorial

## Overview

This beginner lab teaches you the core GitLab CI/CD workflow by building a small, working pipeline anchored to the official quick start: https://docs.gitlab.com/ci/quick_start/. You will create a `.gitlab-ci.yml`, understand stages and jobs, ensure a runner is available to execute your builds, and read your first pipeline’s feedback in the GitLab UI. The focus is on practical mechanics you will reuse in any GitLab project.

This lesson is part of the CI/CD Pipelines track. It is a hands‑on lab on the GitLab platform at a beginner difficulty. By the end, you will be able to commit a change, watch a pipeline run automatically, and iterate on your configuration with confidence.

## Prerequisites

- A GitLab account and a project you can push to (personal namespace or a group where you have Developer or higher permissions)
- Basic Git fluency: clone, create a branch, commit, and push
- Permission to use a runner for the project. For GitLab.com, confirm Shared Runners are enabled for your project or group; for self‑managed GitLab, have access to a registered GitLab Runner (or the ability to register one)
- Optional: a simple application repository so you can replace the placeholder steps with real build/test commands

## Key Takeaways

1. The `.gitlab-ci.yml` at your repo root is the single source of truth for pipelines — it defines stages, jobs, images, caches, rules, and artifacts.
2. Runners execute jobs. A pipeline will remain pending if no eligible runner is available or if job tags do not match runner tags.
3. Stages control order; jobs in the same stage run in parallel, and stages run sequentially. Keep early stages fast to get quick feedback.
4. The Pipeline, Jobs, and Logs views provide an immediate feedback loop — use them to iterate quickly on failing YAML, missing tools, or permissions.

## How to Use

Follow these steps alongside the official quick start, adapting the example to your repository and language.

### Step 1: Confirm You Have a Runner

In your project, go to CI/CD → Runners and verify at least one runner is available to the project:

- If you see “Shared Runners” enabled, you can use them without extra setup on GitLab.com.
- If there are no available runners, register one (self‑hosted) or enable Shared Runners at the group/project level. If your organization uses runner tags, note which tags are required.

Common symptoms when a runner is missing or mismatched: pipelines stuck in “pending” or jobs showing “waiting for resource”. Fix runner availability before troubleshooting YAML.

### Step 2: Create a Minimal `.gitlab-ci.yml`

Add a file named `.gitlab-ci.yml` at the repository root. Start with a three‑stage pipeline that always runs on pushes to any branch:

```yaml
# .gitlab-ci.yml (starter)
stages: [build, test, deploy]

# Example: use a lightweight image with a shell; replace with your language/tooling image
default:
  image: alpine:latest

build:
  stage: build
  script:
    - echo "Building..."

test:
  stage: test
  needs: ["build"]
  script:
    - echo "Running tests..."

deploy:
  stage: deploy
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
  when: manual    # require a click; remove for automatic deploys
  script:
    - echo "Deploying..."
```

Notes:
- Replace `alpine:latest` with a language image (for example, `node:20`, `python:3.12`, or your own registry image) and swap the `echo` commands for your real build/test steps.
- If your organization requires runner tags, add `tags: ["your-tag"]` to each job to target eligible runners.

### Step 3: Commit, Push, and Watch the Pipeline

Commit the new file on a branch and push. Open CI/CD → Pipelines. You should see a pipeline created automatically for your commit. Click into the pipeline to view the stage graph, then open each job to read its log output.

Early troubleshooting checklist:
- If the pipeline does not appear, confirm the file is named exactly `.gitlab-ci.yml` at the repo root and that your branch is not excluded by `rules`.
- If jobs are pending for a long time, verify runner availability and tags.
- If a job fails immediately, open the job log — most YAML and shell errors are obvious from the first 10–20 lines.

### Step 4: Add Caching and Artifacts for Faster Feedback

Speed up builds and keep useful outputs from successful jobs. Extend the starter file:

```yaml
# Add once at top‑level
cache:
  # Example: cache Node.js dependencies; adjust for your toolchain
  paths:
    - node_modules/

test:
  stage: test
  needs: ["build"]
  script:
    - npm ci
    - npm test --if-present
  artifacts:
    when: always
    paths:
      - junit.xml
      - coverage/
    expire_in: 1 week
```

Artifacts let you inspect reports (for example, JUnit or coverage) even when a job fails. Adjust paths to match your project.

### Step 5: Evolve Stages and Rules As You Grow

As the pipeline matures:
- Split long jobs and keep early stages (<5 minutes) focused on fast checks.
- Use `rules:` to scope when jobs run (for example, only on merge requests or only on `main`).
- Add environments for deployments to track history and review apps. Start with `when: manual` until you establish guardrails.
- Protect branches and require passing pipelines before merge to keep your default branch green.

## Practice Notes

- Run hands-on work in a sandbox and keep a short lab log with commands, screenshots or outputs, resources created, cleanup steps, and the one pattern you would reuse in production.
- Tie the lesson back to a real delivery path: source change, validation, artifact creation, deployment, rollback, and audit evidence. Note which step would become a required check in a production repository.
- Completion checkpoint: you can explain the core idea without notes and reproduce the smallest useful example from the resource.
- Portfolio artifact: create a short note titled "GitLab CI/CD Tutorial - applied takeaway" with the scenario you used, the decision you made, and one follow-up task you would assign to yourself or a team.

## Related Resources

- Official quick start (GitLab): https://docs.gitlab.com/ci/quick_start/
- GitLab Runner overview and installation: https://docs.gitlab.com/runner/
- Environments and deployments: https://docs.gitlab.com/ci/environments/
- CI/CD comparison: GitHub Actions documentation — compare GitLab’s stages and runners with GitHub’s jobs and runners: https://docs.github.com/actions
- CI/CD comparison: AWS CodePipeline tutorials — compare GitLab-native pipelines with managed AWS delivery pipelines: https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials.html
- Adjacent track (SRE): Grafana + Prometheus Monitoring Stack — observe build agents and deployed services: https://github.com/stefanprodan/dockprom
- Adjacent track (Cloud Architecture): AWS Well-Architected Labs — connect delivery automation to operational excellence and reliability patterns: https://wellarchitectedlabs.com/

## Estimated Time

- Confirm runner availability and permissions: 5–10 minutes
- Add and push a minimal `.gitlab-ci.yml`: 10–15 minutes
- Observe the first pipeline run and read logs: 5–10 minutes
- Add basic caching/artifacts and iterate once: 15–20 minutes
- Total for this lesson: ~35–55 minutes, depending on runner setup and your project’s build time
