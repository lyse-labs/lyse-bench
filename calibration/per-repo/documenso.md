# Documenso

## Identity

- **URL:** https://github.com/documenso/documenso
- **Commit SHA:** `6a20fefd7b865d1242d9abc559f81116b9d4b23d`
- **Description:** Open-source DocuSign alternative (Remix-based). npm monorepo with a `packages/ui` shadcn-derived DS and a `packages/tailwind-config` token package.
- **Audit path:** `/tmp/calibration-documenso/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 61
- **autoTier:** Quantitative
- **findings:** 675 (info: 1, warning: 674)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 76 | 543 | 9068 | 88 | 76 |
| a11y | 100 | 0 | 194 | 100 | 100 |
| components | 68 | 127 | 788 | 68 | 81 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 0 | 5 | 5 | -80 | 92 |

## Manual inspection notes

- `packages/ui/primitives/` has **60 shadcn-style component files** (accordion, alert-dialog, avatar, badge, button, calendar, card, checkbox, combobox, command, data-table, dialog, dropdown-menu, form/, ...). Largest primitive set in the non-flagship cohort.
- `packages/ui/components/` has higher-level composed components organized into `document/`, `field/`, `recipient/`, `template/`, `animate/`, `common/`.
- **`packages/tailwind-config`** is a dedicated token package: HSL-CSS-variable theme (background, foreground, primary, secondary, ring, warning, ...) — clean shadcn-style token contract.
- `packages/ui/tailwind.config.cjs` presets from `@documenso/tailwind-config` — proper token inheritance.
- Root **`AGENTS.md`** with concrete build/test/lint commands, code style rules (functional components, type over interface, descriptive variable names with auxiliary verbs).
- Other root docs: `ARCHITECTURE.md`, `CODE_STYLE.md`, `MANIFEST.md`, `SIGNING.md`, `WRITING_STYLE.md` — deeply documented.
- **No `components.json` at root**, no `.stories.tsx` files in `packages/ui` — the two gaps holding it below Mantine/shadcn.
- Token drift: 543 / 9068 = **~6% — the lowest drift rate in the cohort**, which is why the tokens axis hits 76 (capped by absoluteCap 76).
- Components axis 68 is the lowest in the cohort (besides Lyse self) — 127 findings over 788 opportunities = 16% miss rate on component conventions. The shadcn-derived primitives are clean, but the higher-level composed components in `packages/ui/components/` are drift-prone.

## Final expert score

**Expert score:** 66  
**Expert tier:** Quantitative (60-79)

## Rationale

Documenso is a well-organized shadcn-derived DS: 60 primitives, dedicated tailwind-config package with HSL CSS variables, AGENTS.md with runnable commands, deep architectural documentation. The auto score of 61 (Quantitative) is close to right; expert score 66 nudges up 5 points to reflect:

1. The **6% token drift rate** is genuinely strong (best in cohort) and rateScore 88 is correctly capped at 76 by absoluteCap — this is the formula working as intended for high-quality token discipline.
2. The 60-primitive shadcn-style primitive set is more mature than the components axis (68) credits it for.
3. Documenso publishes nothing as a public npm library — the DS is internal — so it doesn't reach the upper Quantitative range (70+).

The 5-point auto-vs-expert delta is one of the smallest in the cohort, suggesting Documenso-shaped repos (mid-size shadcn-derived internal DS with good token discipline) are roughly where K=8 calibrates well. The main calibration insight: the **components axis penalty is harsh** on internal-app-style higher-level composed components (the `document/`, `field/`, `recipient/` folders) where one-off styling is more legitimate than in a published DS.
