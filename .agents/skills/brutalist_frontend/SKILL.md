---
name: brutalist_frontend
description: Guide and instructions for building high-performance, controlled-tension brutalist frontend interfaces with optimal UX and modern accessibility.
---

# Brutalist & Controlled-Tension Frontend Design Guide

This guide describes how to design and build modern, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend interfaces. It balances raw, industrial aesthetic choices with strict standards for performance, responsiveness, and accessibility (A11y).

---

## 1. Core Visual Principles

1. **Controlled Tension (Organized Chaos):**
   - The design should feel raw, bold, and edge-like, but must remain mathematically precise, stable, and highly legible.
   - Use sharp geometric borders, solid grids, and high contrast instead of soft borders, dropshadows, or rounded corners (`border-radius: 0 !important`).
2. **High-Contrast Color Palettes:**
   - Use highly contrastive, dark-mode friendly schemes (e.g., pure black/dark grey `--night` + off-white/cream `--bone` + warning/accent yellow `--rust`).
   - Keep selection text and active items clear with high contrast ratios (WCAG AAA standard of at least 7:1).
3. **Monospace & Serif Contrast:**
   - Pair large serif typography for display headings with strict, small monospace fonts for labels, numbers, steps, and indicators to create a "technical blueprint" or "printed catalog" feel.

---

## 2. Grid & Fluid Layout Rules

- **Viewport Containment:** Keep `html` and `body` set to `overflow-x: hidden` and `max-width: 100%` on mobile viewports to completely block horizontal scroll breakages.
- **Section Spacing:** For mobile viewports, enforce a generous section padding (e.g., `48px 20px` or `padding-inline: var(--pad)`) to maintain design breathing room.
- **Single-Column Stacking:** At small viewports (e.g., `< 480px`), stack complex multi-column grids or side-by-side ledger rows into simple single columns to prevent text clipping and squeezed layouts.

---

## 3. Typography & Accessibility (A11y)

- **Minimum Body Copy:** Keep readable paragraph and service descriptions at a minimum of `16px` font size on mobile devices, with a `line-height` of at least `1.5` to `1.55`.
- **Reduced Letter-Spacing for Paragraphs:** While micro-labels can have wide spacing, reading copies and paragraphs should use a tight `letter-spacing` (e.g. `0.02em` or `normal`) to reduce reading fatigue on small screens.
- **Word-Breaking:** Always define `word-break: break-word` and `overflow-wrap: break-word` on headings for mobile viewports to prevent long words from overflowing screen margins.
- **Fluid Header Scaling:** Avoid raw `vw` typography. Use `clamp()` rules to constrain title sizes safely across different device viewports (e.g., `font-size: clamp(2rem, 8vw, 4.5rem)`).

---

## 4. Mobile Menu Interactivity (Accessible Drawer)

For sliding navigation drawers on mobile:
1. **Focus Trap:** When the drawer is open, keep focus trapped inside the drawer loop using JS. Prevent focus from escaping to the background document.
2. **Overlay Click-Outside:** Implement a semi-transparent, blurred backdrop overlay (`.mobile-menu-overlay`) that slides or fades in and closes the drawer when clicked.
3. **Keyboard Escape:** Attach a global key listener that closes the drawer immediately when the `Escape` key is pressed.
4. **Overscroll Containment:** Set `overscroll-behavior: contain` on the drawer to block background page scrolling while navigating the menu.

---

## 5. Touch UX & Safe Areas

- **Touch Target Size:** Ensure all clickable elements (logos, menu toggles, back-to-top buttons, inline contact links) have a minimum touch target area of `44x44px`.
- **iOS Zoom Prevention:** Form inputs, select, and textarea fields must be at least `16px` font size on mobile to stop iOS Safari from auto-zooming and displacing the page layout on focus.
- **Haptic Press Transition:** Set `-webkit-tap-highlight-color: transparent` to hide default OS tap flashes. Replace it with an active scale transition:
  ```css
  a:active, button:active, [role="button"]:active {
    transform: scale(0.98) !important;
    opacity: 0.85 !important;
    transition: transform 0.1s ease, opacity 0.1s ease !important;
  }
  ```
- **Touch Hover Override:** Wrap desktop-only hover effects in `@media (hover: none)` to override and disable hover styles on touch screens, avoiding stuck hover colors after taps.
- **Safe Area Insets (Notch Devices):** Offset fixed or sticky elements (like header bars, sliding drawers, back-to-top buttons) using CSS `env(safe-area-inset-...)` rules.

---

## 6. Performance & Anti-Flicker Techniques

- **Synchronous JS Detection:** Place a light, synchronous `<script is:inline>` block in the HTML `<head>` to immediately flag JavaScript presence before the body is rendered:
  ```html
  <script is:inline>
    if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
      document.documentElement.classList.add('js');
    }
  </script>
  ```
- **CSS Initial Hide:** Match the `.js` class in stylesheets to force reveal-animated elements (like the hero images) to render as fully clipped or hidden on first paint:
  ```css
  html.js .hero {
    --hero-cut: 100%; /* Keeps image hidden until JS animates the clip-path */
  }
  ```
  This prevents a visual flash of unstyled/unclipped image content before the main JS initializes.
