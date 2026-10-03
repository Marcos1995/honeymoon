---
name: web-design
description: Any page, landing, dashboard or visual change (HTML/CSS/JSX), or "diseño", "web bonita", "UI", "landing", "panel". Google Stitch designs; you integrate. Always.
---

# Web design

Google Stitch designs. You do not design the screen yourself.

Any new page, restyle, dashboard or visual change goes through Stitch. A typo, a data field or wiring that does not change layout does not.

## Style = `DESIGN.md`

Missing: create it (≤ 20 lines) with this default plus the user's wishes:

`Minimal, moderno y limpio. Mucho aire, jerarquía clara, una sola acción principal. Mobile first: el mismo layout nace a 360px y escala a 768 y 1280, sin una pantalla aparte de escritorio. Una paleta (neutros cálidos + un acento), claro y oscuro. Nada de degradados morados, arcoíris, glassmorphism, emojis como iconos, todo centrado ni plantillas genéricas.`

Keep the Stitch project id, design-system asset id and screen ids here.

Data page (prices, stats, rankings, metrics, panel): also put the anatomy from `dashboard.md` in this folder into the Stitch prompt (hero KPI, comparisons, charts, source line).

## Steps

1. Reuse the project id in `DESIGN.md`. Missing: `create_project` once, on MCP `stitch` (reads and `create_project` are fast; call them there).
2. Design system once per repo (reuse the asset id). Through `stitch_long`, tool `create_design_system`:
   - `colorMode: LIGHT`, `colorVariant: TONAL_SPOT`, `roundness: ROUND_EIGHT`
   - `customColor`: the accent hex from `DESIGN.md`, or `#2457F5` if none
   - `bodyFont` and `headlineFont`: `DM_SANS`
   - `displayName`: the project name
3. New screen: `stitch_long` with tool `generate_screen_from_text`. Visual change to an existing screen: tool `edit_screens` (same screen id). Arguments:
   - `projectId`, `prompt` (purpose, real copy from the repo, the `DESIGN.md` line, and for data the `dashboard.md` anatomy)
   - `deviceType: MOBILE`, `modelId: GEMINI_3_8_FLASH` (the proxy forces both)
   - `designSystem`: `assets/<id>` from step 2
   - The prompt must say the layout starts at 360px and the same structure scales to tablet and desktop. Do not generate DESKTOP, TABLET or AGNOSTIC screens.
4. If `stitch_long` returns `status: running`, call `stitch_wait(job)` until the result. Do not call generate/edit/variants on MCP `stitch` directly: those die at 60 s. Only past 20 min is a failure: report FALLO.
5. `get_screen` on MCP `stitch`: take its HTML and screenshot. Keep layout, type and palette. Fit the repo (static: one HTML + CSS; React: Tailwind v4 + shadcn/ui). Add a dark theme with the same accent and warm neutrals (`prefers-color-scheme`); Stitch only emits one color mode. Real text, no unused CSS/JS.
6. Write the ids back to `DESIGN.md`.
7. Stitch missing or failing: say so, then hand-build from `DESIGN.md`. Do not invent a second visual system.

## Before HECHO

360 / 768 / 1440 without horizontal scroll, WCAG AA, visible focus, `alt` on images, `prefers-reduced-motion`. Data page: the h1 states the finding, charts have a caption, no incomparable numbers ranked. Look with skill `verify`.
