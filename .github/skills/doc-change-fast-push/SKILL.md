---
name: doc-change-fast-push
description: "Govern project-phase workflow and documentation updates with immediate push discipline for docs changes. Use for docs-only hotfixes, policy updates, workflow governance, and phase-closure documentation after user verification. Also enforce step-by-step development flow, explicit user testing guidance, and regression-first fixing before moving forward."
argument-hint: "What phase/process change should be documented and pushed?"
user-invocable: true
---

# Phase Workflow and Documentation Governance

## Outcome
Produce a safe documentation-only change, validate that no non-doc files are included, and push to `master` immediately.

## When to Use
- Update markdown documentation quickly.
- Add or remove documentation files.
- Apply policy, process, or operational note updates.
- Perform docs-only commits without bundling product code changes.

## Inputs
- Target documentation path(s)
- Requested content changes
- Target branch: `master`

## Quick Checklist
1. Inspect repository state.
2. Confirm all requested paths match documentation rules.
3. Apply minimal documentation edits only.
4. Stage only documentation files.
5. Verify no non-documentation files are staged.
6. Commit with a clear docs-focused message.
7. Push directly to `master`.
8. Report changed files and commit hash.

## Decision Points
- If a requested file is not documentation:
  Treat it as out of scope and do not include it in the commit.
- If unrelated non-doc files are changed in the working tree:
  Leave them untouched and stage only documentation files.
- If push fails:
  report the error and provide the exact retry command.

## Quality Gates
- Only documentation files are staged and committed.
- No destructive git operations are used.
- Existing unrelated changes are never reverted.
- The final report includes changed docs and commit SHA.

## Documentation File Heuristics
- Include: `**/*.md`, `**/*.mdx`, `docs/**`, `README*`, `CHANGELOG*`, `CONTRIBUTING*`.
- Exclude: source code, build files, lockfiles, binaries, generated artifacts.

## Security and Dependency Guardrails
- Do not add dependencies for documentation-only work unless explicitly required.
- If package installation is required, always use organization TR1 JFrog repositories as the package source (for example, tr1.jfrog.io endpoints) for local and CI/CD flows.
- Do not use public registries as the primary source when TR1 JFrog is available; only use alternatives if the user explicitly approves an exception.
- Use environment-injected credentials/tokens for package access (for example, `JFROG_NPM_TOKEN`) and never hardcode secrets in files or commands.
- Keep changes minimal and scoped to the user request.

## Completion Checklist
- Requested documentation content is updated.
- Only documentation files are in the commit.
- Changes are pushed to `master`.
- Final response summarizes files changed and push result.

## Verified-Phase Documentation Rule
- When a process step or phase is completed and user-verified, update related documentation in parallel before continuing to the next phase.
- Push the documentation update promptly after verification as a docs-focused commit.
- Do not defer verified-phase documentation updates to the end of the project.

## Step-By-Step Development Protocol
Apply this collaboration loop for ongoing project work:

1. Work in small, sequential increments.
2. After each meaningful increment, ask the user to test before proceeding.
3. Provide test guidance with:
  - what to run,
  - what to verify,
  - expected outcome.
4. If any issue or regression is found by either the user or the agent, stop forward work and fix it first.
5. Resume next increment only after the fix is validated.
6. Repeat this loop until project completion.

## Wide-Fix Propagation Rule
- When applying a fix or implementing a suggested improvement in one area, proactively scan for equivalent patterns in related features, sections, or document types.
- If matching patterns are found, apply the same fix in those locations within the same change batch, unless explicitly excluded by the user.
- In the final response, clearly state where the wider fix was applied and why those areas required the same correction.
- If some related areas are intentionally not changed, explain the reason and residual risk.
