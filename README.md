# lyse-bench

**The Bench** is Lyse's continuous self-validation system. It audits hundreds of open-source design systems weekly and publishes:

- Per-repo Health Scores tracked over time
- Score-change diffs week-over-week
- False-positive rate per rule, per release
- Calibration corpus + LOO MAE (how wrong our scoring is)

This repo is **read-only** from the public's perspective: the runners + audit logic live in [`lyse-labs/lyse`](https://github.com/lyse-labs/lyse). Reports land here via GitHub Actions cron jobs.

## What's here

```
corpus/                          # The 70-repo corpus audited by The Bench
  tier1.yaml                     # 20 stratified repos audited nightly
  tier2.yaml                     # 50 broader-seed repos audited weekly
  flakes.yaml                    # Auto-blacklisted non-deterministic repos
  _meta.md                       # Stratification + curation rationale

calibration/                     # How Lyse calibrates its Health Score formula
  score-calibration.jsonl        # 8-repo expert-labeled corpus
  calibration-result.json        # Fit result: K value, LOO MAE
  calibration-axis-data.json     # Per-axis data extracted from audits (for reproducibility)
  per-repo/                      # Long-form rationale per labeled repo

reports/                         # Weekly Bench reports (auto-generated)
  tier1/                         # Daily tier-1 results
  tier2/                         # Weekly tier-2 results
```

## Why publish this?

Every audit tool faces a credibility problem: how do you know its 73/100 score actually means something? Most tools dodge this. Lyse doesn't.

- **Falsifiable**: anyone can re-run [`run-calibration.ts`](https://github.com/lyse-labs/lyse/blob/main/internal/recall-suite/run-calibration.ts) on this corpus and verify our published K + LOO MAE.
- **Auditable**: the corpus is public. Reports are public. The runner code is public.
- **Self-incriminating**: our LOO MAE is currently **10.36 pts** (target ≤ 8). We publish it anyway.

See [docs/calibration.md](https://github.com/lyse-labs/lyse/blob/main/docs/calibration.md) and [ADR 0018](https://github.com/lyse-labs/lyse/blob/main/docs/decisions/0018-ai-readiness-score-formula.md) on the Lyse repo for the full methodology.

## Launch gate

Lyse v0.1.0 ships when this repo shows **4 consecutive weeks of green Bench reports** (no determinism breaks, no unexplained regressions, flake rate < 2%).

Current status: bootstrapping. First reports will land within a week of this repo's creation.

## License

The corpus YAML files + reports are CC-BY-4.0. Lyse the tool is AGPL-3.0 + Commercial dual-licensed.

## Contributing

To suggest a repo for the corpus, open a PR against `corpus/tier1.yaml` or `corpus/tier2.yaml`. See [methodology.md](methodology.md) for the rubric.

To dispute a Health Score on your project, open an issue on [`lyse-labs/lyse`](https://github.com/lyse-labs/lyse/issues) — the rule + scoring logic lives there.
