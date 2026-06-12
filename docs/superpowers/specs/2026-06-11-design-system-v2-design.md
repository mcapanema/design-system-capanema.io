# Capanema Design System v2.0 — Design Spec

**Date:** 2026-06-11
**Status:** Approved direction (pre-plan)
**File under design:** `design-system.pen` (Pencil MCP only — never Read/Edit directly)

---

## 1. Overview

Evolve the capanema.io design system from its current state (**v1.1**, not v1.0 as the
original brief assumed — dual Light/Dark theming is already built on board `06 · Themes`) to
**v2.0** through a critical, senior-level audit followed by targeted refinement.

The system supports **capanema.io**, the executive portfolio of a senior technology leader
(CTO / engineering executive / data-platform & AI practitioner). The site's job: help a
visitor quickly grasp who the person is, what problems they've solved, at what scale, how they
think, and what measurable outcomes they delivered. Primary content: case studies, leadership
writing, resume, credibility.

v2.0 is a **quality** evolution, not a compliance pass and not a reinvention. The target
register is **Linear + Stripe + Vercel**: quiet confidence, premium, technical, mature, highly
readable — a premium executive digital *publication* system, not a marketing site, startup
landing page, blog, or designer portfolio.

## 2. Constraints (locked decisions)

1. **Refine within the DNA.** Brand constants are fixed: near-monochrome **cool slate**
   neutrals + a **single blue accent** (`accent-500` `#2563EB`), **Inter** (sans) +
   **JetBrains Mono** (mono), the **8-pt spacing** scale, and subtle executive elevation.
   v2 elevates quality and fixes defects but does **not** change palette hue, typefaces, or
   spacing base.
2. **Parallel v2 board set; v1.1 frozen.** The five existing boards (`01`–`04`, `06 · Themes`)
   and `⟐ Component Masters` are preserved untouched for side-by-side comparison. v2 is a new
   board column built to the right, with a new masters library carrying **fresh component IDs**
   so existing v1.1 instances never break.
3. **Audit-driven scope.** A layer gets a v2 board only where the audit finds meaningful
   improvement. Anything judged already excellent stays as-is and is documented on board 05
   with the rationale for leaving it unchanged.
4. **Dark mode is folded into the v2 boards**, not kept as a separate board. The existing
   `Mode: Light | Dark` theme-axis architecture is carried forward and refined; theming is a
   property of the tokens/components, demonstrated inline.
5. **Content stays executive sample copy** (realistic but not the real capanema.io bio).
   Headlines follow the existing rule: **outcome-oriented**, lead with impact/scale, technology
   is the *how* never the headline.

## 3. Versioning & canvas model

- **Frozen (never edited):** `01 · Foundation` (`bsXHN`), `02 · Tokens` (`gzBR6`),
  `03 · Components` (`Dny8s`), `04 · Patterns` (`jvEua`), `06 · Themes` (`mO19J`),
  `⟐ Component Masters` (`VX0oF`).
- **New v2 boards** (built left→right via `FindEmptySpace`, same board anatomy as v1.1 —
  dark cover, numbered sections, governance footer):
  - `01 · Foundation · v2`
  - `02 · Tokens · v2`
  - `03 · Components · v2`
  - `04 · Patterns · v2`
  - `⟐ Component Masters · v2` (new reusable masters with new IDs)
  - `05 · Design Review & Changelog` (new — the audit + change record)
- A layer's v2 board is only created if the audit warrants it. Current expectation: all four
  layers warrant a v2; this is proven per-layer during the audit phase before building.

## 4. Audit framework (Phase 0 — documented on board 05)

A structured critical pass against the nine review lenses (visual identity, typography, tokens,
components, patterns, content hierarchy, executive credibility, accessibility, governance).
Each finding is severity-tagged: **Defect** (broken/contradictory), **Weakness** (works but
below bar), **Opportunity** (could be elevated). The audit is performed at section-level zoom
(`get_screenshot` on specific section frames, `batch_get(resolveVariables:true)` for real
values) and written up on board 05 *before* the corresponding redesign.

### Findings already confirmed (carry into board 05)

- **D1 — Orphaned conflicting token system.** The variable table contains a second,
  undocumented shadcn-style brand (`--background` `#F2F3F0` bone, `--primary` `#FF8400` orange,
  `--destructive` `#D93C15`, fonts `JetBrains Mono` + `Geist`, `--radius-m` 16, pill radius)
  alongside the documented slate+blue+Inter system. The live boards render the slate+blue
  system; the orange/bone/Geist set is unused but resolvable, so a contributor pulling
  `--primary` gets orange while `action-primary` gets blue. **Resolution:** remove or quarantine
  the orphaned set in Tokens v2; do not let it leak into v2 components/patterns.
- **W1 — Status/feedback colors are off-brand and orphaned.** `--color-success/-error/-warning
  /-info` and `--destructive` exist only inside the orphaned set with values that don't match
  the slate+blue language. **Resolution:** define proper success/error/warning/info semantic
  tokens in the slate+blue palette, themed for Light/Dark, in Tokens v2.

### Findings to assess during Phase 0 (open until reviewed at section zoom)

- Typography: hierarchy strength, line-heights, long-form reading rhythm, display letter-spacing.
- Card architecture, especially the **Case Study Card** (the primary discovery surface).
- Pattern hierarchy: does the hero/homepage prioritize case studies and establish credibility
  fast? Resume presentation. Long-form reading layout for case-study detail & writing.
- Accessibility: re-verify WCAG AA on any changed tokens/components.

## 5. Per-layer v2 scope

Brand constants hold throughout. Specific, confident improvements are listed; items marked
*(audit-gated)* are confirmed/sized during Phase 0.

### 5.1 Foundation v2

- **Typography system** (strongest-aspect target):
  - Explicit **line-height per type step** documented on the scale.
  - A dedicated **long-form "prose" reading style**: constrained measure (~66ch / ~680px),
    generous leading (~1.6–1.7), tuned for case-study and writing body text.
  - **Display letter-spacing** tightening on the large display sizes for an editorial feel.
  - **Section-rhythm** spec (vertical spacing cadence between page sections).
- **Color:** keep slate+blue; surface the cleaned palette (no orphaned set). Add any missing
  neutral/surface steps the component work needs *(audit-gated)*.
- **Spacing & elevation:** retain 8-pt scale and subtle elevation; document refinements only if
  the audit finds gaps *(audit-gated)*.

### 5.2 Tokens v2

- **Purge/quarantine** the orphaned shadcn token set (D1).
- **Status tokens:** success / error / warning / info, each with foreground + surface, themed
  Light/Dark, in the slate+blue language (W1).
- **Prose/reading tokens** to back the long-form reading style (measure, leading).
- Tighten semantic naming and the governance documentation (clear Foundation → Token →
  Component → Pattern → Page chain).
- **Theming rule preserved:** every themed variable written with BOTH explicit
  `{Mode:Light}` and `{Mode:Dark}` entries; verify with `batch_get(resolveVariables:true)` on a
  light-context node (per the known `set_variables` gotcha).

### 5.3 Components v2 (new masters library, new IDs)

Refine the existing 10 masters; add new ones only where they earn their place.

- **Refine:** Button, Icon Button, Nav Item, Tag, Metric, **Case Study Card (priority — the
  discovery engine)**, Article Card, Highlight Card, Metric Card, Timeline Item, Footer.
- **Likely additions (each justified, not decorative):** Blockquote / Pullquote, Callout /
  Aside, Table-of-Contents rail, Breadcrumb, Credibility / logo strip. Final set confirmed by
  the audit's component-needs analysis from the pattern work.
- All components consume **semantic tokens only** (never raw hex, never the orphaned set), and
  inherit theme-safety rules (e.g. labels on accent fills use `$text-on-accent`; always-dark
  surfaces use the on-dark token family).

### 5.4 Patterns v2 (the most important layer)

- **Executive hero** that states positioning with quiet confidence and surfaces credibility
  fast.
- **Case-study discovery model:** a featured case study + an indexed/scannable list that guides
  attention to outcomes and scale.
- **Long-form reading layout** for case-study detail and writing (prose measure, optional
  table-of-contents rail, pullquotes, callouts).
- **Rebuilt resume / experience presentation** tuned for executive credibility (impact and
  scale legible at a glance).
- **Contact** pattern, refined.
- **Page-layout blueprints** (Homepage, Case Study, Writing, Resume) updated to match the new
  patterns and section order.

### 5.5 Board 05 · Design Review & Changelog

Sections: **Executive Summary · Major Findings** (severity-tagged) **· What Changed · Why It
Changed · Before ↔ After Analysis · Design Principles Reinforced · Future Recommendations.**
Built in the house doc style (numbered `## NN` headers, doc panels). Includes the rationale for
any v1.1 section deliberately left unchanged.

## 6. Execution phasing (dependency order)

Each layer references the one above it, so build top-down and verify before proceeding:

1. **Phase 0 — Audit.** Section-zoom review of all v1.1 boards; write findings (feeds board 05).
2. **Foundation v2.**
3. **Tokens v2** (depends on Foundation).
4. **Components v2 + masters** (depends on Tokens).
5. **Patterns v2 + page blueprints** (depends on Components).
6. **Board 05** write-up (synthesizes the whole effort).
7. **Docs:** update `CLAUDE.md` and the Claude memory (`pencil-ds-foundation.md`) to reflect v2
   board/master IDs and the resolved token system.

Verification after each section: `snapshot_layout(parentId, problemsOnly:true)` → "No layout
problems", then `get_screenshot` of the smallest meaningful node. Fix `batch_design` warnings in
the next call; never delete-and-recreate to fix — update existing nodes.

## 7. Success criteria

- A Linear/Stripe/Vercel designer would recognize discipline, restraint, precision,
  sophistication, and systems thinking.
- The system reads as **"I build organizations and systems that scale"** without saying it.
- Typography is publication-grade for long-form reading.
- Case studies are the clear hero of content discovery; resume reads as executive-credible.
- Token system is single-sourced (no conflicting brand), themable, and governed.
- WCAG AA verified on all changed tokens/components.
- v1.1 remains fully intact for comparison.

## 8. Out of scope

- Changing brand hue, typefaces, or the 8-pt spacing base.
- Implementing capanema.io in code (separate future effort).
- Real biographical content (sample executive copy only).
- Motion/responsive/iconography boards (possible future `05+`-style work; not this version).
- Editing or migrating any v1.1 board.

## 9. Open items resolved during execution (not blockers)

- Exact final set of new components (confirmed by Phase 0 component-needs analysis).
- Which v1.1 layers, if any, are left unchanged (expected: none, but proven per-layer).
- Any added Foundation neutral/surface steps (added only if component work needs them).
