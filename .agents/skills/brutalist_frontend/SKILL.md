---
name: brutalist_frontend
description: Rules, implementation templates, and instructions for building high-performance, controlled-tension brutalist web interfaces. Trigger this skill when modifying, styling, or creating layout grids, mobile menu drawers, custom haptic buttons, or handling image render flicker.
---

# Brutalist & Controlled-Tension Frontend Design Specification

This document provides strict engineering guidelines and implementation blueprints for building responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 1. Core Directives

### Visual Hierarchy
- **NEVER** use soft shadows, gradients (unless explicitly requested), or rounded corners. Enforce `border-radius: 0 !important` globally.
- **ALWAYS** contrast raw serif typography (headings) with strict monospace typography (UI labels, numbers, metadata).
- **ALWAYS** design with sharp geometric lines (`var(--line) solid var(--color)`).

### Viewport & Layout Integrity
- **ALWAYS** include `overflow-x: hidden` and `max-width: 100%` on `html` and `body` in mobile media queries to prevent horizontal overflow scrolling.
- **ALWAYS** stack grid elements into a single column on viewports `<= 480px` (e.g., set `.ledger-row { grid-template-columns: 1fr; }` and make grid children span `1 / -1`).

### Typography Rules
- **NEVER** use raw `vw` units for font sizing. **ALWAYS** wrap them in a `clamp()` function to set minimum and maximum constraints (e.g., `font-size: clamp(2rem, 10vw, 3.5rem)`).
- **ALWAYS** apply `word-break: break-word` and `overflow-wrap: break-word` on mobile headings to prevent long words from clipping.
- **ALWAYS** maintain a minimum `16px` font size and `1.5` line-height for body reading copy on mobile.
- **ALWAYS** reduce `letter-spacing` to `0.02em` or `normal` on paragraphs of body copy for optimal reading, even if wide letter-spacing is used for UI labels.

---

## 2. Recipe: Anti-Flicker Hero Image Reveal

To prevent the visual "flash" (FOUC) of an animated clip-path layout on page load before JavaScript executes, you MUST implement the following pattern:

### 1. HTML Layout Setup
Insert a synchronous detection script inside the HTML `<head>` tag:
```html
<head>
  <!-- Place this script inline in the head before any body elements are painted -->
  <script is:inline>
    if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
      document.documentElement.classList.add('js');
    }
  </script>
</head>
```

### 2. Base & State CSS Styles
Define the layout variables to start in a hidden state only when Javascript is loaded.
```css
/* Fallback default for No-JS users (fully visible/partially visible layout) */
.hero {
  --hero-cut: 44%;
}
.hero-image {
  clip-path: inset(0 0 0 var(--hero-cut));
}

/* Instant-hide state during first paint when JS is enabled */
html.js .hero {
  --hero-cut: 100%;
}
```

### 3. JavaScript Animation Execution
Do **NOT** write inline `style.clipPath` overrides during JS initialization. Instead, let the class animations run, and modify the CSS custom variable directly:
```javascript
// GOOD: Updates the layout variable. CSS transitions will handle clip-path changes.
const setHeroCut = (value) => {
  const pct = `${value}%`;
  heroElement.style.setProperty('--hero-cut', pct);
};
```

---

## 3. Recipe: Accessible Navigation Drawer (Mobile Menu)

When implementing the hamburger menu sliding drawer:

### 1. HTML Structure
```html
<!-- Navigation drawer -->
<nav class="mobile-menu" id="mobile-menu" aria-label="Mobile navigation">
  <a href="#section-1">Link 1</a>
  <a href="#section-2">Link 2</a>
</nav>

<!-- Backdrop overlay -->
<div class="mobile-menu-overlay" id="mobile-menu-overlay" aria-hidden="true"></div>
```

### 2. Interaction CSS Styles
```css
/* Drawer container positioning */
.mobile-menu {
  position: fixed;
  z-index: 50;
  top: 0; bottom: 0; right: 0; left: auto;
  width: min(80vw, 360px);
  transform: translateX(100%);
  transition: transform 0.18s steps(3, end); /* Brutalist stepped animation */
  overscroll-behavior: contain; /* Prevents scroll chaining to body */
}
.mobile-menu.open {
  transform: translateX(0);
}

/* Semi-transparent backdrop overlay */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  z-index: 45;
  background: rgba(17, 17, 15, 0.6);
  backdrop-filter: blur(4px);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
}
.mobile-menu-overlay.open {
  opacity: 1;
  pointer-events: auto;
}
```

### 3. JavaScript Interactivity
You MUST manage focus, click-outside, and keyboard Escape events:
```javascript
const toggle = document.querySelector('.menu-toggle');
const menu = document.querySelector('.mobile-menu');
const overlay = document.querySelector('#mobile-menu-overlay');

if (toggle && menu) {
  const menuLinks = menu.querySelectorAll('a');
  const focusable = [toggle, ...menuLinks];
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  const focusTrap = (e) => {
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === first) {
        last.focus();
        e.preventDefault();
      } else if (!e.shiftKey && document.activeElement === last) {
        first.focus();
        e.preventDefault();
      }
    }
  };

  const openMenu = () => {
    menu.classList.add('open');
    overlay.classList.add('open');
    document.body.classList.add('menu-open');
    toggle.setAttribute('aria-expanded', 'true');
    document.addEventListener('keydown', focusTrap);
    menuLinks[0]?.focus();
  };

  const closeMenu = () => {
    menu.classList.remove('open');
    overlay.classList.remove('open');
    document.body.classList.remove('menu-open');
    toggle.setAttribute('aria-expanded', 'false');
    document.removeEventListener('keydown', focusTrap);
    toggle.focus();
  };

  toggle.addEventListener('click', () => {
    menu.classList.contains('open') ? closeMenu() : openMenu();
  });

  overlay.addEventListener('click', closeMenu);
  menuLinks.forEach(link => link.addEventListener('click', closeMenu));
  
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && menu.classList.contains('open')) {
      closeMenu();
    }
  });
}
```

---

## 4. Recipe: Touch Targets & Haptic Feedback

On touch viewports (`max-width: 768px`):

- **Minimum Interactive Size:** Set clickable area (width and height) to at least `44x44px`. Add padding rather than increasing margin if space is tight.
- **Spacing:** Enforce a minimum gap of `8px` between adjacent touch elements.
- **Haptic Press Transition:** Set `-webkit-tap-highlight-color: transparent` and add active scaling:
  ```css
  a:active, button:active, [role="button"]:active {
    transform: scale(0.98) !important;
    opacity: 0.85 !important;
    transition: transform 0.1s ease, opacity 0.1s ease !important;
  }
  ```
- **Stuck Hover Prevention:** Wrap hover rules or disable them on touch devices to avoid stuck states after tapping:
  ```css
  @media (max-width: 768px) and (hover: none) {
    .clickable:hover {
      background: transparent !important;
      color: inherit !important;
    }
  }
  ```

---

## 5. Common Pitfalls & Anti-Patterns to Avoid

- **NEVER** set inline style attributes (like `element.style.clipPath`) that clash with stylesheet classes. This creates specificity overrides that break standard animation flow. Always animate using class toggles or CSS custom variables (`setProperty('--variable', value)`).
- **NEVER** use font sizes below `16px` on input fields. If you do, iOS Safari will automatically zoom the viewport on focus, breaking the page layout.
- **NEVER** design a fixed/sticky header without adding `scroll-margin-top` on anchor section elements. If omitted, the header will overlap the section titles when users navigate through anchor links.
- **NEVER** forget to include notch safe areas (`env(safe-area-inset-top)` / `env(safe-area-inset-bottom)`) on fixed header bars, sticky bottom buttons, or side-drawers. Without these, content will be clipped behind the screen cutouts on modern phones.
