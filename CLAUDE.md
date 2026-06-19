# CLAUDE.md — Capanema Design System

Operational guidance for working on the **capanema.io** design system **in Pencil**. Read this
before touching a `.pen` file or generating site code.

> **For the design system itself** — design language, the four-layer architecture, the full
> token reference, the component inventory, pattern conventions, and version history — see
> [`DESIGN.md`](./DESIGN.md). For the repository layout and file inventory see
> [`README.md`](./README.md). This file is the **agent/contributor operations manual**: how to
> access the `.pen` source, the board & component IDs to edit it, the house style, the gotchas,
> and how to verify work.

**Current version: v3.2** (`design-system-v3.2.pen`). v3.1, v3.0, v2.0, and v1.1 are frozen in
separate files for comparison (see §1). **Next work: implement capanema.io in code from the
v3.2 blueprints** (board `04 · Patterns · v3.2`, `M7VtG`).

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

- **Implement capanema.io in code** from the **v3.2** blueprints (board `04 · Patterns · v3.2`,
`M7VtG`) — the highest-leverage move (no token gaps remain). Default stack: Next.js (App
Router) on Vercel, Red Hat Display + JetBrains Mono, Tailwind with the tokens mapped to CSS variables.
Use `get_variables` to export the exact token values (both Mode resolutions), follow the page
blueprints in board 04 for section order and the content rule for copy, and implement theming
per the v1 file's `06 · Themes` board §08 (`mO19J`): CSS variables + `[data-theme]` +
`prefers-color-scheme` fallback, choice persisted in `localStorage`, applied by an inline head
script before first paint.
- **Validate the remaining Open Risks** (board `05 · Design Review · v3.1`): the
  reduced-frequency accent and illustration policy on real content, and motion timing once built.

Persistent project notes also live in the Claude memory file `design-system-v3.1-file.md` — keep
CLAUDE.md and memory in sync.
