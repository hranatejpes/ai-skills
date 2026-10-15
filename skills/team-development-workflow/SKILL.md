---
name: team-development-workflow
description: Coordinate an implementation task in a PHP/Nette repository from initial synchronization and risk review through dependency setup, testing, PHPStan, commit, and push. Use for code changes, bug fixes, refactors, database migrations, and dependency work; do not use for read-only review or planning.
---

# Team Development Workflow

Deliver each logical change as a reviewed, tested, committed, and pushed unit. Follow repository instructions and established project commands over generic defaults.

Companion skills provide deeper guidance when installed:

- `safe-git-delivery`
- `project-risk-assessment`
- `composer-dependency-maintenance`
- `nette-project-structure`
- `php-quality-gate`
- `relational-database-design`
- `php-84-runtime`

Do not assume that naming a companion skill grants extra permissions. Preserve user work, repository policy, protected branches, and environment approval requirements.

## Start Once

Run this start phase once per task or resumed working session, before changing project files:

1. Read the applicable `AGENTS.md`, `CLAUDE.md`, contributor documentation, and task-specific instructions.
2. Inspect the branch, upstream, remotes, and working tree. Never discard, reset, overwrite, or auto-stash existing changes.
3. If the worktree is clean and the current branch has an upstream, run `git pull --ff-only` once. Do not pull again later in the task. If a safe fast-forward pull is impossible, stop and report the exact condition instead of merging or rebasing automatically.
4. Inspect the synchronized project and identify material risks before implementation. Cover architecture, scope, tests, runtime, dependencies, database impact, security, and deployment compatibility as applicable.
5. Verify that the active PHP runtime and the project's declared platform support PHP 8.4 or newer within the supported PHP major version.
6. When `composer.lock` exists, run the project's trusted Composer install command so `vendor/` matches the lock file. Do not turn routine setup into a dependency update.

Keep the risk report concise. Continue without waiting when risks are understood and safely manageable; pause only when a choice or missing authority would materially change the result.

## Implement

- Make the smallest coherent change that solves the task.
- Follow the repository's existing architecture. Apply the Nette structure guidance strongly to new modules and projects, but do not reorganize an established codebase without scope to do so.
- Keep domain rules out of presenters, templates, and persistence plumbing.
- For schema changes, use versioned migrations and plan compatibility with both the old and new application version when deployments can overlap.
- Treat dependency updates as separate logical work. Prefer targeted updates and keep resulting lock-file changes in a dedicated commit.
- Add or update a test for changed behavior. A bug fix should normally include a regression test that fails without the fix.

## Verify

Before each commit:

1. Review the diff and ensure it contains only the intended logical unit.
2. Run syntax or formatting checks required by the repository.
3. Run the most focused relevant test while iterating.
4. Run the repository's required test suite and PHPStan command before delivery.
5. Exercise the affected behavior directly when tests alone do not demonstrate that it works.

Do not weaken tests, lower PHPStan strictness, regenerate a baseline, or add broad ignores merely to make the gate pass. Documentation-only or metadata-only changes do not require artificial tests, but still require the relevant validation and an explicit explanation of why no behavioral test was added.

## Deliver

After the logical unit passes its quality gate:

1. Recheck `git status` and the staged diff.
2. Stage only files belonging to the unit. Do not include unrelated user changes, secrets, runtime artifacts, `vendor/`, `temp/`, or `log/`.
3. Create an atomic commit using the repository's message convention.
4. Push the current branch to its configured upstream immediately. Never force-push and never bypass branch protection.
5. Report the commit identifier, pushed branch, and verification commands.

If verification fails, do not commit or push the change. Fix failures within scope; otherwise report the blocker and preserve the working tree for the user.

