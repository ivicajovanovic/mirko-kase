---
name: brutalist_frontend
description: Rules, decision trees, code recipes, and pre-flight validation checklists for building high-performance, controlled-tension brutalist web interfaces.
---

# Brutalist & Controlled-Tension Frontend Design Protocol

This document is a strict execution protocol for building responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 1. PRE-FLIGHT CONTEXT DISCOVERY
Before writing any code or making modifications, you MUST discover and document the following context:
- [ ] **Framework & Styling Strategy:**
  - *Vanilla HTML/CSS/Astro:* Put all variables in `:root` in the main `.css` file.
  - *Tailwind CSS:* NEVER write custom `@media` queries manually. Use Tailwind's responsive prefixes (e.g. `sm:`, `md:`, `lg:`).
  - *CSS Modules:* Each responsive style block goes inside the corresponding `.module.css` file, not in the global stylesheet.
  - *If you cannot determine the framework:* STOP and ask the user. Do not make assumptions.
- [ ] **Design System:** Does a variables or token file exist? Check and list the primary variables.
- [ ] **Target Device Layout:** [ ] Desktop-First [ ] Mobile-First
- [ ] **Animations Spec:** Do scroll animations, page transitions, or reveal states exist?
- [ ] **Client Branding & Identity:** Has the client defined their custom brand color codes (background, text, muted, accent) and typography (display serif, monospace)?
  - *If yes:* Retrieve the exact brand assets and plug them into the Token Skeleton below.
  - *If no:* Propose a cohesive brutalist theme (e.g. dark industrial, high-contrast warning) to the user/client, get their explicit approval, and then write the custom styles. Do NOT assume colors or fonts.

---

## 2. STRICT ASSUMPTION LIMITS
- **NEVER** assume the user wants animations (verify or await specific specs).
- **NEVER** assume fonts are pre-loaded (always include `@font-face` or Google Fonts `@import` inside CSS if not present).
- **NEVER** assume a global reset exists (always check for `box-sizing: border-box` and margins resets).
- **NEVER** assume colors can be hardcoded (always define and use `:root` CSS variables).
- **NEVER** assume desktop code is safe when editing mobile layouts (always validate views on `1440px` and `768px`/`480px`).

---

## 3. PROHIBITED DEFAULT LIST
To prevent generating generic, average internet outputs, you are strictly BANNED from using:
- **BANNED Typefaces:** `Inter`, `Roboto`, `Poppins`, `Open Sans` (default AI font output). Use system serifs (e.g., `Didot`, `Times New Roman`, `Bodoni`) for headings and strict monospaces (e.g., `JetBrains Mono`, `Courier New`) for UI labels.
- **BANNED Hero Layout:** Centered hero headline with a subtitle paragraph and two adjacent pill buttons.
- **BANNED Features Section:** 3-column generic card layouts with thin borders, soft shadows, and icons in circles.
- **BANNED Styling:** Purple/blue gradients, standard glassmorphism, soft blur shadows, and rounded buttons (`border-radius: 0 !important` is mandatory).
- **BANNED Touch Targets:** Any clickable elements under `44x44px` or touch items spaced closer than `8px` apart.

---

## 4. LAYOUT DECISION TREE

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

### If building a Navigation / Header:
- **NEVER:** Use a hamburger menu that toggles `display: none` / `display: block` inline.
- **ALWAYS:** Use CSS `transform: translateX()` for the sliding drawer transition.
- **ALWAYS:** Ensure focus trapping, `aria-expanded` toggle state, background scroll locking, and Escape key listeners are attached.

### If building a Testimonials / Social Proof Section:
- **NEVER:** Use carousels with autoplay, infinite loops, or tiny dots.
- **ALWAYS:** Stack them vertically on mobile, treating the quote as a styled block element with an asymmetric grid and numbered indexes.

### If building a Pricing Section:
- **NEVER:** Fit 3 columns or cards side-by-side on mobile.
- **ALWAYS:** Stack them vertically or use an accordion layout with a sticky CTA button at the bottom viewport.

### If building a Modal / Drawer:
- **ALWAYS:** Apply `overscroll-behavior: contain` to prevent scroll chaining.
- **ALWAYS:** Add `aria-modal="true"`, `role="dialog"`, keyboard Focus Trap, and Escape key handlers.

---

## 5. BRUTALIST TOKEN SKELETON

Use this architectural skeleton to organize the typography and color tokens. You MUST NOT use default values without consulting the user. Proactively prompt the user to define these in coordination with the client.

```css
:root {
  /* Typography — DEFINE IN COORDINATION WITH THE CLIENT / USER */
  --font-display: <client-display-serif-font>, Georgia, serif;
  --font-mono:    <client-monospace-font>, monospace;
  
  /* Colors (Max 4) — DEFINE IN COORDINATION WITH THE CLIENT / USER */
  --color-bg:     <primary-bg-color>;      /* dominant background (night) */
  --color-text:   <primary-text-color>;    /* highly legible text (bone) */
  --color-muted:  <muted-label-color>;     /* secondary text, labels (concrete) */
  --color-accent: <brand-accent-color>;    /* single accent color (used rarely) */
  
  /* Lines & Outlines */
  --line:         1px;
  --line-heavy:   2px;
  
  /* Spacing scale (fluid clamp values) */
  --space-xs:  clamp(0.5rem, 1vw, 0.75rem);
  --space-sm:  clamp(0.75rem, 2vw, 1rem);
  --space-md:  clamp(1rem, 3vw, 1.5rem);
  --space-lg:  clamp(2rem, 5vw, 3rem);
  --space-xl:  clamp(3rem, 8vw, 6rem);
}
```

> [!IMPORTANT]
> **ESTHETICS AND IDENTITY ARE NOT AUTOMATICALLY INHERITED FROM THIS SKELETON.**
> As an agent, you MUST NOT assume the client's colors and fonts. If the client has no defined brand identity, it is your duty to propose a visual direction (e.g., dark industrial mode or sharp technical contrast) and request explicit approval before populating variables with concrete hex values.

---

## 6. CODE RECIPES

Use these copy-pasteable CSS/HTML/JS values for common layout problems:

### Fluid Typography
```css
/* Fluid clamp scaling, preventing fixed px or overflow */
font-size: clamp(2rem, 8vw, 4.5rem);
line-height: 1.05;
```

### Safe Area for Notch Devices
```css
/* Add safe area inset offsets to bottom/top positioning */
padding-bottom: calc(24px + env(safe-area-inset-bottom));
padding-top: calc(8px + env(safe-area-inset-top));
```

### iOS Zoom Prevention on Inputs
```css
/* iOS zooms viewport automatically on focus if font-size < 16px */
input, select, textarea {
  font-size: 16px !important; /* Normalization: !important is allowed here */
}
```

### 100vh Fix for Mobile Browsers
```css
/* Prevents layouts jumping when address bar hides/shows */
min-height: 100dvh;
```

### Touch Feedback
```css
/* Deactivate Webkit tap highlight color */
a, button, input, select, textarea, [role="button"] {
  -webkit-tap-highlight-color: transparent !important; /* Normalization: !important is allowed here */
}

/* Active touch press feedback transition */
a:active, button:active, .submit:active, [role="button"]:active {
  transform: scale(0.98);
  opacity: 0.85;
  transition: transform 0.1s ease, opacity 0.1s ease;
}
```

### JavaScript: Hamburger Menu - Brutalist Drawer
```javascript
const nav = document.querySelector('[data-nav]');
const toggle = document.querySelector('[data-nav-toggle]');
const focusable = nav.querySelectorAll('a, button, [tabindex]');
const firstFocus = focusable[0];
const lastFocus = focusable[focusable.length - 1];

const focusTrap = (e) => {
  if (e.key === 'Tab') {
    if (e.shiftKey && document.activeElement === firstFocus) {
      lastFocus.focus();
      e.preventDefault();
    } else if (!e.shiftKey && document.activeElement === lastFocus) {
      firstFocus.focus();
      e.preventDefault();
    }
  }
};

toggle.addEventListener('click', () => {
  const isOpen = toggle.getAttribute('aria-expanded') === 'true';
  toggle.setAttribute('aria-expanded', String(!isOpen));
  nav.setAttribute('data-open', String(!isOpen));
  document.body.style.overflow = isOpen ? '' : 'hidden'; // Scroll lock
  if (!isOpen) {
    document.addEventListener('keydown', focusTrap);
    firstFocus.focus();
  } else {
    document.removeEventListener('keydown', focusTrap);
  }
});

// Escape key closure
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && toggle.getAttribute('aria-expanded') === 'true') {
    toggle.click();
  }
});
```

### JavaScript: Scroll Reveal without Libraries (Intersection Observer)
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.setAttribute('data-visible', 'true');
      observer.unobserve(e.target); // Trigger once
    }
  });
}, { threshold: 0.15 });

document.querySelectorAll('[data-reveal]').forEach(el => observer.observe(el));
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

## 7. !important PROTOCOL
- **NEVER** use `!important` as a shortcut for specificity problems in your layout code. Solve the issue using proper CSS selector specificity.
- **USE ONLY** for browser normalizations (like iOS zoom fix or disabling default tap highlight).
- **USE ONLY** to override styling from third-party libraries (e.g. Google Maps default styles, libraries).
- **NEVER** use it on custom grid, layout, or spacing values.

---

## 8. COMPARATIVE CODE EXAMPLES

### Example 1: Service List Row on Mobile

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
  font-size: 14px;
}
.service-desc {
  font-size: 11px;
  letter-spacing: 0.2em;
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
    border-bottom: var(--line) solid var(--color-bg);
  }
  .service-code {
    font-size: 24px;
    font-family: var(--font-mono);
    color: var(--color-accent);
  }
  .service-title {
    font-size: clamp(1.8rem, 8vw, 2.5rem);
    font-family: var(--font-display);
    word-break: break-word;
    overflow-wrap: break-word;
  }
  .service-desc {
    font-size: 16px;
    line-height: 1.55;
    letter-spacing: 0.02em;
    margin-top: 8px;
  }
}
```

### Example 2: Pricing Columns on Mobile

#### ❌ BAD (Three columns squished or horizontal overflow):
```css
/* Stylesheet */
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Horizontally squishes on mobile */
}
```

#### ✅ GOOD (Stacked vertical layout with clean spacing):
```css
/* Stylesheet */
@media (max-width: 768px) {
  .pricing-grid {
    grid-template-columns: 1fr; /* Stacks to single column */
    gap: var(--space-md);
  }
  .pricing-card {
    border: var(--line) solid var(--color-text);
    padding: var(--space-md);
  }
  .pricing-card:last-child {
    border-bottom: var(--line-heavy) solid var(--color-accent);
  }
}
```

### Example 3: Menu Drawer Open/Close

#### ❌ BAD (Abrupt display toggle):
```css
/* Stylesheet */
.drawer {
  display: none;
}
.drawer.open {
  display: block; /* No transitions or animations supported */
}
```

#### ✅ GOOD (Stepped transform translation):
```css
/* Stylesheet */
@media (max-width: 768px) {
  .drawer {
    position: fixed;
    right: 0;
    width: min(80vw, 360px);
    transform: translateX(100%);
    transition: transform 0.18s steps(3, end);
  }
  .drawer[data-open="true"] {
    transform: translateX(0);
  }
}
```

---

## 9. PRE-FLIGHT OUTPUT CHECKLIST
Before sending the final output, verify:
- [ ] **Viewport Check:** Did you test on `1440px` to guarantee that the desktop layout remains 100% identical?
- [ ] **Mobile Isolation:** Are all mobile layout additions encapsulated inside `@media (max-width: 768px)` or `@media (max-width: 480px)`?
- [ ] **Typographic Clamp:** Do all scalable text variables use `clamp()` instead of raw `vw` units?
- [ ] **Input Zoom:** Are all form input font-sizes `>= 16px` to prevent iOS viewport auto-zooms?
- [ ] **Accessibility Contrast:** Do all text elements meet at least WCAG AA (4.5:1) color contrast ratios?
- [ ] **Scroll Padding:** Do all section wrappers with anchor links contain a `scroll-margin-top` to prevent header overlapping?
- [ ] **Touch Target Size:** Are all interactive touch elements at least `44x44px` in clickable size?
- [ ] **Overscroll Containment:** Is `overscroll-behavior: contain` attached to fixed drawers or modal popups?

## 10. BRUTALIST IDENTITY CHECK
Verify that the output strictly adheres to the brutalist aesthetic:
- [ ] Does any element have border-radius > 0? → Remove or set to 0.
- [ ] Are any of the banned fonts used (Inter, Roboto, Poppins, Open Sans)? → Replace them.
- [ ] Does any section repeat the layout of the previous one? → Break it with a different layout pattern (e.g. table instead of columns).
- [ ] Is there a centered hero section? → Restructure into an asymmetrical grid.
- [ ] Is there a 3-column card grid on mobile? → Convert it into a vertically stacked indexed list.
- [ ] Are there soft box-shadows on any element? → Remove or replace with sharp, hard shadows (e.g., `box-shadow: 4px 4px 0px 0px var(--color-bg)`).
- [ ] Are all all buttons sharp-edged (border-radius: 0)?
- [ ] Is there at least one element that intentionally breaks grid rules (engineered tension, e.g., asymmetric overlaps or negative margins)?
