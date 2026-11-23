---
name: deploy
description: Work in progress, not usable yet. Will take a tdd-studio project from a green local build to a running deployment. Only runs when the owner invokes /tdd-studio:deploy.
disable-model-invocation: true
---

# Deploy (work in progress)

This stage is not written yet. Tell the owner, in one or two lines, that
`/tdd-studio:deploy` is a placeholder. Do not attempt a deployment from this skill.

Planned scope, for when it is written:

- Start from the deployment ADR made during kickoff (or run that discussion now if the
  ADR says "no deployment planned").
- Treat deployment as a milestone with its own done-when test (e.g. a smoke test that
  passes against the deployed URL), planned and built like any other phase.
- CI that runs the same check command as local development.
- Secrets, environments, rollback and a post-deploy check.
- Every outward-facing step (creating cloud resources, pushing to production, DNS) is
  asked about first.
