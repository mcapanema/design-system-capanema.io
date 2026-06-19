# Red Hat Display Compatibility Review — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Audit all 8 boards of `design-system-v3.2.pen` to verify that Red Hat Display renders correctly across every dimension — typography metrics, spacing rhythm, elevation, motion, and component layouts — and apply all necessary adjustments directly in Pencil.

**Architecture:** The font change from Inter to Red Hat Display was applied by updating the `$font-sans` token value. All 17 component masters and all board text nodes that reference `$font-sans` inherit the new font automatically. However, Red Hat Display has different optical metrics (wider character set, taller ascenders, display-optimized shaping) that require explicit letter-spacing, line-height, and weight calibration not captured by the token swap alone.

**Tech Stack:** Pencil MCP (`get_editor_state`, `batch_get`, `batch_design`, `get_screenshot`, `snapshot_layout`, `get_variables`, `set_variables`). No code changes — this is a pure Pencil design-tool task.

## Global Constraints

- Active file: `design-system-v3.2.pen` — must be open in Pencil before any MCP call.
- Call `get_editor_state(include_schema: true)` at the start of a new session to reload schema.
- `.pen` files are encrypted — never use Read/Grep/Edit on them; only Pencil MCP tools.
- Verify work per section: `snapshot_layout(parentId, problemsOnly: true)` → "No layout problems" + `get_screenshot` of the section frame.
- Fix nodes in-place with `batch_design`; never delete-and-recreate.
- All changes must stay within the four-layer architecture: Foundation → Tokens → Components → Patterns.
- Board IDs: Foundation `hmZsC` · Tokens `IF92z` · Component Masters `J6aQ6v` · Components `fEH2K` · Patterns `M7VtG` · Design Review `ch8VD` · Documentation `NnWw8` · Changelog `hNrUG` · Comparison `HuQip`.

---

## Red Hat Display vs Inter — Key Metric Differences

Understanding these differences drives every task in this plan:

| Dimension | Inter | Red Hat Display | Impact |
|---|---|---|---|
| Optimized range | 8–72px | 20–72px (display-first) | Caption/body-s may look softer |
| x-height ratio | ~0.55 | ~0.52 | Slightly smaller effective body; may need more leading |
| Character width | Compressed | Slightly wider | Line wraps earlier; fixed containers risk clipping |
| Letter-spacing at display | Optically compensated | Geometric — needs explicit tracking | display-xl/l/m look loose without -0.02em tracking |
| Weight at body | 400 reads solid | 400 reads lighter; 500 is body weight | Bump body text to weight 500 |
| Mono pairing | Inter + JetBrains (humanist + mono) | Red Hat Display + JetBrains (geometric + mono) | Stronger contrast — deliberate, no fix needed |

---

## Phase 1 — Token & Deprecated-Reference Audit

### Task 1: Verify Font Token Correctness

**Files (Pencil nodes):**
- Inspect: `font-sans` and `font-mono` variable values
- Inspect: deprecated `--font-primary` / `--font-secondary` shadcn tokens

- [ ] **Step 1: Confirm canonical font tokens**

  Call `get_variables` and verify:
  - `font-sans` = `"Red Hat Display"` ✅
  - `font-mono` = `"JetBrains Mono"` ✅

  If either is wrong, call `set_variables` to correct it.

- [ ] **Step 2: Flag the deprecated token inversion**

  Observe that `--font-primary` = `"JetBrains Mono"` and `--font-secondary` = `"Red Hat Display"`. These names are inverted from conventional use (primary = body font). Since DESIGN.md §4 marks all `--`-prefixed tokens as deprecated, **do not change their values** — just confirm no component master references `--font-primary` directly by checking a batch_get on each master root.

- [ ] **Step 3: Audit Component Masters for deprecated font references**

  Call `batch_get` on all 17 master root IDs: `WuNSb`, `GPwQ9`, `M8kBKi`, `UsMEV`, `T2HLO`, `N323Wn`, `xAD0x`, `LA0Vr`, `afs6z`, `BtcCZ`, `WGvhC`, `oV5N6`, `WlpMe`, `Br6Rv`, `ahkiC`, `xTEMM`, `JkvM5`.
  
  Scan the returned node trees for any `fontFamily` value of `"--font-primary"`, `"JetBrains Mono"` (on non-mono nodes), or hardcoded `"Inter"`.
  
  If any are found, fix them with `batch_design` to use `"$font-sans"`.

- [ ] **Step 4: Commit the audit result**

  Note any fixes applied and move to Phase 2.

---

## Phase 2 — Foundation Board: Typography Section

### Task 2: Type Scale Screenshot & Visual Assessment

**Files (Pencil nodes):** Foundation board `hmZsC` — Typography section (the `## 02 Typography` section frame)

- [ ] **Step 1: Screenshot the full typography section**

  Call `get_screenshot` on the Foundation board `hmZsC` scoped to the typography section frame. Save the visual mentally as the baseline.

- [ ] **Step 2: Assess display sizes (72, 64, 56px) for letter-spacing**

  Review the screenshot against this criterion (from impeccable skill):
  - Display heading letter-spacing floor: ≥ −0.04em. Anything tighter and letters touch.
  - Red Hat Display at 72px with default (0) tracking typically looks **too loose** — it's a geometric font, not optically compensated like Inter.
  
  Expected finding: display-xl (72px), display-l (64px), display-m (56px) will look slightly loose without explicit negative tracking.

- [ ] **Step 3: Assess heading sizes (48, 40, 32px) for letter-spacing**

  h1 (48px) and h2 (40px) likely need −0.01em. h3 (32px) is borderline — check visually.

- [ ] **Step 4: Assess body and caption sizes (18, 16, 14, 12px) for rendering quality**

  Key criterion: Red Hat Display was designed for display use; at 12–14px it shapes less sharply than Inter.
  - caption (12px): check if text looks readable or soft/blurry
  - body-s (14px): check visual weight — should feel legible at this size
  - body-m (16px) and body-l (18px): check leading (`leading-normal` 1.6) — may need bump to 1.65 given taller ascenders

- [ ] **Step 5: Assess line heights across the scale**

  With Red Hat Display's slightly taller ascenders:
  - `leading-tight` 1.1 at 72px: lines may feel cramped — check if ascenders/descenders clip
  - `leading-snug` 1.3 at h2/h3: should be fine
  - `leading-normal` 1.6 at body: verify prose feels open enough

- [ ] **Step 6: Document all findings before making any changes**

  List every step with its finding: OK / needs-letter-spacing / needs-leading / needs-weight-bump.

---

### Task 3: Add Letter-Spacing Tokens (if needed)

**Files (Pencil nodes):** Variables system via `set_variables`; Foundation board typography section

Prerequisite: Task 2 findings list.

- [ ] **Step 1: Decide on token vs. inline approach**

  The current variable system has no `letter-spacing` tokens. Two options:
  - **Option A (token):** Add `letter-spacing-display-xl`, `letter-spacing-display-l`, etc. as `number` tokens (in em × 1000, e.g. −20 for −0.02em). More system-correct.
  - **Option B (inline):** Apply letter-spacing directly on specific text nodes in showcase frames and component masters.
  
  **Recommendation:** Use Option A for display steps (xl, l, m, h1) where the value is systematic; use Option B for one-off callouts like covers.

- [ ] **Step 2: Add letter-spacing tokens via set_variables**

  If Option A chosen, add these variables (em values as numbers, multiply by font-size for px):
  ```json
  {
    "letter-spacing-display": { "type": "number", "value": -20 },
    "letter-spacing-heading": { "type": "number", "value": -10 },
    "letter-spacing-body": { "type": "number", "value": 0 },
    "letter-spacing-caption": { "type": "number", "value": 10 }
  }
  ```
  Note: Pencil's `letterSpacing` property accepts px values. Convert: at 72px, −0.02em = −1.44px ≈ −1.5px.

- [ ] **Step 3: Apply letter-spacing to display text nodes in typography showcase**

  Use `batch_design` to update the display size text nodes in the Foundation board's type scale showcase. Recommended values (in px):
  - 72px (display-xl): `letterSpacing: -1.5`
  - 64px (display-l): `letterSpacing: -1.3`
  - 56px (display-m): `letterSpacing: -1.1`
  - 48px (h1): `letterSpacing: -0.5`
  - 40px (h2): `letterSpacing: -0.4`
  - 32px (h3): `letterSpacing: 0` (optional −0.3)
  - 24px (h4): `letterSpacing: 0`
  - 20px (h5): `letterSpacing: 0`
  - 18px (body-l): `letterSpacing: 0`
  - 16px (body-m): `letterSpacing: 0`
  - 14px (body-s): `letterSpacing: 0`
  - 12px (caption): `letterSpacing: 0.1` (slightly open at small sizes)

- [ ] **Step 4: Adjust weight at body sizes if needed**

  If body-m/body-s look visually lighter than intended (Red Hat Display 400 reads lighter than Inter 400), update body text nodes in showcases to `fontWeight: 500`. Note this as a recommendation in the documentation panel.

- [ ] **Step 5: Verify with screenshot**

  Call `get_screenshot` on the updated typography section. Compare with Step 1 baseline. The display text should now feel crisp and optically tight; body text should feel equally solid.

---

### Task 4: Foundation Board — Spacing Section

**Files (Pencil nodes):** Foundation board `hmZsC` — Spacing section

- [ ] **Step 1: Screenshot the spacing section**

  Call `get_screenshot` on the spacing section frame.

- [ ] **Step 2: Check spacing labels and annotations**

  Red Hat Display is slightly wider than Inter. Verify that:
  - Spacing labels (e.g., "space-5 / 24px") don't overflow their annotation containers
  - The 8pt grid tick marks and their labels are still visually aligned
  - Any inline examples of spacing still look proportionally correct

- [ ] **Step 3: Fix any overflow or misalignment**

  If text overflows fixed-width label containers, use `batch_design` to either: widen the container, reduce font size to the next scale step, or switch to a smaller weight.

- [ ] **Step 4: Verify layout**

  Call `snapshot_layout(problemsOnly: true)` on the spacing section. Expected: "No layout problems".

---

### Task 5: Foundation Board — Elevation Section

**Files (Pencil nodes):** Foundation board `hmZsC` — Elevation section

- [ ] **Step 1: Screenshot the elevation section**

  Call `get_screenshot` on the elevation section frame.

- [ ] **Step 2: Assess shadow + type interaction**

  Elevation tokens are unchanged: `shadow-1a` and `shadow-1b` with low alpha. With a new font, the visual weight of the text label inside a card changes. Verify:
  - Level 0 (flat): card text reads cleanly without shadow distraction ✓
  - Level 1 (y1·3px·8%): subtle lift still registers against the new type weight
  - Level 2 (y4·12px·12%): hover state looks appropriately elevated
  - Level 3 (y12·24px·16%): overlay level still reads as clearly "above" the page
  
  Finding: Shadows are independent of font; this step should pass cleanly. Note any discrepancy.

- [ ] **Step 3: No changes expected — document result**

  Elevation section is expected to pass without changes. If issues found, adjust shadow alpha slightly (±0.02) using `set_variables` on `shadow-1a` or `shadow-1b`.

---

### Task 6: Foundation Board — Motion & Responsive Sections

**Files (Pencil nodes):** Foundation board `hmZsC` — Motion and Responsive sections

- [ ] **Step 1: Screenshot motion and responsive sections**

  Call `get_screenshot` on each section frame.

- [ ] **Step 2: Motion section check**

  Motion tokens (`duration-fast` 120ms, `duration-base` 200ms, `duration-slow` 320ms; easing curves) are unchanged and font-independent. The motion choreography spec is a text table — verify:
  - Table text is legible with Red Hat Display
  - Column labels and interaction names don't wrap awkwardly with the slightly wider font
  
  Fix any wrapping issues with `batch_design` (increase container width or reduce font size to body-s).

- [ ] **Step 3: Responsive section check**

  Breakpoint tokens (`sm` 640 · `md` 768 · `lg` 1024 · `xl` 1280) are unchanged. Verify that responsive documentation labels and annotations render cleanly with Red Hat Display.

---

## Phase 3 — Tokens Board

### Task 7: Tokens Board Full Audit

**Files (Pencil nodes):** Tokens board `IF92z`

- [ ] **Step 1: Screenshot the tokens board**

  Call `get_screenshot` on board `IF92z`.

- [ ] **Step 2: Check the Type Scale table (## 07)**

  This table documents all 12 font-size steps. With Red Hat Display:
  - Verify column widths accommodate wider text
  - Verify the "Example" column renders with Red Hat Display at each size
  - Verify the "Font" column correctly says "Red Hat Display" (not "Inter")

- [ ] **Step 3: Check the Icon table (## 10)**

  Icon tokens (`icon-sm` 16, `icon-md` 20, `icon-lg` 24) are unchanged. Verify labels render correctly.

- [ ] **Step 4: Fix any "Inter" references remaining**

  Search the tokens board for any hardcoded "Inter" text (in documentation panels or example text). Replace with "Red Hat Display" using `batch_design` on the specific text node.

- [ ] **Step 5: Verify layout**

  Call `snapshot_layout(problemsOnly: true)` on board `IF92z`. Expected: "No layout problems".

---

## Phase 4 — Component Masters

### Task 8: Text-Heavy Component Masters Audit

These masters carry the most risk: multi-line body text, small labels, and fixed-width containers.

**Files (Pencil nodes):** Component Masters board `J6aQ6v` — all 17 masters

- [ ] **Step 1: Screenshot all component masters**

  Call `get_screenshot` on the Component Masters board `J6aQ6v`.

- [ ] **Step 2: Review Pullquote (oV5N6)**

  Pullquote renders a quote at display/h1 size. With Red Hat Display, verify:
  - Quote text has appropriate letter-spacing (should be negative at large sizes per Task 3)
  - The quote attribution line (smaller text) is legible
  - Multi-line wrapping within the master looks balanced (`text-wrap: balance` equivalent)

  Fix with `batch_design` on child node `giEAq` (Quote) to apply letter-spacing.

- [ ] **Step 3: Review Case Study Card (BtcCZ)**

  Category label (`auz5R`) is now `text-tertiary` (neutral ink). Title (`In95P`) is h4/h3 size. Summary (`p3IzlV`) is body-m/s multi-line. Verify:
  - Title doesn't overflow the card width with Red Hat Display's slightly wider characters
  - Summary wraps cleanly (should not exceed 3 lines)
  - Category label at small size still reads clearly

- [ ] **Step 4: Review Article Card (WGvhC)**

  Title (`vdP6J`) and Excerpt (`UEIi6`) are the key multi-line nodes. Verify:
  - Excerpt at body-s (14px) renders legibly with Red Hat Display's display-optimized shaping
  - ReadTime (`L5b01`) label at caption (12px) — this is the highest-risk small size

- [ ] **Step 5: Review Timeline Item (afs6z)**

  Description (`kGlXz`) is body-m multi-line prose. Verify:
  - Multi-line body text in `leading-normal` 1.6 reads well with Red Hat Display
  - Date (`hnFRC`) at caption size (12px) — check for legibility
  - Outcome text at 13px (off-scale literal) — check rendering

- [ ] **Step 6: Review Callout (WlpMe)**

  Text node `srHuJ` is body-s (14px) prose inside a colored surface. Verify:
  - Body-s text at weight 400 reads legibly with Red Hat Display on a tinted surface
  - If needed, bump to weight 500 for small-text legibility

- [ ] **Step 7: Review remaining masters (Button, Nav Item, Tag, Metric, etc.)**

  These carry short labels (single line, ≥14px). Check for:
  - Button label (`oDdYq`): no overflow in default width
  - Nav Item label (`OF1pw`): single line, no concern
  - Tag label (`v9dYgV`): at ~12px, check legibility
  - Metric value (`HF1zg`): large number — apply negative letter-spacing if display-sized
  - Eyebrow label (`YwOGh`): uses `$font-mono` (JetBrains Mono) — no change needed

- [ ] **Step 8: Apply letter-spacing fixes to masters**

  For any master with display-size text (Metric value, Pullquote quote), apply the same letter-spacing values from Task 3 using `batch_design` on the specific child text nodes.

- [ ] **Step 9: Verify all masters with snapshot**

  Call `snapshot_layout(parentId: "J6aQ6v", problemsOnly: true)`. Expected: "No layout problems". Screenshot for visual confirmation.

---

## Phase 5 — Components & Patterns Boards

### Task 9: Components Board Instance Review

**Files (Pencil nodes):** Components board `fEH2K`

- [ ] **Step 1: Screenshot the components board**

  Call `get_screenshot` on board `fEH2K`.

- [ ] **Step 2: Check component instances for text overflow**

  Red Hat Display is slightly wider; any fixed-width component instance containers may clip. Verify:
  - Button instances at various content lengths
  - Tag instances (smallest text)
  - Card instances (fixed card widths with multi-line text)

- [ ] **Step 3: Fix any clipping or overflow**

  Use `batch_design` to widen fixed containers or reduce text content to fit. Do NOT shrink font sizes — maintain the type scale.

- [ ] **Step 4: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot` on the board. Expected: clean.

---

### Task 10: Patterns Board — Section Layout & Headline Review

**Files (Pencil nodes):** Patterns board `M7VtG` — all section patterns and page blueprints

**This is the highest-impact board** — pattern layouts define how the site will look.

- [ ] **Step 1: Screenshot the full patterns board**

  Call `get_screenshot` on board `M7VtG`.

- [ ] **Step 2: Audit the Hero section (FY2K8)**

  The Hero carries the display-xl headline (72px). With Red Hat Display:
  - Headline must have letter-spacing applied (−1.5px, from Task 3)
  - The Eyebrow instance above the headline uses JetBrains Mono — contrast with Red Hat Display headline is a deliberate pairing choice; verify it reads well
  - Subtitle text at body-l (18px) — verify leading and weight
  - CTA button label — verify no overflow

  Apply letter-spacing fix to the Hero headline text node via `batch_design`.

- [ ] **Step 3: Audit the Case Study Detail section (b8yr0)**

  Case Study title at h1/h2 (48/40px). Verify:
  - Title letter-spacing applied per Task 3 values
  - Body prose text at `leading-prose` 1.7 — verify it reads comfortably with Red Hat Display
  - `measure-prose` 680px line length — with Red Hat Display's slightly wider characters, approximately the same character count per line. Check that prose columns still feel like 65–75ch (impeccable criterion).

- [ ] **Step 4: Audit all other section patterns**

  For each section pattern on board M7VtG:
  - Screenshot the section
  - Check headline size for appropriate letter-spacing
  - Check body text for leading and weight
  - Check small labels (categories, dates, tags) for 12–14px legibility
  - Fix any issues via `batch_design` on the specific text nodes

- [ ] **Step 5: Verify prose column measure**

  The `measure-prose` token is 680px. Red Hat Display at 16px with slightly wider characters renders approximately 68–72 characters per line — comfortably within the 65–75ch cap from impeccable. No change needed, but confirm visually.

- [ ] **Step 6: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot` on board `M7VtG`. Expected: "No layout problems".

---

## Phase 6 — Dark Mode Verification

### Task 11: Dark Mode Rendering Check

**Files (Pencil nodes):** Foundation board `hmZsC`, Components board `fEH2K`, Patterns board `M7VtG` — dark-mode framed sections

- [ ] **Step 1: Screenshot Foundation board's dark-mode showcase**

  The Foundation board has a cover section using `surface-dark`. Call `get_screenshot` on the cover frame. With Red Hat Display on a dark surface (`text-on-dark` = neutral-50), verify:
  - Display text remains crisp and not ghosted
  - White text on near-black surface has sufficient contrast (neutral-50 on dark-900 = ≈ 18.2:1, well above 7:1) ✅
  - Letter-spacing still feels right on the dark background

- [ ] **Step 2: Screenshot a dark-mode-wrapped section in Patterns**

  If any pattern sections use `theme: {Mode: "Dark"}` wrapping frames, screenshot them. Verify Red Hat Display renders as well in dark mode as light mode.

- [ ] **Step 3: Verify Highlight Card dark rendering**

  The Highlight Card (`LA0Vr`) uses `$text-on-dark` and `$text-on-dark-muted` (always-dark surface). Verify:
  - Title and description text render cleanly with Red Hat Display on `surface-dark-raised`
  - No contrast or legibility issues

---

## Phase 7 — Documentation, Changelog & Comparison Boards

### Task 12: Documentation Board Audit

**Files (Pencil nodes):** Documentation board `NnWw8`

- [ ] **Step 1: Screenshot the documentation board**

  Call `get_screenshot` on board `NnWw8`.

- [ ] **Step 2: Search for any remaining "Inter" references**

  Use `batch_get` on the board's text nodes to find any hardcoded "Inter" mentions. The font changed in v3.2 — documentation should say "Red Hat Display".

- [ ] **Step 3: Update any stale font references**

  If any documentation panel says "Inter", update with `batch_design` to say "Red Hat Display". Also update any font stack examples or CSS snippets in the documentation.

- [ ] **Step 4: Add letter-spacing guidance to type scale documentation**

  In the Type Scale section documentation panel, add a note about recommended letter-spacing values for Red Hat Display at display sizes (the values established in Task 3). This serves as the canonical reference for code implementation.

- [ ] **Step 5: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot`.

---

### Task 13: Changelog Board Verification

**Files (Pencil nodes):** Changelog board `hNrUG`

- [ ] **Step 1: Screenshot the changelog board**

  Call `get_screenshot` on board `hNrUG`.

- [ ] **Step 2: Verify v3.2 entry is accurate**

  The v3.2 changelog entry should describe the font change from Inter to Red Hat Display. It should NOT say "zero structural changes needed" without qualification — the letter-spacing and leading adjustments from this review are structural changes at the token/component level.

- [ ] **Step 3: Update the v3.2 changelog entry**

  Use `batch_design` to update the v3.2 Added/Changed section to include:
  - `font-sans` token updated to Red Hat Display
  - Letter-spacing values added for display/heading sizes
  - Component masters updated with font-metric adjustments
  - Body text weight recommendation updated to 500

- [ ] **Step 4: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot`.

---

### Task 14: V3.1 → V3.2 Comparison Board Verification

**Files (Pencil nodes):** Comparison board `HuQip`

- [ ] **Step 1: Screenshot the comparison board**

  Call `get_screenshot` on board `HuQip`.

- [ ] **Step 2: Verify the before/after content is still accurate**

  The comparison should show:
  - v3.1: `font-sans = Inter`
  - v3.2: `font-sans = Red Hat Display`
  - Plus any letter-spacing/leading adjustments established in this review

- [ ] **Step 3: Update comparison board if needed**

  If the review in this plan reveals additional changes beyond the initial font token swap (which is expected), update the comparison board to document them accurately via `batch_design`.

- [ ] **Step 4: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot`.

---

## Phase 8 — Design Review Board Update

### Task 15: Design Review Scorecard Update

**Files (Pencil nodes):** Design Review board `ch8VD`

- [ ] **Step 1: Screenshot the design review board**

  Call `get_screenshot` on board `ch8VD`.

- [ ] **Step 2: Review the Typography dimension in the scorecard**

  The 11-dimension scorecard should be updated to reflect:
  - v3.2 font change to Red Hat Display
  - The letter-spacing and leading calibration work from this review
  - Any new risks identified (small-size rendering at 12–14px)

- [ ] **Step 3: Update the executive summary and open risks**

  If small-size rendering (caption at 12px) remains a concern after Task 8, add it as an Open Risk. Red Hat Display at 12px is not its optimal size — this is a documented design tradeoff worth noting for future code implementation.

- [ ] **Step 4: Verify layout and screenshot**

  `snapshot_layout(problemsOnly: true)` + `get_screenshot`.

---

## Phase 9 — Final Verification & Save

### Task 16: End-to-End Final Check

- [ ] **Step 1: Full board screenshots**

  Take `get_screenshot` of each of the 8 boards. Review all in sequence for visual consistency — the system should look cohesive with Red Hat Display across every board.

- [ ] **Step 2: Cross-board consistency check**

  Verify:
  - Same letter-spacing values used consistently across Foundation showcase, Component Masters, and Patterns
  - Body text weight is consistent (400 or 500 — pick one and apply uniformly)
  - Caption (12px) text: if it looks noticeably softer than the rest of the scale, document as a known limitation in the Documentation board (not a bug — Red Hat Display's design constraint)

- [ ] **Step 3: Save the file in Pencil**

  CRITICAL: Pencil MCP edits a live in-editor document. Changes are NOT written to disk until you manually Save in Pencil (⌘S or File → Save). Do this before committing to git.

- [ ] **Step 4: Commit**

  After saving:
  ```bash
  git add design-system-v3.2.pen
  git commit -m "feat: calibrate Red Hat Display metrics across all v3.2 boards

  - Add letter-spacing adjustments for display/heading sizes
  - Verify and confirm leading values (tight/snug/normal/prose)
  - Update component masters for font-metric compatibility
  - Audit and fix small-size (12-14px) text rendering
  - Update changelog, comparison, and design review boards"
  ```

---

## Self-Review Checklist

| Area | Covered by task | Status |
|---|---|---|
| `font-sans` token correctness | Task 1 | — |
| Deprecated `--font-primary` audit | Task 1 | — |
| Display letter-spacing | Tasks 2, 3 | — |
| Heading letter-spacing | Task 3 | — |
| Body/caption weight & leading | Tasks 2, 3 | — |
| Spacing section compatibility | Task 4 | — |
| Elevation visual verification | Task 5 | — |
| Motion documentation readability | Task 6 | — |
| Tokens board — stale "Inter" refs | Task 7 | — |
| All 17 component masters | Task 8 | — |
| Components board instances | Task 9 | — |
| Patterns — hero headline tracking | Task 10 | — |
| Patterns — prose measure | Task 10 | — |
| Dark mode rendering | Task 11 | — |
| Documentation board — stale refs | Task 12 | — |
| Letter-spacing guidance added | Task 12 | — |
| Changelog accuracy | Task 13 | — |
| Comparison board accuracy | Task 14 | — |
| Design review scorecard | Task 15 | — |
| File saved in Pencil | Task 16 | — |
| Git commit | Task 16 | — |
