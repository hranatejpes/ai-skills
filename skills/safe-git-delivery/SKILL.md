---
name: safe-git-delivery
description: Synchronize a Git branch once at task start, then create atomic commits and push each verified logical change. Use while implementing repository changes; do not use for read-only inspection, history analysis, or when the user explicitly excludes commits or pushes.
---

# Safe Git Delivery

The delivery unit is a coherent, tested change, not every file save. Commit and push each completed unit before starting an unrelated unit.

## Initial Synchronization

Perform this phase once at the beginning of the task:

1. Read repository contribution and branch instructions.
2. Inspect `git status`, the current branch, its upstream, and remotes.
3. Confirm that existing uncommitted or untracked files will not be overwritten or accidentally included.
4. On a clean worktree with a configured upstream, run `git pull --ff-only`.

Do not automatically stash, reset, clean, merge, or rebase. If the worktree is not clean, the branch has no upstream, authentication fails, or fast-forward is impossible, report the condition. Continue without pulling only when the user or repository workflow explicitly makes that safe.

Do not pull again during the same task. A later push rejection is not permission to merge or rebase: fetch and inspect the divergence, then ask for direction when resolution would alter history or combine external changes.

## Prepare the Commit

Commit only after the project's quality gate passes.

- Inspect the complete diff, including staged and untracked files.
- Separate dependency updates, migrations, generated artifacts, and unrelated fixes when they are independently deliverable.
- Stage explicit task files or reviewed hunks. Never use broad staging when unrelated changes are present.
- Do not include secrets, local environment files, caches, logs, coverage output, IDE state, or installed dependencies.
- Follow the project's commit-message convention. Otherwise use a concise imperative subject that states the outcome.
- Do not amend, squash, or rewrite commits created by someone else unless explicitly requested.

## Push

After a successful commit, push the current branch to its configured upstream.

- Never use `--force`, `--force-with-lease`, or a destructive history operation by default.
- Do not bypass hooks or branch protection.
- Do not silently change the destination branch or remote.
- If the current branch is the protected default branch, follow the repository's documented workflow; create or switch branches only when that workflow or the user authorizes it.

If credentials, network access, hooks, or server policy prevent the push, keep the successful local commit and report the exact failure and commit identifier.

## Completion Report

Report:

- commit identifier and subject;
- local branch and pushed remote branch;
- verification commands that passed;
- any remaining uncommitted files, clearly distinguishing unrelated pre-existing work.

