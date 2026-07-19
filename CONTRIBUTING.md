# Contributing to lyse-bench

This repository holds the corpus, calibration data, and audit reports that
back [Lyse](https://github.com/lyse-labs/lyse)'s self-validation program
("The Bench"). It has no runtime code of its own — contributions are data
and documentation changes.

## Code of Conduct

This project adheres to the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md).
By participating, you are expected to uphold it.

## Ways to contribute

- **Add a repo to the corpus** — open a PR against `corpus/tier1.yaml` or
  `corpus/tier2.yaml`. Include `framework`, `stack`, and `maturity` tags, and
  pin the `sha` via `git ls-remote <url> HEAD`. For tier-1 additions, also
  update the stratification tables in `corpus/_meta.md` so reviewers can
  confirm the bucket targets still balance. See the
  [add-repo issue template](.github/ISSUE_TEMPLATE/add-repo.yml).
- **Dispute a Health Score** — the rule and scoring logic live in
  [`lyse-labs/lyse`](https://github.com/lyse-labs/lyse), not here. Open an
  issue there; this repo only stores results. See the
  [dispute-score issue template](.github/ISSUE_TEMPLATE/dispute-score.yml).
- **Fix documentation** — typos, stale numbers, broken links, clarity fixes
  are all welcome as direct PRs.
- **Re-run calibration** — see the "Calibration corpus" section of
  [methodology.md](./methodology.md).

## License

By contributing, you agree your contribution is licensed under this
repository's [CC BY 4.0 license](./LICENSE).
