---
name: cross-source-agreement-rigor
description: |
  Statistical discipline for comparing the same field across multiple data
  vendors/sources when ground truth is unknown. Use when: (1) building or
  defending agreement/confusion matrices between data providers, (2) asked "why
  do these agreement numbers look so low", (3) stratifying a benchmark by size
  or region, (4) choosing or defending a tolerance threshold, (5) two sources
  share an upstream and agreement might be lineage, not corroboration.
author: Claude Code
version: 1.0.0
date: 2026-07-23
---

# Cross-Source Agreement Rigor

## When to Use

- Scoring how often data vendors agree on a field (headcount, funding,
  location) across a shared company/entity list
- A stakeholder or vendor will challenge low-looking percentages
- Designing strata (size bands, regions) and deciding how fine to cut
- Any published cell where n, tolerance, or lineage could be questioned

## When NOT to Use

- Ground truth exists (labels, filings, registry) — score accuracy directly
- Single-source data quality profiling (fill rates, freshness) with no
  cross-source comparison

## Problem

Agreement matrices compound many effects — coverage, freshness, definitions,
lineage, threshold choice, sample size — into one percentage. Published naively
they misrepresent: a "low" cell reads as "bad vendor" when it may mean
"different event universe," and a "high" cell reads as corroboration when it
may be two resellers of one upstream. The defense has to be quantified before
someone asks, and every decomposition below is cheap to compute from the data
you already have.

## Solution

### 1. Truth-unknown framing

Score pairwise agreement, never accuracy. Absence is coverage, never
disagreement. "Who's right" only via triangulation (odd-one-out among ≥3
sources) — and state that an outlier is *unconfirmed*, not proven wrong.

### 2. Every cell carries n; strata are sized before they're promised

CI half-width ≈ 1.96·√(0.25/n): n=70 → ±12pp, n=190 → ±7pp. Before promising a
stratification, count pair-n per cell; cells under ~30–50 pairs are
directional only and must be visibly marked (dashed, greyed) rather than
silently printed. When asked "can we stratify finer?", answer with the cell
counts and the CI widths, not intuition.

### 3. Lineage vs independence

Identify shared upstreams first (aggregator merges source A; vendor resells
registry B). Agreement along a lineage edge is self-confirmation — annotate or
remove it (e.g., drop the downstream aggregator from grids entirely rather
than letting it double-count its own inputs). State in the deliverable that
agreement is corroboration, not independence.

### 4. Conditional decomposition for event-derived fields

Fields that are sums or latests over events (funding totals, last round) need
conditioning on "describing the same event": compare amounts only where the
two sources' event dates fall within a window (e.g., 31 days), and separately
where they clearly differ (>92 days). If same-event agreement is high
(observed: 72–92% vs 8–26% raw), the low headline cells measure event-set
coverage, not numeric quality — a completely different vendor conversation.
This single split is usually the strongest defense available.

### 5. Threshold defense

Compute the gap distribution, not just the pass rate: share within tolerance,
in the near-miss band (e.g., 10–25%), and beyond 2×. If disagreements are
lumpy (whole events present on one side, absent on the other), loosening the
threshold barely moves the cell — say so with the numbers, which answers
"you picked a harsh threshold" before it's asked.

### 6. Artifact checks before narrative

Before interpreting any gap as a quality signal, rule out:
- **Column choice within a vendor** — vendors ship modeled and raw columns for
  the same concept that can disagree with each other >50% of the time; score
  like-for-like columns across vendors
- **Date precision** — share of Jan-1 and month-first dates (year/month
  bucketing); pick date windows accordingly
- **Format/unit mixing** — ISO codes vs names, currency, thousands vs units;
  normalize both sides before counting a mismatch
- **Population plausibility** — is the "missing" data consensus-true? (If no
  source shows a recent event for half the entities, recency gaps are the
  entities' reality, not vendor staleness.)

### 7. Deterministic and reproducible

Mode/tie-breaks sorted-then-max (set iteration order varies across runs);
every published number computed by the pipeline and, where hand-copied into
documents, pinned by asserts.

## Verification

1. For each headline cell you can state: n, CI, tolerance sensitivity, lineage
   status, and (for event fields) the same-event conditional rate.
2. Re-running the pipeline twice produces identical output byte-for-byte.
3. Observed: raw funding agreement of 18–38% decomposed into 72–92% same-round
   agreement plus event-coverage/vintage differences, with the threshold shown
   to move cells by only a few points — the "low numbers" question answered
   with data on the page.

## See also

- `additive-changes-to-pinned-pipelines` — extending the pipeline safely
- `psql-handoff-scripts` — extracting comparison data from big source tables
