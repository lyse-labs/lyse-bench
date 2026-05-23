# Bench corpus — v0.1 metadata

## Size

| Tier | Entries | Cadence |
| --- | --- | --- |
| tier-1 | 20 | nightly |
| tier-2 | 50 | weekly |
| **Total** | **70** | — |

The original v2 spec called for tier-1 = 50 and tier-2 = 200. The v0.1 MVP corpus
is intentionally smaller so the Bench can ship and run reliably on launch
infrastructure. Phase 6+ will grow tier-1 toward 50 and tier-2 toward 200 as the
Bench earns confidence (see `internal/specs/2026-05-23-ai-readiness-score-v2-design.md`
§7 for the full ramp plan).

## Tier-1 stratification

Twenty repos chosen to cover the full Lyse use case along five dimensions.

### Framework

| react | vue | solid | svelte | agnostic |
| --- | --- | --- | --- | --- |
| 12 | 3 | 2 | 2 | 1 |

### Styling stack

| tailwind-v3 | tailwind-v4 | css-in-js | css-modules | vanilla-css |
| --- | --- | --- | --- | --- |
| 8 | 3 | 3 | 3 | 3 |

### Maturity

| early | growing | mature |
| --- | --- | --- |
| 5 | 10 | 5 |

### Size (via `size:*` tag)

| small (<50 components) | medium (50-200) | large (200+ / monorepo) |
| --- | --- | --- |
| 8 | 8 | 4 |

### Use case (via `use-case:*` tag)

| design-system | web-app | component-library | framework-starter |
| --- | --- | --- | --- |
| 8 | 4 | 7 | 1 |

The use-case bucket targets in the spec (8 / 6 / 4 / 2) were relaxed to fit real
candidate availability. Standalone component libraries are over-represented at
the expense of web-apps and starters; this is an explicit MVP trade-off.

## Tier-2 stratification

Looser — 50 repos selected for diversity rather than precision. Coverage spans:

- **Established design systems**: Ant Design, MUI Spectrum, Adobe Spectrum, Carbon, FluentUI, Polaris, Blueprint, BaseWeb
- **Tailwind ecosystem**: daisyUI, Flowbite (vanilla + react + svelte), Tremor, Tabler, originui, magicui, sailboatui
- **Vue / Nuxt**: PrimeVue, PrimeReact, Quasar, Vant, Arco, Amis, Yamada
- **Native / cross-framework**: Tamagui, Material Web, Spectrum Web Components, Lit
- **Notable web apps**: Excalidraw, tldraw, Hoppscotch, AFFiNE, Outline, Rocket.Chat, Mattermost, Bluesky, Supabase, Appwrite, Dub, Coolify, Umami, PostHog, n8n, Reactive-Resume, OpenStatus, Medusa
- **Framework starters / docs**: Starlight, Nextra

## SHA pinning

All SHAs were resolved on **2026-05-23** via:

```sh
git ls-remote "${url}" HEAD | awk '{print $1}'
```

The Bench runner (`internal/bench/run.ts`) handles missing SHAs by falling back
to the cloned default branch (`HEAD`). If a pinned SHA fails to fetch (some Git
hosts reject by-SHA fetches), the runner falls back to a full fetch + checkout.

### Repos that did not resolve at curation time

- `planetscale/beam` — repo appears archived or deleted; **dropped** in favour
  of `vercel/commerce`.

## Adding a repo

1. Open a PR against this directory.
2. Add an entry to `tier1.yaml` or `tier2.yaml` matching the schema in
   `internal/bench/types.ts` (`BenchEntry`).
3. Pin the SHA via `git ls-remote ${url} HEAD`.
4. For tier-1 entries, update the stratification tables above so reviewers can
   confirm the bucket targets still balance.
5. CI will validate format (Task 5.7).

## Removing / quarantining a repo

- A repo that fails non-deterministically across multiple runs should be moved
  to `flakes.yaml` via PR. Task 5.5 will eventually automate this.
- For licence or maintenance reasons, drop the entry and document the reason
  here.
