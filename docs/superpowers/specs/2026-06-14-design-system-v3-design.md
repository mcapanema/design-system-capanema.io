# Design System V3 — Design Spec

**Date:** 2026-06-14
**Author:** (Principal Design Systems Lead session)
**File:** `design-system-v3.pen` (a clone of `design-system-v2.pen`; V2 stays frozen for comparison)
**Status:** Approved — in build

---

## 1. Purpose

V3 is a **token-level evolution** of V2 for capanema.io (executive portfolio of a
senior technology leader). It adopts the accent direction decided in `accent-lab.pen`
and closes two system-consistency gaps V2's own governance flagged. It is an
**evolution, not a redesign**: same DNA, typography, spacing, layout, hierarchy,
components, and patterns. V2 remains the historical reference; V3 is the proposed next
version, living side-by-side as a separate file.

## 2. Scope (locked with the user)

- **Accent: color-only.** Swap to Cobalt Deep. The reduced-frequency *distribution*
  and v6 *texture* treatments explored in the lab are **out of scope** (left as V2's
  existing treatments) — they remain a future "V3.1" opportunity.
- **Add radius scale** (real primitives + re-point existing raw usages).
- **Add motion tokens** (values + documentation; no rendered animation).
- **Deliver** full artifacts: re-versioned Foundation/Tokens/Components/Patterns/Docs,
  a formal V3 Design Review, a V3 Changelog, and a V2↔V3 Side-by-Side comparison.

## 3. The accent decision (source: `accent-lab.pen`)

Funnel v1→v6 locked **Cobalt Deep `#2150B8`** (v6: "Color is decided"). Replaces prior
Tailwind blue `#2563EB`. Darker → every contrast pairing meets/exceeds prior, most AAA.

| Step | Hex | Role |
|---|---|---|
| accent-200 | `#A6C1F0` | dark link-hover |
| accent-300 | `#6E96E2` | dark links / eyebrows / accent text |
| accent-400 | `#3F6BD0` | dark borders / focus / nav |
| accent-500 | `#2150B8` | primary · CTA fill · light links |
| accent-600 | `#1A4097` | hover (light) |
| accent-700 | `#16357A` | pressed (light) |

(The lab also concluded a "balanced reduced-frequency" distribution and a texture
catalog; per user decision these are NOT applied in V3.)

## 4. New tokens

**Radius scale (locked values; snap the two oddballs):**
- `radius-sm` = 8, `radius-md` = 12, `radius-lg` = 16, `radius-pill` = 999
- Re-point existing raw radii: `12`→`radius-md`, `999`→`radius-pill`, `10`→`radius-sm` (−2px),
  `14`→`radius-lg` (+2px). All snaps ≤2px, visually negligible.

**Motion tokens:**
- `duration-fast` = 120 (ms), `duration-base` = 200, `duration-slow` = 320
- `ease-standard` = "cubic-bezier(0.2, 0, 0, 1)", `ease-emphasized` = "cubic-bezier(0.3, 0, 0, 1)"
- (Stored as number/string variables; documented in Foundation/Tokens/Docs.)

## 5. Boards

| Board | Action |
|---|---|
| `01 · Foundation · v3` | Re-version; Color→Cobalt; add Radius + Motion sections |
| `02 · Tokens · v3` | Re-version; add radius + motion token tables; accent re-resolves |
| `03 · Components · v3` | Re-version cover/governance (visuals auto via tokens) |
| `04 · Patterns · v3` | Re-version cover/governance (visuals auto via tokens) |
| `05 · Design Review · v3` | Formal V3 review (Deliverable 2) |
| `06 · Documentation · v3` | Re-version; add Radius + Motion architecture |
| `07 · Changelog · v3` | NEW (Deliverable 3): Added/Changed/Improved/Deprecated/Removed + why/impact |
| `08 · V2 ↔ V3 Comparison` | NEW (Deliverable 4): side-by-side across all layers |

## 6. Build process

Derive from clone. One section at a time: `snapshot_layout(problemsOnly:true)` →
screenshot smallest meaningful node → fix warnings next batch → never delete-and-recreate.
Redefine batch helpers inline each call. Set themed variables with BOTH Light+Dark
entries. The user must Save in Pencil before any git commit (MCP edits the live doc only).

## 7. Out of scope / future (V3.1+)

- Reduced-frequency accent distribution (tags neutral, metrics ink, accent reserved).
- v6 accent texture treatments (eyebrow bars, section ticks, etc.).
- Type-scale tokens (`font-size-*`) and breakpoint tokens.
- Implementing capanema.io in code from the blueprints.
