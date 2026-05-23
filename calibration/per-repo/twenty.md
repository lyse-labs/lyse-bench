# Twenty CRM

## Identity

- **URL:** https://github.com/twentyhq/twenty
- **Commit SHA:** `c2a48a1cc2aebccc5817864239f469005066a94a`
- **Description:** Modern open-source CRM, Nx monorepo, ~20 packages including `twenty-front`, `twenty-server`, `twenty-ui`, `twenty-emails`, `twenty-website`, `twenty-claude-skills`, etc.
- **Audit path:** `/tmp/calibration-twenty/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 46
- **autoTier:** Defined
- **findings:** 6469 (info: 2, warning: 6467)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 0 | 6409 | 8388 | -53 | 67 |
| a11y | 100 | 0 | 151 | 100 | 100 |
| components | 84 | 57 | 5160 | 98 | 84 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 3 | 3 | -33 | 94 |

## Manual inspection notes

- `packages/twenty-ui` is a real DS package with `accessibility/`, `components/`, `display/`, `feedback/`, `input/`, `layout/`, `navigation/`, `theme/`, `theme-constants/`, `utilities/`, `testing/`, and an `index.ts`.
- **`theme-constants/`** ships `ThemeProvider.tsx`, `theme-light.css`, `theme-dark.css`, `themeCssVariables.ts`, `constants.ts`, `getNextThemeColor.ts` — a real light/dark token contract.
- **71 `.stories.*` files** in `twenty-ui` — strong Storybook adoption (third behind Mantine 116 and unique to non-shadcn entries).
- Root **`CLAUDE.md`** with clear commands (`yarn start`, `npx nx test`, etc.).
- **`packages/twenty-claude-skills`** — a dedicated package of Claude Code skills for the project. This is a 2026-vintage AI affordance.
- No `AGENTS.md` at root, no `components.json` — the ai-surface axis scores 0 (and -33 rateScore) because it can't see the `twenty-claude-skills` package.
- Token drift: 6409 / 8388 = ~76% — **extremely high**. This is a monorepo-dilution artifact: the audit walks twenty-server, twenty-emails, twenty-website, twenty-front (60+ component files in twenty-front/src alone). The DS lives in twenty-ui (35 tsx files); everything else is app code.
- Components axis 84 with rateScore 98 (only 57 findings over 5160 opportunities = 1.1%) — strong DS-shape signal that the tokens axis drowns out.

## Final expert score

**Expert score:** 70  
**Expert tier:** Quantitative (60-79)

## Rationale

Twenty is one of the most under-scored repos in the cohort. The DS surface in `twenty-ui` is mature: light/dark CSS variables, ThemeProvider, themeCssVariables.ts, 71 stories, plus the unique `twenty-claude-skills` package which is a clear 2026-style AI affordance. The auto score of 46 (Defined) is 24 points too low because:

1. **Tokens axis at 0** is monorepo dilution — the audit walks twenty-server (NestJS backend code), twenty-emails, twenty-website, twenty-front (the actual app), twenty-docs, etc. Only twenty-ui is the DS; the rest are app/server code where hardcoded values are normal.
2. **ai-surface axis at 0** misses both the root CLAUDE.md AND the entire `twenty-claude-skills` package. The rule needs to detect skill/MCP packages within the monorepo, not just root files.

Expert score 70 places Twenty solidly in the Quantitative tier, behind Mantine (78) because Mantine's DS is published as an npm library and Twenty's `twenty-ui` is internal-only, but ahead of Plane (68) because Twenty's storybook adoption + AI-skills package + light/dark token contract is more comprehensive. The 24-point auto-vs-expert delta is the second-largest in the cohort (after Mantine 32) and reinforces that **K=8 over-penalizes tokens drift in monorepos that mix UI + server + marketing code**.
