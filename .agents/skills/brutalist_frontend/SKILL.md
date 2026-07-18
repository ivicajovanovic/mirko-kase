---
name: brutalist_frontend
description: Rules, decision trees, code recipes, and pre-flight validation checklists for building high-performance, controlled-tension brutalist web interfaces.
---

# Brutalist & Controlled-Tension Frontend Design Protocol

This document is a strict execution protocol for building responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 1. PRE-FLIGHT CONTEXT DISCOVERY (PRE NEGO ŠTO POČNEŠ)
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

## 5. BRUTALIST TOKEN SKELETON (DIZAJN TOKENI)

Use this architectural skeleton to organize the typography and color tokens. You MUST NOT use default values without consulting the user. Proactively prompt the user to define these in coordination with the client.

```css
:root {
  /* Tipografija — DEFINIŠI U SARADNJI SA KLIJENTOM / KORISNIKOM */
  --font-display: <client-display-serif-font>, Georgia, serif;
  --font-mono:    <client-monospace-font>, monospace;
  
  /* Boje (Maksimalno 4) — DEFINIŠI U SARADNJI SA KLIJENTOM / KORISNIKOM */
  --color-bg:     <primary-bg-color>;      /* dominantna pozadina */
  --color-text:   <primary-text-color>;    /* čitljiv tekst */
  --color-muted:  <muted-label-color>;     /* sekundarni tekst, labele */
  --color-accent: <brand-accent-color>;    /* jedina akcentna boja (retko se koristi) */
  
  /* Linije i Konstrukcije */
  --line:         1px;
  --line-heavy:   2px;
  
  /* Spacing skala (fluidne clamp vrednosti) */
  --space-xs:  clamp(0.5rem, 1vw, 0.75rem);
  --space-sm:  clamp(0.75rem, 2vw, 1rem);
  --space-md:  clamp(1rem, 3vw, 1.5rem);
  --space-lg:  clamp(2rem, 5vw, 3rem);
  --space-xl:  clamp(3rem, 8vw, 6rem);
}
```

> [!IMPORTANT]
> **ESTETIKA I IDENTITET SE NE PREUZIMAJU AUTOMATSKI IZ OVOG ŠABLONA.**
> Kao agent, ne smeš pretpostavljati boje i fontove klijenta. Ako klijent nema definisan identitet, tvoja je dužnost da predložiš vizuelni pravac (npr. industrijski tamni mod ili oštri tehnički kontrast) i zatražiš eksplicitan pristanak pre nego što popuniš varijable konkretnim vrednostima.

---

## 6. CODE RECIPES (KODNI RECEPTI)

Use these copy-pasteable CSS/HTML/JS values for common layout problems:

### Fluid Typography (Fluidna tipografija)
```css
/* Fluid clamp scaling, preventing fixed px or overflow */
font-size: clamp(2rem, 8vw, 4.5rem);
line-height: 1.05;
```

### Safe Area for Notch Devices (Notch podrška)
```css
/* Add safe area inset offsets to bottom/top positioning */
padding-bottom: calc(24px + env(safe-area-inset-bottom));
padding-top: calc(8px + env(safe-area-inset-top));
```

### iOS Zoom Prevention on Inputs (Sprečavanje iOS auto-zooma)
```css
/* iOS zooms viewport automatically on focus if font-size < 16px */
input, select, textarea {
  font-size: 16px !important; /* Normalization: !important is allowed here */
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
  -webkit-tap-highlight-color: transparent !important; /* Normalization: !important is allowed here */
}

/* Active touch press feedback transition */
a:active, button:active, .submit:active, [role="button"]:active {
  transform: scale(0.98);
  opacity: 0.85;
  transition: transform 0.1s ease, opacity 0.1s ease;
}
```

### JavaScript: Hamburger Menu - Brutalist Drawer (Mobilni Meni)
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

## 7. !important PROTOCOL (KADA SE KORISTI)
- **NEVER** use `!important` as a shortcut for specificity problems in your layout code. Solve the issue using proper CSS selector specificity.
- **USE ONLY** for browser normalizations (like iOS zoom fix or disabling default tap highlight).
- **USE ONLY** to override styling from third-party libraries (e.g. Google Maps default styles, libraries).
- **NEVER** use it on custom grid, layout, or spacing values.

---

## 8. COMPARATIVE CODE EXAMPLES (PRIMERI OUTPUTA)

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

## 9. PRE-FLIGHT OUTPUT CHECKLIST (IZLAZNA ČEKLISTA)
Before sending the final output, verify:
- [ ] **Viewport Check:** Did you test on `1440px` to guarantee that the desktop layout remains 100% identical?
- [ ] **Mobile Isolation:** Are all mobile layout additions encapsulated inside `@media (max-width: 768px)` or `@media (max-width: 480px)`?
- [ ] **Typographic Clamp:** Do all scalable text variables use `clamp()` instead of raw `vw` units?
- [ ] **Input Zoom:** Are all form input font-sizes `>= 16px` to prevent iOS viewport auto-zooms?
- [ ] **Accessibility Contrast:** Do all text elements meet at least WCAG AA (4.5:1) color contrast ratios?
- [ ] **Scroll Padding:** Do all section wrappers with anchor links contain a `scroll-margin-top` to prevent header overlapping?
- [ ] **Touch Target Size:** Are all interactive touch elements at least `44x44px` in clickable size?
- [ ] **Overscroll Containment:** Is `overscroll-behavior: contain` attached to fixed drawers or modal popups?

## 10. BRUTALIST IDENTITY CHECK (PROVERA ESTETIKE)
Verify that the output strictly adheres to the brutalist aesthetic:
- [ ] Postoji li ijedan element sa border-radius > 0? → Ukloni ili zaokruži na 0.
- [ ] Koristi li se ijedan od zabranjenih fontova (Inter, Roboto, Poppins, Open Sans)? → Zameni.
- [ ] Ima li sekcija koja ponavlja layout prethodne? → Razbij je drugim layout modelom (npr. tabela umesto kolona).
- [ ] Ima li centrirani hero? → Restrukturiraj u asimetrični raspored.
- [ ] Ima li card grida sa 3 kolone na mobilnom? → Konvertuj u vertikalno skiciranu listu.
- [ ] Ima li mekih senki (soft box-shadow) na ijednom elementu? → Ukloni ili zameni oštrim crnim senkama (`box-shadow: 4px 4px 0px 0px var(--color-bg)`).
- [ ] Jesu li sva dugmad oštrih ivica (border-radius: 0)?
- [ ] Postoji li barem jedan elemenat koji "krši" grid pravila? (namerna tenzija, npr. asimetrični prelaz ili negativna margina koja gura element u stranu).
