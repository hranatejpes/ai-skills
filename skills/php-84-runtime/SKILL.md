---
name: php-84-runtime
description: Keep PHP/Nette code, Composer metadata, containers, tooling, and CI on a PHP 8.4-or-newer PHP 8.x baseline. Use when creating a project, changing PHP code or dependencies, or reviewing runtime compatibility; do not silently widen support to PHP 9.
---

# PHP 8.4+ Runtime Policy

PHP 8.4 is the minimum supported runtime. Support newer PHP 8.x versions when project dependencies and deployment environments allow them. Treat a future PHP major version as an explicit compatibility decision.

## Verify the Runtime Contract

Inspect and align:

- the actual PHP CLI or container used for Composer, tests, PHPStan, and application commands;
- the root Composer `require.php` constraint and any `config.platform.php` override;
- Docker images, CI matrices, deployment manifests, and production documentation;
- required extensions and their availability in all environments;
- dependency constraints in `composer.lock`.

For an application intended to support PHP 8.4 and later PHP 8.x releases, prefer a Composer constraint equivalent to `^8.4`. Do not use an unbounded `>=8.4` constraint unless PHP 9 compatibility is intentionally supported and tested.

Fail early when a command is running on PHP below 8.4. Do not let Composer platform emulation conceal an incompatible execution runtime.

## Write Modern, Maintainable PHP

- Use `declare(strict_types=1);` in new project-owned PHP files unless the repository has a documented conflicting convention.
- Use native parameter, return, property, union, intersection, and nullable types where they accurately express the contract.
- Prefer value objects, enums, readonly state, constructor property promotion, and other modern features when they improve domain correctness or clarity; do not use a feature merely for novelty.
- Handle nullability and initialization explicitly. Avoid dynamic properties and implicit type coercion.
- Use exceptions and error handling appropriate to the application boundary; do not suppress warnings or deprecations globally.
- Keep code compatible with the project's declared upper runtime range. Do not use syntax or APIs newer than the minimum unless the runtime contract is updated as part of the task.

## Dependencies and Tooling

- Require production and development packages that support the declared PHP range.
- Declare required PHP extensions in Composer when they are runtime requirements.
- Run Composer, tests, code generation, and PHPStan in the same class of environment used by the project, preferably its maintained container or task runner.
- Do not bypass platform requirements with `--ignore-platform-reqs` as a normal workflow.
- Treat deprecations on newer supported PHP 8.x versions as compatibility defects to investigate, not output to hide.

## CI and Verification

Test the minimum supported PHP 8.4 runtime. When the project supports an additional deployed PHP 8.x version, include it in CI or an equivalent release gate. Run the project's tests, PHPStan, Composer validation, and platform requirement checks after runtime or dependency changes.

If CI, local development, and production disagree about the PHP version, report and resolve the mismatch before claiming compatibility.

