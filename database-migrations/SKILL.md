---
name: "Database Migrations"
description: "Use when writing or reviewing a schema or data migration: plan its locks against live traffic, its failure and recovery, its reverse path, and test it under the application's real concurrent load."
---
# Database Migrations

A migration runs against live traffic, under the project's migration runner, on production-sized data. Those three, more than the SQL, decide whether it is safe. Read the runner before the migration: whether each file, or the whole batch, runs as one transaction, what state a failure leaves (for example a dirty version that blocks later runs), whether it retries, and when it runs relative to the deploy.

## Locks

Write a lock plan: each statement, in order, and the locks it takes on every table it touches. Look up lock modes in the database's documentation for the installed version rather than recalling them; they vary by statement form (in Postgres, `ALTER TABLE ... ADD FOREIGN KEY` locks both tables). Then:

- Find every application transaction that touches those tables while the migration runs, including implicit locks: foreign-key actions and key checks, triggers, and partitions covered by a lock on their parent. Derive them from the catalog (in Postgres, `pg_constraint` rows with `contype = 'f'` in both directions, and `pg_trigger`); grepping the code for table names misses them. Confirm by running each conflicting operation in an open transaction on a scratch database and reading its locks (`pg_locks`).
- Compare orders in both directions. If the migration locks A then B and any transaction locks B then A, they can deadlock. A foreign key is locked parent→child by every delete or key update on the parent (the action or check queries the child, with or without `CASCADE`) and child→parent by key checks on insert and update, so there may be no order safe against every path. Prefer one table per transaction. Otherwise take every lock up front, at its strongest mode, in the order that conflicts with the fewest paths. A single statement that locks two tables (`ADD FOREIGN KEY`) can't be split: keep it brief with `NOT VALID`, then `VALIDATE` in its own transaction (SHARE UPDATE EXCLUSIVE, plus ROW SHARE on the referenced table), and bound its wait.
- Flag lock upgrades: a weaker lock followed by a stronger one on the same table (for example `CREATE INDEX` then `ALTER TABLE ... ADD COLUMN`) can deadlock with any transaction that reads and then writes it. Fix: split the statements into separate migrations, or take the strongest lock first (`LOCK TABLE t IN ACCESS EXCLUSIVE MODE`), which then holds it through every slower statement after it.
- A deadlock doesn't hang: the database aborts one side, and which one is hard to predict, so the cost is a failed user request or a failed run. In Postgres, a `lock_timeout` below `deadlock_timeout` makes the migration give up first unless the application was already waiting longer than the difference; make the run safe to retry.
- Bound the wait (`lock_timeout` applies to each lock acquisition, not to how long locks are held). A waiting request blocks every later request that conflicts with it: a waiting ACCESS EXCLUSIVE blocks plain SELECTs, a waiting `CREATE INDEX` blocks writes. Know what a timeout leaves behind and how to recover.

## Size and duration

Measure on production data, read-only: table and index sizes, row counts, long-running transactions, and whether a validated constraint already proves what the migration adds (a CHECK proving NOT NULL avoids a full scan). Estimate how long each exclusive lock is held, and say whether the estimate is measured or extrapolated. Prefer forms that avoid long exclusive locks (in Postgres, `CREATE INDEX CONCURRENTLY`, or `NOT VALID` then `VALIDATE`); `CONCURRENTLY` can't run inside the runner's transaction and leaves an invalid index on failure.

## Data migrations

Batch backfills and commit between batches so no transaction holds row locks for long, and touch rows in the same order as the application.

## Reverse path

The down migration gets the same lock plan, ordering check, and test as the up migration. Write down the recovery for a failed run in the runner's own terms, for example which version to set and with what flag.

## Testing

Run it through the real runner, against the real schema, on an isolated environment. Reproduce each order conflict or upgrade in the lock plan deterministically with two sessions: open the application transaction and run its first step, start the migration, then run the application's second step. Then run the real conflicting operations in a loop while the up and down migrations run, as a smoke test only: a clean loop proves little, and holding a single competing lock only tests the timeout. Record the schema before and after, and the runner's version state after each failure.
