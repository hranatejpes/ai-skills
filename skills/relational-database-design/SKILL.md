---
name: relational-database-design
description: Design and change relational database schemas, migrations, constraints, indexes, transactions, and data access for PHP/Nette applications. Use when a task adds or changes persistent data or queries; adapt decisions to the actual database engine, workload, ORM, and deployment process.
---

# Relational Database Design

There is no universally optimal schema. Derive the design from domain invariants, access patterns, data volume, database engine, deployment topology, and the project's established persistence layer.

## Model the Data

- Give every table a stable primary key using the project's consistent identifier strategy.
- Represent relationships with foreign keys when the engine and lifecycle permit them. Define deletion and update behavior deliberately rather than relying on defaults.
- Normalize transactional data by default. Denormalize only for a measured access need and define how duplicated values stay consistent.
- Encode durable invariants with `NOT NULL`, unique constraints, foreign keys, and check constraints where supported. Application validation complements database integrity; it does not replace it.
- Distinguish missing, unknown, empty, and zero values. Make columns nullable only when `NULL` has a defined domain meaning.
- Use appropriate native types. Avoid storing structured relational fields in free-form text or JSON merely for convenience.
- Store money as a precise decimal or integer minor units together with an explicit currency, according to project convention. Never use binary floating point for monetary values.
- Store instants consistently, normally in UTC, and preserve a separate time-zone identifier when local civil time is part of the domain.
- Treat enums and status columns as domain contracts. Choose database constraints versus lookup tables based on how frequently values change and how deployments overlap.

## Index for Actual Access

- Start from real filters, joins, ordering, uniqueness, and foreign-key operations.
- Design composite indexes in an order supported by the target engine and query shapes.
- Avoid redundant or speculative indexes; each index consumes storage and write capacity.
- Inspect query plans for important or changed queries at realistic scale when performance matters.
- Prevent N+1 access patterns and unbounded reads in application code. Paginate or stream large result sets.

## Transactions and Concurrency

- Define the transaction boundary around one business operation, not an entire HTTP lifecycle by habit.
- Keep transactions short and avoid remote calls while locks are held.
- Choose locking, isolation, optimistic concurrency, idempotency, and retry behavior from the actual race conditions.
- Make retryable operations safe to repeat and do not retry arbitrary failures indefinitely.

## Migrations

- Express every schema or durable data change as a versioned migration using the repository's migration tool and location.
- Do not edit a migration that may already have run in a shared environment; add a corrective migration.
- For deployments where application versions overlap, use expand-migrate-contract: add compatible structures, deploy dual-compatible code, migrate/backfill safely, then remove obsolete structures in a later deployment.
- Consider table size, lock duration, transaction-log growth, replicas, backups, and rollback or roll-forward recovery before a destructive or blocking operation.
- Make backfills bounded, restartable, observable, and idempotent when they can be large.
- Never drop, truncate, rewrite, or irreversibly transform material data without explicit authority and a recovery plan.

## Security and Isolation

- Use parameterized queries through the project's database layer.
- Enforce authorization and tenant scoping on every applicable read and write, including background jobs and administrative paths.
- Minimize stored sensitive data, avoid secrets in database content or migrations, and account for encryption, retention, erasure, and audit requirements where relevant.

## Verify

Test migrations from the previous schema state, application behavior on the resulting schema, constraints, important queries, transaction failure paths, and tenant isolation where applicable. Run the repository's schema validation and database integration tests before commit.

