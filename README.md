# Capanema Design System

Source files, version history, and documentation for the **Capanema Design System** — the design system behind **capanema.io**, a personal executive portfolio and publishing platform.

The system is designed around two primary goals:

1. **Publishing high-quality long-form content** such as engineering leadership articles, technical essays, and in-depth case studies.
2. **Presenting an executive career narrative** through timelines, achievements, business outcomes, leadership impact, and measurable results.

Rather than behaving like a traditional personal website or marketing site, capanema.io is intended to read like a premium publication or annual report: calm, information-dense, highly readable, and optimized for thoughtful consumption of content.

The design language is inspired by the clarity of Linear, the craftsmanship of Stripe, and the restraint of Vercel: typography-first, near-monochrome slate with a single cobalt accent, spacious layouts, strong hierarchy, and content-first presentation.

The design system places special emphasis on:

- Long-form reading experience and reading comfort
- Information hierarchy and editorial typography
- Case-study storytelling and business outcomes
- Career timeline visualization
- Executive credibility through metrics, achievements, and highlights
- Consistent visual language across content, portfolio, and professional narrative

---

## Design Principles

The system is guided by a small set of principles that influence every design decision.

### 1. Content Over Decoration

Visual design exists to support comprehension, not compete for attention. Every element should contribute to understanding the content.

### 2. Reading Before Interaction

Reading is the primary activity. Typography, spacing, hierarchy, and pacing take precedence over interactive novelty.

### 3. Evidence Over Claims

Impact should be demonstrated through outcomes, metrics, case studies, and artifacts rather than marketing language.

### 4. Consistency Through Systems

Reusable tokens, components, and patterns create coherence across pages and future iterations.

### 5. Calm Visual Hierarchy

The interface should feel confident and restrained. Emphasis is earned through structure and contrast, not visual noise.

---

## Scope

The Capanema Design System is optimized for:

- Long-form editorial content
- Executive portfolios
- Engineering leadership writing
- Professional storytelling
- Career timelines
- Business and technology case studies
- Outcome and metric-driven narratives

It is not intended for:

- Consumer SaaS applications
- E-commerce experiences
- Marketing-heavy landing pages
- Growth-oriented conversion funnels
- Dashboard-centric products
- Enterprise application interfaces

The system prioritizes clarity, credibility, and depth over engagement mechanics or marketing patterns.

---

## Why a Separate Design System?

The design system is maintained independently from capanema.io to:

- Preserve design history across versions
- Allow experimentation without affecting implementation
- Document design decisions and rationale
- Create a reusable foundation for future publishing projects
- Establish a durable source of truth independent of any implementation technology

Separating design from implementation makes the evolution of the system visible, auditable, and easier to reason about over time.

---

## Architecture

The system is organized as a layered hierarchy:

```text
Design Principles
        ↓
Foundation
        ↓
Design Tokens
        ↓
Components
        ↓
Patterns
        ↓
Content Systems
        ↓
Portfolio & Publishing Experience
```

This structure ensures that visual decisions originate from principles and flow consistently through increasingly concrete layers of the system.

---

## Documentation

New here?

Start with this README for project context and philosophy, then continue to `DESIGN.md` for the complete design system specification.

| File | Audience | Contents |
|------|----------|----------|
| [`DESIGN.md`](./DESIGN.md) | Anyone | Complete design system specification, architecture, principles, token reference, component inventory, patterns, conventions, and version history |
| [`CLAUDE.md`](./CLAUDE.md) | Agents and contributors editing the `.pen` source | Pencil workflow, MCP access rules, board and component IDs, editing guidelines, verification procedures, and contributor instructions |

---

## Files

The design system is authored in **Pencil** (https://www.pencil.dev/), which serves as the canonical source of truth for the project.

All design work, exploration, and version history are maintained as `.pen` files. These files should be opened through Pencil for inspection, editing, comparison, and reference.

Each major version is preserved in its own file to maintain a complete historical record of the system's evolution.

| File | Contents |
|--------|----------|
| `design-system-v3.2.pen` | Current version — structural Cobalt accent, texture system, typography refinements (Red Hat Display), motion choreography, iconography, and expanded token set |
| `design-system-v3.pen` | Cobalt Deep accent introduction, radius system, and motion tokens |
| `design-system-v2.pen` | Publication-grade typography, long-form reading layer, status system, and prose tokens |
| `design-system-v1.pen` | Original Foundation → Tokens → Components → Patterns → Themes architecture |
| `accent-lab.pen` | Accent exploration workspace documenting the progression from v1–v6 concepts and the selection of Cobalt Deep |

The retired combined `design-system.pen` remains available in git history at commit `a7a241a`.

---

## Versioning Philosophy

Major design iterations are preserved as immutable snapshots.

Rather than continuously overwriting a single design file, each version captures the state of the system at a specific point in its evolution. This approach makes design decisions traceable, comparisons straightforward, and historical context easy to preserve.

The repository should be understood as both the current design system and the historical record of how it evolved.

- **v3.2** — Updated sans-serif font from Inter to Red Hat Display for improved modern typography and enhanced readability while maintaining the executive aesthetic. The monospaced font (JetBrains Mono) remains unchanged. All components automatically adapt through the font-sans token with zero structural changes needed.

---

## Relationship to Implementation

This repository defines the design language, structure, and behavior of the system, but it intentionally remains independent from implementation details.

Frameworks, rendering technologies, and deployment choices may evolve over time. The design system should remain stable and portable across those changes.

The `.pen` files are therefore considered the authoritative source, while any code implementation is an interpretation of the design system at a particular point in time.
