# Capanema Design System

The design documentation for **capanema.io** — the personal executive portfolio of a
senior technology leader (CTO / Engineering Executive / data-platform & AI practitioner).

This document describes the design system itself: the brief, the design language, the
four-layer architecture, the complete token reference, the component inventory, the
pattern conventions, and the version history. For **how the repository is organized** see
[`README.md`](./README.md); for **how to work on the `.pen` source in Pencil** see
[`CLAUDE.md`](./CLAUDE.md).

> The system is authored in **Pencil** (the `.pen` design tool), not in code. **v3.1 is the
> current version** (`design-system-v3.1.pen`). The next step is implementing capanema.io in
> code from the v3.1 blueprints.

---

## 1. The brief

The site is an **executive portfolio**, not a startup landing page. Its job: help a visitor
quickly understand who the person is, what problems they've solved, at what scale, how they
think, and what measurable outcomes they delivered. Primary content: case studies, leadership
writing, resume, achievements.

Every decision should read as: executive presence, trust, technical sophistication, precision,
clarity, operational excellence. Closer to a premium annual report than a marketing site.

---

## 2. Design language

A blend of **Linear · Stripe · Vercel · premium annual reports · executive briefings**.

**Favor:** typography-first design, spacious layouts, strong hierarchy, restraint, content-first,
near-monochrome slate palette with a single blue accent, subtle shadows.

**Avoid:** excessive gradients, glassmorphism, neumorphism, heavy shadows, trendy effects,
decorative visuals, startup-marketing/blog/designer-portfolio aesthetics, buzzwords.

Hierarchy comes from **size and weight, never color**. Emphasis comes from size, weight, and
the structural accent textures (the eyebrow bar and section tick) — never from spreading color
around.

---

## 3. Architecture — the four layers

The governing principle, enforced by the system rather than merely documented:

```
Foundation → Theme Tokens → Components → Patterns → Pages
```

Each layer references only the one directly above it:

- **Foundation** — static primitives (raw hex, type scale, spacing, radius, motion). Never
  themed, never referenced directly by components.
- **Theme Tokens** — semantic aliases that point at Foundation primitives. This is the
  abstraction layer, themed on `Mode: Light | Dark`.
- **Components** — consume **tokens**, never raw hex.
- **Patterns** — compose **components**, never new primitives.
- **Pages** — assemble **patterns**, and **set the theme**. A `theme: {Mode:"Dark"}` on a frame
  re-resolves every token beneath it; nothing else changes.

The dual-theme (Light / Dark) carries through every layer with **zero component overrides**.

---

## 4. Token reference — the single source of truth

Semantic tokens are defined as **aliases** that point at Foundation primitives. **When building
anything new, consume the semantic tokens, not the raw Foundation primitives.**

### Foundation primitives (static, never themed — don't reference directly in components/patterns)

- **Brand:** `primary-900` `#0F172A`, `primary-700` `#334155`
- **Accent — Cobalt Deep (v3):** `accent-500` `#2150B8`, `accent-600` `#1A4097` (hover),
  `accent-700` `#16357A` (pressed), plus the dark-mode ramp `accent-400` `#3F6BD0`,
  `accent-300` `#6E96E2`, `accent-200` `#A6C1F0`. A deep, slightly desaturated cobalt in the
  blue family, integrated purely at these six primitives so every semantic accent token,
  component, and pattern re-resolves in both themes with zero structural change. (The prior
  accent was Tailwind blue-600 `#2563EB`; Cobalt Deep was chosen via the `accent-lab.pen`
  v1–v6 funnel.)
- **Neutrals:** `neutral-0` `#FFFFFF`, `-50` `#F8FAFC`, `-200` `#E2E8F0`, `-300` `#CBD5E1`,
  `-400` `#94A3B8`, `-500` `#64748B`, `-600` `#475569`, `-700` `#334155`, `-800` `#1E293B`,
  `-900` `#0F172A`
- **Dark surfaces:** `dark-900` `#0B1020` (bg), `dark-800` `#111827`, `dark-700` `#172033`
  (elevated)

### Semantic tokens (use these — all themed on `Mode: Light | Dark`; format: Light → / Dark →)

- **Text:** `text-primary` (900 / 50) · `text-secondary` (600 / 300) · `text-tertiary`
  (500 / 400) · `text-muted` (500 / 400) · `text-inverse` (0 / 900, only on `action-secondary`
  fills) · `text-accent` / `text-success` (accent-500 / accent-300)
- **Static text helpers:** `text-on-accent` (→0, labels on accent fills) · `text-on-dark`
  (→50) · `text-on-dark-muted` (→400) — for always-dark surfaces (covers, Highlight Card)
- **Surface:** `surface-primary` (0 / dark-900) · `surface-secondary` (50 / dark-800) ·
  `surface-tertiary` (200 / 800) · `surface-elevated` (0 / dark-700) · `surface-dark`
  (900 / dark-700) · `surface-inverse` (900 / 50) · `surface-accent` (static accent-500) ·
  `surface-dark-raised` (static 800)
- **Border:** `border-subtle` (200 / 800) · `border-default` (300 / 700) · `border-strong`
  (400 / 600) · `border-accent` (accent-500 / accent-400)
- **Action:** `action-primary` (static accent-500) · `action-primary-hover` (600 / 400 — hover
  *lightens* in dark) · `action-primary-pressed` (700 / 600) · `action-secondary` (900 / 50 —
  inverts) · `action-secondary-hover` (700 / 200) · `link` (500 / 300) · `link-hover`
  (600 / 200) · `focus-ring` (500 / 400) · `disabled` (300 / 700) · `disabled-text` (static 500)
- **Status (v2, muted/functional — never brand):** `status-success-fg`/`-surface`,
  `status-error-fg`/`-surface`, `status-warning-fg`/`-surface`, `status-info-fg`/`-surface`
- **Prose/reading (v2):** `text-prose` (neutral-800 / neutral-200), `measure-prose` (680),
  `leading-prose` (1.7), `leading-tight`/`-snug`/`-normal`
- **Elevation:** `shadow-1a` (`#0F172A14` / `#00000059`) · `shadow-1b` (`#0F172A0F` / `#00000040`)

> **Deprecated:** the `--`-prefixed shadcn set (`--primary`, `--background`, `--color-*`,
> `--sidebar-*`, …) was a stray second brand. It is now **neutralized** — each re-pointed to a
> canonical slate+blue alias. Do not reference any `--`-prefixed token in new work.

### Type, spacing, radius, motion, breakpoints, icons

- **Fonts:** `font-sans` = **Red Hat Display**, `font-mono` = **JetBrains Mono**.
- **Type scale — `font-size-*` (v3.1, 12 steps, non-themed):** `-display-xl` 72 · `-display-l`
  64 · `-display-m` 56 · `-h1` 48 · `-h2` 40 · `-h3` 32 · `-h4` 24 · `-h5` 20 · `-body-l` 18 ·
  `-body-m` 16 · `-body-s` 14 · `-caption` 12. Weights 400/500/600/700. (A handful of 15/13/11px
  literals remain raw — off the documented ramp.)
- **Spacing (8-pt):** `space-0..space-12` = 0, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 128.
- **Radius (v3, non-themed):** `radius-sm` 8 · `radius-md` 12 · `radius-lg` 16 · `radius-pill`
  999. Components consume these, never raw corner values.
- **Motion (v3, non-themed; defined for code, not animated on the boards):** `duration-fast`
  120 · `duration-base` 200 · `duration-slow` 320 (ms); `ease-standard`
  `cubic-bezier(0.2,0,0,1)` · `ease-emphasized` `cubic-bezier(0.3,0,0,1)`. v3.1 adds a
  **motion-choreography** spec (interaction→token table + button state ladder).
- **Breakpoints (v3.1, non-themed):** `breakpoint-sm` 640 · `-md` 768 · `-lg` 1024 · `-xl` 1280.
- **Icon sizes (v3.1, non-themed):** `icon-sm` 16 · `icon-md` 20 · `icon-lg` 24.
- **Elevation (subtle, executive):** 0 flat / 1 raised (`y1·3px·8%`) / 2 hover (`y4·12px·12%`) /
  3 overlay (`y12·24px·16%`). Layered outer `#0F172A` shadows with low alpha.

### The structural-accent rule (v3.1)

Reserve accent for **eyebrows, CTAs, links, and active nav**. Categories and tags are neutral
(ink); metrics and outcomes are ink. Emphasis comes from size, weight, and two textures — an
**eyebrow accent bar** and a **section tick** (both via the reusable Eyebrow master) — never
from spreading color.

### Theming

To render anything in dark mode, set `theme: {Mode: "Dark"}` on a wrapping frame — every token
beneath re-resolves; components and patterns need zero overrides. Light is the default
resolution. The full mode map, the WCAG AA accessibility audit, and contributor rules live on
the Themes board (documented in the v1.1 file).

---

## 5. Component & pattern conventions

### Component inventory

The system ships a set of reusable component **masters**: Button, Icon Button, Nav Item, Tag,
Metric, Metric Card, Highlight Card, Case Study Card, Article Card, Timeline Item, Pullquote,
Callout, ToC Rail, Breadcrumb, Credibility Strip, Footer, and (v3.1) the **Eyebrow** texture
master. Variants and states are produced by **overriding tokens** on an instance — e.g. a Text
button is the Button with a transparent fill and a `link`-colored label; Disabled is `disabled`
fill + `disabled-text` label. (The Pencil master IDs and override recipes live in
[`CLAUDE.md`](./CLAUDE.md) §Component masters.)

### Patterns

- **Section patterns** are rendered with real component instances and executive sample content.
- **Page Layout blueprints** (Homepage, Case Study, Writing, Resume) are labeled "browser"
  mocks: a chrome bar plus a vertical stack of **bands**, each a left label rail (section name +
  which pattern it uses) and a right schematic (placeholder bars suggesting the real layout).

### Content rule (followed everywhere)

**Headlines are outcome-oriented.** Lead with business / organizational / technical impact and
scale; the technology is the *how*, never the headline.

- Good: "Scaling Clinical Operations Through Optimization"
- Bad: "Using OR-Tools for Scheduling"

---

## 6. Version history

The system is versioned as separate `.pen` files so each release is frozen for comparison. See
[`README.md`](./README.md) for the file inventory.

- **v1.1** — the original system: Foundation, Tokens, Components, Patterns, Themes (dual Light /
  Dark with a full mode map and WCAG AA audit).
- **v2.0** — refined the system within its DNA: publication-grade type, a long-form reading
  layer, status + prose tokens, a rebuilt case-study card, impact-first resume, and the
  neutralization of the stray shadcn token set.
- **v3.0** — a token-level evolution of v2: the accent became **Cobalt Deep** `#2150B8`
  (replacing Tailwind blue, decided via the `accent-lab.pen` funnel), plus a **radius** scale
  and **motion** token set. v3.0 swapped the accent **color only**.
- **v3.1 (current)** — makes the accent **structural** and closes the remaining token gaps:
- **(a) Structural accent** — accent reserved for eyebrows, CTAs, links, and active nav;
categories/tags neutralized to ink; metrics/outcomes rendered as ink; plus two textures
(an eyebrow accent bar and a section tick) via a new reusable **Eyebrow** master.
- **(b) Closed token gaps** — `font-size-*` (the 12-step type scale, consumed by all 16
masters), `breakpoint-*` (responsive contract), `icon-*` (size tokens), and a
**motion-choreography** spec. Iconography was folded into the Foundation board as a new
section (plus an Icon token table). No type/spacing values, layouts, or component inventory
changed; the dual-theme carries through with zero overrides.

- **v3.2** — updated the sans-serif font from Inter to Red Hat Display for improved modern typography and enhanced readability while maintaining the executive aesthetic. The monospaced font (JetBrains Mono) remains unchanged. All components automatically adapt through the font-sans token with zero structural changes needed.

---

## 7. Status & what's next

**v3.1 is feature-complete at the design-tool layer.** No token gaps remain. The
highest-leverage next move is to **implement capanema.io in code** from the v3.1 blueprints.

Default stack assumption: Next.js (App Router) on Vercel, Inter + JetBrains Mono, Tailwind with
the tokens above mapped to CSS variables. Follow the page blueprints for section order and the
content rule for copy. Implement theming as CSS variables + `[data-theme]` with a
`prefers-color-scheme` fallback, the choice persisted in `localStorage` and applied by an inline
head script before first paint.

Remaining open risks to validate on real content: the reduced-frequency accent and the
illustration policy, plus motion timing once built. The live backlog lives on the v3.1
Documentation board's "Governance & Known Gaps" section.
