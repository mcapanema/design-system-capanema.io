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
server), **not in code**. v1.1 is complete — all four layers exist **plus a dual-theme
architecture (Light / Dark) on the `Mode` theme axis, documented on board `06 · Themes`**.
Any further work is refinement, a new board, or implementing the site in code from these
blueprints.

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

## 3. The file & how to access it

- **File:** `design-system.pen` (Pencil MCP `filePath` argument — actual filename on disk).
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

## 4. Architecture — four boards

Each board is a top-level frame in the document, laid out left→right via `FindEmptySpace`.
All share the same anatomy: a dark **Cover**, numbered content sections (each with a section
header `## NN Title`, a description, showcase, and an engineering documentation panel), and a
**Governance** section showing the chain with the current layer marked `HERE`.

| Board | Frame ID | Contents |
|-------|----------|----------|
| `01 · Foundation` | `bsXHN` | Colors, Typography, Spacing, Elevation, Roadmap |
| `02 · Tokens` | `gzBR6` | Text / Surface / Border / Action tokens, Governance & Usage |
| `03 · Components` | `Dny8s` | Buttons, Navigation, Cards, Tags, Metrics, Timeline, Footer, Governance |
| `04 · Patterns` | `jvEua` | Hero, Case Study, Writing, Resume, Contact patterns; Page Layout blueprints; Content Guidelines; Governance |
| `06 · Themes` | `mO19J` | Theme Philosophy, Light Theme, Dark Theme, Theme Tokens (mode map), Components & Patterns in both modes, Accessibility audit, Theme Governance |

Reusable component **masters** live in the `⟐ Component Masters` library frame (id `VX0oF`,
far right of the Components board).

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

### Foundation primitives (don't reference directly in components/patterns; static, never themed)
- Brand: `primary-900` `#0F172A`, `primary-700` `#334155`
- Accent: `accent-500` `#2563EB`, `accent-600` `#1D4ED8` (hover), `accent-700` `#1E40AF` (pressed),
  plus the dark-mode ramp `accent-400` `#3B82F6`, `accent-300` `#60A5FA`, `accent-200` `#93C5FD`
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

### Type & spacing
- Fonts: `font-sans` = **Inter**, `font-mono` = **JetBrains Mono**.
- Type scale (px): Display XL 72 / L 64 / M 56 · H1 48 · H2 40 · H3 32 · H4 24 · H5 20 ·
  Body L 18 / M 16 / S 14 · Caption 12. Weights 400/500/600/700. Hierarchy comes from size &
  weight, **never color**.
- Spacing (8-pt): `space-0..space-12` = 0, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 128.
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

The four design layers plus the dual-theme layer (board `06 · Themes`) are done. Likely next steps:
- A `05` board (e.g. responsive/mobile specs, motion, or iconography), **or**
- **Implement capanema.io in code** from these blueprints. Default stack assumption:
  Next.js (App Router) on Vercel, Inter + JetBrains Mono, Tailwind with the tokens above mapped
  to CSS variables. Use `get_variables` to export the exact token values (both Mode resolutions),
  follow the page blueprints in board 04 for section order and content guidelines for copy, and
  implement theming per board 06 §08: CSS variables + `[data-theme]` + `prefers-color-scheme`
  fallback, choice persisted in `localStorage`, applied by an inline head script before first paint.

Persistent project notes also live in the Claude memory file
`pencil-ds-foundation.md` (board IDs, master IDs, gotchas) — keep both in sync.
