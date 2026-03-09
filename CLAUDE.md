# WH40K Simulator UI Lab

Design-first repo for the WH40K simulator UI. **Intentionally separate from the simulator engine repo** (`warhammer-40k-simulator`) to prevent context pollution.

---

## What this repo is

A phased design workspace. No engine code. No game logic. Pure UI/UX iteration.

**Goal:** Nail the design before touching the simulator. Previous approach (simultaneous design + code) produced UI with persistent artifacts, local optima, and subpar UX.

---

## Phase 0: Ground Truth (locked — do not skip)

All decisions below are resolved. Do not re-derive them from conversation.

### Design direction

- **Mockup 1** (dark, symbolic, top-down 2D) is the base direction
- Enrich with specific elements from Mockup 2 — but only the ones listed below
- Do **not** add: miniature art, portrait slots, terrain tile art, 3D/isometric elements

**What to steal from Mockup 2 (and nothing else):**
- Faction color identity applied consistently to every unit token + panel highlight
- Corner-bracket panel chrome (CSS only, no images)
- Heavy uppercase display typography for headers
- `BATTLESHOCKED` status indicator (this is the only mid-turn status that matters)

### Primary visual reference

**Frozen Synapse only.** No other game references.

- Reason: every other candidate (Into The Breach, XCOM, Battletech, Advance Wars) is isometric, orthographic, or uses representational 3D sprites. Feeding those to an LLM will push output toward the wrong visual model.
- Frozen Synapse is pure top-down, symbolic, neon-on-dark, 2D. It is the only clean reference.
- Use actual screenshots of Frozen Synapse as image inputs when prompting — do not describe it, show it.

### Vibe words

`tactical` `grimdark` `data-dense` `precise`

These are used as a filter at every design gate: *"Does this element feel tactical, grimdark, data-dense, and precise?"*

### Interaction model (plain English UX contract)

- **Turn structure:** Command → Movement → Shooting → Charge → Fight (phases, player alternates)
- **Active phase** is always visible and dominant in the UI — it is the primary orientation cue
- **Unit selection:** click a unit → stats panel opens on right, legal actions appear in bottom bar
- **Always visible:** battlefield, unit tokens, phase indicator, both players' scores
- **On-demand:** unit stats (only when selected), weapon details, range overlays
- **Range/movement:** shown as overlays on the battlefield when a unit is selected or an action is chosen — not persistent decorations
- **Status:** only `BATTLESHOCKED` needs a visible indicator. No "Ready", "Activated", or other states.
- **No tooltips required in Phase 1/2** — data density should be legible inline

### What must NOT appear (explicit prohibitions for LLMs)

- Dotted lines except for: dashed movement path, dashed range arc (only when action is selected)
- Drop shadows on battlefield unit tokens
- Gradients on unit tokens
- Portrait/image slots in unit list or stats panel
- Random decorative borders or dividers not part of the panel chrome system
- Any isometric or perspective projection — strictly top-down flat

---

## Phase 1: Design System (in progress)

A single HTML style guide page that renders all design components as living examples. Built section by section — each section approved before the next starts.

**At each section: produce 3 variations. James picks one before moving to the next section.**

### Locked palette: V1-D
- Backgrounds: #080c10 / #0f1520 / #162030 / #1e2d44
- Imperium accent: #00d4ff (cyan) — NOT gold; gold is unreadable at stat label sizes
- Orks accent: #ff4020
- Text: #e0e8f0 / #8090a0 / #4a6080 / #2a3a4a
- Battleshocked: #cc2020
- Display/phase headings: Anton
- All UI labels, stats, body: Rajdhani
- Gold (#fbca1b) is NOT used — confirmed unreadable at small sizes

### Section order
1. **Colors + typography** ✅ locked — V1-D
2. **Panel chrome** ✅ locked — V2-B
3. Atomic components (unit token, stat row, weapon row, button, BATTLESHOCKED pill)
4. **Animations** ← current


### Output format
- One HTML file per variation: `design-system/v[section]-[A|B|C].html`
- Each file is self-contained (no external dependencies except Google Fonts)
- Rendered on GitHub Pages for review

Gate: James approves one variation per section before next section starts.

---

## Phase 2: Wireframes (not started)

Gray-box HTML only. No colors, no tokens from Phase 1.

Key screens to wireframe (one at a time):
1. Battlefield view — units placed, active phase shown, no unit selected
2. Unit selected state — stats panel visible, action bar populated
3. Combat resolution — dice result display

Gate: James approves each wireframe layout before Phase 3.

---

## Phase 3: Composition (not started)

Apply Phase 1 tokens to Phase 2 wireframes. This is purely mechanical — the design language and structure are both locked before this starts.

---

## Working rules for LLMs

1. **Read this file before generating anything.** Do not infer design decisions from code or previous outputs.
2. **One component at a time.** Never generate a full page until all component atoms are approved.
3. **Never iterate on broken HTML.** If a generation has artifacts, start from the clean base — do not patch.
4. **Cite the constraint** when you add or remove something: "Removing dotted border per CLAUDE.md prohibition."
5. **Screenshot → structured critique → targeted fix.** Critique lists problems ranked by severity. Fix addresses top 3 only. One thing at a time.
6. **Do not hallucinate new UI elements.** If something isn't in the wireframe or token spec, it does not appear.

### Locked panel chrome: V2-B
- 1px border: #00d4ff22 (low opacity)
- Top edge: 2px solid var(--imp) — default cyan, red when battleshocked
- Header: background var(--bg-raised), separated by 1px border #00d4ff18
- Selected: background var(--bg-selected), border #00d4ff55
- Battleshocked: background #100808, top edge var(--battleshocked), title color var(--battleshocked)
