# Mantine

## Identity

- **URL:** https://github.com/mantinedev/mantine
- **Commit SHA:** `36f342822649a36bfb03e32df3de75efa4552205`
- **Description:** Flagship React UI library and design system. Yarn monorepo of `@mantine/*` packages including `core`, `dates`, `charts`, `notifications`, `spotlight`, `vanilla-extract`, `mcp-server`, etc.
- **Audit path:** `/tmp/calibration-mantine/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 46
- **autoTier:** Defined
- **findings:** 3690 (info: 1, warning: 3689)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 0 | 3641 | 6283 | -16 | 69 |
| a11y | 100 | 0 | 293 | 100 | 100 |
| components | 84 | 46 | 2029 | 95 | 84 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 3 | 5 | 0 | 94 |

## Manual inspection notes

- **110 components in `@mantine/core/src/components`** — the largest component count in the cohort.
- **116 story files** under `@mantine/core/src` (`*.story.tsx` convention).
- Ships both `AGENTS.md` and `CLAUDE.md` at root with runnable commands (typecheck, oxlint, jest, stylelint, syncpack).
- Dedicated `llms/` folder (currently just `testing.md` but indicates intent).
- 19 sibling `@mantine/*` packages (charts, dates, dropzone, spotlight, notifications, ...) — comprehensive DS surface.
- `MantineProvider` with first-class theme tokens, `vanilla-extract` integration.
- `mcp-server` package — first-class AI tooling.
- Token drift rate: 3641 / 6283 = ~58%, which **looks bad but is misleading** — the audit walks `apps/docs` and `apps/website` too, where hardcoded values for marketing pages are expected.
- Components axis 84 / cap 84 = the rateScore is 95 (only 46 findings over 2029 opportunities = 2.3%) — this is the real signal of DS health.

## Final expert score

**Expert score:** 78  
**Expert tier:** Quantitative (60-79)

## Rationale

Mantine is one of the most mature React design systems in OSS — comparable to Chakra UI or Radix Themes in scope, with arguably stronger AI affordances (mcp-server package, llms/ folder, both AGENTS.md and CLAUDE.md). The auto score of 46 (Defined) is **wildly off** because the tokens axis collapses to 0: the audit walks the whole monorepo including apps/docs and apps/website, where marketing-page hardcoded values dominate. The components axis (84) correctly identifies the DS quality but isn't weighted heavily enough to compensate.

The 32-point auto-vs-expert delta is the largest in the cohort and is the strongest signal that **K=8 is too aggressive on the tokens axis for repos with mixed app+library content**. If the audit could isolate the `packages/@mantine` subtree, the tokens axis would almost certainly be 60+ rather than 0.

Expert score 78 places Mantine at the top of Quantitative, just below the Autonomous threshold. It doesn't quite reach 80 because there is no root `components.json` manifest (shadcn-style) and no equivalent canonical AI-readable spec — the AGENTS.md is good but command-oriented, not component-manifest oriented.
