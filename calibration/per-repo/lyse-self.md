# lyse (self)

## Identity

- **URL:** https://github.com/lyse-labs/lyse
- **Commit SHA:** `1e2ad82dd8008f1b4c05010c93473223645daddd`
- **Description:** Lyse itself — design system drift scanner CLI + library + MCP server. This is the dogfood data point.
- **Audit path:** `/tmp/calibration-lyse/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 37
- **autoTier:** Managed
- **findings:** 527 (info: 2, warning: 525)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 0 | 521 | 708 | -47 | 76 |
| a11y | 100 | 0 | 96 | 100 | 100 |
| components | 47 | 4 | 15 | 47 | 92 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 2 | 2 | 0 | 96 |

## Manual inspection notes

- Lyse is a **CLI tool, not a UI repo** — there is no design system to audit.
- The audit walks `packages/core/src/**` which contains TypeScript engine code, not styled components.
- 521 token findings against 708 opportunities = ~74% "drift" rate. This is a measurement artifact: the audit treats any styled-token-like construct as a finding, but Lyse legitimately has no token contract.
- 4 component findings / 15 opportunities is tiny because there are barely any tsx/jsx files.
- No `AGENTS.md`, no `LYSE.md`, no `components.json`. Only `CLAUDE.md` at root (developer instructions for AI agents).
- No Storybook, no design tokens, no `tailwind.config.ts`.
- a11y axis is 100 because there is essentially no markup to fail.

## Final expert score

**Expert score:** 35  
**Expert tier:** Managed (20-39)

## Rationale

Lyse self is a "control" data point — a repo with legitimately no design system surface. The auto score of 37 is broadly defensible: the formula correctly identifies the absence of any DS signal (tokens 0, ai-surface 0, components 47 with only 15 opportunities). The expert score lands at 35 (slightly below 37) because the absoluteCap-driven score of 76 on tokens is generous — for a non-DS repo, a Managed-tier label is the honest answer, not a Defined-tier one.

This data point is useful for calibration as a lower-bound anchor: it confirms the formula handles "non-DS" cases without collapsing to 0 or inflating. The 2-point auto-vs-expert delta here is the smallest in the cohort, which suggests the formula's behavior on edge cases (no real DS) is roughly correct already.
