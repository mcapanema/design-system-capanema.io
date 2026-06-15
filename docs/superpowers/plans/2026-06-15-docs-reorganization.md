# Documentation Reorganization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Split the Capanema Design System documentation into three clearly-scoped files — a public `README.md` (repo front door), a new human-readable `DESIGN.md` (the design system as a design artifact), and a trimmed `CLAUDE.md` (agent-operational instructions for working in Pencil) — with no duplicated content and correct cross-references.

**Architecture:** All design *content* (design language, token reference, component inventory, patterns, version evolution) moves to `DESIGN.md`. All Pencil *operational* content (MCP access rules, board/frame IDs, master IDs, house style, gotchas, verification, next steps) stays in `CLAUDE.md`. `README.md` becomes a concise navigation hub with a file inventory. Each file links to the others rather than repeating them.

**Tech Stack:** Markdown only. No code, no build. Verification = visual read-back + grep for broken cross-references.

---

## Content division reference

This is the single source of truth for "what goes where." Apply it exactly.

| Current CLAUDE.md section | Destination | Notes |
|---|---|---|
| §1 What this project is — *the brief* (executive portfolio purpose) | DESIGN.md §1 + README intro | Design rationale → DESIGN; one-paragraph summary → README |
| §1 — *version evolution narrative* (v2→v3→v3.1) | DESIGN.md §6 (Version history) | Full narrative |
| §1 — *Persistence note* (save-before-commit) | CLAUDE.md | Operational |
| §2 Design language | DESIGN.md §2 | Verbatim move |
| §3 The files & how to access them — *file inventory* | README.md (table) + DESIGN.md §6 | README = inventory; DESIGN = version meaning |
| §3 — *Pencil MCP access rules* (encrypted, single-editor binding, schema-first) | CLAUDE.md | Operational |
| §4 Architecture — *four-layer governing principle* | DESIGN.md §3 | Conceptual |
| §4 — *board→frame-ID tables, master IDs note* | CLAUDE.md | Operational navigation map |
| §5 Tokens — *token reference* (primitives, semantic, type/spacing/radius/motion/breakpoint/icon) | DESIGN.md §4 | Core design documentation |
| §5 — *Pencil token syntax* ($ prefix, set_variables themed-array gotcha) | CLAUDE.md | Operational |
| §6 Component masters (IDs for instancing) | CLAUDE.md | Operational; DESIGN gets conceptual inventory only |
| §7 Pattern & page conventions | DESIGN.md §5 | Design content |
| §8 House style for building in Pencil | CLAUDE.md | Operational |
| §9 Pencil gotchas | CLAUDE.md | Operational |
| §10 Verifying work | CLAUDE.md | Operational |
| §11 Where to go next | CLAUDE.md (operational steps) + DESIGN.md §7 (design status) | Split |

---

### Task 1: Create DESIGN.md

**Files:**
- Create: `/Users/murilo/Workspace/design-system-capanema.io/DESIGN.md`

- [ ] **Step 1: Write the file** with the exact content below.

````markdown
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

- **Fonts:** `font-sans` = **Inter**, `font-mono` = **JetBrains Mono**.
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
````

- [ ] **Step 2: Verify the file reads correctly.**

Run: `head -40 /Users/murilo/Workspace/design-system-capanema.io/DESIGN.md`
Expected: title "# Capanema Design System" and the §1 brief render with no stray ```` ``` ```` fences leaking (the outer code fence in this plan must NOT be copied into the file — only its inner content).

- [ ] **Step 3: Confirm cross-reference targets exist.**

Run: `ls /Users/murilo/Workspace/design-system-capanema.io/{README.md,CLAUDE.md}`
Expected: both paths listed (DESIGN.md links to both).

---

### Task 2: Rewrite README.md

**Files:**
- Modify (full replace): `/Users/murilo/Workspace/design-system-capanema.io/README.md`

- [ ] **Step 1: Replace the file** with the exact content below.

````markdown
# Capanema Design System

Source files, version history, and documentation for the **Capanema Design System** — the
design system behind [capanema.io](https://capanema.io), a personal executive portfolio and
publishing platform for an engineering leader (case studies, leadership writing, resume,
achievements).

The design language is inspired by the clarity of **Linear**, the craftsmanship of **Stripe**,
and the restraint of **Vercel**: typography-first, near-monochrome slate with a single cobalt
accent, spacious, content-first, and built to read like a premium annual report rather than a
marketing site.

## Documentation

| File | Audience | Contents |
|------|----------|----------|
| [`DESIGN.md`](./DESIGN.md) | Anyone | The design system itself — design language, four-layer architecture, full token reference, component inventory, pattern conventions, version history |
| [`CLAUDE.md`](./CLAUDE.md) | Agents / contributors editing the `.pen` source | How to work in Pencil — MCP access rules, board & component IDs, house style, gotchas, verification |

## Files

The system is authored in **Pencil** (the `.pen` design tool). Each version is frozen in its
own file for clean comparison. **`.pen` files are encrypted** and can only be opened with Pencil.

| File | Status | Contents |
|------|--------|----------|
| `design-system-v3.1.pen` | **Current** | v3.1 — structural Cobalt accent + texture; `font-size-*` / `breakpoint-*` / `icon-*` tokens; motion choreography; iconography |
| `design-system-v3.pen` | Frozen | v3.0 — Cobalt Deep accent (color only) + radius & motion tokens |
| `design-system-v2.pen` | Frozen | v2.0 — publication-grade type, long-form reading layer, status + prose tokens |
| `design-system-v1.pen` | Frozen | v1.1 — original Foundation / Tokens / Components / Patterns / Themes |
| `accent-lab.pen` | Reference | The v1–v6 accent exploration funnel that chose Cobalt Deep |

The retired combined `design-system.pen` remains in git history at commit `a7a241a`.

## Repository layout

```
design-system-*.pen          # versioned design source (open in Pencil)
accent-lab.pen               # accent exploration
DESIGN.md                    # the design system documentation
CLAUDE.md                    # agent / contributor guide for editing in Pencil
docs/superpowers/
  specs/                     # design specs per version
  plans/                     # implementation plans
  audits/                    # audit findings
```

## Status

**v3.1 is feature-complete at the design-tool layer.** The next step is to implement
capanema.io in code from the v3.1 blueprints (Next.js + Tailwind assumed). See
[`DESIGN.md` §7](./DESIGN.md) for the roadmap.
````

- [ ] **Step 2: Verify.**

Run: `head -20 /Users/murilo/Workspace/design-system-capanema.io/README.md`
Expected: the new title + intro + Documentation table; the old single-paragraph README is gone.

---

### Task 3: Trim CLAUDE.md to agent-operational content

**Files:**
- Modify (full replace): `/Users/murilo/Workspace/design-system-capanema.io/CLAUDE.md`

The goal: remove the design-content sections that now live in `DESIGN.md` (design language,
full token *reference* prose, version evolution narrative, pattern philosophy) and keep
everything an agent needs to *operate* in Pencil. Add a short pointer to `DESIGN.md` at the top
of the relevant sections so the operational instructions still make sense.

- [ ] **Step 1: Replace the file** with the exact content below.

````markdown
# CLAUDE.md — Capanema Design System

Operational guidance for working on the **capanema.io** design system **in Pencil**. Read this
before touching a `.pen` file or generating site code.

> **For the design system itself** — design language, the four-layer architecture, the full
> token reference, the component inventory, pattern conventions, and version history — see
> [`DESIGN.md`](./DESIGN.md). For the repository layout and file inventory see
> [`README.md`](./README.md). This file is the **agent/contributor operations manual**: how to
> access the `.pen` source, the board & component IDs to edit it, the house style, the gotchas,
> and how to verify work.

**Current version: v3.1** (`design-system-v3.1.pen`). v3.0, v2.0, and v1.1 are frozen in
separate files for comparison (see §1). **Next work: implement capanema.io in code from the
v3.1 blueprints** (board `04 · Patterns · v3.1`, `M7VtG`).

> **Persistence note:** Pencil MCP edits a **live in-editor document**; changes are not written
> to the `.pen` on disk until the file is **saved in Pencil**, and the MCP only ever sees the
> single open document (the `filePath` argument is ignored). Save before committing the binary
> to git, or git will not capture the work.

---

## 1. The files & how to access them

The versioned `.pen` files (and `accent-lab.pen`) are inventoried in
[`README.md`](./README.md), and what each version *means* is in [`DESIGN.md` §6](./DESIGN.md).
The operational rules for touching them:

- **Open the file you intend to work on in Pencil first.** Critical limitation: the Pencil MCP
  is bound to the **single active editor** — the `filePath` argument is **ignored** for both
  reads and writes, and changes only reach disk when **you Save in Pencil**. You cannot target a
  non-open file by path, and you cannot create/split files from the MCP; those are GUI actions.
  Confirm which file is active with `get_editor_state` before editing.
- `.pen` files are **encrypted** — only ever access them through `pencil` MCP tools. Never
  `Read`/`Grep`/`Edit` a `.pen` file directly.
- **Always** call `get_editor_state(include_schema: true)` at the start of a session if the
  `.pen` schema isn't already in context — it's required before any other Pencil tool works.
- Load `get_guidelines(category:"guide", name:"Design System")` for composition patterns when
  building screens.

### Pencil tools you'll use
`get_editor_state` · `get_guidelines` · `batch_get` · `batch_design` · `snapshot_layout` ·
`get_screenshot` · `get_variables` · `set_variables` · `export_nodes`

---

## 2. Board navigation — frame IDs

Each board is a top-level frame, laid out left→right via `FindEmptySpace`. All share the same
anatomy: a dark **Cover**, numbered content sections (each with a `## NN Title` header, a
description, a showcase, and an engineering documentation panel), and a **Governance** section
showing the chain with the current layer marked `HERE`. **Frame IDs are unique per file**, but
each version was cloned from the previous one, so IDs carry forward: `design-system-v3.1.pen`
was cloned from the v3 file, which was cloned from v2 — so boards 01–08 reuse the **same IDs**
across v2/v3/v3.1. v1.1 IDs live in `design-system-v1.pen`.

**v3.1 — current, in `design-system-v3.1.pen`** (cloned from v3 → boards 01–08 share v3's IDs):

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation · v3.1` | `hmZsC` | Color, Typography, Spacing, Elevation, Radius, Motion, **+ Responsive (`## 07`)**, **+ Motion choreography block**, **+ Iconography (`## 08`)**, Governance |
| `02 · Tokens · v3.1` | `IF92z` | Text/Surface/Border/Action/Status/Prose, **+ Type Scale (`## 07`)**; Radius→`## 08`, Motion→`## 09`, **+ Icon (`## 10`)** table, Governance→`## 11` |
| `03 · Components · v3.1` | `fEH2K` | Re-versioned; categories/metric-eyebrows neutralized to `text-tertiary`; masters re-pointed to `font-size-*`; **+ Accent Distribution rule card** in Governance |
| `04 · Patterns · v3.1` | `M7VtG` | Re-versioned; **accent texture applied** — Hero (`FY2K8`) & Case Study Detail (`b8yr0`) eyebrows swapped to Eyebrow instances + a section tick above each headline |
| `05 · Design Review · v3.1` | `ch8VD` | Refreshed: exec summary, **11-dim** scorecard (Scalability & Brand → 5.0, + Iconography), rewritten Open Risks, v3.1 Final Assessment |
| `06 · Documentation · v3.1` | `NnWw8` | Type-scale / breakpoint / icon gaps marked **RESOLVED**; primitives table extended with `font-size-*`, `breakpoint-*`, `icon-*` |
| `07 · Changelog · v3.1` | `hNrUG` | v3.1 entries prepended to Added / Changed / Improved (tagged `(v3.1)`) |
| `08 · V3 → V3.1 Comparison` | `HuQip` | Rebuilt as a focused v3→v3.1 diff: Headline (accent-distribution before/after chips), Layer-by-layer (`V3 \| V3.1`), Design Review delta, New-in-v3.1 chip grid |

(No standalone Iconography board — it lives as Foundation `## 08` + the Tokens `## 10` Icon table.)

**Frozen files** (do not edit). Boards 01–06 in `design-system-v2.pen` (IDs `hmZsC`, `IF92z`,
`fEH2K`, `M7VtG`, `ch8VD`, `NnWw8`) and `design-system-v3.pen` (same IDs + Changelog `hNrUG`,
Comparison `HuQip`) match the v3.1 IDs above because each was cloned forward. v1.1
(`design-system-v1.pen`): Foundation `bsXHN`, Tokens `gzBR6`, Components `Dny8s`, Patterns
`jvEua`, Themes `mO19J`. `accent-lab.pen` holds the v1–v6 accent funnel + an older color-only
`07 · V3 · Cobalt Deep` doc board (`uFQye`) superseded by the full v3 file.

---

## 3. Component masters (IDs for instancing)

Insert an instance with `{type:"ref", ref:<masterId>, descendants:{<childId>:{...overrides}}}`.
Override `fill` on the instance root for variants/states.

**v1.1 masters** live in `⟐ Component Masters` (id `VX0oF`); they are also kept in the v2 file
because the Changelog's Before↔After instances the old Case Study Card `SG5xZ`.

| Component | Master | Key child IDs |
|-----------|--------|---------------|
| Button | `I7lVO` | Label `zSLyh`, Icon `Eoj0x` (trailing, off by default) |
| Icon Button | `vrUW5` | Icon `I0q3fP` |
| Nav Item | `WotMW` | Label `CBLj9` — active = `strokeWidth:{bottom:2}` + Label `text-primary` |
| Tag | `wKzU6` | Dot `goYJl` (off by default), Label `I8IvH` |
| Metric | `IlUJh` | Value `Q4ur5`, Label `rjhHM` |
| Case Study Card | `SG5xZ` | Category `dz0xY`, Title `R6da8`, Summary `SEFD1`, Outcome `vvVjL`, Outcome Value `RjJHz`, Outcome Label `pC6tx`, CTA `UDEWz`, CTA Label `cCQ8o` |
| Article Card | `q0cIY` | Category `f8awlg`, Date `WViCH`, Title `qedlf`, Excerpt `J2tPF`, Read Time `L9VA1` |
| Highlight Card | `iPsTc` | Icon `BTPcJ`, Title `ckzpv`, Description `xFhyU` |
| Metric Card | `g0x0UR` | Eyebrow `Q0Fa7B`, Value `dax28`, Label `SyMer` |
| Timeline Item | `y8sbCU` | Dot `FiK5i`, Date `C8EEMa`, Title `D9Dxj`, Description `TDaWh`, Outcome `YyWsR`, Outcome Text `z9LUb` |

**v2 masters** live in `⟐ Component Masters · v2` (id `J6aQ6v`, present in the v2/v3/v3.1 files)
with fresh IDs: Button `WuNSb` (Label `oDdYq`, Icon `OngS2`) · Icon Button `GPwQ9` · Nav Item
`M8kBKi` (Label `OF1pw`) · Tag `UsMEV` (Dot `Z6RdKt`, Label `v9dYgV`) · Metric `N323Wn` (Value
`HF1zg`, Label `WHhXN`) · Metric Card `xAD0x` (Eyebrow `YGXVS`, Value `v4Q8dH`, Label `dTkA7`) ·
Highlight Card `LA0Vr` · Timeline Item `afs6z` (Date `hnFRC`, Title `EK5vT`, Desc `kGlXz`,
Outcome `ewLYz`; last item set `strokeWidth:0`) · Case Study Card `BtcCZ` (Category `auz5R`,
Title `In95P`, Summary `p3IzlV`) · Article Card `WGvhC` (Category `U3qGMx`, Date `SMe7N`, Title
`vdP6J`, Excerpt `UEIi6`, ReadTime `L5b01`) · Pullquote `oV5N6` (Quote `giEAq`) · Callout
`WlpMe` (Icon `ZYRK5`, Title `VEi4E`, Text `srHuJ`; variants swap fill+icon to `status-*`) ·
ToC Rail `Br6Rv` · Breadcrumb `ahkiC` · Credibility Strip `xTEMM` · Footer `JkvM5`.

**v3 corner tokens:** in `design-system-v3.pen` these masters consume the `radius-*` scale —
Button/Icon Button `radius-sm`; Metric/Highlight/Case Study/Article cards `radius-md`; Tag, Tag
Dot, and Timeline Dot `radius-pill`; Callout `radius-sm`.

**v3.1 structural-accent edits** (same `J6aQ6v` masters): Metric Card eyebrow `YGXVS`, Case
Study category `auz5R`, Article category `U3qGMx` → `text-tertiary` (no longer accent); Timeline
outcome `ewLYz` → `text-primary` + weight 600 with icon `JFEbz` → `text-tertiary` (ink metric).
**Reserved accent left intact:** CS CTA `bedzm`/`Aw13Z` (`link`), Tag/Timeline dots, ToC active
`c2XYSn` (`border-accent`). All on-scale text re-pointed to `font-size-*` (15/13/11px literals
left raw). New reusable **Eyebrow** master `T2HLO` (Bar `baWVB` = `surface-accent`/`radius-pill`,
Label `YwOGh` = `text-accent` mono) lives **inside the Component Masters board** (`J6aQ6v`) and
is instanced in the Hero/Case-Study eyebrows.

**Variants are made by overriding tokens:** a Text button = Button with `fill:"#00000000"` +
Label `fill:"$link"`; Secondary = `fill:"$action-secondary"` **+ Label `fill:"$text-inverse"`**
(required — the master's label defaults to `$text-on-accent`, static white for accent fills);
Disabled = `fill:"$disabled"` + Label `fill:"$disabled-text"`. Theme-safety baked into the
masters: Button label/icon use `$text-on-accent`; Highlight Card uses `$text-on-dark` /
`$text-on-dark-muted` / `$surface-dark-raised` (always-dark in both modes); Case Study Card
shadows use `$shadow-1a`/`$shadow-1b`. Timeline Item draws its connector as a left border
(`strokeWidth:{left:2}`) with an absolutely-positioned dot — the **last** item must override
`strokeWidth:0` so the line doesn't dangle.

---

## 4. Tokens in Pencil — syntax & theming

The token *reference* (what every token resolves to) is in [`DESIGN.md` §4](./DESIGN.md). The
Pencil mechanics:

- Reference any variable from a property with a `$` prefix (e.g. `fill: "$text-primary"`,
  `gap: "$space-5"`). Variable **names** must not start with `$`.
- Semantic tokens are **aliases** that point at Foundation primitives. **Consume the semantic
  tokens when building anything new, not the raw Foundation primitives.**
- Set variable values as objects: `{type:"color", value:"#.." | "$alias"}`. Reference with `$`.
- `letterSpacing` is in **px** (negative on display). `font-size-*`, `breakpoint-*`, `icon-*`,
  `radius-*`, and `motion` tokens are **non-themed numbers**; `text-*`/`surface-*`/`border-*`/
  `action-*`/`status-*` are **themed** on `Mode: Light | Dark`.
- **To render dark mode:** set `theme: {Mode: "Dark"}` on a wrapping frame — every token beneath
  re-resolves; components/patterns need zero overrides. Light is the default resolution.

**Theming gotchas:**

- **`set_variables` (critical):** merging a themed value array onto an existing plain value
  **drops the unthemed base entry** — the token then resolves to `#000000` in the default
  context and every light surface goes black. Always write themed variables with BOTH explicit
  entries: `[{theme:{Mode:"Light"},value:..},{theme:{Mode:"Dark"},value:..}]`, then verify with
  `batch_get(resolveVariables:true)` on a light-context node.
- A `fill:"$token"` set *before* that token exists is silently stored as `#000000` — define
  tokens first, then reference.
- **Icon size won't bind a variable:** setting `width`/`height` to `"$icon-md"` on an `icon`
  node silently coerces to **0** (unlike `fill`, which binds fine). Set literal px equal to the
  token value instead.

---

## 5. House style for building in Pencil

- **One reusable doc pattern per section:** a `## NN` header (mono accent number + 40px title +
  17px description, 760–820px wide), a showcase frame (`surface-secondary`, padding 40–48,
  radius 12, `border-subtle`), and an 8-field **documentation panel** (2-col grid:
  Purpose · Usage · Anatomy · Variants · States · Token Usage · Spacing · Accessibility).
- Covers are `surface-dark`, padding `[96,96]`, with a mono brand line, version badge, big
  96px title, subtitle, and a meta row.
- Sections are full-width frames, padding `[88,96]`, `surface-primary`, with a bottom
  `border-subtle` to separate them (last section omits it).
- Each `batch_design` runs in its own scope — helper functions (`header`, `subLabel`, `pHead`,
  `docPanel`, etc.) must be **redefined inline in every batch**. Persist a created node across
  batches by assigning without `const`/`let` (`board = "..."`).
- Always set a human-readable `name` on every node. Build/verify **one section at a time**.

### Pattern & page conventions (when building board 04)

- **Section patterns** use real component instances + executive sample content.
- **Page Layout blueprints** are labeled "browser" mocks: a chrome bar + a vertical stack of
  **bands**, each a left label rail (section name + which pattern it uses) and a right schematic
  (placeholder bars).
- **Headlines are outcome-oriented** — lead with impact and scale; the technology is the *how*,
  never the headline. (See [`DESIGN.md` §5](./DESIGN.md).)

---

## 6. Pencil gotchas (learned the hard way)

- `alignItems` accepts only `start` / `center` / `end` — **no `baseline`, no `stretch`**.
- **Circular sizing:** a `fit_content` parent with a `fill_container` child on the same axis
  collapses to zero and errors. Fixes: give the parent/row a fixed size, make the parent
  `fill_container`, or make the child not fill. (Came up on nav underlines, timeline rails, and
  blueprint bands — solved by giving band rows a fixed height per type.)
- Disabling a `fill_container` descendant inside an instance (e.g. Compact card hiding its
  Summary) emits a harmless **"not inside flexbox"** warning — ignore if visuals are correct.
- **Render lag:** `get_screenshot`/`export_nodes` can return a **blank white** image for a
  freshly-created section low in a tall board. It's render-pipeline lag, **not** a real problem.
  Verify with `snapshot_layout(problemsOnly:true)` (should say "No layout problems") and
  `batch_get(resolveVariables:true)` to confirm real fills; re-screenshot after later edits.
- No `image` node type — images are **fills** applied via the `Generate` function.
- `batch_design` globals (assignment without `const`/`let`) may NOT persist across calls —
  capture the returned IDs and hardcode them in the next call.
- **Fresh top-level board snapshot lag:** a brand-new top-level board can report its first child
  at `y:50` and its `fit_content` height ~50px short, so `snapshot_layout(problemsOnly)` flags
  the last section "partially clipped" even though the node tree is correct. Toggling
  placeholder/clip/Move doesn't clear it in-session; it recomputes on Save/reopen. Confirm the
  node tree via `batch_get` and compare against a settled sibling board.

(The `set_variables` themed-array trap, the pre-definition `#000000` trap, and the icon-size
binding trap are in §4.)

---

## 7. Verifying work

1. After each section: `snapshot_layout(parentId, problemsOnly:true)` → expect
   "No layout problems".
2. `get_screenshot` the section frame (smallest meaningful node) to check color/type/alignment.
   If blank, see the render-lag note in §6.
3. Fix any `batch_design` warnings in the next call; never delete-and-recreate to fix — update
   the existing nodes.

---

## 8. Where to go next

**v3.1 is done** — `design-system-v3.1.pen` holds the full system (see [`DESIGN.md`](./DESIGN.md)
for what shipped). Boards 01–08 are re-versioned to v3.1; v3.0, v2.0, v1.1 are frozen for
comparison. **Backlog lives on board `06 · Documentation · v3.1`'s "Governance & Known Gaps"
section** (type-scale + breakpoint + icon now RESOLVED). Likely next steps:

- **Implement capanema.io in code** from the **v3.1** blueprints (board `04 · Patterns · v3.1`,
  `M7VtG`) — the highest-leverage move (no token gaps remain). Default stack: Next.js (App
  Router) on Vercel, Inter + JetBrains Mono, Tailwind with the tokens mapped to CSS variables.
  Use `get_variables` to export the exact token values (both Mode resolutions), follow the page
  blueprints in board 04 for section order and the content rule for copy, and implement theming
  per the v1 file's `06 · Themes` board §08 (`mO19J`): CSS variables + `[data-theme]` +
  `prefers-color-scheme` fallback, choice persisted in `localStorage`, applied by an inline head
  script before first paint.
- **Validate the remaining Open Risks** (board `05 · Design Review · v3.1`): the
  reduced-frequency accent and illustration policy on real content, and motion timing once built.

Persistent project notes also live in the Claude memory file `design-system-v3.1-file.md` — keep
CLAUDE.md and memory in sync.
````

- [ ] **Step 2: Verify the trim.**

Run: `wc -l /Users/murilo/Workspace/design-system-capanema.io/CLAUDE.md` and
`grep -n "DESIGN.md" /Users/murilo/Workspace/design-system-capanema.io/CLAUDE.md`
Expected: file is meaningfully shorter than the original (~29KB), and at least 4 `DESIGN.md`
cross-references are present (top pointer + §1 + §4 + §5).

- [ ] **Step 3: Confirm no design-reference duplication leaked.**

Run: `grep -n "neutral-0\|#FFFFFF\|Linear · Stripe" /Users/murilo/Workspace/design-system-capanema.io/CLAUDE.md`
Expected: **no matches** — the full hex token reference and the design-language blurb now live
only in `DESIGN.md`.

---

### Task 4: Cross-file consistency & commit

**Files:**
- All three: `README.md`, `DESIGN.md`, `CLAUDE.md`

- [ ] **Step 1: Verify all cross-references resolve.**

Run:
```bash
cd /Users/murilo/Workspace/design-system-capanema.io
grep -rno "\(README\|DESIGN\|CLAUDE\)\.md" README.md DESIGN.md CLAUDE.md
ls README.md DESIGN.md CLAUDE.md
```
Expected: every referenced file (`README.md`, `DESIGN.md`, `CLAUDE.md`) exists; no link points
at a missing file.

- [ ] **Step 2: Confirm CLAUDE.md still parses as the agent manual.**

Read `CLAUDE.md` top to bottom. Confirm: it opens with the DESIGN.md/README.md pointer, retains
all operational content (access rules, board IDs, master IDs, Pencil token syntax, house style,
gotchas, verification, next steps), and contains **no** standalone design-language section or
full hex primitive table.

- [ ] **Step 3: Commit.**

```bash
cd /Users/murilo/Workspace/design-system-capanema.io
git add README.md DESIGN.md CLAUDE.md docs/superpowers/plans/2026-06-15-docs-reorganization.md
git commit -m "docs: split design docs into README / DESIGN.md / CLAUDE.md

- README.md: repo front door — intro, file inventory, navigation
- DESIGN.md (new): design system as a design artifact — language, layers,
  token reference, component inventory, patterns, version history
- CLAUDE.md: trimmed to agent-operational Pencil guidance with pointers to DESIGN.md

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

## Self-Review

**Spec coverage:**
- "Update README.md" → Task 2 (full rewrite with file inventory + doc map). ✓
- "Create a DESIGN.md" → Task 1 (full new file). ✓
- "Review what should be in DESIGN.md or CLAUDE.md to clean up CLAUDE.md" → Content division
  table (top of plan) + Task 3 (trimmed CLAUDE.md, design content removed, pointers added). ✓

**Placeholder scan:** No TBD/TODO/"similar to" — every file's complete content is inline. ✓

**Consistency:** Token values, board IDs, and master IDs in DESIGN.md/CLAUDE.md are copied
verbatim from the current CLAUDE.md so nothing drifts. Cross-reference filenames
(`README.md` / `DESIGN.md` / `CLAUDE.md`) are spelled identically across all three. ✓

**Note on the fenced content:** Tasks 1–3 wrap each file's content in an outer ```` ```` ````
fence for readability. When executing, write only the **inner** content to disk — do not include
the outer fence.
