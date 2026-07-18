---
name: brutalist_frontend
description: Rules, decision trees, code recipes, and pre-flight validation checklists for building high-performance, controlled-tension brutalist web interfaces.
---

# Brutalist & Controlled-Tension Frontend Design Protocol

This document is a strict execution protocol for building responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 1. PRE-FLIGHT CONTEXT DISCOVERY (PRE NEGO ŠTO POČNEŠ)
Before writing any code or making modifications, you MUST discover and document:
- [ ] **Framework:** (e.g. Vanilla HTML/CSS, Astro, React, Next.js)
- [ ] **CSS Approach:** (e.g. Vanilla CSS, Tailwind, CSS Modules)
- [ ] **Design System:** Does a variables or token file exist? (e.g. `src/styles/global.css` `:root` variables)
- [ ] **Target Device Layout:** [ ] Desktop-First [ ] Mobile-First
- [ ] **Animations Spec:** Do scroll animations, page transitions, or reveal states exist?

---

## 2. STRICT ASSUMPTION LIMITS (NIKAD NE PRETPOSTAVLJAJ)
- **NEVER** assume the user wants animations (verify or await specific specs).
- **NEVER** assume fonts are pre-loaded (always include `@font-face` or Google Fonts `@import` inside CSS if not present).
- **NEVER** assume a global reset exists (always check for `box-sizing: border-box` and margins resets).
- **NEVER** assume colors can be hardcoded (always define and use `:root` CSS variables).
- **NEVER** assume desktop code is safe when editing mobile layouts (always validate views on `1440px` and `768px`/`480px`).

---

## 3. PROHIBITED DEFAULT LIST (STROGO ZABRANJENO)
To prevent generating generic, average internet outputs, you are strictly BANNED from using:
- **BANNED Typefaces:** `Inter`, `Roboto`, `Poppins`, `Open Sans` (default AI font output). Use system serifs (e.g., `Didot`, `Times New Roman`, `Bodoni`) for headings and strict monospaces (e.g., `JetBrains Mono`, `Courier New`) for UI labels.
- **BANNED Hero Layout:** Centered hero headline with a subtitle paragraph and two adjacent pill buttons.
- **BANNED Features Section:** 3-column generic card layouts with thin borders, soft shadows, and icons in circles.
- **BANNED Styling:** Purple/blue gradients, standard glassmorphism, soft blur shadows, and rounded buttons (`border-radius: 0 !important` is mandatory).
- **BANNED Touch Targets:** Any clickable elements under `44x44px` or touch items spaced closer than `8px` apart.

---

## 4. LAYOUT DECISION TREE (ODLUKE ZA LAYOUT)

### If building a Hero Section:
- **DO NOT** center-align elements.
- **USE:** Offset asymmetrical grids, bold headlines spanning multiple lines with different indents (e.g., lines 1 and 3 left-aligned, line 2 indented by `7vw`), and high-contrast vertical dividers.

### If building a Features / Services List:
- **DO NOT** use grid cards.
- **USE:** Numbered indexes (`01`, `02`), horizontal border lines as separators, and asymmetric grids where names and copies bleed into columns (like a technical spreadsheet).

### If building a Contact Form:
- **DO NOT** place labels inline next to inputs.
- **DO NOT** use placeholder text as a label replacement.
- **ALWAYS** place labels strictly above inputs.
- **ALWAYS** make the submit button full-width on mobile.

---

## 5. CODE RECIPES (KODNI RECEPTI)

Use these copy-pasteable CSS/HTML/JS values for common layout problems:

### Fluid Typography (Fluidna tipografija)
```css
/* Fluid clamp scaling, preventing fixed px or overflow */
font-size: clamp(2rem, 8vw, 4.5rem) !important;
line-height: 1.05 !important;
```

### Safe Area for Notch Devices (Notch podrška)
```css
/* Add safe area inset offsets to bottom/top positioning */
padding-bottom: calc(24px + env(safe-area-inset-bottom)) !important;
padding-top: calc(8px + env(safe-area-inset-top)) !important;
```

### iOS Zoom Prevention on Inputs (Sprečavanje iOS auto-zooma)
```css
/* iOS zooms viewport automatically on focus if font-size < 16px */
input, select, textarea {
  font-size: 16px !important;
  line-height: 1.5 !important;
}
```

### 100vh Fix for Mobile Browsers (Rešenje za 100vh skokove)
```css
/* Prevents layouts jumping when address bar hides/shows */
min-height: 100dvh;
```

### Touch Feedback (Haptički odziv)
```css
/* Deactivate Webkit tap highlight color */
a, button, input, select, textarea, [role="button"] {
  -webkit-tap-highlight-color: transparent !important;
}

/* Active touch press feedback transition */
a:active, button:active, .submit:active, [role="button"]:active {
  transform: scale(0.98) !important;
  opacity: 0.85 !important;
  transition: transform 0.1s ease, opacity 0.1s ease !important;
}
```

### Anti-Flicker Hero Image Reveal Setup

**Step 1:** Synchronous head detection:
```html
<head>
  <script is:inline>
    if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
      document.documentElement.classList.add('js');
    }
  </script>
</head>
```
**Step 2:** CSS variable override:
```css
.hero { --hero-cut: 44%; }
.hero-image { clip-path: inset(0 0 0 var(--hero-cut)); }
html.js .hero { --hero-cut: 100%; } /* Initial hide when JS is active */
```
**Step 3:** JS Variable animation:
```javascript
const setHeroCut = (value) => {
  const pct = `${value}%`;
  heroElement.style.setProperty('--hero-cut', pct);
};
```

---

## 6. COMPARATIVE CODE EXAMPLES (PRIMERI OUTPUTA)

### Scenario: Creating a Service List row on Mobile

#### ❌ BAD (Generic, squeezed layout):
```css
/* Stylesheet */
.service-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}
.service-title {
  font-size: 14px; /* Too small for mobile */
}
.service-desc {
  font-size: 11px; /* Illegible */
  letter-spacing: 0.2em; /* Too wide for mobile reading */
}
```

#### ✅ GOOD (Controlled brutalist stack with minimum accessibility):
```css
/* Stylesheet */
@media (max-width: 768px) {
  .service-item {
    display: grid;
    grid-template-columns: 1fr; /* Single column stack */
    padding: 16px 0;
    border-bottom: var(--line) solid var(--night);
  }
  .service-code {
    font-size: 24px;
    font-family: var(--mono);
    color: var(--rust);
  }
  .service-title {
    font-size: clamp(1.8rem, 8vw, 2.5rem);
    font-family: var(--serif);
    word-break: break-word;
    overflow-wrap: break-word;
  }
  .service-desc {
    font-size: 16px; /* High legibility */
    line-height: 1.55; /* Relaxed readability spacing */
    letter-spacing: 0.02em; /* Clean reading density */
    margin-top: 8px;
  }
}
```

---

## 7. PRE-FLIGHT OUTPUT CHECKLIST (IZLAZNA ČEKLISTA)
Before sending the final output, verify:
- [ ] **Viewport Check:** Did you test on `1440px` to guarantee that the desktop layout remains 100% identical?
- [ ] **Mobile Isolation:** Are all mobile layout additions encapsulated inside `@media (max-width: 768px)` or `@media (max-width: 480px)`?
- [ ] **Typographic Clamp:** Do all scalable text variables use `clamp()` instead of raw `vw` units?
- [ ] **Input Zoom:** Are all form input font-sizes `>= 16px` to prevent iOS viewport auto-zooms?
- [ ] **Accessibility Contrast:** Do all text elements meet at least WCAG AA (4.5:1) color contrast ratios?
- [ ] **Scroll Padding:** Do all section wrappers with anchor links contain a `scroll-margin-top` to prevent header overlapping?
- [ ] **Touch Target Size:** Are all interactive touch elements at least `44x44px` in clickable size?
- [ ] **Overscroll Containment:** Is `overscroll-behavior: contain` attached to fixed drawers or modaly popups?
