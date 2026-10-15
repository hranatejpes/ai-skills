---
name: php-quality-gate
description: Add appropriate automated tests and run PHP syntax checks, the project test suite, PHPStan, and behavior verification before a PHP change is committed. Use for PHP implementation, bug fixes, refactors, migrations, and dependency changes; do not invent behavioral tests for documentation-only edits.
---

# PHP Quality Gate

No changed behavior is complete until an automated test covers it and the relevant quality checks pass. Use project-defined commands from documentation, Composer scripts, task runners, containers, and CI before falling back to direct tool commands.

## Establish the Gate

Inspect:

- Composer scripts and development dependencies;
- PHPUnit, Nette Tester, or another established test configuration;
- PHPStan configuration, extensions, bootstrap files, and existing baseline;
- CI jobs and their PHP version, services, environment variables, and command order;
- test database, fixtures, temporary directories, and external-service substitutes.

Run a focused existing test or check before editing when that is needed to distinguish a pre-existing failure from a regression.

If tests or PHPStan are absent, do not download an untracked global tool. For a task that includes establishing team quality tooling, add the appropriate Composer development dependency and configuration as a separate reviewed dependency change. Prefer `level: max` for a new codebase; for an established codebase, preserve its configured level and improve it deliberately rather than hiding errors in a broad new baseline.

## Write the Test

- Add or update a test for each meaningful behavior change.
- For a bug, first capture the failure with a regression test when practical.
- Test observable outcomes and contracts, not private implementation details.
- Choose the narrowest useful layer: unit test for isolated rules, integration test for persistence or framework wiring, and HTTP/browser test for behavior that depends on the full request flow.
- Include relevant success, validation, authorization, boundary, and failure cases without duplicating equivalent scenarios.
- Keep tests deterministic and isolated. Control time, randomness, filesystem state, network access, and database state where relevant.
- Never point automated tests at production services or production data.

Do not weaken an assertion, delete coverage, add sleeps, or introduce unconditional skips to make a change pass.

Documentation-only, comment-only, and non-behavioral metadata changes do not need artificial tests. Run the relevant formatter, parser, schema, or configuration validation instead and state why no behavioral test was added.

## Run Checks

While iterating, run the smallest relevant test. Before committing, run all repository-required checks, normally including:

1. PHP syntax validation for changed PHP files or the project's equivalent lint command;
2. the focused test covering the change;
3. the required broader test suite;
4. PHPStan through the project's Composer script or `vendor/bin/phpstan analyse` using its discovered configuration;
5. direct runtime verification when the test suite does not prove the user-visible or integration behavior.

Analyse project-owned source and tests, not `vendor/`. Treat a nonzero PHPStan exit as a failed gate.

## Do Not Mask Failures

- Fix the cause instead of lowering the PHPStan level.
- Do not regenerate or expand a PHPStan baseline as an incidental fix.
- Add a narrow ignore only for a demonstrated analyzer limitation, with the smallest scope and a reason consistent with project policy.
- Do not claim a full pass when only a focused subset ran.
- If resource limits make a full suite impossible, report the unrun checks and do not present the change as fully verified.

Commit only after the required gate passes. In the completion report, list the exact commands and results.

