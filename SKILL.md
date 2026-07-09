---
name: liquid-glass
description: Apply the Apple-style liquid glass refraction effect to any web component (cards, navs, modals, buttons). Use when the user asks for liquid glass, glassmorphism with real refraction, iOS 26-style glass, or frosted glass with edge bending.
---

# Liquid Glass Effect

A proven, tuned implementation of Apple-style liquid glass: a real refraction
bulge at the rim plus a chromatic fringe, with a frosted-blur fallback for
Safari and Firefox. **Reuse it — do not re-derive it.**

Source: https://github.com/deepika-builds/liquid-glass
The module lives next to this file: `liquid-glass.js` (drop-in, no dependencies).
API: `liquidGlass(el, { scale, chroma, border, mapBlur, blur, saturate,
radius, fallbackBlur })` returns `{ supported, refresh, destroy }`.

## How to apply to a new component

1. Copy `liquid-glass.js` (in this skill folder) into the target project, or
   reference it with a `<script>` tag.
2. Call `liquidGlass(element)`. Defaults are tuned; tweak `scale` (strength)
   and `chroma` (fringe) to taste. Subtle: -60 / 4. Default: -112 / 6.
   Dramatic: -180.
3. Add the CSS dressing (border-radius, tint gradient, inset highlights, drop
   shadow) so it reads as real glass. See the recipe in `README.md`.
4. In React/Vue: call it inside an effect hook on the ref; `destroy()` on unmount.

## Non-negotiable gotchas

- The SVG `<filter>` MUST set `color-interpolation-filters="sRGB"`. Without it,
  linearRGB remaps the map's neutral gray and the whole backdrop ghosts
  up-and-left. (The module already sets this.)
- Refraction is Chromium-only; Safari/Firefox get the frosted-blur fallback
  automatically. Never make refraction carry meaning.
- Keep the interior legible: refraction belongs at the rim. If content smears,
  lower `scale`, raise `blur`, or increase `border`.
- Avoid the effect on elements larger than ~800px per side (GPU cost), and
  don't regenerate the map for position-only animation — only size changes.
