---
name: additive-changes-to-pinned-pipelines
description: |
  Pattern for extending a data pipeline whose headline numbers are pinned by
  asserts and already shipped in deliverables. Use when: (1) adding a new data
  source, column, or metric to a comparison/benchmark whose existing numbers are
  locked, (2) an EXPECTED-style assert block guards published figures, (3) a
  vendor ships an alternate column for an already-scored field, (4) new input
  files may not exist yet when the pipeline runs.
author: Claude Code
version: 1.0.0
date: 2026-07-23
---

# Additive Changes to Pinned Pipelines

## When to Use

- A build script ends with an assert block pinning numbers that appear verbatim
  in shipped briefs/PDFs, and you need to add a source, field, stratification,
  or metric without moving any of them
- A source turns out to have a second, better column for an already-locked
  field (score it alongside, never swap in place)
- New inputs (extracts, vendor returns) will land later; the pipeline must
  build correctly both before and after they exist

## When NOT to Use

- Nothing downstream is shipped or pinned — just change the code
- The pinned number is genuinely wrong — then the fix is deliberate: change
  method + assert + every surface together, and say so; this skill is for
  *extending around* locked numbers, not correcting them

## Problem

Shipped numbers are load-bearing: stakeholders quoted them, PDFs carry them,
and an assert block is the only thing keeping code, JSON, and documents in
sync. Naive extension (adding a source to the same lists that feed the pinned
stats, or swapping a column in place) silently shifts locked values, fails the
asserts at best, or — worse — ships a brief that contradicts the one already
sent. The non-obvious part is that *every* extension can be expressed
additively if you separate data keys from display.

## Solution

### 1. New keys, never mutated values

Add sources/fields as new keys (new pairs, new JSON nodes, new CSV columns
appended). Anything pinned keeps computing from exactly the inputs it had.
Keep the assert block untouched and let it run every build — it is the
tripwire proving the extension was additive. Pin sets that feed locked numbers
explicitly (e.g., a triangulation or stratifier defined over the *original*
source list by name, not "all sources"), so membership doesn't shift when
sources arrive later.

### 2. Existence-gated loading

```python
if os.path.exists(path):
    load(...)   # else: pipeline builds without it, columns stay empty
```

The pipeline must run green before the data lands and pick it up on the next
build with zero further changes. Make targets gate the same way (wildcard on
the raw input).

### 3. Pseudo-source for an alternate column

When a vendor's return has a second column for an already-scored field (e.g.,
a modeled headcount that was scored plus a raw LinkedIn headcount that
wasn't), do not overwrite the scored values. Register a pseudo-source key
carrying only that field, compute its pairs separately, and substitute it at
the **display layer** (grids, capability tables, outlier passes) with a
visible label (`AFF*`) and a disclosure of what the default column does. The
locked pairs survive; the fair comparison ships.

### 4. Per-field role whitelist

A new source is often trustworthy per-field, not wholesale (one feed's funding
frozen for years; another's headcount a model rather than a measurement). Gate
fields with an explicit whitelist mapping source → allowed fields, and write
the *reason* next to each exclusion — the whitelist is a methodological claim,
and the reader of the code needs the why.

### 5. Coverage-driven display

Render a source in a grid only where its coverage for that field is > 0. Then
adding sources never requires touching display code, and role-restricted
sources never show empty rows in grids they don't participate in.

### 6. Deliverables are regenerable — preserve superseded versions on request

If generators are deterministic and pinned numbers didn't move, an overwritten
earlier deliverable can be reconstructed faithfully (same inputs → same
output) and saved under a versioned name. Prefer that over letting a "we lost
the original" moment happen.

## Verification

1. Full build passes with the new inputs absent AND present; the pinned assert
   block passes unchanged in both cases.
2. Diff the regenerated deliverable against the shipped one: every previously
   published number identical; only new sections/columns differ.
3. Observed: two sources plus a pseudo-source and a five-stratum section were
   added to a locked five-source benchmark this way; every pinned value held
   across all builds.

## See also

- `cross-source-agreement-rigor` — methodology for the numbers themselves
- `psql-handoff-scripts` — safely producing the new inputs
