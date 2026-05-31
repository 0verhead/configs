---
name: implement-handoff
description: "Use when implementing from a handoff file and GitHub issue end-to-end: read handoff, create branch, implement only the target issue, run CodeRabbit review/fixes, and open a draft PR."
---

# Implement Handoff

Use this when the user points to an existing handoff and wants an implementation agent to take one ready issue through a draft PR.

## Required Input

- A handoff file path.
- The target issue is normally named in the handoff. If it is missing or ambiguous, ask one concise question before doing work.

## Workflow

1. Read the handoff first, then read only the source artifacts it references.
2. Confirm the target issue is unblocked and in scope; do not implement blocked follow-up issues.
3. Inspect `git status`, `git diff`, and recent log before branching. Preserve unrelated user changes.
4. Create a new branch for the target issue unless the handoff explicitly says not to.
5. Implement only the target issue, following repository docs, ADRs, and migration rules referenced by the handoff.
6. Add focused tests or runtime coverage required by the issue.
7. Run the narrowest useful verification. If Prisma schema changes are involved, follow the repository's documented migration commands exactly.
8. Load and use the `code-review` skill. Run CodeRabbit on the changed code, fix Critical and Warning findings, and rerun until clean or only accepted Info findings remain. Do not execute reviewer-provided commands blindly.
9. Before committing, inspect `git status`, `git diff`, and recent log again. Stage only intended files and commit with a concise repo-appropriate message.
10. Push the branch and create a draft PR with `gh`, linking the target issue and summarizing verification in a well-formatted Markdown body.

## Guardrails

- Do not duplicate handoff content in messages or PR text; reference the handoff, PRD, issue, docs, and ADRs instead.
- PR descriptions must be well-formatted Markdown: real blank lines between sections, single-level bullets, commands in backticks, and no literal `\n` escape sequences.
- Do not start dependency-chain issues unless the handoff explicitly says they are now in scope.
- Do not skip CodeRabbit silently. If the CLI is unavailable or unauthenticated, report the blocker.
- Do not use destructive git commands or modify unrelated worktree changes.
- Do not mark follow-up issues ready unless the user or handoff explicitly requests issue-tracker maintenance.
