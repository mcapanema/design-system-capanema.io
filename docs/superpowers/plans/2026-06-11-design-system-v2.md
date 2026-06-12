# Capanema Design System v2.0 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Audit the v1.1 Capanema design system and build a parallel v2.0 board set (Foundation, Tokens, Components, Patterns), a Design Review & Changelog board, and refreshed governance docs — refining quality within the existing brand DNA while keeping v1.1 frozen for comparison.

**Architecture:** Five new Pencil boards built left→right beside the frozen v1.1 boards, in dependency order (Foundation → Tokens → Components → Patterns → Changelog). Document-global variables are edited additively (new status/prose tokens) and the orphaned shadcn token set is removed only after a zero-reference check. A new component-masters library (`⟐ Component Masters · v2`) carries fresh IDs so v1.1 instances never break.

**Tech Stack:** Pencil (`pencil` MCP server, file `design-system.pen`) — tools `get_editor_state`, `get_variables`, `set_variables`, `batch_get`, `batch_design`, `snapshot_layout`, `get_screenshot`, `export_nodes`. Markdown artifacts in `docs/superpowers/`. Git for `.pen` binary + doc snapshots.

---

## Reference data (used across tasks)

### Spec & source of truth
- Design spec: `docs/superpowers/specs/2026-06-11-design-system-v2-design.md`
- Project rules & house style: `CLAUDE.md` (read §5–§10 before any Pencil work).

### Frozen v1.1 board / master IDs (NEVER edit these)
- Boards: `01 · Foundation` `bsXHN` · `02 · Tokens` `gzBR6` · `03 · Components` `Dny8s` · `04 · Patterns` `jvEua` · `06 · Themes` `mO19J` · `⟐ Component Masters` `VX0oF`
- v1.1 masters: Button `I7lVO` · Icon Button `vrUW5` · Nav Item `WotMW` · Tag `wKzU6` · Metric `IlUJh` · Case Study Card `SG5xZ` · Article Card `q0cIY` · Highlight Card `iPsTc` · Metric Card `g0x0UR` · Timeline Item `y8sbCU`

### Existing semantic tokens (consume these; do NOT reference raw primitives in components/patterns)
Text: `text-primary` `text-secondary` `text-tertiary` `text-muted` `text-inverse` `text-accent` `text-on-accent` `text-on-dark` `text-on-dark-muted`
Surface: `surface-primary` `surface-secondary` `surface-tertiary` `surface-elevated` `surface-dark` `surface-inverse` `surface-accent` `surface-dark-raised` `surface-subtle`
Border: `border-subtle` `border-default` `border-strong` `border-accent`
Action: `action-primary` `action-primary-hover` `action-primary-pressed` `action-secondary` `action-secondary-hover` `link` `link-hover` `focus-ring` `disabled` `disabled-text`
Elevation: `shadow-1a` `shadow-1b`
Spacing: `space-0..space-12` (0,4,8,12,16,24,32,40,48,64,80,96,128)
Fonts: `font-sans`=Inter, `font-mono`=JetBrains Mono

### Orphaned tokens to remove (Phase 2, after zero-reference check)
`--accent` `--accent-foreground` `--background` `--black` `--border` `--card` `--card-foreground` `--color-error` `--color-error-foreground` `--color-info` `--color-info-foreground` `--color-success` `--color-success-foreground` `--color-warning` `--color-warning-foreground` `--destructive` `--font-primary` `--font-secondary` `--foreground` `--input` `--muted` `--muted-foreground` `--popover` `--popover-foreground` `--primary` `--primary-foreground` `--radius-m` `--radius-none` `--radius-pill` `--ring` `--secondary` `--secondary-foreground` `--sidebar` `--sidebar-accent` `--sidebar-accent-foreground` `--sidebar-border` `--sidebar-foreground` `--sidebar-primary` `--sidebar-primary-foreground` `--sidebar-ring` `--white`

### NEW v2 tokens (exact values — added in Phase 2)

**Status tokens** — muted, executive, themed `[Light, Dark]`, each foreground + surface:
```
status-success-fg      Light #15803D  Dark #4ADE80
status-success-surface Light #F0FDF4  Dark #11271A
status-error-fg        Light #B91C1C  Dark #F87171
status-error-surface   Light #FEF2F2  Dark #2A1414
status-warning-fg      Light #B45309  Dark #FBBF24
status-warning-surface Light #FFFBEB  Dark #2A2009
status-info-fg         Light $accent-500  Dark $accent-300
status-info-surface    Light #EFF6FF  Dark $dark-700
```
**Prose / long-form reading tokens:**
```
text-prose   (color)  Light $neutral-800  Dark $neutral-200   // softened body for long reading
measure-prose (number) 680   // px max line length (~66ch at 18px)
leading-prose (number) 1.7   // lineHeight multiplier for prose body
leading-tight (number) 1.1   // display/heading leading
leading-snug  (number) 1.3   // sub-heading leading
leading-normal(number) 1.6   // default body leading
```

### Type scale with line-height + tracking (documented Foundation v2; applied in components/patterns)
| Step | px | lineHeight | letterSpacing(px) | weight |
|------|----|-----------|--------------------|--------|
| Display XL | 72 | 1.05 | -1.5 | 600 |
| Display L | 64 | 1.05 | -1.3 | 600 |
| Display M | 56 | 1.08 | -1.1 | 600 |
| H1 | 48 | 1.10 | -1.0 | 600 |
| H2 | 40 | 1.15 | -0.8 | 600 |
| H3 | 32 | 1.20 | -0.5 | 600 |
| H4 | 24 | 1.30 | -0.3 | 600 |
| H5 | 20 | 1.40 | 0 | 600 |
| Body L | 18 | 1.60 | 0 | 400 |
| Body M | 16 | 1.60 | 0 | 400 |
| Body S | 14 | 1.50 | 0 | 400 |
| Caption | 12 | 1.40 | 0 | 500 |
| Mono label | 13 | 1.40 | 1.0 | 500 (uppercase) |
| Prose body | 18 | 1.70 | 0 | 400 |

> **letterSpacing unit check:** on first use in Task 1.3, render one display heading with `letterSpacing:-1.5` and screenshot. If glyphs collide/overlap wildly, the unit is em — switch to em values (XL `-0.02`, L `-0.02`, M `-0.018`, H1 `-0.015`, H2 `-0.01`, H3 `-0.008`). Record the confirmed unit in the audit doc and use it consistently.

### House-style helper template (redefine inline in EVERY `batch_design` — globals don't persist)
```js
const header=(p,n,num,title,desc)=>{const h=Insert(p,{type:"frame",name:n,layout:"vertical",gap:"$space-3",width:760});Insert(h,{type:"text",name:"Num",fontFamily:"JetBrains Mono",fontSize:13,letterSpacing:1,fill:"$text-accent",content:num});Insert(h,{type:"text",name:"Title",textGrowth:"fixed-width",width:"fill_container",fontFamily:"Inter",fontSize:40,fontWeight:"600",lineHeight:1.15,letterSpacing:-0.8,fill:"$text-primary",content:title});if(desc)Insert(h,{type:"text",name:"Desc",textGrowth:"fixed-width",width:"fill_container",fontFamily:"Inter",fontSize:17,lineHeight:1.6,fill:"$text-secondary",content:desc});return h};
const showcase=(p,n)=>Insert(p,{type:"frame",name:n,layout:"vertical",gap:"$space-6",width:"fill_container",padding:"$space-8",cornerRadius:12,fill:"$surface-secondary",stroke:"$border-subtle",strokeWidth:1});
```
Doc panel = 2-col grid of 8 fields: Purpose · Usage · Anatomy · Variants · States · Token Usage · Spacing · Accessibility.

### Pencil gotchas (from CLAUDE.md §9 — honor all)
- `alignItems` only `start`/`center`/`end`. No baseline/stretch.
- Circular sizing: never have a `fit_content` parent whose only children all `fill_container` the same axis. Give the parent/row a fixed cross-axis size.
- `set_variables` themed values: ALWAYS write BOTH `[{theme:{Mode:"Light"},value},{theme:{Mode:"Dark"},value}]`. Merging a themed array onto a plain value drops the base and resolves to `#000000`.
- Render lag: a freshly-created tall section may screenshot blank-white. Confirm with `snapshot_layout(problemsOnly:true)` + `batch_get(resolveVariables:true)`; re-screenshot after later edits.
- `batch_design` globals (assignment without const/let) may NOT persist across calls — capture returned IDs and hardcode them in the next call.
- Always set a human-readable `name` on every node. Build/verify one section at a time.

---

## Phase 0 — Audit (feeds board 05)

### Task 0.1: Set up the audit findings doc

**Files:**
- Create: `docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md`

- [ ] **Step 1: Create the findings file with section scaffold**

Write this exact content:
```markdown
# v1.1 Design System Audit — Findings

Severity legend: **D**=Defect (broken/contradictory) · **W**=Weakness (below bar) · **O**=Opportunity (elevate)

## Confirmed before audit
- **D1 — Orphaned shadcn token set.** Second brand (orange `--primary` #FF8400, bone `--background` #F2F3F0, Geist font) coexists with the live slate+blue+Inter system. Live boards use slate+blue; orphaned set is unused but resolvable. Resolution: remove after zero-reference check (Phase 2).
- **W1 — Status colors off-brand & orphaned.** success/error/warning/info exist only in the orphaned set. Resolution: define proper status tokens in slate+blue (Phase 2).

## 1. Visual identity
## 2. Typography
## 3. Tokens
## 4. Components
## 5. Patterns
## 6. Content hierarchy
## 7. Executive credibility
## 8. Accessibility
## 9. Governance

## letterSpacing unit (confirmed in Task 1.3): TBD
```

- [ ] **Step 2: Commit**
```bash
git add docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md
git commit -m "Add v1.1 audit findings scaffold"
```

### Task 0.2: Audit Foundation + Tokens (v1.1)

**Acceptance check:** Sections 1, 2, 3 of the findings doc each have ≥2 concrete, severity-tagged findings citing specific section frames.

- [ ] **Step 1: Pull structure of boards `bsXHN` and `gzBR6`**

Tool: `batch_get` with `{filePath:"design-system.pen", nodeIds:["bsXHN","gzBR6"], readDepth:2}` to list section frame IDs and names.

- [ ] **Step 2: Screenshot each Foundation section at section zoom**

For each section frame ID returned (Colors, Typography, Spacing, Elevation, Roadmap), call `get_screenshot(filePath, <sectionId>)`. If blank, re-run after a `batch_get(resolveVariables:true)` per render-lag note.

- [ ] **Step 3: Record findings**

Append to sections 1/2/3 of the findings doc. Assess against these explicit questions and write a tagged bullet for each issue found:
- Typography: Are line-heights defined per step? Is there a long-form reading style/measure? Is hierarchy from size+weight only (never color)? Is display tracking tight enough for editorial feel?
- Color: Is the palette single-sourced? Contrast of `text-muted`/`text-tertiary` on light surfaces (target AA 4.5:1 for body, 3:1 for large)?
- Spacing/Elevation: Is there a section-rhythm spec? Are shadows subtle/executive?

- [ ] **Step 4: Commit**
```bash
git add docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md
git commit -m "Audit: Foundation + Tokens findings"
```

### Task 0.3: Audit Components (v1.1)

**Acceptance check:** Section 4 has a tagged finding for each component family (Button, Nav, Tag, Metric, Cards, Timeline, Footer), with the Case Study Card analyzed in most depth.

- [ ] **Step 1: Read masters resolved**

Tool: `batch_get` `{filePath, nodeIds:["VX0oF"], readDepth:3}` then `batch_get` each master ID with `{resolveInstances:true, resolveVariables:true, readDepth:3}`.

- [ ] **Step 2: Screenshot the Components board sections**

`batch_get("Dny8s", readDepth:2)` for section IDs, then `get_screenshot` each.

- [ ] **Step 3: Record findings in section 4**

For each family answer: refined or generic? Would it exist in Linear/Stripe? Does it support content discovery? For **Case Study Card** specifically: does it lead with outcome+scale, is the hierarchy (category→title→summary→outcome→CTA) correct, is the outcome metric prominent enough?

- [ ] **Step 4: Commit**
```bash
git add -A && git commit -m "Audit: Components findings"
```

### Task 0.4: Audit Patterns + content hierarchy + executive credibility + governance

**Acceptance check:** Sections 5, 6, 7, 9 each have ≥2 tagged findings; section 5 covers Hero, Case Study, Writing, Resume, Contact, Page Layouts.

- [ ] **Step 1: Screenshot Patterns board sections**

`batch_get("jvEua", readDepth:2)` → section IDs → `get_screenshot` each.

- [ ] **Step 2: Record findings (sections 5/6/7/9)**

Answer per the brief: Do patterns guide attention to case studies? Support long-form reading? Establish credibility fast? Would a recruiter/board member/investor grasp the value prop immediately? Is the resume executive-credible? Are layer responsibilities cleanly separated (Foundation→Token→Component→Pattern→Page)?

- [ ] **Step 3: Commit**
```bash
git add -A && git commit -m "Audit: Patterns, hierarchy, credibility, governance findings"
```

### Task 0.5: Accessibility check + orphaned-token reference scan

**Acceptance check:** Section 8 lists contrast results for changed/at-risk tokens; the doc states definitively whether ANY node references the orphaned tokens.

- [ ] **Step 1: Reference-scan the orphaned tokens**

Tool: `batch_get` `{filePath, patterns:[{}], readDepth:6}` is too broad — instead scan per board: for each frozen board ID, `batch_get(<boardId>, readDepth:8, resolveVariables:false)` and grep the returned JSON for each orphaned token name (`$--primary`, `$--background`, etc.). Record in section 3: "orphaned tokens referenced: NONE" or list references.

- [ ] **Step 2: Contrast spot-check**

For `text-muted` (#64748B) on `surface-primary` (#FFFFFF light): compute contrast (expect ~4.6:1, AA body pass). For any token flagged at-risk in Task 0.2, compute and record pass/fail at AA.

- [ ] **Step 3: Commit**
```bash
git add -A && git commit -m "Audit: accessibility + orphaned-token reference scan"
```

---

## Phase 1 — Foundation v2 board

> All Foundation v2 sections live in a new board frame. Build cover + scaffold first, then one section per task, verifying each.

### Task 1.1: Create `01 · Foundation · v2` board scaffold (cover + governance)

**Acceptance check:** Board frame exists right of v1.1 boards; dark cover renders with brand line, v2 badge, 96px title; `snapshot_layout(problemsOnly:true)` clean.

- [ ] **Step 1: Find space and create the board + cover**

`batch_design`:
```js
const pos=FindEmptySpace({width:1600,height:4000,direction:"right",padding:160})
fdnV2=Insert(document,{type:"frame",name:"01 · Foundation · v2",x:pos.x,y:pos.y,layout:"vertical",width:1600,fill:"$surface-primary",clip:true,placeholder:true})
cover=Insert(fdnV2,{type:"frame",name:"Cover",layout:"vertical",gap:"$space-4",width:"fill_container",padding:["$space-11","$space-11"],fill:"$surface-dark"})
Insert(cover,{type:"text",name:"Brand",fontFamily:"JetBrains Mono",fontSize:13,letterSpacing:1,fill:"$text-on-dark-muted",content:"CAPANEMA.IO · DESIGN SYSTEM"})
badge=Insert(cover,{type:"frame",name:"Badge",padding:["$space-1","$space-3"],cornerRadius:999,fill:"$surface-accent"})
Insert(badge,{type:"text",name:"V",fontFamily:"JetBrains Mono",fontSize:12,fill:"$text-on-accent",content:"v2.0"})
Insert(cover,{type:"text",name:"Title",fontFamily:"Inter",fontSize:96,fontWeight:"600",lineHeight:1.05,letterSpacing:-2,fill:"$text-on-dark",content:"Foundation"})
Insert(cover,{type:"text",name:"Sub",textGrowth:"fixed-width",width:820,fontFamily:"Inter",fontSize:20,lineHeight:1.5,fill:"$text-on-dark-muted",content:"Colors, typography, spacing, elevation — the static primitives every higher layer resolves against."})
```

- [ ] **Step 2: Verify**

`snapshot_layout(parentId:fdnV2_id, problemsOnly:true)` → expect "No layout problems". `get_screenshot(cover_id)` → dark cover, white title, blue v2 badge.

- [ ] **Step 3: Add Governance footer scaffold**

`batch_design` (use captured `fdnV2` id): append a `Governance` section showing the chain `Foundation (HERE) → Theme Tokens → Components → Patterns → Pages` as mono labels in a horizontal frame, `HERE` marked with `$text-accent`.

- [ ] **Step 4: Commit checkpoint**
```bash
git add design-system.pen && git commit -m "Foundation v2: board scaffold (cover + governance)"
```

### Task 1.2: Foundation v2 — Colors section

**Acceptance check:** Section shows the cleaned slate ramp + blue accent ramp + dark surfaces as labeled swatches; no orange/bone swatches; layout clean.

- [ ] **Step 1: Build the section**

`batch_design` — redefine `header`/`showcase` helpers inline (see Reference). Insert a full-width section frame `padding:["$space-11","$space-11"]`, `fill:"$surface-primary"`, bottom `border-subtle`. Add `header(section,"H-Colors","## 01","Color","Near-monochrome cool slate with a single blue accent. Hierarchy comes from value, not hue.")`. Then a `showcase` containing three swatch rows built with a loop:
```js
const ramp=(p,label,vars)=>{const r=Insert(p,{type:"frame",name:label,layout:"vertical",gap:"$space-2",width:"fill_container"});Insert(r,{type:"text",name:"L",fontFamily:"JetBrains Mono",fontSize:12,letterSpacing:1,fill:"$text-tertiary",content:label});const row=Insert(r,{type:"frame",name:"Sw",layout:"horizontal",gap:"$space-2",width:"fill_container"});for(const [n,v] of vars){const c=Insert(row,{type:"frame",name:n,layout:"vertical",gap:"$space-1",width:"fill_container"});Insert(c,{type:"frame",name:"Chip",height:64,width:"fill_container",cornerRadius:8,fill:v,stroke:"$border-subtle",strokeWidth:1});Insert(c,{type:"text",name:"N",fontFamily:"JetBrains Mono",fontSize:11,fill:"$text-tertiary",content:n});}return r};
```
Rows: Neutrals (`neutral-0,50,200,300,400,500,600,700,800,900`), Accent (`accent-200,300,400,500,600,700`), Dark surfaces (`dark-900,800,700`). Reference each by `$name`.

- [ ] **Step 2: Verify**

`snapshot_layout(section_id, problemsOnly:true)` → clean. `get_screenshot(section_id)` → confirm slate ramp + blue accent, no warm/orange tones.

- [ ] **Step 3: Commit**
```bash
git add design-system.pen && git commit -m "Foundation v2: Colors section"
```

### Task 1.3: Foundation v2 — Typography section (scale + line-height + tracking + prose)

**Acceptance check:** Every type step from the Reference table is shown as a live specimen with its px/lineHeight/tracking labeled; a dedicated **Prose reading specimen** shows a paragraph at 18px/1.7 constrained to 680px; letterSpacing unit confirmed and recorded.

- [ ] **Step 1: Build the scale specimens**

`batch_design`: section frame + `header(...,"## 02","Typography","Inter for UI and display; JetBrains Mono for labels and code. Hierarchy is size and weight — never color.")`. Then loop the type table:
```js
const SCALE=[["Display XL",72,1.05,-1.5,"600"],["Display L",64,1.05,-1.3,"600"],["Display M",56,1.08,-1.1,"600"],["H1",48,1.10,-1.0,"600"],["H2",40,1.15,-0.8,"600"],["H3",32,1.20,-0.5,"600"],["H4",24,1.30,-0.3,"600"],["H5",20,1.40,0,"600"],["Body L",18,1.60,0,"400"],["Body M",16,1.60,0,"400"],["Body S",14,1.50,0,"400"],["Caption",12,1.40,0,"500"]];
const list=Insert(showcaseFrame,{type:"frame",name:"Scale",layout:"vertical",gap:"$space-6",width:"fill_container"});
for(const [n,px,lh,ls,w] of SCALE){const row=Insert(list,{type:"frame",name:n,layout:"horizontal",gap:"$space-6",width:"fill_container",alignItems:"center"});Insert(row,{type:"text",name:"Meta",textGrowth:"fixed-width",width:160,fontFamily:"JetBrains Mono",fontSize:12,lineHeight:1.4,fill:"$text-tertiary",content:`${n}\n${px}/${lh} · ${ls}`});Insert(row,{type:"text",name:"Spec",textGrowth:"fixed-width",width:"fill_container",fontFamily:"Inter",fontSize:px,fontWeight:w,lineHeight:lh,letterSpacing:ls,fill:"$text-primary",content:"Scaling systems and teams"});}
```

- [ ] **Step 2: Confirm letterSpacing unit**

`get_screenshot(section_id)`. Inspect the Display XL line. If letters overlap badly, the unit is em — re-run Step 1 with the em fallback values from the Reference note, and edit the row Meta labels accordingly. Record the confirmed unit in the audit doc (`## letterSpacing unit` line).

- [ ] **Step 3: Add the Prose reading specimen**

`batch_design`: append a `Prose` block to the typography showcase:
```js
const prose=Insert(showcaseFrame,{type:"frame",name:"Prose Reading",layout:"vertical",gap:"$space-3",width:680});
Insert(prose,{type:"text",name:"Eyebrow",fontFamily:"JetBrains Mono",fontSize:13,letterSpacing:1,fill:"$text-accent",content:"LONG-FORM READING"});
Insert(prose,{type:"text",name:"P",textGrowth:"fixed-width",width:"fill_container",fontFamily:"Inter",fontSize:18,lineHeight:1.7,fill:"$text-prose",content:"Case studies and essays use a constrained measure of roughly sixty-six characters with generous leading. The line length keeps the eye from fatiguing on long passages, and the softened body color reduces glare against white while staying comfortably above the AA contrast floor."});
```
> `$text-prose` is added in Phase 2; until then this resolves via `$text-primary`. After Phase 2 it picks up the softened value automatically.

- [ ] **Step 4: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; `get_screenshot` confirms measure ~680px and comfortable leading.
```bash
git add design-system.pen docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md && git commit -m "Foundation v2: Typography section + prose specimen"
```

### Task 1.4: Foundation v2 — Spacing + section rhythm

**Acceptance check:** 8-pt scale `space-0..space-12` shown as labeled bars; a "Section rhythm" sub-block documents vertical cadence (section padding `space-11` top/bottom, `space-8` between section header and showcase).

- [ ] **Step 1: Build**

`batch_design`: section + header `## 03 Spacing`. Loop `space-1..space-12` rendering each as a horizontal bar (`fill:"$surface-accent"`, `height:16`, `width:` the literal px value) with a mono label `space-N · Npx`. Add a `Section rhythm` text block stating the cadence rule.

- [ ] **Step 2: Verify + commit**

`snapshot_layout` clean; screenshot. `git add design-system.pen && git commit -m "Foundation v2: Spacing + section rhythm"`

### Task 1.5: Foundation v2 — Elevation

**Acceptance check:** Four elevation levels (0 flat, 1 raised, 2 hover, 3 overlay) shown as cards with the documented shadows; shadows use layered `#0F172A` low-alpha (subtle/executive).

- [ ] **Step 1: Build**

`batch_design`: section + header `## 04 Elevation`. Four cards in a row, each `fill:"$surface-elevated"`, `cornerRadius:12`, with `effect`:
```js
const EL=[["0 · Flat",[]],["1 · Raised",[{type:"shadow",offset:{x:0,y:1},blur:3,spread:0,color:"$shadow-1a"}]],["2 · Hover",[{type:"shadow",offset:{x:0,y:4},blur:12,spread:0,color:"$shadow-1a"}]],["3 · Overlay",[{type:"shadow",offset:{x:0,y:12},blur:24,spread:0,color:"$shadow-1a"}]]];
```
Each card labeled with name + the shadow spec.

- [ ] **Step 2: Verify + commit**

`snapshot_layout` clean; screenshot confirms subtle shadows. `git add design-system.pen && git commit -m "Foundation v2: Elevation"`

### Task 1.6: Foundation v2 — finalize board

- [ ] **Step 1: Unset placeholder**

`batch_design`: `Update(fdnV2_id,{placeholder:false})`.

- [ ] **Step 2: Whole-board verify**

`snapshot_layout(fdnV2_id, problemsOnly:true)` → "No layout problems". `get_screenshot(fdnV2_id)` (expect possible render lag on a tall board — re-shoot if blank).

- [ ] **Step 3: Commit**
```bash
git add design-system.pen && git commit -m "Foundation v2: finalize board"
```

---

## Phase 2 — Tokens v2 (variables + documentation board)

> Variables are document-global. Adding tokens is safe/additive. Removing the orphaned set is gated on the Task 0.5 zero-reference result. v1.1 boards must render identically after this phase.

### Task 2.1: Add status + prose tokens (additive, themed)

**Acceptance check:** All new tokens from the Reference exist; `batch_get(resolveVariables:true)` on a light-context node shows non-black Light values; v1.1 boards unchanged.

- [ ] **Step 1: Add tokens via `set_variables`**

Tool: `set_variables`. For EVERY themed token write BOTH entries. Exact payload (status):
```json
{
  "status-success-fg":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#15803D"},{"theme":{"Mode":"Dark"},"value":"#4ADE80"}]},
  "status-success-surface":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#F0FDF4"},{"theme":{"Mode":"Dark"},"value":"#11271A"}]},
  "status-error-fg":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#B91C1C"},{"theme":{"Mode":"Dark"},"value":"#F87171"}]},
  "status-error-surface":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#FEF2F2"},{"theme":{"Mode":"Dark"},"value":"#2A1414"}]},
  "status-warning-fg":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#B45309"},{"theme":{"Mode":"Dark"},"value":"#FBBF24"}]},
  "status-warning-surface":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#FFFBEB"},{"theme":{"Mode":"Dark"},"value":"#2A2009"}]},
  "status-info-fg":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"$accent-500"},{"theme":{"Mode":"Dark"},"value":"$accent-300"}]},
  "status-info-surface":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"#EFF6FF"},{"theme":{"Mode":"Dark"},"value":"$dark-700"}]}
}
```
Then prose tokens:
```json
{
  "text-prose":{"type":"color","value":[{"theme":{"Mode":"Light"},"value":"$neutral-800"},{"theme":{"Mode":"Dark"},"value":"$neutral-200"}]},
  "measure-prose":{"type":"number","value":680},
  "leading-prose":{"type":"number","value":1.7},
  "leading-tight":{"type":"number","value":1.1},
  "leading-snug":{"type":"number","value":1.3},
  "leading-normal":{"type":"number","value":1.6}
}
```

- [ ] **Step 2: Verify no black-resolution regression**

`batch_get` `{filePath, nodeIds:["bsXHN"], resolveVariables:true, readDepth:2}` — confirm a known light surface still resolves to `#FFFFFF`/slate, NOT `#000000`. `get_screenshot("bsXHN")` — v1.1 Foundation cover unchanged.

- [ ] **Step 3: Commit**
```bash
git add design-system.pen && git commit -m "Tokens v2: add status + prose tokens (additive, themed)"
```

### Task 2.2: Remove orphaned shadcn token set (gated)

**Acceptance check:** If Task 0.5 found zero references, all orphaned tokens are gone and v1.1 boards render identically. If references exist, tokens are renamed `z-deprecated-<name>` instead and the finding is updated.

- [ ] **Step 1: Branch on the reference-scan result**

Read `docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md` section 3. IF "orphaned tokens referenced: NONE": proceed to Step 2 (remove). ELSE: skip to Step 3 (quarantine the referenced ones; remove the rest).

- [ ] **Step 2: Remove (no-reference path)**

`set_variables` cannot delete by itself in all cases — confirm the deletion semantics with a single test removal first: remove `--black` and `--white`, then `get_variables` to confirm they're gone. If removal works, remove the full orphaned list from the Reference. (If `set_variables` only upserts, document that and use the editor's variable deletion path; record the method used.)

- [ ] **Step 3: Quarantine path (only if references exist)**

For each referenced orphaned token, leave it but document it as deprecated in the Tokens v2 board governance section. Do NOT change its value (would alter v1.1 visuals).

- [ ] **Step 4: Verify v1.1 intact + commit**

`get_screenshot("bsXHN")`, `get_screenshot("Dny8s")` — unchanged vs Phase 0 screenshots.
```bash
git add design-system.pen docs/superpowers/audits/2026-06-11-v1.1-audit-findings.md && git commit -m "Tokens v2: remove/quarantine orphaned shadcn token set"
```

### Task 2.3: Build `02 · Tokens · v2` documentation board

**Acceptance check:** Board has cover + sections for Text / Surface / Border / Action / Status / Prose token tables (token name, Light value, Dark value, usage) + a Governance & Usage section; layout clean.

- [ ] **Step 1: Scaffold board + cover**

`batch_design`: `FindEmptySpace` right of Foundation v2; create `02 · Tokens · v2` frame (same cover pattern as Task 1.1, title "Tokens", subtitle about semantic aliases resolving Foundation primitives, themed on Mode).

- [ ] **Step 2: Token table sections**

For each family build a section with `header` + a table (frame→row→cell per CLAUDE.md table rules). Row = `[token, lightSwatch+hex, darkSwatch+hex, usage]`. Drive from arrays of the existing semantic tokens (Reference) + the new status/prose tokens. Render swatches as small chips with `fill` bound to the token (Light) and a second chip inside a `theme:{Mode:"Dark"}` wrapper frame for the Dark value.

- [ ] **Step 3: Governance & Usage section**

Text block: the chain `Foundation → Theme Tokens → Components → Patterns → Pages`; rule "components consume semantic tokens, never raw primitives or the (now removed) shadcn set"; the dual-entry themed-variable rule.

- [ ] **Step 4: Verify + finalize + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot key sections; `Update(board,{placeholder:false})`.
```bash
git add design-system.pen && git commit -m "Tokens v2: documentation board"
```

---

## Phase 3 — Components v2 (masters library + documentation board)

> Build masters in `⟐ Component Masters · v2` (new IDs). Each master in its own `batch_design` so IDs return. Refine the 10 v1.1 families; add new components confirmed by audit (default set: Blockquote/Pullquote, Callout, ToC rail, Breadcrumb, Credibility strip). Then a documentation board instances them with sample content.

### Task 3.1: Create `⟐ Component Masters · v2` library frame

- [ ] **Step 1: Create the library frame**

`batch_design`: `FindEmptySpace` far right; `mastersV2=Insert(document,{type:"frame",name:"⟐ Component Masters · v2",x,y,layout:"vertical",gap:"$space-8",padding:"$space-9",fill:"$surface-secondary",placeholder:true})`. Capture `mastersV2` id.

- [ ] **Step 2: Commit**
```bash
git add design-system.pen && git commit -m "Components v2: masters library frame"
```

### Task 3.2: Refine core controls — Button, Icon Button, Nav Item, Tag

**Acceptance check:** Four reusable masters exist with v2 refinements; each renders all documented variants/states via override; tokens only (no raw hex).

- [ ] **Step 1: Build Button master**

`batch_design` (own call): create reusable Button = frame `padding:["$space-3","$space-5"]`, `gap:"$space-2"`, `cornerRadius:8`, `fill:"$action-primary"`, `alignItems:"center"`, with Label (`fontFamily:"Inter",fontSize:14,fontWeight:"600",fill:"$text-on-accent"`) + trailing Icon (lucide `arrow-right`, `enabled:false` by default). Refinement vs v1.1: add `focus-ring` documentation, ensure `$text-on-accent` label. Capture root + Label + Icon IDs.

- [ ] **Step 2: Build Icon Button, Nav Item, Tag masters**

Separate `batch_design` calls (one per master to capture IDs). Nav Item: Label `fontSize:14,fill:"$text-secondary"`; active state documented as `strokeWidth:{bottom:2}` + Label `$text-primary`. Tag: pill `cornerRadius:999`, `fill:"$surface-tertiary"`, optional leading Dot (`enabled:false`), Label `fontSize:13,fill:"$text-secondary"`. Capture all IDs.

- [ ] **Step 3: Verify**

For each: `snapshot_layout(problemsOnly:true)` clean; `get_screenshot` the master. Confirm contrast: Button label `$text-on-accent` (#FFFFFF) on `$action-primary` (#2563EB) ≈ 4.5:1+ (AA pass for 14px bold/large).

- [ ] **Step 4: Commit**
```bash
git add design-system.pen && git commit -m "Components v2: core controls (Button, Icon Button, Nav, Tag)"
```

### Task 3.3: Refine data-display — Metric, Metric Card, Highlight Card, Timeline Item

**Acceptance check:** Four masters exist; Metric value uses a large tabular display weight; Timeline Item draws connector as left border with absolute dot and documents the "last item `strokeWidth:0`" rule; Highlight Card uses on-dark token family.

- [ ] **Step 1: Build Metric + Metric Card**

Separate `batch_design` calls. Metric: vertical frame, Value (`fontFamily:"Inter",fontSize:40,fontWeight:"600",letterSpacing:-0.8,fill:"$text-primary"`) + Label (`fontSize:13,fill:"$text-secondary"`). Metric Card: adds Eyebrow (mono, `$text-accent`) above, `surface-elevated` bg, `border-subtle`, `cornerRadius:12`, elevation-1 shadow. Capture IDs.

- [ ] **Step 2: Build Highlight Card + Timeline Item**

Separate calls. Highlight Card = always-dark surface: `fill:"$surface-dark-raised"`, Icon `$text-on-dark`, Title `$text-on-dark`, Description `$text-on-dark-muted`. Timeline Item: row with left `strokeWidth:{left:2}` `stroke:"$border-default"`, absolutely-positioned Dot (`layoutPosition:"absolute"`, `fill:"$surface-accent"`), Date/Title/Description/Outcome texts. Capture IDs. Document: last timeline item overrides `strokeWidth:0`.

- [ ] **Step 3: Verify + commit**

`snapshot_layout` clean each; screenshots. `git add design-system.pen && git commit -m "Components v2: data-display (Metric, Metric Card, Highlight, Timeline)"`

### Task 3.4: Rebuild the discovery cards — Case Study Card (priority) + Article Card

**Acceptance check:** Case Study Card leads with category + outcome-oriented title, shows a prominent **outcome metric** (value + label) and a clear CTA; Article Card shows category/date/title/excerpt/read-time. Both token-only, theme-safe, shadows via `$shadow-1a/1b`.

- [ ] **Step 1: Build Case Study Card master**

`batch_design` (own call). Structure (vertical frame, `surface-elevated`, `cornerRadius:12`, `border-subtle`, elevation-1, `padding:"$space-7"`, `gap:"$space-5"`):
```
Category   (mono 13, $text-accent, uppercase)
Title      (Inter 24, 600, lineHeight 1.3, $text-primary)  // outcome-oriented
Summary    (Inter 16, 1.6, $text-secondary)
[divider border-subtle]
Outcome row: Outcome Value (Inter 32, 600, $text-primary) + Outcome Label (13, $text-secondary)
CTA: text "Read case study" ($link) + arrow icon
```
Capture root + each child ID (Category, Title, Summary, Outcome Value, Outcome Label, CTA Label).

- [ ] **Step 2: Build Article Card master**

`batch_design` (own call): Category + Date row (mono, `$text-tertiary`), Title (Inter 20, 600), Excerpt (16/1.6, `$text-secondary`), Read Time (mono 12, `$text-tertiary`). Capture IDs.

- [ ] **Step 3: Verify + commit**

`snapshot_layout` clean; screenshot both at Light and (wrap in `theme:{Mode:"Dark"}` test frame, then delete it) Dark to confirm theme-safety.
```bash
git add design-system.pen && git commit -m "Components v2: Case Study Card + Article Card"
```

### Task 3.5: New long-form components — Blockquote/Pullquote, Callout, Table-of-Contents rail

**Acceptance check:** Three new masters exist supporting the reading layout; Callout supports status variants via `status-*` tokens; ToC rail lists anchor links with an active state.

- [ ] **Step 1: Build Pullquote master**

`batch_design`: vertical frame, left `strokeWidth:{left:3}` `stroke:"$border-accent"`, `padding:["$space-2","$space-6"]`, Quote text (Inter 24, 500, lineHeight 1.4, `$text-primary`), optional Attribution (mono 13, `$text-tertiary`). Capture IDs.

- [ ] **Step 2: Build Callout master**

`batch_design`: horizontal frame, `cornerRadius:8`, `fill:"$status-info-surface"`, `padding:"$space-5"`, gap, leading Icon (`$status-info-fg`) + body (Title `$text-primary` + Text `$text-secondary`). Document variants: swap fill+icon to `status-success/warning/error` token pairs. Capture IDs.

- [ ] **Step 3: Build ToC rail master**

`batch_design`: vertical frame, `gap:"$space-2"`, mono eyebrow "ON THIS PAGE" (`$text-tertiary`), then link rows (`$text-secondary`); active row `$text-primary` + left `strokeWidth:{left:2}` `stroke:"$border-accent"`. Capture IDs.

- [ ] **Step 4: Verify + commit**

`snapshot_layout` clean; screenshots. `git add design-system.pen && git commit -m "Components v2: long-form (Pullquote, Callout, ToC rail)"`

### Task 3.6: New wayfinding/credibility — Breadcrumb, Credibility strip + refine Footer

**Acceptance check:** Breadcrumb shows segments + separators with last segment `$text-primary`; Credibility strip lays out a row of monochrome company labels under a mono eyebrow; Footer master refined with token-only fills.

- [ ] **Step 1: Build Breadcrumb + Credibility strip masters**

`batch_design` (one per master to capture IDs). Breadcrumb: horizontal frame, segments (`$text-tertiary`) separated by chevron icons (`$text-tertiary`), last segment `$text-primary`. Credibility strip: vertical frame, mono eyebrow "TRUSTED BY TEAMS AT" (`$text-tertiary`), then a horizontal row of company name texts (Inter 18, 500, `$text-tertiary`) with `space-8` gap.

- [ ] **Step 2: Build refined Footer master**

`batch_design`: `surface-dark` frame, columns of links (`$text-on-dark-muted`), brand line (`$text-on-dark`), bottom legal row. Token-only. Capture root id.

- [ ] **Step 3: Verify + finalize masters library + commit**

`snapshot_layout(mastersV2_id, problemsOnly:true)` clean; `Update(mastersV2_id,{placeholder:false})`.
```bash
git add design-system.pen && git commit -m "Components v2: Breadcrumb, Credibility strip, Footer"
```

### Task 3.7: Build `03 · Components · v2` documentation board

**Acceptance check:** Board has cover + one documented section per component family (showcase with live instances + 8-field doc panel) + Governance; all showcases render; layout clean.

- [ ] **Step 1: Scaffold board + cover**

`batch_design`: `FindEmptySpace` right of Tokens v2; create `03 · Components · v2` (cover title "Components", subtitle about composing tokens into reusable parts).

- [ ] **Step 2: Document each family (loop of sections)**

For each master built in 3.2–3.6, add a section: `header(## NN, name, purpose)` + a `showcase` containing live `ref` instances showing default + key variants/states (override fills per CLAUDE.md variant rules) + the 8-field doc panel. Build a few sections per `batch_design` call, verifying between.

- [ ] **Step 3: Governance section + finalize + commit**

Add Governance (chain with Components = HERE). `Update(board,{placeholder:false})`. `snapshot_layout(problemsOnly:true)` clean.
```bash
git add design-system.pen && git commit -m "Components v2: documentation board"
```

---

## Phase 4 — Patterns v2 (board + page blueprints)

> The most important layer. Patterns compose v2 component instances with executive sample content. Build the board scaffold, then one pattern section per task, verifying each, then page-layout blueprints.

### Task 4.1: Scaffold `04 · Patterns · v2` board + cover

- [ ] **Step 1: Create board + cover**

`batch_design`: `FindEmptySpace` right of Components v2; `04 · Patterns · v2` frame, cover title "Patterns", subtitle "Reusable compositions that guide attention to outcomes and make long-form reading effortless." Capture board id.

- [ ] **Step 2: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot cover. `git add design-system.pen && git commit -m "Patterns v2: board scaffold"`

### Task 4.2: Hero pattern (executive positioning + credibility)

**Acceptance check:** Hero leads with an outcome-oriented headline, a concise positioning subline, primary+secondary CTAs (Button instances), and a credibility strip; reads as executive, not marketing.

- [ ] **Step 1: Build**

`batch_design`: section + `header(## 01, "Hero", ...)` + showcase containing a hero composition: mono eyebrow ("CTO · PLATFORM & AI"), Display headline (`fontSize:64,lineHeight:1.05,letterSpacing:-1.3,$text-primary`) e.g. "Scaling engineering organizations and the platforms they ship.", subline (Body L, `$text-secondary`, measure ~640), CTA row (primary Button "View case studies" + secondary Button instance), then a Credibility strip instance below.

- [ ] **Step 2: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot. `git add design-system.pen && git commit -m "Patterns v2: Hero"`

### Task 4.3: Case-study discovery pattern (featured + index)

**Acceptance check:** A featured case study (large card, prominent outcome metric) sits above a scannable index of Case Study Card instances; attention clearly flows to outcomes and scale.

- [ ] **Step 1: Build featured + index**

`batch_design`: section + header `## 02 Case Study Discovery`. Featured = a wide Case Study Card instance with larger title and Metric instances row (e.g. "3.2M patients/yr", "−38% cost", "12 → 140 eng"). Below: a 2- or 3-column grid (manual row frames per CLAUDE.md grid rule) of Case Study Card instances with distinct outcome-oriented sample content.

- [ ] **Step 2: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot confirms outcome metrics are the visual anchor. `git add design-system.pen && git commit -m "Patterns v2: Case-study discovery"`

### Task 4.4: Case-study detail (long-form reading layout)

**Acceptance check:** A reading layout with a ToC rail aside + a prose column at `measure-prose` width using `text-prose`/`leading-prose`, including a Pullquote and a Callout instance; metrics band near the top.

- [ ] **Step 1: Build**

`batch_design`: section + header `## 03 Case Study (Long-form)`. Two-column: left ToC rail instance (fixed width ~220), right prose column (`width:"$measure-prose"` → 680) with: title (H1), meta row, a Metric row, several prose paragraphs (18/1.7/`$text-prose`), a Pullquote instance, a Callout instance. (Honor the circular-sizing rule: give the outer row a fixed height or make columns not both fill the same axis.)

- [ ] **Step 2: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot confirms measure + leading. `git add design-system.pen && git commit -m "Patterns v2: Case-study detail (long-form)"`

### Task 4.5: Writing pattern + Resume pattern

**Acceptance check:** Writing = index of Article Card instances + a featured essay; Resume = executive experience presentation using refined Timeline Items with visible outcomes + a Metrics band + a credibility strip — scale legible at a glance.

- [ ] **Step 1: Build Writing section**

`batch_design`: section `## 04 Writing` — featured essay block + grid of Article Card instances with sample leadership-writing titles.

- [ ] **Step 2: Build Resume section**

`batch_design`: section `## 05 Resume` — header (name, role, one-line positioning), a Metrics band (Metric Card instances: years, teams scaled, platforms), then a Timeline of roles (Timeline Item instances, each with an outcome line; LAST item `strokeWidth:0`), then a skills/areas row. Outcome-oriented role titles.

- [ ] **Step 3: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshots. `git add design-system.pen && git commit -m "Patterns v2: Writing + Resume"`

### Task 4.6: Contact pattern + Page-layout blueprints + Governance

**Acceptance check:** Contact pattern present; four labeled "browser" blueprints (Homepage, Case Study, Writing, Resume) show band stacks (label rail + schematic) in correct section order; Governance section marks Patterns = HERE.

- [ ] **Step 1: Build Contact section**

`batch_design`: section `## 06 Contact` — concise headline, email/links row, no heavy form (executive restraint).

- [ ] **Step 2: Build Page-layout blueprints**

`batch_design`: section `## 07 Page Layouts`. For each page, a "browser" mock = chrome bar + vertical band stack; each band = left label rail (section name + pattern used) + right schematic (placeholder bars). Give each band row a FIXED height per type so label + schematic can both `fill_container` the cross axis (circular-sizing rule). Band order per page:
- Homepage: Nav → Hero → Featured case study → Case-study index → Writing teaser → Contact → Footer
- Case Study: Nav → Breadcrumb → Title/meta → Metrics band → Long-form body (ToC + prose) → Next case → Footer
- Writing: Nav → Page header → Featured essay → Article index → Footer
- Resume: Nav → Header → Metrics band → Experience timeline → Skills → Contact → Footer

- [ ] **Step 3: Governance + finalize + commit**

Add Governance (Patterns = HERE) + a Content Guidelines block (outcome-oriented headline rule with good/bad examples). `Update(board,{placeholder:false})`. `snapshot_layout(problemsOnly:true)` clean.
```bash
git add design-system.pen && git commit -m "Patterns v2: Contact, page blueprints, governance"
```

---

## Phase 5 — Board `05 · Design Review & Changelog`

### Task 5.1: Scaffold board + Executive Summary + Major Findings

**Acceptance check:** Board exists with cover; Executive Summary section + Major Findings section (severity-tagged, transcribed from the audit doc) render cleanly.

- [ ] **Step 1: Scaffold + cover**

`batch_design`: `FindEmptySpace` (place as the right-most or in the `05` slot left of `06 · Themes` — use `direction:"right"` from Patterns v2). Frame `05 · Design Review & Changelog`, cover title "Design Review", subtitle "What we audited, what changed, and why — v1.1 → v2.0."

- [ ] **Step 2: Executive Summary + Major Findings**

`batch_design`: section `## 01 Executive Summary` (3–4 sentence narrative: the system was sound in DNA; v2 fixed a token-governance defect, elevated typography for long-form, and rebuilt discovery/resume). Section `## 02 Major Findings` — render each finding from `2026-06-11-v1.1-audit-findings.md` as a row with a severity chip (D=`status-error`, W=`status-warning`, O=`status-info` token pairs) + finding text.

- [ ] **Step 3: Verify + commit**

`snapshot_layout(problemsOnly:true)` clean; screenshot. `git add design-system.pen && git commit -m "Board 05: Executive Summary + Major Findings"`

### Task 5.2: What Changed / Why / Before↔After / Principles / Future

**Acceptance check:** Five sections present and populated with the actual v2 changes; Before↔After uses side-by-side mini-references or screenshots-as-fills where useful; layout clean.

- [ ] **Step 1: What Changed + Why It Changed**

`batch_design`: section `## 03 What Changed` (bulleted by layer: Foundation — line-heights + prose + display tracking; Tokens — orphaned set removed, status + prose tokens added; Components — Case Study Card rebuilt + new long-form/wayfinding parts; Patterns — discovery model, long-form reading, executive resume). Section `## 04 Why It Changed` (map each change to a brief principle: readability, credibility, governance, discovery).

- [ ] **Step 2: Before↔After + Principles + Future**

`batch_design`: section `## 05 Before ↔ After` — 2–3 paired mini-comparisons (e.g., v1.1 Case Study Card instance beside v2 instance; v1.1 token table note beside v2). Section `## 06 Design Principles Reinforced` (quiet confidence, restraint, hierarchy from size+weight, tokens as single source, theme-safety). Section `## 07 Future Recommendations` (responsive/motion/iconography board `07`; code implementation on Next.js+Tailwind per CLAUDE.md §11; real-content pass).

- [ ] **Step 3: Finalize + verify + commit**

`Update(board,{placeholder:false})`. `snapshot_layout(problemsOnly:true)` clean; screenshot. `git add design-system.pen && git commit -m "Board 05: changelog (what/why/before-after/principles/future)"`

---

## Phase 6 — Documentation sync

### Task 6.1: Update CLAUDE.md for v2

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Edit CLAUDE.md**

Update §1 (system is now v2.0 — list the v2 board set + frozen v1.1), §4 (add v2 board IDs table — fill in real IDs captured during build), §5 (note status + prose tokens, orphaned set removed), §6 (add v2 master IDs table), §11 (mark v2 done; next = board 07 or code). Keep v1.1 references as the frozen-comparison record.

- [ ] **Step 2: Commit**
```bash
git add CLAUDE.md && git commit -m "Docs: update CLAUDE.md for v2.0"
```

### Task 6.2: Update Claude memory

**Files:**
- Modify: `/Users/murilo/.claude/projects/-Users-murilo-Workspace-ds-capanema-io/memory/pencil-ds-foundation.md`
- Modify: `/Users/murilo/.claude/projects/-Users-murilo-Workspace-ds-capanema-io/memory/MEMORY.md`

- [ ] **Step 1: Update the foundation memory**

Append a v2.0 section: new board IDs, new master IDs, status + prose token names, orphaned-set removal, letterSpacing unit confirmed. Update the MEMORY.md one-liner hook to mention v2.0 done.

- [ ] **Step 2: Commit (memory dir is outside repo — no git; just save files)**

Memory files live under `~/.claude`, not the repo; saving them is sufficient.

### Task 6.3: Final whole-system verification

**Acceptance check:** All five v2 boards report "No layout problems"; v1.1 boards visually unchanged from Phase 0 screenshots; both themes render on a sample v2 pattern.

- [ ] **Step 1: Layout sweep**

`snapshot_layout(problemsOnly:true)` on each v2 board id → all "No layout problems".

- [ ] **Step 2: Theme sweep**

Screenshot one v2 pattern section in Light, then temporarily wrap-test in `theme:{Mode:"Dark"}` (or screenshot a board already carrying the theme) → confirm tokens re-resolve, no `#000000` regressions.

- [ ] **Step 3: v1.1 integrity**

Re-screenshot `bsXHN`, `Dny8s`, `jvEua` → identical to Phase 0. 

- [ ] **Step 4: Final commit**
```bash
git add design-system.pen && git commit -m "v2.0: final verification sweep"
```

---

## Self-Review (completed)

**Spec coverage:** Every spec section maps to tasks — §3 versioning→1.1/2.3/3.1/4.1/5.1; §4 audit→Phase 0; §5.1 Foundation→Phase 1; §5.2 Tokens→Phase 2; §5.3 Components→Phase 3; §5.4 Patterns→Phase 4; §5.5 board 05→Phase 5; §6 phasing→phase order; §7 success criteria→4.x + 6.3; docs→Phase 6. No gaps.

**Placeholder scan:** No "TBD/TODO/handle edge cases". The two intentional runtime branches (letterSpacing unit in 1.3; orphaned-token removal-vs-quarantine in 2.2) are fully specified decisions with both paths written, not placeholders. Real token hex values, type-scale numbers, and `set_variables` payloads are concrete.

**Type/name consistency:** Token names match the Reference across Phase 2 (creation) → Phases 1/3/4 (consumption): `text-prose`, `measure-prose`, `leading-prose`, `status-*-fg/-surface`. Master child-name vocabulary (Category/Title/Summary/Outcome Value/Outcome Label/CTA) consistent between Task 3.4 and Task 4.3. Board names consistent (`NN · Name · v2`).
