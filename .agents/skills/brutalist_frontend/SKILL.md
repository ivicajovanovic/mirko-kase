---
name: brutalist_frontend
description: Autonomous execution protocol, decision trees, code recipes, and bash verification scripts for building high-performance, controlled-tension brutalist web interfaces.
---

# Brutalist & Controlled-Tension Frontend Design Protocol

This document is a strict, autonomous execution protocol for building, modifying, and verifying responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 0. BOOTSTRAP MODE (READ BEFORE PROCEEDING)

### DETECT REPO STATE
Before writing any code or analyzing the workspace, execute this check:
```bash
ls -la | grep -E "DESIGN_BRIEF|SITEMAP|COPY|ASSETS"
```

- **IF output is empty (no foundation files found):**
  - DO NOT proceed to Section 1.
  - ENTER FOUNDATION INTERVIEW MODE (see below).
  - DO NOT write any code until all foundation files are created.
- **IF all 4 files exist:**
  - Read files in order: `DESIGN_BRIEF.md` → `SITEMAP.md` → `COPY.md` → `ASSETS.md`.
  - Proceed to Section 1 (Pre-Flight Context Discovery).
- **IF partial files exist:**
  - Identify and list missing files in chat.
  - Run interview prompts ONLY for missing files. Do not repeat interview questions for files that already exist.

---

### FOUNDATION INTERVIEW MODE
Guide the user through 4 phases, one by one. Wait for user input after each phase. Ask NO MORE than 3 questions at a time.

#### Phase 1: Project Identity
Ask the user:
1. Who is the client and what do they do? (Name, industry, one-sentence description)
2. Who are the primary site users? (Customers, partners, general public)
3. Which 3 words describe the visual tone the site MUST have? Which 3 words describe what the site DEFINITIVNO NE SME (definitely must not) be?

*Action:* Extract findings, propose a 1-sentence design direction, ask for confirmation. Upon confirmation, write `/home/ivica1979/projekti/mirko-kase/DESIGN_BRIEF.md` (Identity Section).

#### Phase 2: Visual Identity
Ask the user:
1. Are there pre-defined brand colors? If yes, list hex codes. If no, what reference site or image describes the target mood?
2. Does a logo exist? If yes, what format?
3. Font preferences: Do you have specific font files/names, or should I propose options?

*Action:* Propose 2-3 options from the permitted typography list if choice is free. Do not write style code until colors and fonts are confirmed. Update `/home/ivica1979/projekti/mirko-kase/DESIGN_BRIEF.md` (Visual Tokens Section).

#### Phase 3: Site Structure
Ask the user:
1. Which pages need to exist?
2. What sections must exist on the homepage, and in what order?
3. What is the most critical action (primary CTA) the user should take?

*Action:* Write `/home/ivica1979/projekti/mirko-kase/SITEMAP.md` and display it to the user. Wait for confirmation.

#### Phase 4: Copy & Assets
Ask the user:
1. Do you have copy written? (If yes, organize it in COPY.md. If no, request permission to write placeholder copy).
2. What visual assets exist? (Logo path, photographs, icons)

*Action:* Create `/home/ivica1979/projekti/mirko-kase/COPY.md` and `/home/ivica1979/projekti/mirko-kase/ASSETS.md`. Explicitly mark placeholders: `<!-- PLACEHOLDER: replace before launch -->`.

#### Bootstrap Transition
Once all 4 files are saved, present the summary of tokens, fonts, pages, and copy status. Ask the user for priority on what page to build first. Then transition to Section 1.

#### Bootstrap Rules
- NEVER ask more than 3 questions at once.
- ALWAYS wait for confirmation before saving files or transitioning phases.
- NEVER invent brand colors or fonts without explicit confirmation.
- If user skips a question, mark as `TBD` in the file. Do not block progress.

---

## 1. PRE-FLIGHT CONTEXT DISCOVERY
You MUST programmatically discover and document the codebase state. Execute the following sequence:

### Framework & Styling Discovery
1. **Framework Type:**
   Check dependencies:
   ```bash
   cat package.json | grep -E "tailwind|vite|next|astro|svelte|react"
   ```
2. **Tailwind Check:**
   Verify if Tailwind classes are used:
   ```bash
   grep -r "tailwind\|@apply\|className=" src/ --include="*.{js,jsx,ts,tsx,astro,html}" -l | head -n 10
   ```
3. **CSS Modules Check:**
   Locate component module stylesheets:
   ```bash
   find src/ -name "*.module.css" -o -name "*.module.scss"
   ```
4. **Fallback:** If framework cannot be determined, default to **Vanilla CSS** and log this assumption in `/tmp/AGENT_LOG.md`.

### Brand Color Discovery
Extract existing hex color usage from the codebase:
```bash
grep -r "color\|background\|#[0-9a-fA-F]" src/ --include="*.css" -h 2>/dev/null | sort | uniq | head -n 30
```
Use these extracted values as the source of truth for the token skeleton. Do not invent new colors.

---

## 2. FILE DISCOVERY SEQUENCE
To prevent duplicating classes and modifying incorrect stylesheets, locate target files:

1. **Locate Design Tokens / Root Variables:**
   ```bash
   find . -name "*.css" | xargs grep -l ":root" 2>/dev/null
   find . -name "tokens.*" -o -name "variables.*" -o -name "theme.*" 2>/dev/null
   ```
2. **Locate Existing Media Queries:**
   ```bash
   grep -rn "@media" src/ --include="*.css" --include="*.module.css"
   ```
3. **Locate Target Component Files:**
   ```bash
   find src/ -name "*.astro" -o -name "*.jsx" -o -name "*.tsx" -o -name "*.html" | head -n 30
   ```
4. **Locate Mobile Breakpoint References:**
   ```bash
   grep -rn "max-width\|min-width\|768\|480\|375" src/ --include="*.css"
   ```
5. **Pre-flight Documentation:**
   Write your discoveries to `/tmp/agent_preflight.md` before writing layout edits.

---

## 3. CHANGE ISOLATION PROTOCOL
To protect the desktop viewport configuration and prevent layout regression, adhere to these rules:

1. **Backup Source Files:**
   Before making edits, backup the stylesheet:
   ```bash
   cp src/styles/global.css /tmp/global.css.bak
   ```
2. **Media Query Isolation:**
   NEVER write or edit CSS properties outside `@media` queries unless:
   - Defining a new global custom variable inside `:root`.
   - Adding missing baseline resets (e.g. `box-sizing: border-box`).
   - Documenting the addition: `/* AGENT: added baseline reset, no layout change */`.
3. **Verification via Diff:**
   After every file change, run a diff verification check:
   ```bash
   diff /tmp/global.css.bak src/styles/global.css || true
   ```
   If the diff reveals visual changes outside `@media` blocks that are not documented exceptions, REVERT and re-approach.

---

## 4. STRICT ASSUMPTION LIMITS
- **NEVER** assume the user wants animations (verify or await specific specs).
- **NEVER** assume fonts are pre-loaded (always include `@font-face` or Google Fonts `@import` inside CSS if not present).
- **NEVER** assume a global reset exists (always check for `box-sizing: border-box` and margins resets).
- **NEVER** assume colors can be hardcoded (always define and use `:root` CSS variables).
- **NEVER** assume desktop code is safe when editing mobile layouts (always validate views on `1440px` and `768px`/`480px`).

---

## 5. PROHIBITED DEFAULT LIST
To prevent generating generic, average internet outputs, you are strictly BANNED from using:
- **BANNED Typefaces:** `Inter`, `Roboto`, `Poppins`, `Open Sans` (default AI font output). Use system serifs (e.g., `Didot`, `Times New Roman`, `Bodoni`) for headings and strict monospaces (e.g., `JetBrains Mono`, `Courier New`) for UI labels.
- **BANNED Hero Layout:** Centered hero headline with a subtitle paragraph and two adjacent pill buttons.
- **BANNED Features Section:** 3-column generic card layouts with thin borders, soft shadows, and icons in circles.
- **BANNED Styling:** Purple/blue gradients, standard glassmorphism, soft blur shadows, and rounded buttons (`border-radius: 0 !important` is mandatory).
- **BANNED Touch Targets:** Any clickable elements under `44x44px` or touch items spaced closer than `8px` apart.

---

## 6. LAYOUT DECISION TREE WITH DETERMINISTIC FALLBACKS

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
- **DETECT:**
  ```bash
  grep -n "nav\|header\|hamburger\|menu" src/ -r --include="*.css" --include="*.astro"
  ```
- **IF existing nav is found:**
  - Read current implementation.
  - Identify transition mechanism (e.g., toggled display vs transform).
  - IF `display: none / block` is used → Replace with transform drawer animation (see Recipe below).
  - IF `transform` is already implemented → Add only missing focus trap, scroll lock, and Escape listeners.
  - DO NOT restructure working markup layout; add functionality on top.
- **IF no nav is found:**
  - Scaffold drawer layout from the Navigation Recipe.
  - Create `src/components/Nav.astro` (or relevant framework layout file).
  - Inject CSS styles into the main detected stylesheet.
- **IF nav is a third-party module:**
  - Do NOT modify the core library styles.
  - Apply custom overriding styles externally. Document this in `/tmp/AGENT_LOG.md`.

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

## 7. BRUTALIST TOKEN SKELETON

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

## 8. CODE RECIPES & PLACEMENT PROCEDURES

### Fluid Typography
- **Usage:** Set heading sizes.
- **Code:**
  ```css
  font-size: clamp(2rem, 8vw, 4.5rem);
  line-height: 1.05;
  ```

### Safe Area for Notch Devices
- **Usage:** Fixed headers, floating back-to-top buttons, sticky menus.
- **Code:**
  ```css
  padding-bottom: calc(24px + env(safe-area-inset-bottom));
  padding-top: calc(8px + env(safe-area-inset-top));
  ```

### iOS Zoom Prevention on Inputs
- **Usage:** Input normalizations.
- **Code:**
  ```css
  input, select, textarea {
    font-size: 16px !important; /* Normalization: !important is allowed here */
  }
  ```

### 100vh Fix for Mobile Browsers
- **Usage:** Full viewport heights.
- **Code:**
  ```css
  min-height: 100dvh;
  ```

### Touch Feedback
- **Usage:** Mobile interactive links/buttons.
- **Code:**
  ```css
  a, button, input, select, textarea, [role="button"] {
    -webkit-tap-highlight-color: transparent !important; /* Normalization: !important is allowed here */
  }
  a:active, button:active, .submit:active, [role="button"]:active {
    transform: scale(0.98);
    opacity: 0.85;
    transition: transform 0.1s ease, opacity 0.1s ease;
  }
  ```

### Hamburger Menu Script (Mobile Menu Interactivity)

#### Placement Decision:
- **IF Astro:** Save to `src/scripts/nav.ts` and import via `<script>` inside `Layout.astro` or `Nav.astro`.
- **IF Next.js / React:** Implement as React state and load with `useEffect()` inside the navigation component.
- **IF Vanilla HTML:** Place script tag immediately before the closing `</body>` tag of `index.html`.
- **Verification:** Confirm tags match markup:
  ```bash
  grep -r "data-nav\|data-nav-toggle" src/
  ```

#### Implementation Code:
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

document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && toggle.getAttribute('aria-expanded') === 'true') {
    toggle.click();
  }
});
```

### Intersection Observer Script (Scroll Reveal without Libraries)

#### Placement Decision:
- Save to main scripts or index bundle, target element selector attributes matching `[data-reveal]`.

#### Implementation Code:
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

**Step 1:** Synchronous head detection (Layout.astro):
```html
<head>
  <script is:inline>
    if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
      document.documentElement.classList.add('js');
    }
  </script>
</head>
```
**Step 2:** CSS variable override (global.css):
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

## 9. !important PROTOCOL
- **NEVER** use `!important` as a shortcut for specificity problems in your layout code. Solve the issue using proper CSS selector specificity.
- **USE ONLY** for browser normalizations (like iOS zoom fix or disabling default tap highlight).
- **USE ONLY** to override styling from third-party libraries (e.g. Google Maps default styles, libraries).
- **NEVER** use it on custom grid, layout, or spacing values.

---

## 10. COMPARATIVE CODE EXAMPLES

### Example 1: Service List Row on Mobile

#### ❌ BAD (Generic, squeezed layout):
```css
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
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Horizontally squishes on mobile */
}
```

#### ✅ GOOD (Stacked vertical layout with clean spacing):
```css
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
.drawer {
  display: none;
}
.drawer.open {
  display: block; /* No transitions or animations supported */
}
```

#### ✅ GOOD (Stepped transform translation):
```css
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

## 11. AUTOMATED VERIFICATION SEQUENCE
Before finishing execution, you MUST run these command checks to programmatically audit your changes:

1. **Horizontal Overflow Check:**
   ```bash
   grep -rn "overflow-x\|min-width" src/ --include="*.css"
   ```
   *Expected:* `overflow-x: hidden` is on body/main; no viewport-exceeding min-widths are present.
2. **Font Size Legibility Audit:**
   ```bash
   grep -rn "font-size" src/ --include="*.css" | grep -v "clamp\|rem\|em\|16px\|var(" || true
   ```
   *Expected:* Empty/minimal output. No raw small pixel sizing overrides.
3. **Media Query Isolation Verification:**
   Verify mobile rules are encapsulated:
   ```bash
   grep -n "768\|480\|mobile" src/styles/*.css
   ```
4. **Touch Target Size Check:**
   Ensure dimension declarations are compliant:
   ```bash
   grep -rn "width\|height\|padding" src/ --include="*.css" | grep -E "[0-9]{1,2}px" | grep -v "@media" | head -n 30
   ```
5. **Banned Fonts Check:**
   ```bash
   grep -rn "Inter\|Roboto\|Poppins\|Open Sans" src/ --include="*.css" --include="*.html" --include="*.astro" || true
   ```
   *Expected:* Empty output.
6. **Border Radius Audit:**
   Verify sharp brutalist borders:
   ```bash
   grep -rn "border-radius" src/ --include="*.css" | grep -v "border-radius: 0\|var(" || true
   ```
   *Expected:* Empty output or variable binds.
7. **!important Protocol Verification:**
   ```bash
   grep -rn "!important" src/ --include="*.css" | grep -v "tap-highlight\|font-size.*16px\|text-size-adjust" || true
   ```
   *Expected:* Empty output. Only normalization !important values allowed.
8. **Token Consistency Check:**
   Find hardcoded hex colors outside root configuration files:
   ```bash
   grep -rn "#[0-9a-fA-F]\{3,6\}" src/styles/ --include="*.css" | grep -v ":root\|\/\*" || true
   ```
   *Expected:* Empty output. All custom values bind to `:root`.

---

## 12. AGENT LOGGING PROTOCOL
You MUST maintain `/tmp/AGENT_LOG.md` throughout your execution. Populate and update the file after each phase:

```markdown
## Format:
### [TIMESTAMP] PRE-FLIGHT
- Framework detected: [result]
- Token file found at: [path or NONE]
- Existing breakpoints: [list]
- Assumptions made: [list with reasoning]

### [TIMESTAMP] CHANGES MADE
- File: [path]
- Change: [what and why]
- Scope: [mobile-only / token addition / reset]

### [TIMESTAMP] VERIFICATION
- Command: [grep command run]
- Result: [output]
- Status: PASS / FAIL / NEEDS MANUAL REVIEW

### [TIMESTAMP] CANNOT FIX (escalation to human required)
- Issue: [description]
- Reason: [why agent cannot fix autonomously]
- Recommended action: [what human should do]
```
