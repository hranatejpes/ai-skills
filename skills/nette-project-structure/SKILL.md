---
name: nette-project-structure
description: Design or extend a maintainable Nette application structure with clear domain, presentation, infrastructure, configuration, test, and public-file boundaries. Use for new Nette projects, modules, or structural decisions; preserve established conventions in existing projects unless restructuring is explicitly requested.
---

# Nette Project Structure

Nette does not require one fixed layout. Prefer a predictable Web Project shape for new applications, then organize application code by business capability rather than by technical class type alone.

## Repository Layout

Use this baseline for a new project and adapt it only for a concrete reason:

```text
app/          application code
assets/       source assets, when used
bin/          CLI entry points and project commands
config/       application configuration
log/          runtime logs; not source code
temp/         cache and temporary runtime data
tests/        automated tests and test support
vendor/       Composer-managed dependencies
www/          the only public document root
```

Keep `composer.json`, `composer.lock`, quality-tool configuration, and deployment files at the repository root unless the project or monorepo establishes another boundary.

- Configure the web server so only `www/` is public.
- Do not place configuration secrets, source files, `vendor/`, `temp/`, `log/`, test fixtures, or source asset trees under the document root.
- Do not edit code under `vendor/`; change constraints, configuration, or upstream integration instead.
- Do not commit runtime cache, logs, local secrets, coverage data, or installed dependencies without an explicit project policy.

## Application Layout

A useful starting point is:

```text
app/
├── Core/          cross-cutting application infrastructure
├── Model/         domain and application logic grouped by capability
├── Presentation/  presenters, components, forms, templates, and UI modules
├── Tasks/         independent CLI/background tasks, when needed
└── Bootstrap.php
```

Within `Model/`, prefer domain-oriented units such as `Order/`, `Payment/`, or `Product/`. Keep related services, repositories, value objects, policies, and entities close to their domain. Avoid a project-wide collection of generic `Services/`, `Repositories/`, and `Entities/` when it fragments one capability across many folders.

Match namespaces to paths and Composer PSR-4 mappings. Use consistent singular names for cohesive namespace concepts (`Model`, `Core`, `Presentation`, `Product`) and plural names for genuine collections of independent items (`Tasks`).

## Boundaries

- Presenters coordinate HTTP/UI flow; they should not contain core business rules or persistence algorithms.
- Forms and components handle interaction concerns, delegate application operations, and return structured validation errors.
- Templates render already-prepared view data. Avoid database queries and business decisions in templates.
- Domain/application services express use cases and transaction boundaries without depending on HTTP request state.
- Persistence implementations contain query and mapping details behind the project's established abstraction. Do not create interfaces that have no substitution, testing, or architectural value.
- Cross-cutting infrastructure belongs in `Core/` only when it is genuinely shared; keep domain-specific infrastructure with its domain.

Use Nette dependency injection and explicit constructor dependencies. Avoid new service-locator access, hidden global state, or manual container lookups inside application logic.

## Existing Projects

Inspect the current layout before adding files. Prefer local consistency over cosmetic conformance. Do not perform a mass move, namespace rewrite, or module reorganization as a side effect of a feature or bug fix. When the current layout causes a concrete problem, propose the migration separately and include autoloading, configuration, template paths, tests, and deployment impact.

