---
name: psql-handoff-scripts
description: |
  Review checklist for SQL scripts handed to a user to run via psql (\i or -f),
  especially against large tables. Use when: (1) writing a .sql file someone else
  will execute interactively, (2) a join condition contains computed expressions
  (regexp_replace, lower, split_part) on the big table's side, (3) a script "goes
  silent" mid-run on a multi-million-row join, (4) a script stages temp tables or
  uses \copy.
author: Claude Code
version: 1.0.0
date: 2026-07-23
---

# psql Handoff Scripts: Hashability, Idempotency, Client-Side Semantics

## When to Use

- Writing or reviewing a `.sql` script another person will run via `\i` in a
  connected psql session, or via `psql -f`
- Any join whose condition computes expressions over the large table's columns
- A handed-off script produced output up to some point, then went silent for
  many minutes (the join after that point is likely nested-looping)
- Scripts that create temp tables, temp functions, or write CSVs with `\copy`

## When NOT to Use

- Queries you run yourself with the ability to EXPLAIN and iterate interactively
- Small-table joins (row-count product under ~10M) where plan shape barely matters
- ORM/application SQL (different review surface: parameterization, N+1, indexes)

## Problem

Postgres can only hash-join or merge-join on a **single equality**. A join
written as

```sql
JOIN big b ON t.key IN (expr1(b.col), expr2(b.col), expr3(b.col))
```

is an OR of three equalities, so the planner falls back to a nested loop:
every probe row × every big-table row, evaluating the expressions per pair.
1,500 probe rows × 3.7M big rows ≈ 5.5B regexp evaluations — hours, and the
person you handed the script to just sees psql hang with no error. The failure
is silent and lands on someone else's terminal, which is why it belongs in a
pre-handoff review, not post-hoc debugging.

Separately, scripts written assuming "one psql invocation per script" break
when run via `\i` in a shared interactive session: temp tables persist for the
session, so a second `\i` (or a preflight script that staged the same table)
collides or silently double-loads via `\copy` append.

## Solution

### 1. Make every join a single hashable equality

Move multi-key matching to the big table's side with a lateral unnest, so each
expression is computed once per big-table row and the join key is one column:

```sql
FROM big b
CROSS JOIN LATERAL unnest(ARRAY[expr1(b.col), expr2(b.col), expr3(b.col)]) AS k(key)
JOIN probe t ON t.key = k.key
```

Add `SELECT DISTINCT` (or dedupe downstream): one big-table row can now match
the same probe row through more than one key. Cost drops to one seq scan of
the big table + a hash join — seconds to minutes.

### 2. Estimate the row-count product before shipping

For every join in the script, ask: rows(left) × rows(right) if this
nested-loops — and does the condition allow a hash join (single `=` between
one expression of each side)? If you cannot answer, EXPLAIN it or restructure.

### 3. Idempotency for interactive sessions

- `DROP TABLE IF EXISTS <every temp table>` before each `CREATE TEMP TABLE`
  (a failed CREATE with `ON_ERROR_STOP off` leaves the old table, and a
  rerun's `\copy` then appends — silently doubling rows).
- `CREATE OR REPLACE FUNCTION pg_temp.<fn>` — plain CREATE fails on re-run.
- Remember `\set ON_ERROR_STOP on` persists in the user's session after the
  script ends; tell them, or reset it at the end.

### 4. Two-script pattern: preflight + extract

Preflight runs with `ON_ERROR_STOP off` so every section prints what it can
(schema presence, row counts, freshness, value-format samples, match rates);
the extract runs with it on. The preflight output tells you how to fix the
extract without another round-trip through the user.

### 5. `\copy` semantics

`\copy` is client-side: paths resolve on the runner's machine, so absolute
paths work regardless of where the server is. It must be a single line — build
a temp result table first, then `\copy that_table TO '<abs path>' WITH (FORMAT
csv, HEADER true)`. End the script with a summary SELECT (matched counts,
per-field fill) so the runner can paste one block that tells you everything.

## Verification

1. Every join in the script is a single-equality hash-join candidate; no `IN
   (expr, expr, ...)` across a join boundary.
2. Running the script twice in one psql session produces identical output and
   row counts (no collisions, no doubled `\copy` loads).
3. Observed: the pre-fix version of this pattern hung a user's session on a
   1,502 × 3.68M nested loop; the lateral-unnest rewrite completed in minutes
   in the same session.

## See also

- `cross-source-agreement-rigor` — what to do with the extracted data
