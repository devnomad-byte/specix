---
name: canvas
description: Use when explicitly building frontend components or pages — design thinking with distinctive aesthetics guidance, triggered only for UI work
---

# Canvas: Frontend Design

**Module type:** Clay (flexible)
**Trigger:** Only when the user explicitly requests frontend component or page construction.
**Alias prefix:** `specix:canvas`

---

## Purpose

Canvas prevents the most common failure mode of AI-generated frontend: every output looking like the same bland, corporate, gradient-heavy, purple-accented default. Canvas forces deliberate aesthetic decisions before code generation begins.

---

## When Canvas Activates

Canvas activates only in these situations:
- User says "build a page," "create a component," "design a UI," or similar
- The blueprint includes frontend deliverables
- The user explicitly invokes `/specix:canvas`

Canvas does NOT activate for:
- Backend API work, database schemas, DevOps, CLI tools
- Any task where the user never mentions UI

---

## Design Thinking Sequence

Before writing a single line of code, work through these four dimensions:

### Step 1: Purpose

- What problem does this interface solve? Who uses it?
- What is the single most important action?
- What information must be visible at all times? What can be hidden until needed?

Design serves purpose. If you cannot articulate the purpose, you are not ready to design.

### Step 2: Tone — Commit to a Bold Direction

Pick an extreme aesthetic direction. These are inspiration — design one true to the context:

| Direction | Character |
|-----------|-----------|
| Brutally minimal | Extreme restraint, purposeful emptiness |
| Maximalist chaos | Dense, layered, overwhelming detail |
| Retro-futuristic | Neon, CRT textures, sci-fi chrome |
| Organic / natural | Soft curves, earth tones, flowing shapes |
| Luxury / refined | Heavy type, muted palette, generous whitespace |
| Playful / toy-like | Bright colors, rounded shapes, bouncy motion |
| Editorial / magazine | Strong typography, column layouts, pull quotes |
| Brutalist / raw | Exposed structure, harsh contrasts, no decoration |
| Art deco / geometric | Symmetry, gold accents, ornamental patterns |
| Soft / pastel | Gentle gradients, rounded corners, light palette |
| Industrial / utilitarian | Monospace, grid-heavy, data-dense, functional |
| Dark immersive | Deep backgrounds, glowing accents, atmospheric |

**CRITICAL**: Bold maximalism and refined minimalism both work — the key is **intentionality**, not intensity. Choose one direction and execute with precision.

### Step 3: Constraints

Document the hard boundaries:
- Target browsers and devices
- Accessibility requirements (WCAG level, screen reader support)
- Performance budget (bundle size, LCP target)
- Design system in use (if exists, follow it precisely)
- Framework constraints (React, Vue, plain HTML/CSS)

Constraints are not limitations — they are the frame that makes the design coherent.

### Step 4: Differentiation

- What makes this interface UNFORGETTABLE?
- What is the one thing someone will remember?
- What interaction pattern is unexpected but improves usability?

If you cannot name one distinctive element, the design is generic. Generic design is the enemy.

---

## Aesthetics Guidelines

### Typography

- Choose fonts that are beautiful, unique, and characterful
- Avoid generic fonts: Arial, Inter, Roboto, system-ui, Space Grotesk
- Pair a distinctive display font with a refined body font
- Establish a type scale with 3-5 distinct sizes
- Weight contrast creates hierarchy: bold headings, regular body, light captions
- Line length: 50-75 characters for body text
- Line height: 1.5-1.7 for body, 1.1-1.3 for headings

### Color & Theme

- Commit to a cohesive aesthetic — use CSS variables for consistency
- Dominant colors with sharp accents beat timid, evenly-distributed palettes
- Limit to 3-5 colors total (including neutrals)
- Every color choice must answer "why this color here?"
- Ensure WCAG AA contrast ratios for all text/background combinations
- Explicitly forbidden: purple gradients on white backgrounds

### Motion

- Prioritize CSS-only solutions for plain HTML
- Use Motion library (framer-motion successor) for React when available
- Focus on high-impact moments: one well-orchestrated page load with staggered reveals (`animation-delay`) creates more delight than scattered micro-interactions
- Transitions under 300ms feel responsive; over 500ms feel sluggish
- Use scroll-triggered animations and hover states that surprise
- Respect `prefers-reduced-motion` — always provide a non-animated fallback
- Never animate for decoration alone; motion should communicate meaning

### Spatial Composition

- Unexpected layouts: asymmetry, overlap, diagonal flow, grid-breaking elements
- Generous negative space OR controlled density — commit to one
- Grid and spacing follow a consistent base unit (4px, 8px)
- Alignment creates order — mixed alignment creates chaos (do it intentionally or not at all)
- Responsive breakpoints serve the content, not arbitrary device widths

### Backgrounds & Visual Details

Create atmosphere and depth rather than defaulting to solid colors:
- Gradient meshes and layered transparencies
- Noise textures and grain overlays
- Geometric patterns and decorative borders
- Dramatic shadows and depth effects
- Custom cursors where contextually appropriate
- Every visual detail should reinforce the aesthetic direction

---

## Anti-AI-Default Rules

The following patterns are the default output of undirected AI frontend generation. They are **prohibited** unless the user explicitly requests them:

| Prohibited Default | What to Do Instead |
|--------------------|--------------------|
| Inter / Roboto / Arial / system-ui fonts | Choose distinctive, characterful typefaces |
| Space Grotesk (repeatedly across generations) | Vary font choices every time |
| Purple-to-blue gradient backgrounds | Derive palette from the chosen tone |
| Rounded pill buttons on everything | Button shapes must match the tone |
| Hero sections with centered text + CTA | Structure layout around the primary purpose |
| Card grids with identical shadows | Vary elevation intentionally or eliminate shadows |
| "Clean and modern" as a design goal | Name a specific aesthetic tradition |
| Placeholder images from Unsplash | Use real content or design for the absence of imagery |
| Predictable layouts and component patterns | Asymmetry, grid-breaking, unexpected composition |

**No design should be the same.** Vary between light and dark themes, different fonts, different aesthetics across every generation. NEVER converge on common choices.

---

## Implementation Complexity

Match implementation effort to the aesthetic vision:

- **Maximalist designs** need elaborate code with extensive animations, layered effects, and rich detail
- **Minimalist designs** need restraint, precision, and careful attention to spacing, typography, and subtle details

Elegance comes from executing the vision well — not from choosing a "safer" middle ground.

---

## Output

Canvas does not write spec files. It produces the design direction in conversation, which then informs actual code generation:

```
CANVAS DIRECTION
================

Purpose: [single sentence]
Tone: [chosen direction from Step 2]
Constraints: [list]
Differentiation: [signature element — what makes it unforgettable]

Typography: [distinctive font choices and scale]
Color: [CSS variable palette]
Motion: [animation approach]
Spatial: [layout decisions]
Visual details: [backgrounds, textures, atmospheric elements]

Ready for implementation under this aesthetic direction.
```

Implementation itself is done through specix:grid with specix:redgreen, using Canvas's direction as guidance.
