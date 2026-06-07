---
name: picture-book-audit
description: Use when someone wants a visual, show-don't-tell review of a project — what it looks like today, what to polish, and what to build next — delivered as an illustrated before/after HTML report themed in the project's own brand. Triggers include "show me what to improve", "how can we make this better/look good", "what should we work on next", "scan the UI/UX", "design/polish/roadmap review", "what to fix", or asking for pictures/diagrams instead of a text list. Works in ANY project; mostly UI/UX but can cover performance, backend, and accessibility.
---

# Picture-Book Audit

## Overview

Produce a self-contained HTML "picture book" that reviews a project visually: **show, don't tell.** Every point is a *picture* — a faithful mockup of the real UI with the problem circled and the fix drawn beside it — not a paragraph. Captions are one line. Reading is minimal; seeing is the point.

**Two things are fixed, one thing flexes:**
- **Fixed: the format** (the plates, before→after pairing, matrix, shortlist, figure captions). This is the skill's signature.
- **Fixed: the production discipline** (ground every plate in real code; verify the render before delivering).
- **Flexes: the skin.** Colors come from the *audited project's* brand so each book looks native to its app. Never reuse another project's palette — a same-every-time look is a failure.

## When to Use

- "Show me what we should improve / work on next" · "make it look good" · "audit the UI/UX"
- A stakeholder wants a polish roadmap they can *look at*, not read
- You're tempted to write a bullet list of improvements — build this instead

**Not for:** shipping the fixes (that's implementation), or a quick one-line answer.

## The fixed format

1. **Cover** — kicker, big serif headline, one-line lede, meta row.
2. **Plate 00 — "the app today"** — one faithful full mockup of the current main screen, so the reader recognizes it instantly.
3. **6–10 finding plates** — each: number + title + a severity tag, a **Before → After** pair of mockups (problem on the left circled in red, fix on the right in blue), and ONE figure-caption line grounded in a real file.
4. **"Add next" plates** — net-new features shown as *After-only* mockups.
5. **Impact × effort matrix** — every finding plotted; low-effort/high-impact corner marked "ship this week".
6. **Quick-win shortlist** — 3–5 items with rough effort estimates.
7. **Footer.**

Coverage is *mostly UI/UX* but may include perf/backend/a11y as **diagrams** (e.g. "cold start 2.1s → 0.8s", a request-waterfall, a focus-ring fix). Render those as visual blocks, never prose.

## Process

**1 — Derive two palettes from the project (REQUIRED).**
Read the project's real brand before drawing anything: CSS custom properties, `tailwind.config`, theme/token files, SCSS vars, design-system constants, or the rendered app. Fill:
- **Book palette** — the report wrapper (`--paper/--ink/--accent/...`). Tasteful, derived from the brand's accent + a neutral ground.
- **App palette** — the audited app's *actual* UI colors (`--app-bg/--app-card/--app-acc/...`), light or dark to match how it really looks.

If the project has no discernible palette, choose a distinctive one that fits its domain — never a generic gray.

**2 — Scan and ground.** Read the key UI files and any perf/backend hot spots. Every finding must point at a real file (`path · symbol`). Do not invent problems or UI that doesn't exist.

**3 — Build faithful plates.** Copy `template.html`, set the palettes/fonts, and replace the placeholder plates. Reuse the component kit (device frame, plate, before/after, pins/rings/flags, matrix, shortlist, diagrams). Mockups should read like screenshots of *this* app.

**4 — Fonts.** Default to the house pairing the report ships with — a distinctive display serif (Fraunces) + clean sans (Spline Sans). Swap only if the project has its own strong type identity worth honoring. Never Inter / Arial / Roboto / system default.

**5 — Verify the render (MANDATORY — do not skip).**
```bash
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless=new --disable-gpu --hide-scrollbars --force-device-scale-factor=1 \
  --virtual-time-budget=2500 --window-size=1200,9000 \
  --screenshot=/tmp/pba.png "file://$PWD/<output>.html"
```
Read the PNG. Fix overflow, clipping, mispositioned pins, unloaded fonts. Re-render until clean. THEN open it for the user.

**Output path:** `design/<project>-picture-book.html` (create `design/` if absent). Remove temp render PNGs when done.

## Common mistakes

| Mistake | Fix |
|---|---|
| Reusing a fixed palette (e.g. cream + indigo) every time | Derive colors from THIS project's brand. Same-look = failure. |
| Text-heavy sections, paragraphs, bullet lists of problems | One figure-caption line per plate. The picture carries it. |
| Generic browser-chrome mockups | Faithful plates in the app's own palette — read like its screenshots. |
| Inventing UI or problems | Ground every plate in a real file; cite it. |
| Delivering without rendering | Always headless-screenshot, inspect, fix, THEN open. |
| Lorem ipsum / placeholder copy | Use real, plausible content from the project's domain. |
| Generic fonts | Distinctive serif + clean sans; never Inter/Arial/Roboto. |

## Reference

`template.html` — the locked layout + the full reusable component kit (CSS-variable palettes at top, device/plate/before-after/annotation/matrix/shortlist/diagram components) with placeholder plates to replace. Start every audit from it.
