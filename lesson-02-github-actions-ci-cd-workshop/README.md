---
title: "GitHub Actions CI/CD Workshop"
type: lab
difficulty: beginner
tier: free
platform: "GitHub"
url: "https://skills.github.com"
tags: ["github-actions", "cicd", "workshop", "automation"]
---

# GitHub Actions CI/CD Workshop

## Overview

GitHub Skills offers free, self-paced, hands-on courses that run inside real GitHub repositories you control. For CI/CD, the GitHub Actions courses are a fast way to learn by doing: a bot guides you through issues and pull requests, you edit workflow files, and you watch builds, tests, and deployments run in GitHub Actions on every push or PR. This workshop points you to the GitHub Skills catalog as the practical starting point for learning GitHub Actions and building a working CI pipeline you can adapt to your own projects.

You will practice core CI/CD concepts such as triggers (`push`, `pull_request`), jobs and runners, caching and matrix builds, secrets and environments, and gated deployments. Because the exercises run in your own repo, you gain experience with the actual GitHub UI, logs, and permissions you will use day to day. Use this lesson to select an introductory Actions course, complete it end to end, and then copy the resulting workflow into one of your active repositories as your first production-ready CI pipeline.

## Prerequisites

- A GitHub account with permission to create repositories under your user or organization
- Basic Git and GitHub familiarity: commit, branch, open a pull request, merge
- A repo you can use for practice (the Skills courses can create one for you)
- Optional: language toolchain installed locally if you choose a language-specific CI example (for example Node.js, Python, or Java), though you can complete most lessons entirely in the browser
- If you plan to try deployments during the workshop: non-production credentials for your target platform and the ability to add repository or environment secrets in GitHub

## Key Takeaways

1. Hands-on, safe practice for Actions: you learn CI/CD by editing real workflows in a sandboxed repo, guided by a bot that reviews your changes.
2. Fundamental building blocks: triggers, jobs, runners, caching, matrices, artifacts, and secrets — the essentials you need to stand up a reliable pipeline.
3. Environments and approvals: how to ship safely with required reviewers, protected branches, and per-environment secrets.
4. Effective debugging: reading logs, re-running failed jobs, and using artifact uploads to investigate failures.

## How to Use

### Step 1: Open the GitHub Skills Catalog

Go to https://skills.github.com and sign in. Use the catalog filters to find the GitHub Actions courses. Choose an introductory course focused on CI or testing.

### Step 2: Create Your Course Repository

Start the course. The Skills bot will create a new repository for you (under your user or org) with starter code and a first issue. Keep the repository public unless your org requires private by default.

### Step 3: Follow the Guided Issues and PRs

Work through the issues one by one. You will:

- Create or edit workflow YAML under `.github/workflows/`
- Configure triggers such as `on: push` and `on: pull_request`
- Add jobs that run on `ubuntu-latest` and install your language/tooling
- Save artifacts, enable caching, or add a build-and-test matrix when prompted

Each step is validated by a pull request check that runs the workflow you just edited. Read the run logs to confirm what changed.

### Step 4: Add Secrets and Environments (Optional)

If your chosen course includes deployments, add repository or environment secrets in Settings → Secrets and variables. Create an `environment` (for example `staging`) and require reviewers to practice gated deployments.

### Step 5: Port the Workflow to a Real Repository

When you finish the course, copy the working workflow file(s) into an active project. Adapt job names, paths, and language versions. Add branch protection and mark key checks as “required” so PRs cannot merge unless CI is green. Push a test branch and verify the checks run as expected.

### Step 6: Iterate and Document

Add caching if builds are slow, split jobs for parallelism, and upload artifacts you need for debugging. Document what the pipeline does, how to run it locally if needed, and how to recover from common failures.

## Deliverable

Create a **GitHub Actions CI portfolio bundle** for a sandbox repository:

- A working workflow based on [lab/ci.yml](lab/ci.yml)
- A short pipeline run note with the run URL, trigger event, jobs that passed, and artifacts uploaded
- A completed [workflow hardening checklist](lab/workflow-hardening-checklist.md)
- A brief rollback note explaining how you would disable a bad deployment workflow or revert a broken workflow change

Reusable artifact: the hardening checklist can be reused before copying any GitHub Actions workflow into a production repository.

## Validation

Run these checks in the repository where you install the workflow:

```bash
gh workflow list
gh run list --limit 5
gh run view --log-failed
```

Expected result: the workflow appears in the list, the latest run reaches `completed` with `success` for the required CI jobs, and `gh run view --log-failed` prints no failed job logs for a passing run.

## Self-Assessment

Scenario question for Train Station app integration: if an app reviewer only saw your workflow file and one run URL, what evidence would convince them the workflow is safe enough to require on pull requests? Mention permissions, required checks, artifact handling, and failure recovery.

## Practice Notes

- Run hands-on work in a sandbox and keep a short lab log with commands, screenshots or outputs, resources created, cleanup steps, and the one pattern you would reuse in production.
- Tie the lesson back to a real delivery path: source change, validation, artifact creation, deployment, rollback, and audit evidence. Note which step would become a required check in a production repository.
- Completion checkpoint: you can explain the core idea without notes and reproduce the smallest useful example from the resource.
- Portfolio artifact: create a short note titled "GitHub Actions CI/CD Workshop - applied takeaway" with the scenario you used, the decision you made, and one follow-up task you would assign to yourself or a team.

## Related Resources

- GitHub Skills catalog (official): https://skills.github.com
- GitHub Actions documentation: https://docs.github.com/actions
- GitHub Marketplace (reusable actions): https://github.com/marketplace?type=actions
- Adjacent track: AWS Well-Architected Labs — practice disciplined delivery with cloud architecture patterns: https://wellarchitectedlabs.com/
- Adjacent track: Observability for pipelines and services
  - AWS Observability Workshop (official): https://observability.workshop.aws/
  - Grafana + Prometheus Monitoring Stack (reference repo): https://github.com/stefanprodan/dockprom

## Estimated Time

- Selecting an introductory Actions course and creating the repo: 10-15 minutes
- Completing the core CI steps in the course (build + test): 30-60 minutes
- Optional deployment or environments module (if included in your chosen course): 20-40 minutes
- Porting the workflow to a real repository and adding branch protection/required checks: 30-60 minutes
- Documenting the pipeline and next steps: 10-15 minutes
- Total for this lesson: ~1.5–3 hours depending on whether you include a deployment step
