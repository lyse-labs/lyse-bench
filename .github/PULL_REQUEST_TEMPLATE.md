## What

<!-- 1-2 sentences: which file(s) changed and how (e.g. "add repo X to tier-2", "fix stale wording in methodology.md"). -->

## Why

<!-- Link an issue, or give the motivation in 1 sentence. -->

## Test plan

- [ ] If touching `corpus/tier1.yaml` or `corpus/tier2.yaml`: entry follows the `BenchEntry` schema (framework/stack/maturity/tags), and the `sha` was pinned via `git ls-remote <url> HEAD`
- [ ] If touching a tier-1 entry: `corpus/_meta.md` stratification tables updated to match
- [ ] Links checked (no dead references)
