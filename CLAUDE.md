# CLAUDE.md — Capanema Design System

Guidance for working on the **capanema.io** design system. Read this before touching the
`.pen` file or generating site code.

---

## 1. What this project is

A complete, four-layer design system for **capanema.io** — the personal executive portfolio
of a senior technology leader (CTO / Engineering Executive / data-platform & AI practitioner).

The site is an **executive portfolio**, not a startup landing page. Its job: help a visitor
quickly understand who the person is, what problems they've solved, at what scale, how they
think, and what measurable outcomes they delivered. Primary content: case studies, leadership
writing, resume, achievements.

**The system is built in Pencil** (the `.pen` design tool, accessed via the `pencil` MCP
server), **not in code**. **v3.1 is the current version**, living in its own file
(`design-system-v3.1.pen`); v3.0, v2.0 and v1.1 are frozen in separate files for comparison
(see §3). v2.0 refined the system within its DNA (publication-grade type, long-form reading
layer, status + prose tokens, rebuilt case-study card, impact-first resume, shadcn set
neutralized). **v3.0 was a token-level evolution** of v2: the accent became **Cobalt Deep**
(`#2150B8`, decided via the `accent-lab.pen` funnel — replacing Tailwind blue `#2563EB`), plus
a **radius scale** and **motion** token set — but v3.0 swapped the accent **color only**.
**v3.1 makes the accent _structural_ and closes the remaining token gaps.** It (a) reserves
accent for **eyebrows, CTAs, links, and active nav** while neutralizing categories/tags (ink)
and rendering metrics/outcomes as ink — plus two textures (an **eyebrow accent bar** and a
**section tick**, via a new reusable **Eyebrow** master); and (b) adds **`font-size-*`** (12-step
type scale, consumed by all 16 masters), **`breakpoint-*`** (responsive contract), **`icon-*`**
(size tokens), and a **motion-choreography** spec (interaction→token table + button state
ladder). It also adds a new **`09 · Iconography`** board, re-versions boards 01–08 to v3.1,
rebuilds the Design Review, and extends the Changelog and Comparison. No type/spacing values,
layouts, or component inventory changed. The dual-theme (Light / Dark, `Mode` axis) carries
through with zero component overrides. Next work: **implement capanema.io in code** from the
v3.1 blueprints.

> **Persistence note:** Pencil MCP edits a **live in-editor document**; changes are not written
> to the `.pen` on disk until the file is **saved in Pencil**, and the MCP only ever sees the
> single open document (the `filePath` argument is ignored). Save before committing the binary
> to git, or git will not capture the work.

---

## 2. Design language

Blend of **Linear · Stripe · Vercel · premium annual reports · executive briefings**.

**Favor:** typography-first design, spacious layouts, strong hierarchy, restraint, content-first,
near-monochrome slate palette with a single blue accent, subtle shadows.

**Avoid:** excessive gradients, glassmorphism, neumorphism, heavy shadows, trendy effects,
decorative visuals, startup-marketing/blog/designer-portfolio aesthetics, buzzwords.

Every decision should read as: executive presence, trust, technical sophistication, precision,
clarity, operational excellence. Closer to a premium annual report than a marketing site.

---

## 3. The files & how to access them

- **Versioned files** (split for clean version separation):
  - **`design-system-v3.1.pen`** — the **v3.1** system and **current working file** (boards
    `01–08 · v3.1` + new `09 · Iconography · v3.1`). Structural Cobalt accent + texture,
    `font-size-*`/`breakpoint-*`/`icon-*` tokens, motion choreography. **Cloned from the v3
    file**, so boards 01–08 reuse v3's frame IDs (see §4); the Eyebrow master and board 09 have
    fresh IDs.
  - **`design-system-v3.pen`** — the **frozen v3.0** system (boards `01–06 · v3` + `07 ·
    Changelog · v3` + `08 · V2 ↔ V3 Comparison`). Cobalt color-only, radius + motion. Do not edit.
  - **`design-system-v2.pen`** — the **frozen v2.0** system (boards `01–04 · v2` + `05 · Design
    Review & Changelog` + `06 · Documentation · v2`), kept for reference/comparison. Do not edit.
  - **`design-system-v1.pen`** — the **frozen v1.1** system (boards `01–04` + `06 · Themes`),
    kept for reference/comparison. Do not edit.
  - **`accent-lab.pen`** — accent explorations (the v1–v6 funnel that chose Cobalt Deep) plus an
    older **color-only** `07 · V3 · Cobalt Deep` doc board (`uFQye`) that the full v3 file
    supersedes. Reference only.
  - The old combined `design-system.pen` was retired (still in git history at commit `a7a241a`).
- **Open the file you intend to work on in Pencil first.** Critical limitation learned: the
  Pencil MCP is bound to the **single active editor** — the `filePath` argument is **ignored**
  for both reads and writes, and changes only reach disk when **you Save in Pencil**. So you
  cannot target a non-open file by path, and you cannot create/split files from the MCP; those
  are GUI actions. Confirm which file is active with `get_editor_state` before editing.
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

## 4. Architecture — boards

Each board is a top-level frame, laid out left→right via `FindEmptySpace`. All share the same
anatomy: a dark **Cover**, numbered content sections (each with a section header `## NN Title`,
a description, showcase, and an engineering documentation panel), and a **Governance** section
showing the chain with the current layer marked `HERE`. **Frame IDs are unique per file**, but
each version was cloned from the previous one, so IDs carry forward: `design-system-v3.1.pen`
was cloned from the v3 file, which was cloned from v2 — so boards 01–08 reuse the **same IDs**
across v2/v3/v3.1 (below). v3.1's new `09 · Iconography` board (`C328oX`) and the Eyebrow master
(`T2HLO`) have their own IDs. v1.1 IDs live in `design-system-v1.pen`.

**v1.1 — frozen, in `design-system-v1.pen`** (do not edit):

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation` | `bsXHN` | Colors, Typography, Spacing, Elevation, Roadmap |
| `02 · Tokens` | `gzBR6` | Text / Surface / Border / Action tokens, Governance & Usage |
| `03 · Components` | `Dny8s` | Buttons, Navigation, Cards, Tags, Metrics, Timeline, Footer, Governance |
| `04 · Patterns` | `jvEua` | Hero, Case Study, Writing, Resume, Contact patterns; Page Layout blueprints; Content Guidelines; Governance |
| `06 · Themes` | `mO19J` | Theme Philosophy, Light Theme, Dark Theme, Theme Tokens (mode map), Components & Patterns in both modes, Accessibility audit, Theme Governance |

**v2.0 — frozen, in `design-system-v2.pen`** (boards 01–06; do not edit):

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation · v2` | `hmZsC` | Color, Typography (scale + line-heights + tracking + prose specimen), Spacing & Rhythm, Elevation, Governance |
| `02 · Tokens · v2` | `IF92z` | Text / Surface / Border / Action / Status / Prose token tables (Light+Dark swatches), Governance & Usage |
| `03 · Components · v2` | `fEH2K` | Controls, Data Display, Discovery Cards, Long-form, Wayfinding & Credibility, Governance |
| `04 · Patterns · v2` | `M7VtG` | Hero, Case Study Discovery, Case Study Detail, Writing, Resume, Contact, Page Layout Blueprints, Governance & Content |
| `05 · Design Review & Changelog` | `ch8VD` | Executive Summary, Major Findings, What/Why Changed, Before↔After, Principles, Future |
| `06 · Documentation · v2` | `NnWw8` | Principles, Accessibility, Naming Conventions, Theme Architecture, Component Standards, AI Contribution Rules, Governance & Known Gaps |

**v3.0 — frozen, in `design-system-v3.pen`** (cloned from v2 → boards 01–06 share v2's IDs):

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation · v3` | `hmZsC` | Color (Cobalt Deep), Typography, Spacing, Elevation, **+ Radius (`## 05`)**, **+ Motion (`## 06`)**, Governance |
| `02 · Tokens · v3` | `IF92z` | Text/Surface/Border/Action/Status/Prose tables (accent re-resolved), **+ Radius (`## 07`)** & **Motion (`## 08`)** token tables, Governance |
| `03 · Components · v3` | `fEH2K` | Re-versioned; visuals auto-resolve Cobalt; masters consume `radius-*` tokens |
| `04 · Patterns · v3` | `M7VtG` | Re-versioned; visuals auto-resolve Cobalt |
| `05 · Design Review · v3` | `ch8VD` | Rebuilt for v3: scored audit |
| `06 · Documentation · v3` | `NnWw8` | Re-versioned; radius + motion gaps marked RESOLVED |
| `07 · Changelog · v3` | `hNrUG` | Added / Changed / Improved / Deprecated / Removed |
| `08 · V2 ↔ V3 Comparison` | `HuQip` | Accent before↔after, layer-by-layer table, score deltas, changelog summary |

**v3.1 — current, in `design-system-v3.1.pen`** (cloned from v3 → boards 01–08 share v3's IDs):

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation · v3.1` | `hmZsC` | Color, Typography, Spacing, Elevation, Radius, Motion, **+ Responsive (`## 07`, breakpoint table + reflow schematic)**, **+ Motion choreography block** (in the Motion section: interaction→token table + Button state ladder), Governance |
| `02 · Tokens · v3.1` | `IF92z` | Text/Surface/Border/Action/Status/Prose, **+ Type Scale (`## 07`)** table; Radius→`## 08`, Motion→`## 09`, Governance→`## 10` |
| `03 · Components · v3.1` | `fEH2K` | Re-versioned; categories/metric-eyebrows neutralized to `text-tertiary`; masters re-pointed to `font-size-*`; **+ Accent Distribution rule card** in Governance |
| `04 · Patterns · v3.1` | `M7VtG` | Re-versioned; **accent texture applied** — Hero (`FY2K8`) & Case Study Detail (`b8yr0`) eyebrows swapped to Eyebrow instances + a section tick above each headline |
| `05 · Design Review · v3.1` | `ch8VD` | Refreshed: exec summary, **11-dim** scorecard (Scalability & Brand → 5.0, + Iconography), rewritten Open Risks, v3.1 Final Assessment |
| `06 · Documentation · v3.1` | `NnWw8` | Type-scale / breakpoint / icon gaps marked **RESOLVED**; primitives table extended with `font-size-*`, `breakpoint-*`, `icon-*` |
| `07 · Changelog · v3.1` | `hNrUG` | v3.1 entries prepended to Added / Changed / Improved (tagged `(v3.1)`) |
| `08 · V2 ↔ V3 ↔ V3.1 Comparison` | `HuQip` | Layer table gains a **V3.1 column** + 5 new-layer rows (Type scale, Breakpoints, Iconography, Motion choreography, Accent distribution) |
| `09 · Iconography · v3.1` | `C328oX` | System, Sizes (`icon-*` table w/ live previews), Inventory (3×3 lucide grid), Illustration policy, Governance |

Reusable component **masters**: v1.1 uses `⟐ Component Masters` (id `VX0oF`) — present in the
v1 file, and also kept in the **v2 file** solely because the Changelog's Before↔After instances
the old Case Study Card `SG5xZ`. **v2 masters** live in `⟐ Component Masters · v2` (id `J6aQ6v`,
v2 file only) with fresh IDs:
Button `WuNSb` (Label `oDdYq`, Icon `OngS2`) · Icon Button `GPwQ9` · Nav Item `M8kBKi` (Label `OF1pw`) ·
Tag `UsMEV` (Dot `Z6RdKt`, Label `v9dYgV`) · Metric `N323Wn` (Value `HF1zg`, Label `WHhXN`) ·
Metric Card `xAD0x` (Eyebrow `YGXVS`, Value `v4Q8dH`, Label `dTkA7`) · Highlight Card `LA0Vr` ·
Timeline Item `afs6z` (Date `hnFRC`, Title `EK5vT`, Desc `kGlXz`, Outcome `ewLYz`; last item set `strokeWidth:0`) ·
Case Study Card `BtcCZ` (Category `auz5R`, Title `In95P`, Summary `p3IzlV`) · Article Card `WGvhC`
(Category `U3qGMx`, Date `SMe7N`, Title `vdP6J`, Excerpt `UEIi6`, ReadTime `L5b01`) ·
Pullquote `oV5N6` (Quote `giEAq`) · Callout `WlpMe` (Icon `ZYRK5`, Title `VEi4E`, Text `srHuJ`; variants swap fill+icon to `status-*`) ·
ToC Rail `Br6Rv` · Breadcrumb `ahkiC` · Credibility Strip `xTEMM` · Footer `JkvM5`.

**In `design-system-v3.pen`** these same masters (cloned, identical IDs in `⟐ Component
Masters · v2`, id `J6aQ6v`) carry the Cobalt accent via tokens and now consume the new
`radius-*` scale for their corners: Button/Icon Button `radius-sm`; Metric/Highlight/Case
Study/Article cards `radius-md`; Tag, Tag Dot, and Timeline Dot `radius-pill`; Callout
`radius-sm` (snapped from raw 10).

**In `design-system-v3.1.pen`** (same `J6aQ6v` masters, same IDs) the structural-accent change
edits these masters: Metric Card eyebrow `YGXVS`, Case Study category `auz5R`, and Article
category `U3qGMx` → `text-tertiary` (no longer accent); Timeline outcome `ewLYz` → `text-primary`
+ weight 600 with icon `JFEbz` → `text-tertiary` (ink metric). **Reserved accent left intact:**
CS CTA `bedzm`/`Aw13Z` (`link`), Tag/Timeline dots, ToC active `c2XYSn` (`border-accent`). All
on-scale text re-pointed to `font-size-*` (15/13/11px literals left as-is — off the documented
ramp). New reusable **Eyebrow** master `T2HLO` (Bar `baWVB` = `surface-accent`/`radius-pill`,
Label `YwOGh` = `text-accent` mono) — instanced in the Hero/Case-Study eyebrows.

**The governing principle (enforced by the system, not just documented):**
`Foundation → Theme Tokens → Components → Patterns → Pages`. Each layer references only the one
directly above it. Components consume **tokens**, never raw hex. Patterns compose **components**,
never new primitives. Pages assemble **patterns** — and **pages set the theme** (a `theme:
{Mode:"Dark"}` on a frame re-resolves every token beneath it; nothing else changes).

---

## 5. Tokens — the single source of truth

Reference any variable from a property with a `$` prefix (e.g. `fill: "$text-primary"`,
`gap: "$space-5"`). Variable **names** must not start with `$`. Semantic tokens are defined as
**aliases** that point at Foundation primitives — this is the abstraction layer. **When building
anything new, consume the semantic tokens below, not the raw Foundation primitives.**

> **v2.0 token additions (themed Light/Dark unless noted):** **Status** — `status-success-fg`/
> `-surface`, `status-error-fg`/`-surface`, `status-warning-fg`/`-surface`, `status-info-fg`/
> `-surface` (muted, functional feedback only — never brand). **Prose/reading** — `text-prose`
> (softened body: neutral-800 / neutral-200), `measure-prose` (680), `leading-prose` (1.7),
> `leading-tight`/`-snug`/`-normal`. **Deprecated:** the `--`-prefixed shadcn set
> (`--primary`, `--background`, `--font-secondary`, `--color-*`, `--sidebar-*`, …) was a stray
> second brand; it is now **neutralized** — each re-pointed to a canonical slate+blue alias.
> Do not reference any `--`-prefixed token in new work. **Gotcha confirmed:** a `fill:"$token"`
> set *before* that token exists is silently stored as `#000000` — define tokens first, then
> reference. `letterSpacing` is in **px** (negative on display).

> **v3.1 token additions (non-themed numbers):** **`font-size-*`** — the 12-step type ramp
> tokenized: `-display-xl` 72 · `-display-l` 64 · `-display-m` 56 · `-h1` 48 · `-h2` 40 · `-h3`
> 32 · `-h4` 24 · `-h5` 20 · `-body-l` 18 · `-body-m` 16 · `-body-s` 14 · `-caption` 12. All 16
> masters consume them; **15/13/11px literals stay raw** (off the documented ramp — expanding it
> was out of scope). **`breakpoint-*`** — `-sm` 640 · `-md` 768 · `-lg` 1024 · `-xl` 1280
> (responsive contract; documented on the Foundation Responsive section). **`icon-*`** — `-sm`
> 16 · `-md` 20 · `-lg` 24 (icon sizing; **note:** icon `width`/`height` does **not** accept a
> variable binding — it coerces to 0 — so set literal px equal to the token). **Structural-accent
> rule:** reserve accent for **eyebrows, CTAs, links, active nav**; categories/tags are neutral,
> metrics/outcomes are ink; emphasis comes from size, weight, and the eyebrow bar / section tick
> — never from spreading color.

### Foundation primitives (don't reference directly in components/patterns; static, never themed)
- Brand: `primary-900` `#0F172A`, `primary-700` `#334155`
- Accent (**V3 · Cobalt Deep** — replaced the prior Tailwind-blue ramp): `accent-500` `#2150B8`,
  `accent-600` `#1A4097` (hover), `accent-700` `#16357A` (pressed), plus the dark-mode ramp
  `accent-400` `#3F6BD0`, `accent-300` `#6E96E2`, `accent-200` `#A6C1F0`. A deeper, slightly
  desaturated cobalt in the same blue family; integrated purely at these six primitives, so every
  semantic accent token + component + pattern re-resolved in both themes with zero structural change.
  (Prior accent was `#2563EB` = Tailwind blue-600.) Documented on `01 · Foundation · v3` /
  `02 · Tokens · v3`; chosen via the `accent-lab.pen` v1–v6 funnel.
- Neutrals: `neutral-0` `#FFFFFF`, `-50` `#F8FAFC`, `-200` `#E2E8F0`, `-300` `#CBD5E1`,
  `-400` `#94A3B8`, `-500` `#64748B`, `-600` `#475569`, `-700` `#334155`, `-800` `#1E293B`,
  `-900` `#0F172A`
- Dark surfaces: `dark-900` `#0B1020` (bg), `dark-800` `#111827`, `dark-700` `#172033` (elevated)

### Semantic tokens (use these — all themed on `Mode: Light | Dark`; format: Light → / Dark →)
- **Text:** `text-primary`(900 / 50) · `text-secondary`(600 / 300) · `text-tertiary`(500 / 400) ·
  `text-muted`(500 / 400 — raised from 400 in light for AA) · `text-inverse`(0 / 900, only on
  `action-secondary` fills) · `text-accent`/`text-success`(accent-500 / accent-300)
- **Static text helpers:** `text-on-accent`(→0, labels on accent fills) · `text-on-dark`(→50) ·
  `text-on-dark-muted`(→400) — for always-dark surfaces (covers, Highlight Card)
- **Surface:** `surface-primary`(0 / dark-900) · `surface-secondary`(50 / dark-800) ·
  `surface-tertiary`(200 / 800) · `surface-elevated`(0 / dark-700) · `surface-dark`(900 / dark-700) ·
  `surface-inverse`(900 / 50) · `surface-accent`(static accent-500) · `surface-dark-raised`(static 800)
- **Border:** `border-subtle`(200 / 800) · `border-default`(300 / 700) · `border-strong`(400 / 600) ·
  `border-accent`(accent-500 / accent-400)
- **Action:** `action-primary`(static accent-500) · `action-primary-hover`(600 / 400 — hover
  *lightens* in dark) · `action-primary-pressed`(700 / 600) · `action-secondary`(900 / 50 —
  inverts) · `action-secondary-hover`(700 / 200) · `link`(500 / 300) · `link-hover`(600 / 200) ·
  `focus-ring`(500 / 400) · `disabled`(300 / 700) · `disabled-text`(static 500)
- **Elevation:** `shadow-1a`(`#0F172A14` / `#00000059`) · `shadow-1b`(`#0F172A0F` / `#00000040`)

**To render anything in dark mode:** set `theme: {Mode: "Dark"}` on a wrapping frame — every
token beneath re-resolves; components and patterns need zero overrides. Light is the default
resolution. Full mode map, accessibility audit (WCAG AA verified) and contributor rules live on
board `06 · Themes`.

### Type, spacing, radius & motion
- Fonts: `font-sans` = **Inter**, `font-mono` = **JetBrains Mono**.
- Type scale: now tokenized as **`font-size-*`** (v3.1) — Display XL 72 / L 64 / M 56 · H1 48 ·
  H2 40 · H3 32 · H4 24 · H5 20 · Body L 18 / M 16 / S 14 · Caption 12. Weights 400/500/600/700.
  Hierarchy comes from size & weight, **never color**.
- Spacing (8-pt): `space-0..space-12` = 0, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 128.
- **Breakpoints** (v3.1, non-themed): `breakpoint-sm` 640 · `-md` 768 · `-lg` 1024 · `-xl` 1280.
- **Icon sizes** (v3.1, non-themed): `icon-sm` 16 · `icon-md` 20 · `icon-lg` 24 (set literal px;
  icon size can't bind a variable).
- **Radius** (v3, non-themed): `radius-sm` 8 · `radius-md` 12 · `radius-lg` 16 · `radius-pill`
  999. Components consume these, never raw corner values.
- **Motion** (v3, non-themed; defined for code, not animated on the boards): `duration-fast` 120 ·
  `duration-base` 200 · `duration-slow` 320 (ms); `ease-standard` `cubic-bezier(0.2,0,0,1)` ·
  `ease-emphasized` `cubic-bezier(0.3,0,0,1)`.
- Elevation (subtle, executive): Elevation 0 flat / 1 raised (`y1·3px·8%`) / 2 hover
  (`y4·12px·12%`) / 3 overlay (`y12·24px·16%`). Shadows are layered outer `#0F172A` with low alpha.

---

## 6. Component masters (IDs for instancing in Patterns/Pages)

Insert an instance with `{type:"ref", ref:<masterId>, descendants:{<childId>:{...overrides}}}`.
Override `fill` on the instance root for variants/states.

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

**Variants are made by overriding tokens**, e.g. a Text button = Button with
`fill:"#00000000"` + Label `fill:"$link"`; Secondary = `fill:"$action-secondary"` **+ Label
`fill:"$text-inverse"`** (required — the master's label defaults to `$text-on-accent`, which is
static white for accent fills); Disabled = `fill:"$disabled"` + Label `fill:"$disabled-text"`.
Theme-safety rules baked into the masters (v1.1): Button label/icon use `$text-on-accent`;
Highlight Card uses `$text-on-dark` / `$text-on-dark-muted` / `$surface-dark-raised` (it is an
always-dark surface in both modes); Case Study Card shadows use `$shadow-1a`/`$shadow-1b`. Timeline Item draws its connector as a
left border (`strokeWidth:{left:2}`) with an absolutely-positioned dot — the **last** item in a
timeline must override `strokeWidth:0` so the line doesn't dangle.

---

## 7. Pattern & page conventions (board 04)

- **Section patterns** are rendered with real component instances + executive sample content.
- **Page Layout blueprints** (Homepage, Case Study, Writing, Resume) are labeled "browser"
  mocks: a chrome bar + a vertical stack of **bands**, each = a left label rail (section name +
  which pattern it uses) and a right schematic (placeholder bars suggesting the real layout).
- Content rule, documented and to be followed everywhere: **headlines are outcome-oriented**.
  Lead with business / organizational / technical impact and scale; the technology is the *how*,
  never the headline. Good: "Scaling Clinical Operations Through Optimization". Bad: "Using
  OR-Tools for Scheduling".

---

## 8. House style for building in Pencil

- **One reusable doc pattern per section:** a `## NN` header (mono accent number + 40px title +
  17px description, 760–820px wide), a showcase frame (`surface-secondary`, padding 40–48,
  radius 12, `border-subtle`), and an 8-field **documentation panel** (2-col grid:
  Purpose · Usage · Anatomy · Variants · States · Token Usage · Spacing · Accessibility).
- Covers are `surface-dark`, padding `[96,96]`, with a mono brand line, version badge, big
  96px title, subtitle, and a meta row.
- Sections are full-width frames, padding `[88,96]`, `surface-primary`, with a bottom
  `border-subtle` to separate them (last section omits it).
- Each `batch_design` runs in its own scope — helper functions (`header`, `subLabel`,
  `pHead`, `docPanel`, etc.) must be **redefined inline in every batch**. Persist a created
  node across batches by assigning without `const`/`let` (`board = "..."`).
- Always set a human-readable `name` on every node. Build/verify **one section at a time**.

---

## 9. Pencil gotchas (learned the hard way)

- `alignItems` accepts only `start` / `center` / `end` — **no `baseline`, no `stretch`**.
- **Circular sizing:** a `fit_content` parent with a `fill_container` child on the same axis
  collapses to zero and errors. Fixes: give the parent/row a fixed size, make the parent
  `fill_container`, or make the child not fill. (Came up on nav underlines, timeline rails, and
  blueprint bands — solved by giving band rows a fixed height per type so label + schematic can
  both `fill_container` the cross axis.)
- Disabling a `fill_container` descendant inside an instance (e.g. Compact card hiding its
  Summary) emits a harmless **"not inside flexbox"** warning — ignore if visuals are correct.
- **Render lag:** `get_screenshot`/`export_nodes` can return a **blank white** image for a
  freshly-created section low in a tall board. It's a render-pipeline lag, **not** a real
  problem. Verify with `snapshot_layout(problemsOnly:true)` (should say "No layout problems")
  and `batch_get(resolveVariables:true)` to confirm real fills; re-screenshot after later edits
  and it renders.
- No `image` node type — images are **fills** applied via the `Generate` function.
- Set variable values as objects: `{type:"color", value:"#.." | "$alias"}`. Reference with `$`.
- **`set_variables` theming (critical):** merging a themed value array onto an existing plain
  value **drops the unthemed base entry** — the token then resolves to `#000000` in the default
  context and every light surface goes black. Always write themed variables with BOTH explicit
  entries: `[{theme:{Mode:"Light"},value:..},{theme:{Mode:"Dark"},value:..}]`, then verify with
  `batch_get(resolveVariables:true)` on a light-context node.
- `batch_design` globals (assignment without `const`/`let`) may NOT persist across calls —
  capture the returned IDs and hardcode them in the next call.
- **Icon size won't bind a variable:** setting `width`/`height` to `"$icon-md"` on an `icon`
  node silently coerces to **0** (unlike `fill`, which binds fine). Set literal px equal to the
  token value instead.
- **Fresh top-level board snapshot lag:** a brand-new top-level board can report its first child
  at `y:50` and its `fit_content` height ~50px short, so `snapshot_layout(problemsOnly)` flags
  the last section "partially clipped" even though the node tree is correct (no stray `x`/`y`).
  Toggling placeholder/clip/Move doesn't clear it in-session; it recomputes correctly on
  Save/reopen. Confirm the node tree via `batch_get` and compare against a settled sibling board.

---

## 10. Verifying work

1. After each section: `snapshot_layout(parentId, problemsOnly:true)` → expect
   "No layout problems".
2. `get_screenshot` the section frame (smallest meaningful node) to check color/type/alignment.
   If blank, see the render-lag note above.
3. Fix any `batch_design` warnings in the next call; never delete-and-recreate to fix — update
   the existing nodes.

---

## 11. Where to go next

**v3.1 is done** — `design-system-v3.1.pen` holds the full system: the accent is now
**structural** (reserved + textured via the Eyebrow master), and the type-scale (`font-size-*`),
breakpoint (`breakpoint-*`), icon (`icon-*`), and motion-choreography gaps are closed. Boards
01–08 are re-versioned to v3.1, the Design Review is refreshed, the Changelog and Comparison are
extended, and a new `09 · Iconography` board is added. v3.0, v2.0, v1.1 are frozen for
comparison; dual-theme carries through. **Backlog lives on board `06 · Documentation · v3.1`'s
"Governance & Known Gaps" section** (type-scale + breakpoint + icon now RESOLVED). Likely next steps:
- **Implement capanema.io in code** from the **v3.1** blueprints (board `04 · Patterns · v3.1`,
  `M7VtG`) — this is now the highest-leverage move (no token gaps remain). Default stack
  assumption: Next.js (App Router) on Vercel, Inter + JetBrains Mono, Tailwind with the tokens
  above mapped to CSS variables. Use `get_variables` to export the exact token values (both Mode
  resolutions), follow the page blueprints in board 04 for section order and content guidelines
  for copy, and implement theming per the v1 file's `06 · Themes` board §08 (mO19J): CSS
  variables + `[data-theme]` + `prefers-color-scheme` fallback, choice persisted in
  `localStorage`, applied by an inline head script before first paint.
- **Validate the remaining Open Risks** (board `05 · Design Review · v3.1`): the reduced-frequency
  accent and illustration policy on real content, and motion timing once built.

Persistent project notes also live in the Claude memory file `design-system-v3.1-file.md`
(v3.1 file, boards, tokens, gotchas) — keep CLAUDE.md and memory in sync.
