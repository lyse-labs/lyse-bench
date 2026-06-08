# Bench methodology

This document explains how The Bench audits repos, picks corpus entries, and reports results. It is the public contract for what a "green Bench week" means and what daily/weekly quality looks like for Lyse.

## Audit invocation

For every corpus entry, The Bench runs:

```bash
git clone --depth 1 <url> /tmp/lyse-bench/<slug>
cd /tmp/lyse-bench/<slug>
git checkout <sha>                          # if pinned; falls back to HEAD
lyse audit . --format json --static-only    # no LLM augmentation
```

`--static-only` ensures determinism: no network calls beyond the initial clone, no model variance.

## Corpus tiers

### Tier 1 — 20 stratified repos, audited **nightly**

Tier 1 is the *fast-feedback* corpus. Stratified across:

| Dimension | Buckets (count out of 20) |
|---|---|
| Framework | react 12, vue 3, solid 2, svelte 2, framework-agnostic 1 |
| Stack | tailwind-v3 8, tailwind-v4 3, css-in-js 3, css-modules 3, vanilla-css 3 |
| Maturity | early 5, growing 10, mature 5 |
| Size | small 8, medium 8, large 4 |
| Use case | design-system 8, web-app 4, component-library 7, framework-starter 1 |

Picked so a single PR touching a core rule produces ≥ 3 score deltas in the next nightly. If a refactor touches no findings on any of the 20, the rule's coverage is suspect.

### Tier 2 — 50 broader-seed repos, audited **weekly** (sharded 4×)

Tier 2 is the *long-tail* corpus. Looser curation. Goal: detect drift across the OSS ecosystem that tier-1 misses (e.g., a CSS framework we don't cover, a niche component pattern).

The corpus grows over time via [`internal/bench/discover.ts`](https://github.com/lyse-labs/lyse/blob/main/internal/bench/discover.ts) (auto-discovery, currently stub; tracked at lyse-labs/lyse#127). Target by v1.0: 200+ entries.

### flakes.yaml

Auto-populated when a repo fails non-deterministically twice in a row. Flagged repos are skipped in subsequent runs until a maintainer investigates. Tracked at lyse-labs/lyse#126.

## What a "green Bench week" looks like

The 4 criteria a Bench week must meet to be "green":

1. **No determinism breaks**: every PR-merged scorer/rule change produces byte-identical results in tier-1 audits run twice (tracked at lyse-labs/lyse#125).
2. **No unexplained regressions**: any repo whose score drops > 5 points week-over-week has a tracked PR explaining the cause (e.g., "we fixed FP X, score dropped because we now catch real drift").
3. **Flake rate < 2%**: < 1 of 50 tier-2 repos in flakes.yaml at any moment.
4. **FP rate < 5% per rule**: per the [v2 spec](https://github.com/lyse-labs/lyse/blob/main/internal/specs/2026-05-23-ai-readiness-score-v2-design.md) §4.2, published per release at `fp-rates/`.

These criteria define ongoing quality. **The 4-week-green gate is no longer a v0.1.0 launch precondition** (waived 2026-06-08); the Bench continues as a daily quality observatory and rule-promotion validator.

## Calibration corpus

The 8-repo `calibration/score-calibration.jsonl` is the labeled set used to fit the scorer's `K` constant. Currently:

- `K = 0` (fitted; the `absoluteCap` term is inactive on this corpus)
- LOO MAE = **10.36 pts** (target ≤ 8; not met for v0.1)

See [ADR 0018](https://github.com/lyse-labs/lyse/blob/main/docs/decisions/0018-ai-readiness-score-formula.md) for full reasoning. Re-fit:

```bash
git clone https://github.com/lyse-labs/lyse
cd lyse
pnpm install
pnpm exec tsx internal/recall-suite/run-calibration.ts
```

Output is byte-identical to `calibration/calibration-result.json` in this repo.

## How to interpret a Bench result

For each repo:
- `finalScore`: integer 0-100 or `"N/A"`.
- `tier`: "Foundational" (0-19), "Managed" (20-39), "Defined" (40-59), "Quantitative" (60-79), "Autonomous" (80-100).
- `axes[]`: per-axis breakdown (`tokens`, `components`, `a11y`, `stories`, `ai-surface`).
- `findingCounts`: `{ error, warning, info }`.

A score of "N/A" means an axis had zero opportunities (e.g., a backend-only repo with no UI surface → tokens axis is N/A). N/A axes don't drag the final score.

## Reporting cadence

| Cadence | Trigger | Output |
|---|---|---|
| Nightly 02:30 UTC | tier-1 corpus | `reports/tier1/YYYY-MM-DD.{json,jsonl,md}` |
| Weekly 03:00 UTC Monday | tier-2 corpus, sharded 4× | `reports/tier2/YYYY-WW.{json,jsonl,md}` |
| Per-PR | tier-1 corpus, run twice | Determinism gate (lyse-labs/lyse#125) |

Each report includes:
- Full result for every repo
- Diff vs the previous run (score changes, finding shifts, new repos, removed repos)
- A markdown summary

## Contributing

- **Add a repo**: PR against `corpus/tier1.yaml` or `corpus/tier2.yaml`. Include framework + stack + maturity tags.
- **Dispute a score**: open an issue on [`lyse-labs/lyse`](https://github.com/lyse-labs/lyse/issues). The rule + scoring logic lives there; this repo only stores results.
- **Re-run calibration**: see the calibration corpus section above.
