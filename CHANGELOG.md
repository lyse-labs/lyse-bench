# Changelog

All notable changes to this repository will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
This repository has no versioned releases — it's a data corpus (`corpus/`,
`calibration/`) plus auto-generated reports (`reports/weekly/`), not
software. This changelog tracks notable changes to the corpus and
methodology, not every commit; day-to-day weekly report additions are not
listed here — see the git history or `reports/weekly/` directly for those.

## [Unreleased]

### Changed

- Repository hygiene pass: fixed `reports/README.md` (described a report
  schema that was never built), deduplicated the `tremorlabs/tremor` corpus
  entry (was listed in both `corpus/tier1.yaml` and `corpus/tier2.yaml`),
  restored standard CC BY 4.0 license text for GitHub license detection, and
  added community-health files.
