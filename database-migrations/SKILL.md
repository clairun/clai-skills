---
name: "Database Migrations"
description: "Use when writing or reviewing a schema or data migration: plan its locks against live traffic, its failure and recovery, its reverse path, and test it under the application's real concurrent load."
---
# Database Migrations

A migration runs against live traffic, under the project's migration runner, on production-sized data. Most failures come from those three rather than from the SQL being wrong. Read the runner before the migration: whether the file runs as one transaction, what state a failure leaves (for example a dirty version that blocks later runs), whether it retries, and when it runs relative to the deploy.

## Locks

Write a lock plan: each statement, the lock it takes, the table, in order. Look up lock modes in the database's documentation for the installed version rather than recalling them. Then:

- Find every application transaction that touches those tables while the migration runs, including implicit ones: foreign-key cascades, triggers, partitions. Derive the cascade graph from the catalog (in Postgres, `pg_constraint`); grepping the code for table names misses it.
- Compare orders. If the migration locks A then B and any transaction locks B then A, they can deadlock. Take all the locks up front, in the application's order, or split the migration so each transaction locks one table.
- Flag lock upgrades: a weaker lock followed by a stronger one on the same table (for example `CREATE INDEX` then `ALTER TABLE`) can deadlock with any transaction that reads and then writes it.
- Bound the wait (in Postgres, `lock_timeout`). A waiting exclusive lock queues every later query behind it, reads included. Know what a timeout leaves behind and how to recover.
- Read the repo's recent migrations of the same kind. A lock pattern they use and this one lacks needs a reason.

## Size and duration

Measure on production data, read-only: table and index sizes, row counts, whether rewritten columns are already constrained, and long-running transactions. Estimate how long each exclusive lock is held, and say whether the estimate is measured or extrapolated.

## Reverse path

The down migration gets the same lock plan, ordering check, and test as the up migration. Write down the recovery for a failed run in the runner's own terms, for example which version to set and with what flag.

## Testing

Run it through the real runner, against the real schema, on an isolated environment. Then try to break it: run the application's conflicting operations in a loop (writes, deletes, and cascades on the touched tables) while the up and down migrations run, several times. Holding a single competing lock only tests the timeout; it cannot reveal a deadlock or a lock-order problem. Record the schema before and after, and the runner's version state after each failure.
