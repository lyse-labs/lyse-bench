# shadcn/ui

## Identity

- **URL:** https://github.com/shadcn-ui/ui
- **Commit SHA:** `4a4dc8eb0fc793d8e9225e780183ad605f15d2c2`
- **Description:** Copy-paste React component library built on Radix + Tailwind. The dominant convention for AI-readable design systems (the `components.json` schema is now an industry standard). pnpm + Turbo monorepo with `packages/shadcn` (CLI + registry + MCP) and `apps/v4`.
- **Audit path:** `/tmp/calibration-shadcn/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 60
- **autoTier:** Quantitative
- **findings:** 6037 (info: 2, warning: 6035)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 64 | 5971 | 33180 | 64 | 67 |
| a11y | 83 | 58 | 685 | 83 | 83 |
| components | 91 | 6 | 2019 | 99 | 91 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 2 | 2 | 0 | 96 |

## Manual inspection notes

- **`apps/v4/components.json`** — the canonical AI-readable DS manifest. `$schema: "https://ui.shadcn.com/schema.json"`, style "new-york", baseColor "neutral", cssVariables true, aliases mapping for components/utils/ui/lib/hooks, iconLibrary "lucide".
- **`apps/v4/registry.json`** — explicit component manifest the CLI uses to copy components into user projects.
- **`packages/shadcn/src/mcp/`** — a dedicated MCP server package.
- **`packages/shadcn/src/skills/`** — referenced in `apps/v4/` as `skills/` — AI workflow skills for the CLI.
- `packages/shadcn/src/base-colors.ts`, `colors.ts`, `tailwind.css`, `migrations/`, `preflights/`, `preset/` — token plumbing.
- `apps/v4/registry/` contains `__blocks__.json`, `__index__.tsx`, `base-colors.ts`, `bases.ts`, `bases/`, ... — a fully-structured registry.
- **No root `AGENTS.md` or `CLAUDE.md`** — the components.json IS the manifest. The ai-surface axis returns 0 because it expects root-level files.
- Token opportunities: **33180** — the largest in the cohort. The DS surface is enormous.
- Token drift: 5971 / 33180 = ~18%, which is healthy at that volume.
- a11y axis 83 is the lowest among non-Lyse repos — 58 findings on 685 opportunities (8.5%) is real signal, likely from registry examples / dashboard demos.

## Final expert score

**Expert score:** 80  
**Expert tier:** Autonomous (80-100)

## Rationale

shadcn/ui is the only repo in the cohort that reaches the Autonomous tier in my judgment. It is the canonical example of what an AI-readable design system looks like in 2026:
- `components.json` is now an industry-wide schema, with thousands of downstream repos adopting it
- A `registry.json` makes every component a named, addressable, copy-installable artifact
- An `mcp/` package ships the DS as a tool surface for AI agents
- A `skills/` directory codifies repeatable workflows

The auto score of 60 (Quantitative) is **20 points light** because the ai-surface axis scores 0 — the rule presumably looks for `AGENTS.md` / `LYSE.md` at root, but shadcn's whole AI strategy is structured around `components.json` and `registry.json`. This is the most important calibration signal in the corpus: **the ai-surface rule needs to detect `components.json` as a positive signal**, not just AGENTS.md.

Expert score 80 places shadcn right at the Quantitative / Autonomous boundary. It doesn't go higher (e.g., 85) because the a11y axis at 83 reflects real gaps in the v4 dashboard examples, and the token drift at 18% is good but not zero. The 20-point auto-vs-expert delta is the clearest evidence in this calibration corpus that the ai-surface axis needs reweighting for the `components.json`-style of AI manifest.
