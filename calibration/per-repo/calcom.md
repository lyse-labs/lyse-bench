# Cal.com

## Identity

- **URL:** https://github.com/calcom/cal.com
- **Commit SHA:** `180ede28f0bddf2738933a6e60a8e80f6116d7da`
- **Description:** Open-source scheduling platform (Next.js, Yarn + Turbo monorepo). Has its own `packages/ui` design system, plus an `apps/web/components.json` (shadcn-style).
- **Audit path:** `/tmp/calibration-cal/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 58
- **autoTier:** Defined
- **findings:** 4256 (info: 1, warning: 4255)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 48 | 4163 | 16003 | 48 | 69 |
| a11y | 100 | 0 | 393 | 100 | 100 |
| components | 82 | 89 | 1303 | 86 | 82 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 4 | 5 | -40 | 93 |

## Manual inspection notes

- `AGENTS.md` is one of the most detailed in OSS: concrete do/don't list, conventional commits, direct import rule (no barrel imports), Prisma `select` over `include`, type imports, etc.
- `CLAUDE.md` also present at root.
- `packages/ui` has `classNames.ts`, `components/`, `styles/` (shared-globals.css + useCalcomTheme), and a `scripts/` folder.
- `apps/web/components.json` exists (shadcn registry config), but at the app level rather than root.
- Biome enforced via `biome.json` + `biome-staged.json`.
- 16003 token opportunities is the second-largest in the cohort → real DS surface.
- Token drift rate: 4163 / 16003 = ~26%, which is mid-tier — visible but not catastrophic.
- Components axis 82 reflects the disciplined packages/ui module organization.
- ai-surface 0 is harsh: AGENTS.md is at root but no LYSE.md, no components.json at root.

## Final expert score

**Expert score:** 62  
**Expert tier:** Quantitative (60-79)

## Rationale

Cal.com sits firmly in the Quantitative tier in my judgment. The auto score of 58 (Defined) feels 4-5 points light because it underweights two strong governance signals: (1) the depth and command-density of `AGENTS.md` (which a human reading the repo would immediately flag as "this team is serious about AI ergonomics"), and (2) the fact that the `apps/web/components.json` does exist — it's just not at root, so the audit doesn't pick it up. The 26% token drift rate is real and pulls the score down honestly; the disciplined `packages/ui` organization (components, styles, classNames utility) pulls it back up.

Expert score 62 places Cal.com just inside Quantitative. Compared to Mantine (78) and shadcn (80), Cal.com has weaker DS-as-product signals (no Storybook, no published library, the DS exists to serve the app) — so it shouldn't reach the upper-quartile Quantitative range. The 4-point delta (auto 58 → expert 62) is small and suggests the formula's handling of Cal.com-shaped repos is close to right.
