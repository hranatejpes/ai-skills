---
name: project-risk-assessment
description: Inspect a software repository before implementation and identify concrete architectural, operational, security, dependency, testing, and database risks. Use at the start of a change or when entering an unfamiliar project; do not use as a substitute for a requested deep security audit.
---

# Project Risk Assessment

Build a current, evidence-based picture of the project before editing it. Keep the assessment proportional to the requested change and avoid speculative audits outside its scope.

## Inspect

Start with read-only inspection:

- applicable agent instructions, README, contribution, deployment, and architecture documentation;
- Git branch, upstream, status, recent relevant history, and current diff;
- `composer.json`, `composer.lock`, autoloading, scripts, PHP platform requirements, and installed-tool configuration;
- Nette bootstrap, configuration layout, modules, domain boundaries, presenters, services, and persistence approach;
- test framework, test layout, fixtures, PHPStan configuration, CI jobs, and project quality commands;
- container, runtime, web-server, queue, cron, cache, and external-service configuration relevant to the task;
- migration history, schema conventions, transaction patterns, and backward-compatibility requirements when data is affected.

Do not print secrets. Avoid reading real credential files or production data unless the task requires it and access is authorized. Configuration examples and key names are usually sufficient.

## Identify Material Risks

Look for risks that can change the implementation plan or validation strategy:

- uncommitted work, branch divergence, generated files, or unclear ownership of modified files;
- mismatch between PHP 8.4+, Composer platform settings, containers, CI, and production;
- missing lock file, stale dependency metadata, abandoned packages, or executable Composer scripts/plugins in an untrusted repository;
- weak or absent tests around the touched behavior, slow integration setup, nondeterministic fixtures, or known failing checks;
- unclear Nette boundaries, business logic in presentation code, service-locator usage, circular dependencies, or inconsistent namespaces;
- destructive migrations, large-table locks, missing rollback/roll-forward strategy, nullable transition hazards, or deployments where two application versions overlap;
- authorization, tenant isolation, validation, escaping, file upload, deserialization, command execution, SSRF, or sensitive-data exposure relevant to the change;
- API, CLI, event, database, or configuration changes that can break consumers or deployment order.

Confirm a risk from project evidence where practical. Distinguish an observed fact from an inference.

## Report and Proceed

Before implementation, summarize:

1. relevant project shape and commands;
2. material risks with severity and evidence;
3. the mitigation or verification that will address each risk;
4. any blocker requiring a user choice.

Proceed autonomously when mitigations are clear and in scope. Pause when continuing would overwrite user work, require destructive data handling, choose between materially different architectures, or need authority not already granted.

