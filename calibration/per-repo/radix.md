# Radix UI

## Identity

- **URL:** https://github.com/radix-ui/primitives
- **Commit SHA:** `22473d16404bfd446305db5b6c9308aece99fdec`
- **Description:** Low-level, unstyled, accessible UI primitives for React. The de-facto canonical headless component library (used by shadcn/ui, Vercel, Linear, etc.). pnpm monorepo with one package per primitive.
- **Audit path:** `/tmp/calibration-radix/lyse.json`

## Audit (auto-computed, scoring-v1, K=8)

- **finalScore:** 57
- **autoTier:** Defined
- **findings:** 499 (info: 2, warning: 497)

| axis | score | findings | opportunities | rateScore | absoluteCap |
|---|---|---|---|---|---|
| tokens | 0 | 495 | 766 | -29 | 76 |
| a11y | 100 | 0 | 309 | 100 | 100 |
| components | 94 | 2 | 310 | 99 | 94 |
| stories | N/A | 0 | 0 | N/A | N/A |
| ai-surface | 33 | 2 | 3 | 33 | 96 |

## Manual inspection notes

- **30+ primitives**, each in its own `packages/react/<name>` package: accordion, alert-dialog, avatar, checkbox, collapsible, dialog, dropdown-menu, hover-card, navigation-menu, popover, slider, switch, tabs, toast, tooltip, ...
- Includes infrastructure packages too: collection, compose-refs, context, direction, dismissable-layer, focus-guards, focus-scope, primitive, slot.
- **No styling library** by design — Radix is headless. Tokens axis at 0 with 65% drift rate is a measurement artifact: most "drift" is on test/demo code, not on the primitives themselves.
- Components axis 94 / cap 94, with only 2 findings over 310 opportunities — the **cleanest component surface in the cohort**.
- `philosophy.md` at root with design rationale.
- `context7.json` present (AI documentation surface).
- **No `AGENTS.md`, no `CLAUDE.md`, no `components.json`.** The ai-surface axis catches context7.json (score 33).
- a11y axis 100 with 309 opportunities — this is a high-volume a11y surface, and zero findings is real signal (accessibility is Radix's entire raison d'être).

## Final expert score

**Expert score:** 65  
**Expert tier:** Quantitative (60-79)

## Rationale

Radix Primitives is the gold standard for headless accessible components — every other React DS in this cohort either uses Radix directly (shadcn, Documenso) or competes with it (Mantine). The auto score of 57 (Defined) underweights this canonical status because the tokens axis is structurally 0 for a headless library (no styling = no tokens to drift from), and the ai-surface axis is 33 because there's no root AGENTS.md or LYSE.md.

Expert score 65 lifts Radix into Quantitative, reflecting:
- The components axis 94 (essentially perfect)
- The a11y axis 100 with 309 opportunities (real signal, not noise)
- The clean module-per-primitive architecture
- The `philosophy.md` + `context7.json` AI-readable surface

But Radix doesn't reach the upper Quantitative range (70-80) because it lacks a unified AI manifest (no AGENTS.md with runnable commands, no components.json mapping every primitive). The 8-point auto-vs-expert delta is moderate and suggests the formula should give a small bonus for headless libraries where token drift is structurally zero.
