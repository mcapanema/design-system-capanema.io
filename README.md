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
