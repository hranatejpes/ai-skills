---
name: composer-dependency-maintenance
description: Keep PHP Composer dependencies reproducible during normal development and perform controlled, tested dependency updates when explicitly in scope. Use when preparing a PHP project, changing composer files, adding packages, or updating dependencies; do not update all packages as routine task startup.
---

# Composer Dependency Maintenance

Use the committed lock file as the source of reproducible dependency versions. Routine project preparation installs locked versions; dependency resolution changes are separate work.

## Routine Start

After the initial Git synchronization and risk inspection:

1. Inspect `composer.json`, `composer.lock`, configured repositories, `allow-plugins`, scripts, and the project runtime wrapper.
2. Use the project's documented execution environment, such as its container or task runner, rather than an arbitrary host PHP binary.
3. When `composer.lock` exists, run the trusted project equivalent of `composer install --no-interaction --prefer-dist` with development dependencies available for tests and PHPStan.
4. Check that `composer.json` and the lock file agree. Do not hide a stale lock warning.

Composer plugins and scripts execute code. In an untrusted repository, inspect them before running Composer and stop if their safety cannot be established.

If an application repository has no lock file, do not silently resolve and commit an entire dependency graph as routine setup. Report that reproducible installation is unavailable and treat lock creation as an explicit dependency change.

## Dependency Changes

Only update dependency resolution when adding, removing, upgrading, or repairing dependencies is part of the task.

- Prefer `composer require`, `composer remove`, or a targeted `composer update vendor/package --with-all-dependencies` over an unrestricted update.
- Use a full `composer update --with-all-dependencies` only for an explicitly requested broad refresh.
- Preserve the repository's stability flags, platform settings, repositories, plugin policy, and package constraints unless changing them is required and explained.
- Review the solver output and the `composer.json`/`composer.lock` diff. Investigate unexpected transitive upgrades, removals, abandoned packages, scripts, or platform changes.
- Do not edit `composer.lock` manually and do not commit `vendor/` unless the repository has an explicit vendoring policy.
- Keep a dependency-only change in its own atomic commit when it can be delivered independently.

## Verify an Update

Before committing a dependency change, run the repository equivalents of:

1. `composer validate --strict`;
2. `composer audit` when network and repository policy permit it;
3. the relevant application tests and full required test suite;
4. PHPStan using the project's configuration;
5. `composer check-platform-reqs` in the deployment-compatible environment when applicable.

Exercise the affected integration when an upgrade changes runtime behavior. If the update fails validation or introduces unresolved advisories or incompatibilities, do not commit it as successful work.

