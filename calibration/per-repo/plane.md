# Plane

## Identity

- **URL:** https://github.com/makeplane/plane
- **Commit SHA:** `039d582fbb904ef0d1d3d6b6e1c9fef9696beb13`
- **Description:** Open-source project management platform (Next.js, pnpm + Turbo monorepo). Ships its own `@plane/ui` design system with a dedicated `packages/tailwind-config` token package.
- **Audit path:** `/tmp/calibration-plane/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 64
- **autoTier:** Quantitative
- **findings:** 2559 (info: 1, warning: 2558)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 70 | 2519 | 18069 | 72 | 70 |
| a11y | 100 | 0 | 930 | 100 | 100 |
| components | 85 | 36 | 2532 | 97 | 85 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 4 | 5 | -40 | 93 |

## Manual inspection notes

- **Best token discipline in the cohort:** dedicated `packages/tailwind-config` with `variables.css`, `animations.css`, `index.css`, and a token-scoped `AGENTS.md`.
- Root `AGENTS.md` is concise and command-first: `pnpm dev`, `pnpm check`, `pnpm fix`, `pnpm --filter=@plane/ui storybook`.
- `packages/ui/src` is organized by component family: auth-form, avatar, badge, breadcrumbs, button, card, color-picker, dropdown, ... — well-named, well-grouped.
- Storybook setup exists but only **6 story files** — large gap vs. Mantine (116) and Twenty (71).
- Token drift rate: 2519 / 18069 = ~14% — the **lowest in the cohort**.
- Uses OxLint, oxfmt — modern fast tooling, consistent with the project's modernization signals.
- 18069 token opportunities is the second-largest in the cohort (after shadcn) → real DS surface to measure against.

## Final expert score

**Expert score:** 68  
**Expert tier:** Quantitative (60-79)

## Rationale

Plane has the most disciplined token architecture of the cohort — a separate `tailwind-config` package with its own AGENTS.md is exactly what the Lyse formula should reward. The auto score of 64 (Quantitative) is close to right; expert score 68 nudges up 4 points because (1) the `packages/tailwind-config` scoped AGENTS.md is a strong AI-readable signal the ai-surface axis misses (it requires root-level files), and (2) the 14% token drift rate is genuinely the best in the cohort, which the formula's `min(rateScore=72, cap=70)` handicaps slightly by the cap.

Plane sits below Mantine (78) because the component surface is smaller and the Storybook adoption (6 stories) is weak. It sits above Documenso (66) because its token discipline is more clearly architected. The 4-point delta confirms the formula already handles repos with clean token packages well — Plane is one of the cases where K=8 is roughly correct.
