# Design System v3.1 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Evolve Design System v3 → v3.1 by making the accent *structural* (reduced-frequency distribution + texture), and closing the remaining token gaps — type-scale, breakpoints, motion choreography, and iconography — all inside a new `design-system-v3.1.pen` file.

**Architecture:** v3.1 is a clone of `design-system-v3.pen`, so every frame/master/token ID is preserved and can be addressed directly. Work is done through the `pencil` MCP against the single active editor. Changes propagate through the existing token→master→pattern chain: edit masters and they re-resolve in patterns automatically. New tokens are added with `set_variables`; visual sections are authored with `batch_design` following the house style in `CLAUDE.md` §8.

**Tech Stack:** Pencil (`.pen`) via `pencil` MCP — `get_editor_state`, `get_variables`, `set_variables`, `batch_get`, `batch_design`, `snapshot_layout`, `get_screenshot`. No code/runtime; this is a design artifact.

---

## Verification model (read first — replaces TDD)

This is a design file, not code, so there are no unit tests. Every task's "test" is a **design verification triple**, run after the edit:

1. **Layout:** `snapshot_layout(parentId, problemsOnly:true)` → expect exactly `"No layout problems."`
2. **Tokens (when a token/fill changed):** `batch_get(nodeIds, resolveVariables:true)` → confirm the named field resolves to the expected hex/number.
3. **Visual:** `get_screenshot(smallest meaningful node)` → confirm the stated visual expectation. If blank, it is render-lag (CLAUDE.md §9) — re-shoot after the next edit; trust the layout+token checks.

"Commit" steps assume the user has **Saved in Pencil first** (MCP edits the live doc; nothing reaches disk until Save). Each task ends by noting what to commit; batch the actual `git commit` at the end of each phase after a Save.

---

## Scope

**In scope** — v3 Design Review "Future Opportunities" items **01–05**:
1. **Accent structural** — reduced-frequency distribution (neutral categories/tags, ink metrics; accent reserved for eyebrows, CTA, links, active nav) + texture (eyebrow accent bar, section accent tick). Source: `accent-lab.pen` v5 (B2 "Balanced") + v6 (texture catalog).
2. **Type-scale tokens** — `font-size-*` ramp; re-point the 16 masters.
3. **Breakpoint tokens** — `breakpoint-sm/-md/-lg/-xl` + a responsive spec section.
4. **Motion choreography** — interaction→token mapping + state ladders (the in-file deliverable for the "motion in code" item).
5. **Iconography & illustration** — `icon-*` size tokens + a new Iconography board (inventory, sizing, stroke, usage, illustration policy).

**Out of scope:** item 06 (implement capanema.io in code); changing typography/spacing values, layout, or the component inventory; introducing any new color/visual direction. v3.1 deepens v3's existing accent and tokens — it is not a redesign.

**Each phase is independently committable and verifiable.** Phase 6 (re-version + governance + review/changelog/comparison) runs last because it documents the others.

---

## Prerequisites (Phase 0)

### Task 0.1: Confirm the working file and clone state

**Files:** `design-system-v3.1.pen` (live editor)

- [ ] **Step 1:** Confirm the user has duplicated `design-system-v3.pen` → `design-system-v3.1.pen` in Pencil and made it the **active editor**.

Run: `get_editor_state(include_schema:true)`
Expected: active editor path ends `design-system-v3.1.pen`; 9 top-level nodes (`hmZsC`, `IF92z`, `J6aQ6v`, `fEH2K`, `M7VtG`, `ch8VD`, `NnWw8`, `hNrUG`, `HuQip`); 16 reusable components (`WuNSb`…`JkvM5`). These IDs match v3 — the rest of the plan relies on it.

- [ ] **Step 2:** Snapshot the current accent so changes are measurable.

Run: `get_variables()`
Expected: `accent-500` = `#2150B8`; `radius-*` and `duration-*`/`ease-*` present; **no** `font-size-*`, `breakpoint-*`, or `icon-*` yet.

---

## Phase 1 — Accent becomes structural

Goal: reserve accent for **eyebrows, CTAs, links, active nav**; neutralize **categories, metric eyebrows, and outcome metrics** (ink, not accent); add two **texture** treatments (eyebrow bar, section tick). Editing masters propagates to all pattern instances.

### Task 1.1: Neutralize category & metric-eyebrow labels on masters

**Files:** masters in `design-system-v3.1.pen` — `xAD0x` (Metric Card), `BtcCZ` (Case Study Card), `WGvhC` (Article Card)

Current accent leaks: `YGXVS` (MC eyebrow "SCALE"), `auz5R` (CS category), `U3qGMx` (Article category) all use `$text-accent`. Reduced-frequency makes categories/eyebrows-on-cards **neutral**; emphasis stays via mono + tracking.

- [ ] **Step 1: Apply**

```
batch_design:
Update("YGXVS",{fill:"$text-tertiary"})
Update("auz5R",{fill:"$text-tertiary"})
Update("U3qGMx",{fill:"$text-tertiary"})
```

- [ ] **Step 2: Verify tokens** — `batch_get(["YGXVS","auz5R","U3qGMx"], resolveVariables:true)` → each `fill` resolves to `#64748B` (light `text-tertiary`).
- [ ] **Step 3: Verify visual** — `get_screenshot("J6aQ6v")` → the Metric Card eyebrow, Case Study category, and Article category now read slate-grey, not blue. CTA link "Read case study" stays blue.
- [ ] **Step 4: Commit (after Save):** `design-system-v3.1.pen` — "v3.1: neutralize card categories & metric eyebrows".

### Task 1.2: Make timeline outcomes read as ink metrics

**Files:** master `afs6z` (Timeline Item) — children `ewLYz` (outcome text), `JFEbz` (outcome icon)

Outcome "−38% infrastructure cost" is a metric; reduced-frequency = ink + weight, not accent.

- [ ] **Step 1: Apply**

```
batch_design:
Update("ewLYz",{fill:"$text-primary",fontWeight:"600"})
Update("JFEbz",{fill:"$text-tertiary"})
```

- [ ] **Step 2: Verify tokens** — `batch_get(["ewLYz","JFEbz"], resolveVariables:true)` → `ewLYz.fill` = `#0F172A`; `JFEbz.fill` = `#64748B`.
- [ ] **Step 3: Verify visual** — `get_screenshot("afs6z")` → outcome is bold ink with a neutral trend icon; the left connector + dot are unchanged.
- [ ] **Step 4: Commit (after Save):** "v3.1: timeline outcomes render as ink metrics".

### Task 1.3: Confirm reserved-accent surfaces are unchanged

No edit — a guard check that accent is still present exactly where reduced-frequency *keeps* it.

- [ ] **Step 1: Verify** — `batch_get(["bedzm","Aw13Z","Z6RdKt","cD9Ae","c2XYSn"], resolveVariables:true)`
Expected: CS CTA label `bedzm` + icon `Aw13Z` = `$link` (`#2150B8`); Tag dot `Z6RdKt`/Timeline dot `cD9Ae` = accent (still off-by-default where applicable); ToC active item `c2XYSn` keeps `border-accent`. These are the "links / active" reserved uses — leave them.
- [ ] **Step 2:** No commit (verification only).

### Task 1.4: Build the "Accent texture" treatments as a reusable pattern

**Files:** new master in `design-system-v3.1.pen` (top-level, `reusable:true`)

Two treatments from `accent-lab.pen` v6: an **eyebrow with an accent bar prefix**, and a **section header with an accent tick**. Build one reusable "Eyebrow" master so patterns share it.

- [ ] **Step 1: Create the Eyebrow master**

```
batch_design:
pos=FindEmptySpace({width:320,height:40,direction:"bottom",padding:120,nodeId:"J6aQ6v"})
eb=Insert(document,{type:"frame",name:"Eyebrow",x:pos.x,y:pos.y,reusable:true,layout:"horizontal",gap:"$space-3",alignItems:"center",placeholder:true})
Insert(eb,{type:"frame",name:"Bar",width:3,height:14,fill:"$surface-accent",cornerRadius:"$radius-pill"})
Insert(eb,{type:"text",name:"Label",content:"CASE STUDY",fill:"$text-accent",fontFamily:"JetBrains Mono",fontSize:12,fontWeight:"500",letterSpacing:1})
Update(eb,{placeholder:false})
```

- [ ] **Step 2: Verify** — capture the returned IDs (`Eyebrow`, `Bar`, `Label`). `snapshot_layout(<Eyebrow id>, problemsOnly:true)` → "No layout problems"; `get_screenshot(<Eyebrow id>)` → a short accent bar followed by an accent mono label.
- [ ] **Step 3:** Record the new IDs in this plan's margin for Task 1.5. Commit (after Save): "v3.1: add Eyebrow master with accent bar".

### Task 1.5: Apply texture in the Hero and Case Study Detail patterns

**Files:** board `M7VtG` (Patterns) — Hero showcase `FY2K8`, Case Study Detail showcase `b8yr0`

The patterns currently use plain text eyebrows. Replace the eyebrow text with the new Eyebrow master instance, and add an accent tick to each section header.

- [ ] **Step 1: Locate the eyebrow text nodes** — `batch_get(["FY2K8","b8yr0"], readDepth:4)`; find the mono eyebrow/category text node in each (e.g. the hero kicker, the case-study category).
- [ ] **Step 2: Replace each with an Eyebrow instance** (use the master id from Task 1.4 as `EB`):

```
batch_design:
// for each located eyebrow text node <id> inside parent <p>, with content <C>:
Replace("<id>",{type:"ref",ref:"<EB>",descendants:{"Label":{content:"<C>"}}})
```

- [ ] **Step 3: Add a section accent tick** to the hero/section header — a 24×3 accent rule above the headline:

```
batch_design:
// inside the header stack <H>, as the first child:
tick=Insert("<H>",{type:"frame",name:"Tick",width:24,height:3,fill:"$surface-accent",cornerRadius:"$radius-pill"})
Move(tick,"<H>",0)
```

- [ ] **Step 4: Verify** — `snapshot_layout("M7VtG", problemsOnly:true)` → "No layout problems"; `get_screenshot("FY2K8")` and `get_screenshot("b8yr0")` → eyebrow shows the accent bar + label; an accent tick sits above each headline.
- [ ] **Step 5: Commit (after Save):** "v3.1: apply accent texture (eyebrow bar + section tick) in patterns".

### Task 1.6: Document the distribution rule on the Components board

**Files:** board `fEH2K` (Components) — add a short "Accent distribution" callout in the Governance section `I70R3u`

- [ ] **Step 1: Add the rule card**

```
batch_design:
card=Insert("I70R3u",{type:"frame",name:"Accent Distribution",layout:"vertical",gap:"$space-2",padding:[24,28],width:"fill_container",fill:"$surface-primary",cornerRadius:"$radius-sm",stroke:"$border-accent",strokeAlignment:"inner",strokeWidth:{left:3}})
Insert(card,{type:"text",name:"T",content:"Accent is structural, not decorative",fill:"$text-primary",fontFamily:"Inter",fontSize:17,fontWeight:"600",lineHeight:1.3,textGrowth:"fixed-width",width:"fill_container"})
Insert(card,{type:"text",name:"B",content:"Reserve accent for eyebrows, primary CTAs, links, and active nav. Categories and tags are neutral; metrics and outcomes are ink. Emphasis comes from size, weight, and the eyebrow bar / section tick — never from spreading color.",fill:"$text-secondary",fontFamily:"Inter",fontSize:15,lineHeight:1.55,textGrowth:"fixed-width",width:"fill_container"})
```

- [ ] **Step 2: Verify** — `snapshot_layout("fEH2K", problemsOnly:true)` → "No layout problems"; `get_screenshot(card id)` → the rule reads correctly.
- [ ] **Step 3: Commit (after Save):** "v3.1: document accent distribution rule".

---

## Phase 2 — Type-scale tokens

Goal: tokenize the documented type ramp and re-point the masters so size is governed like color and space.

### Task 2.1: Define the `font-size-*` tokens

**Files:** variables in `design-system-v3.1.pen`

- [ ] **Step 1: Apply**

```
set_variables:
{
 "font-size-display-xl":{"type":"number","value":72},
 "font-size-display-l":{"type":"number","value":64},
 "font-size-display-m":{"type":"number","value":56},
 "font-size-h1":{"type":"number","value":48},
 "font-size-h2":{"type":"number","value":40},
 "font-size-h3":{"type":"number","value":32},
 "font-size-h4":{"type":"number","value":24},
 "font-size-h5":{"type":"number","value":20},
 "font-size-body-l":{"type":"number","value":18},
 "font-size-body-m":{"type":"number","value":16},
 "font-size-body-s":{"type":"number","value":14},
 "font-size-caption":{"type":"number","value":12}
}
```

- [ ] **Step 2: Verify** — `get_variables()` → all twelve `font-size-*` present with the values above.
- [ ] **Step 3: Commit (after Save):** "v3.1: add font-size token scale".

### Task 2.2: Re-point the 16 masters' text to font-size tokens

**Files:** masters `WuNSb,GPwQ9,M8kBKi,UsMEV,N323Wn,xAD0x,LA0Vr,afs6z,BtcCZ,WGvhC,oV5N6,WlpMe,Br6Rv,ahkiC,xTEMM,JkvM5`

Map each text node's literal `fontSize` to the matching token (12→caption, 14→body-s, 15→body-s¹, 16→body-m, 18→body-l, 20→h5, 24→h4, 32→h3, 40→h2). ¹15px has no token; **leave 15px literals as-is** (summary/excerpt body) and note it — adding a `body-s-plus` would expand the ramp beyond the documented scale (out of scope).

- [ ] **Step 1: Read current sizes** — `batch_get([... all 16 ...], readDepth:3)` and list every `fontSize`. (Most were captured in CLAUDE.md §4/§6 and the v3 build; confirm before editing.)
- [ ] **Step 2: Apply** the per-node updates. Example for the well-known nodes (extend to all):

```
batch_design:
Update("oDdYq",{fontSize:"$font-size-body-s"})    // Button label 14
Update("HF1zg",{fontSize:"$font-size-h2"})         // Metric value 40
Update("WHhXN",{fontSize:"$font-size-body-s"})     // Metric label 14
Update("v4Q8dH",{fontSize:"$font-size-h2"})        // Metric Card value 40
Update("YGXVS",{fontSize:"$font-size-caption"})    // MC eyebrow 12
Update("dTkA7",{fontSize:"$font-size-body-s"})     // MC label 14
Update("In95P",{fontSize:"$font-size-h4"})         // Case Study title 24
Update("vdP6J",{fontSize:"$font-size-h5"})         // Article title 20
Update("giEAq",{fontSize:"$font-size-h4"})         // Pullquote 24
// …repeat for every text node per the Step 1 inventory…
```

- [ ] **Step 3: Verify tokens** — `batch_get(["HF1zg","In95P","giEAq"], resolveVariables:true)` → `fontSize` resolves to 40 / 24 / 24 respectively (unchanged values, now token-bound).
- [ ] **Step 4: Verify visual** — `get_screenshot("J6aQ6v")` → masters look **identical** to before (token re-point must not change rendering).
- [ ] **Step 5: Commit (after Save):** "v3.1: re-point master typography to font-size tokens".

### Task 2.3: Add the Type Scale token table to the Tokens board

**Files:** board `IF92z` — insert a "Type Scale" section before `Radius` (currently `## 07`); renumber Radius→08, Motion→09, Governance→10.

- [ ] **Step 1: Renumber existing headers** — `batch_get` to find the Radius/Motion/Governance Num text nodes (`## 07`/`## 08`/`## 09`), then:

```
batch_design:
Update("<radius Num>",{content:"## 08"})
Update("<motion Num>",{content:"## 09"})
Update("C1NpGN",{content:"## 10"})   // Governance (was ## 09 in v3)
```

- [ ] **Step 2: Build the Type Scale table** (TOKEN | VALUE | ROLE), following the radius-table pattern (house style, full-width section, bottom border, `## 07`). Rows = the twelve tokens with px values and roles (Display XL "Hero numerals/headlines" … Caption "Eyebrows, metadata"). Insert into `IF92z`, then `Move` to index 7 (after Prose, before Radius).
- [ ] **Step 3: Verify** — `snapshot_layout("IF92z", problemsOnly:true)` → "No layout problems"; `get_screenshot(section id)` → 12-row table renders.
- [ ] **Step 4: Commit (after Save):** "v3.1: add type-scale token table".

---

## Phase 3 — Breakpoint tokens

Goal: define responsive breakpoints as a contract and document reflow behavior.

### Task 3.1: Define `breakpoint-*` tokens

- [ ] **Step 1: Apply**

```
set_variables:
{
 "breakpoint-sm":{"type":"number","value":640},
 "breakpoint-md":{"type":"number","value":768},
 "breakpoint-lg":{"type":"number","value":1024},
 "breakpoint-xl":{"type":"number","value":1280}
}
```

- [ ] **Step 2: Verify** — `get_variables()` → four `breakpoint-*` present.
- [ ] **Step 3: Commit (after Save):** "v3.1: add breakpoint tokens".

### Task 3.2: Add a Responsive section to the Foundation board

**Files:** board `hmZsC` — append a "Responsive" section after Motion (`## 06`), numbered `## 07`, before Governance.

- [ ] **Step 1: Build the section** — header `## 07 Responsive`; a token table (TOKEN | VALUE | RANGE | NOTES) for the four breakpoints; plus a 4-step schematic showing a key pattern (e.g. Case Study Discovery grid) reflowing: 1-col (sm) → 2-col (md) → 3-col (lg) → 3-col + rail (xl). Use plain placeholder bars (no live instances) per blueprint convention (CLAUDE.md §7). Insert into `hmZsC`, `Move` before Governance `DD1pM`. Give the prior last content section (Motion `u1R6I`) a bottom border; new Responsive section also bottom border.

Copy for the four rows:
- `breakpoint-sm` · 640 · ≥640px · large phones; single column, stacked nav
- `breakpoint-md` · 768 · ≥768px · tablets; 2-col cards, inline nav
- `breakpoint-lg` · 1024 · ≥1024px · laptops; 3-col discovery, ToC rail appears
- `breakpoint-xl` · 1280 · ≥1280px · desktop; max content measure, wide margins

- [ ] **Step 2: Verify** — `snapshot_layout("hmZsC", problemsOnly:true)` → "No layout problems"; `get_screenshot(section id)` → table + reflow schematic render.
- [ ] **Step 3: Commit (after Save):** "v3.1: add responsive/breakpoint foundation section".

---

## Phase 4 — Motion choreography (the in-file motion deliverable)

Goal: specify *how* the existing motion tokens apply — interactions, properties, state ladders. (Animation itself is the code track, out of scope.)

### Task 4.1: Add a "Motion choreography" section to the Foundation Motion area

**Files:** board `hmZsC` — extend the Motion section `u1R6I` with a choreography block (or add an adjacent block under it).

- [ ] **Step 1: Build the interaction table** (INTERACTION | DURATION | EASING | PROPERTY), full-width, house style. Rows:
- Button hover · `duration-fast` · `ease-standard` · background + shadow
- Link hover · `duration-fast` · `ease-standard` · color + underline
- Nav active change · `duration-base` · `ease-standard` · underline offset
- Card hover lift · `duration-base` · `ease-standard` · translateY + shadow
- Theme toggle · `duration-base` · `ease-emphasized` · color cross-fade
- Overlay / dialog · `duration-slow` · `ease-emphasized` · opacity + translateY

- [ ] **Step 2: Build a state ladder** for the Button — four chips (Rest / Hover / Active / Focus) showing the fills (`action-primary` → `action-primary-hover` → `action-primary-pressed` → `action-primary` + `focus-ring` outline), with the transition token labeled between them. Use real `WuNSb` instances with `fill` overrides + a `focus-ring` stroke on the focus chip.

- [ ] **Step 3: Verify** — `snapshot_layout("hmZsC", problemsOnly:true)` → "No layout problems"; `get_screenshot(choreography block)` → table + 4-state ladder render, ladder fills resolve to the cobalt action ramp.
- [ ] **Step 4: Commit (after Save):** "v3.1: add motion choreography & button state ladder".

---

## Phase 5 — Iconography & illustration

Goal: define the icon system (library, sizes, stroke, usage) and an illustration policy as a new board.

### Task 5.1: Define `icon-*` size tokens

- [ ] **Step 1: Apply**

```
set_variables:
{
 "icon-sm":{"type":"number","value":16},
 "icon-md":{"type":"number","value":20},
 "icon-lg":{"type":"number","value":24}
}
```

- [ ] **Step 2: Verify** — `get_variables()` → three `icon-*` present.
- [ ] **Step 3: Commit (after Save):** "v3.1: add icon-size tokens".

### Task 5.2: Create the Iconography board (`09 · Iconography · v3.1`)

**Files:** new top-level frame in `design-system-v3.1.pen`, placed right of `HuQip`.

- [ ] **Step 1: Create board + cover** following the house-style cover (surface-dark, brand line, `v3.1` badge, "09" num, "Iconography" 96px title, subtitle). Use `FindEmptySpace({width:1680,height:2400,direction:"right",padding:200,nodeId:"HuQip"})`.

- [ ] **Step 2: Section "01 System"** — library = **lucide** (consistent line icons), stroke ~1.5–2px, no filled/duotone, no emoji. State the rule: "Icons clarify, never decorate. One library, consistent weight."

- [ ] **Step 3: Section "02 Sizes"** — token table (TOKEN | VALUE | USAGE): `icon-sm` 16 "inline with body / buttons", `icon-md` 20 "callouts / list markers", `icon-lg` 24 "feature/Highlight cards". Show a real `lucide` icon at each size as preview (type `icon`, `library:"lucide"`, `icon:"arrow-right"`, width/height = token).

- [ ] **Step 4: Section "03 Inventory"** — a grid of the icons actually used in the system (from masters): `arrow-right`, `arrow-up-right`, `chevron-right`, `layers`, `trending-up`, `info`, plus status set (`check-circle`, `alert-triangle`, `x-circle`). Each cell: the icon (`icon-md`, `$text-secondary`) + its name (mono caption).

- [ ] **Step 5: Section "04 Illustration policy"** — text: the system uses **no decorative illustration**; imagery is limited to case-study screenshots/diagrams presented in neutral frames with `border-subtle` and `radius-md`; never stock/marketing imagery (ties to CLAUDE.md §2 "Avoid").

- [ ] **Step 6: Section "05 Governance"** — the layer chain card, marking Iconography as a Foundation-adjacent primitive.

- [ ] **Step 7: Verify** (per section) — `snapshot_layout(board id, problemsOnly:true)` → "No layout problems"; `get_screenshot(each section)` → icons render at correct sizes, inventory grid aligned.
- [ ] **Step 8: Commit (after Save):** "v3.1: add Iconography & illustration board".

---

## Phase 6 — Re-version, governance, review, changelog, comparison

Goal: stamp v3.1 across the file, mark the closed gaps, and produce the review/changelog/comparison artifacts.

### Task 6.1: Re-version boards and covers to v3.1

**Files:** all board frames + cover badges.

- [ ] **Step 1: Rename frames** — `Update` each board `name`: `01 · Foundation · v3.1` (`hmZsC`), `02 · Tokens · v3.1` (`IF92z`), `⟐ Component Masters · v3.1` (`J6aQ6v`), `03 · Components · v3.1` (`fEH2K`), `04 · Patterns · v3.1` (`M7VtG`), `05 · Design Review · v3.1` (`ch8VD`), `06 · Documentation · v3.1` (`NnWw8`), `07 · Changelog · v3.1` (`hNrUG`), `08 · V2 ↔ V3 Comparison`→ leave as historical OR add `09 Iconography`/comparison note (see 6.4).
- [ ] **Step 2: Update cover badges** — set each cover version badge text to `v3.1` (Foundation `O4VTY`, Tokens `kVHUM`, Components `LBORH`, Patterns `U2zGMH`, Review `X218fM`→`v3.0 → v3.1`, Docs `EFpBP`→`v3.1 · GOVERNANCE`, Changelog `AG5wz`, plus the masters label `pspZW`→`⟐ COMPONENT MASTERS · v3.1`).
- [ ] **Step 3: Verify** — `get_editor_state()` → all board names read `· v3.1`. `get_screenshot` two covers to confirm badges.
- [ ] **Step 4: Commit (after Save):** "v3.1: re-version boards and covers".

### Task 6.2: Mark closed gaps & extend primitives in Documentation

**Files:** board `NnWw8` — gap cards `aN8ta` (type-scale), `o6pTsU` (breakpoints), `JDV0r` (icon usage); primitives table `WQ7IO`.

- [ ] **Step 1: Mark RESOLVED** — for each of the three gap cards, update its Sev chip label→`RESOLVED` + fill→`$status-success-surface`/`$status-success-fg`, and rewrite the title/body to "…resolved in v3.1" with what was added (mirror the v3 radius/motion resolution pattern). Read each card's Sev/title/body child IDs first via `batch_get`.
- [ ] **Step 2: Extend the primitives table** `WQ7IO` with three rows: `font-size-*` "12-step type ramp", `breakpoint-*` "responsive contract", `icon-sm/-md/-lg` "icon sizing". Give the prior last row a bottom border; new last row none.
- [ ] **Step 3: Verify** — `snapshot_layout("NnWw8", problemsOnly:true)` → "No layout problems"; `get_screenshot` the gap section + primitives table.
- [ ] **Step 4: Commit (after Save):** "v3.1: resolve type-scale/breakpoint/icon gaps in docs".

### Task 6.3: Update the Design Review for v3.1

**Files:** board `ch8VD`.

- [ ] **Step 1:** Update the Executive Summary body (`u87T1a` → describe the structural-accent + type/breakpoint/icon/motion-choreography work). Update the Evaluation table — bump **Scalability** and **Brand alignment** to `5.0` (now that the accent is structural and tokens are complete); add a short row/note "Iconography 5.0 — system defined." Rewrite "Open Risks" to drop the now-closed items (accent decorative; type-scale/breakpoint raw) and leave only genuine remaining risks (e.g. motion unvalidated until code; illustration policy untested on real content). Update the Final Assessment statement to reference v3.1.
- [ ] **Step 2: Verify** — `snapshot_layout("ch8VD", problemsOnly:true)` → "No layout problems"; screenshot the Evaluation table + Final Assessment.
- [ ] **Step 3: Commit (after Save):** "v3.1: refresh Design Review".

### Task 6.4: Add a v3.1 Changelog and extend the Comparison

**Files:** board `hNrUG` (Changelog), board `HuQip` (Comparison).

- [ ] **Step 1: Changelog** — add a top "v3.1" version block (or new category entries) under Added/Changed/Improved: Added = type-scale, breakpoint, icon tokens + Iconography board + motion choreography + Eyebrow master; Changed = accent now structural (distribution + texture); Improved = brand distinctiveness, scalability. Each with why · problem · impact (reuse the `entry()` helper shape from the v3 build).
- [ ] **Step 2: Comparison** — extend board `HuQip`: rename to `08 · V2 ↔ V3 ↔ V3.1 Comparison`; add a "V3.1" column (or a third panel in the headline) and rows for the new layers (Type scale, Breakpoints, Iconography, Motion choreography, Accent distribution) showing V2 "—" / V3 "color only" / V3.1 "structural + tokenized".
- [ ] **Step 3: Verify** — `snapshot_layout` both boards `problemsOnly:true` → "No layout problems"; screenshot the new changelog block + comparison column.
- [ ] **Step 4: Commit (after Save):** "v3.1: changelog + V2↔V3↔V3.1 comparison".

### Task 6.5: Sync CLAUDE.md and memory; open PR

**Files:** `CLAUDE.md`, memory, git.

- [ ] **Step 1:** Update `CLAUDE.md` §1/§3/§4/§5/§11 to add `design-system-v3.1.pen` (current), its boards (incl. `09 · Iconography`), the new tokens (`font-size-*`, `breakpoint-*`, `icon-*`), the structural-accent distribution rule, and the new "next" (implement in code). Move v3 to "frozen/previous."
- [ ] **Step 2:** Update the `design-system-v3-file.md` memory (or add `design-system-v3.1-file.md`) noting v3.1 is current.
- [ ] **Step 3:** Branch `design-system-v3.1`, commit any remaining staged work + `CLAUDE.md`, push, open PR with a summary mirroring this plan's phases.

---

## Self-Review

- **Spec coverage:** Opportunity 01 → Phase 1 (1.1–1.6). 02 → Phase 2. 03 → Phase 3. 04 → Phase 4 (reframed as choreography; noted). 05 → Phase 5. Re-version/governance/review/changelog/comparison → Phase 6. ✔
- **Known gaps deliberately left:** 15px body text keeps a literal size (no matching token; expanding the ramp is out of scope) — called out in Task 2.2. Motion remains spec-only (no runtime) — called out in Scope + Phase 4. Both are honest, documented limits, not placeholders.
- **ID consistency:** All edit targets use v3's preserved IDs (verified to exist in the v3 build). New-node tasks capture returned IDs before reuse (Eyebrow master 1.4→1.5; new sections 6.x read child IDs before editing) per the CLAUDE.md §9 "globals may not persist" rule.
- **Read-before-edit:** Tasks that touch nested text whose child IDs weren't captured in the v3 build (pattern eyebrows 1.5; gap-card chips 6.2; review bodies 6.3) explicitly start with a `batch_get` read step.
