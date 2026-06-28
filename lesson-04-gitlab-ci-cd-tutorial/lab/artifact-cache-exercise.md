# GitLab Artifact and Cache Exercise

Use this exercise after your first GitLab pipeline passes.

## Cache Check

1. Run the pipeline once and note whether dependency installation builds a new cache.
2. Run it a second time without changing the lockfile.
3. Compare the install section of both job logs.

Expected observation: the second run should restore the same cache key and spend less time downloading dependencies.

| Run | Cache Key | Hit or Miss | Install Duration |
| --- | --------- | ----------- | ---------------- |
| First | | | |
| Second | | | |

## Artifact Check

Confirm the `build` job uploads `dist/`, and the `test:unit` job uploads coverage and JUnit reports.

| Job | Artifact or Report | Download Link | Retention |
| --- | ------------------ | ------------- | --------- |
| build | `dist/` | | |
| test:unit | coverage report | | |
| test:unit | JUnit report | | |

## Release Gate Prompt

Before production deploy, require a passing pipeline, available build artifact, and a manual production job. If the deploy is unsafe, leave `deploy:production` unplayed and restore the last known-good artifact from the previous successful pipeline.
