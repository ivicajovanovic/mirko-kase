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

Determine project root dynamically:
```bash
PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
echo "PROJECT_ROOT=\"$PROJECT_ROOT\"" > /tmp/AGENT_SESSION.env
```

#### Phase 1: Project Identity
Ask the user:
1. Who is the client and what do they do? (Name, industry, one-sentence description)
2. Who are the primary site users? (Customers, partners, general public)
3. Which 3 words describe the visual tone the site MUST have? Which 3 words describe what the site DEFINITIVNO NE SME (definitely must not) be?

*Action:* Extract findings, propose a 1-sentence design direction, ask for confirmation. Upon confirmation, write `DESIGN_BRIEF.md` inside `$PROJECT_ROOT` (Identity Section).

#### Phase 2: Visual Identity
Ask the user:
1. Are there pre-defined brand colors? If yes, list hex codes. If no, what reference site or image describes the target mood?
2. Does a logo exist? If yes, what format?
3. Font preferences: Do you have specific font files/names, or should I propose options?

*Action:* Propose 2-3 options from the permitted typography list if choice is free. Do not write style code until colors and fonts are confirmed. Update `DESIGN_BRIEF.md` inside `$PROJECT_ROOT` (Visual Tokens Section).

#### Phase 3: Site Structure
Ask the user:
1. Which pages need to exist?
2. What sections must exist on the homepage, and in what order?
3. What is the most critical action (primary CTA) the user should take?

*Action:* Write `SITEMAP.md` inside `$PROJECT_ROOT` and display it to the user. Wait for confirmation.

#### Phase 4: Copy & Assets
Ask the user:
1. Do you have copy written? (If yes, organize it in COPY.md. If no, request permission to write placeholder copy).
2. What visual assets exist? (Logo path, photographs, icons)

*Action:* Create `COPY.md` and `ASSETS.md` inside `$PROJECT_ROOT`. Explicitly mark placeholders: `<!-- PLACEHOLDER: replace before launch -->`.

#### Bootstrap Transition & TBD Fallback
Before transitioning to Section 1, check for unassigned design tokens:
```bash
grep -n "TBD" DESIGN_BRIEF.md | wc -l
```

- **IF result > 0:**
  - List all TBD items in chat.
  - State: "These tokens are undefined. I will use SAFE DEFAULTS until you confirm final values. Safe defaults will be marked with `/* TBD-PLACEHOLDER */` in code for easy global find/replace."
  - Propose Safe defaults:
    - `--color-bg: #0f0e0d /* TBD-PLACEHOLDER */`
    - `--color-text: #f0ece4 /* TBD-PLACEHOLDER */`
    - `--color-muted: #6b6560 /* TBD-PLACEHOLDER */`
    - `--color-accent: #d4391a /* TBD-PLACEHOLDER */`
  - Do NOT block execution. Assign placeholders and proceed.

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
   - Documenting the addition: `/* AGENT: added baseline reset, not layout change */`.
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

## 6. LAYOUT DECISION TREE WITH DETECT-FIRST PROTOCOLS

### If building a Hero Section:
- **DETECT FIRST:**
  ```bash
  grep -rn "hero\|Hero\|banner" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF hero component exists:* Read it before modifying.
  - *IF hero component not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
  - **DO NOT** center-align elements.
  - **USE:** Offset asymmetrical grids, bold headlines spanning multiple lines with different indents (e.g., lines 1 and 3 left-aligned, line 2 indented by `7vw`), and high-contrast vertical dividers.

### If building a Features / Services List:
- **DETECT FIRST:**
  ```bash
  grep -rn "feature\|service\|Material\|Office" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF components exist:* Read them before modifying.
  - *IF components not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
  - **DO NOT** use grid cards.
  - **USE:** Numbered indexes (`01`, `02`), horizontal border lines as separators, and asymmetric grids where names and copies bleed into columns (like a technical spreadsheet).

### If building a Contact Form:
- **DETECT FIRST:**
  ```bash
  grep -rn "form\|commission\|contact" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF components exist:* Read them before modifying.
  - *IF components not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
  - **DO NOT** place labels inline next to inputs.
  - **DO NOT** use placeholder text as a label replacement.
  - **ALWAYS** place labels strictly above inputs.
  - **ALWAYS** make the submit button full-width on mobile.

### If building a Navigation / Header:
- **DETECT FIRST:**
  ```bash
  grep -n "nav\|header\|hamburger\|menu" src/ -r --include="*.css" --include="*.astro" --include="*.html"
  ```
  - *IF existing nav is found:*
    - Read current implementation.
    - Identify transition mechanism (e.g., toggled display vs transform).
    - IF `display: none / block` is used → Replace with transform drawer animation (see Recipe below).
    - IF `transform` is already implemented → Add only missing focus trap, scroll lock, and Escape listeners.
    - DO NOT restructure working markup layout; add functionality on top.
  - *IF no nav is found:*
    - Scaffold drawer layout from the Navigation Recipe.
    - Create `src/components/Nav.astro` (or relevant framework layout file).
    - Inject CSS styles into the main detected stylesheet.
  - *IF nav is a third-party module:*
    - Do NOT modify the core library styles.
    - Apply custom overriding styles externally. Document this in `/tmp/AGENT_LOG.md`.

### If building a Testimonials / Social Proof Section:
- **DETECT FIRST:**
  ```bash
  grep -rn "testimonial\|social-proof\|review" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF components exist:* Read them before modifying.
  - *IF components not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
  - **NEVER:** Use carousels with autoplay, infinite loops, or tiny dots.
  - **ALWAYS:** Stack them vertically on mobile, treating the quote as a styled block element with an asymmetric grid and numbered indexes.

### If building a Pricing Section:
- **DETECT FIRST:**
  ```bash
  grep -rn "price\|pricing\|tariff\|plan" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF components exist:* Read them before modifying.
  - *IF components not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
  - **NEVER:** Fit 3 columns or cards side-by-side on mobile.
  - **ALWAYS:** Stack them vertically or use an accordion layout with a sticky CTA button at the bottom viewport.

### If building a Modal / Drawer:
- **DETECT FIRST:**
  ```bash
  grep -rn "modal\|dialog\|drawer" src/ --include="*.astro" --include="*.html" --include="*.jsx" -l
  ```
  - *IF components exist:* Read them before modifying.
  - *IF components not found:* Scaffold layout using the rules below.
- **LAYOUT RULES:**
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

## 8. CODE RECIPES & EXCLUSION PROTOCOLS

### Fluid Typography
- **USE WHEN:** Sizing structural headings (`h1`, `h2`, display subtitles).
- **DO NOT USE ON:** Body text, menus, buttons, forms, labels, or microcopy. Use standard scaling (`rem`/`em`) instead.
- **Code:**
  ```css
  font-size: clamp(2rem, 8vw, 4.5rem);
  line-height: 1.05;
  ```

### Safe Area for Notch Devices
- **USE WHEN:** Elements are fixed/sticky at the viewport edges (header bars, bottom drawer toggles, back-to-top buttons).
- **DO NOT USE ON:** Inline sections, grid cards, elements centered in the flow, or normal static paragraphs.
- **Code:**
  ```css
  padding-bottom: calc(24px + env(safe-area-inset-bottom));
  padding-top: calc(8px + env(safe-area-inset-top));
  ```

### iOS Zoom Prevention on Inputs
- **USE WHEN:** Input fields, select forms, and textarea tags on responsive mobile views.
- **DO NOT USE ON:** Labels, form title wraps, buttons, helper text descriptions, or inputs on desktop views (keep specificity mobile-bound).
- **Code:**
  ```css
  input, select, textarea {
    font-size: 16px !important; /* Normalization: !important is allowed here */
  }
  ```

### 100vh Fix for Mobile Browsers
- **USE WHEN:** Full screen viewport sections (e.g., initial loading screens, full-height landing heroes).
- **DO NOT USE ON:** Standard content sections (forms, pricing lists, services), or any containers where vertical space should adjust to wrapped elements.
- **Code:**
  ```css
  min-height: 100dvh;
  ```

### Touch Feedback
- **USE WHEN:** Custom interactive links, anchors, or form action buttons.
- **DO NOT USE ON:** Native inputs (text, numbers, checkboxes), non-clickable cards, text elements, or decorative items.
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

### JavaScript: Hamburger Menu - Brutalist Drawer

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

### JavaScript: Scroll Reveal without Libraries (Intersection Observer)

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

## 11. AUTOMATED VERIFICATION & ESCALATION SEQUENCE
You MUST run these bash pipelines before finishing a task. Treat all non-empty results as failures.

### Execution Checks:

1. **Horizontal Overflow Check:**
   ```bash
   grep -rn "overflow-x\|min-width" src/ --include="*.css"
   ```
   *Expected:* `overflow-x: hidden` exists on body/main; no unconstrained viewport-exceeding min-widths.

2. **Font Size Legibility Audit:**
   ```bash
   grep -rn "font-size" src/ --include="*.css" | grep -v "clamp\|rem\|em\|16px\|var(" || true
   ```
   *Expected:* Empty/minimal output. No raw static pixel sizing overrides.

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
   *Expected:* Empty output.

8. **Token Consistency Check:**
   Find hardcoded hex colors outside root configuration files:
   ```bash
   grep -rn "#[0-9a-fA-F]\{3,6\}" src/styles/ --include="*.css" | grep -v ":root\|\/\*" || true
   ```
   *Expected:* Empty output. All custom values bind to `:root`.

---

### ESCALATION & AUTOFix FLOW
IF a verification pipeline check returns unexpected output/failure:
1. Log the failure state inside `/tmp/AGENT_LOG.md` under a `[TIMESTAMP] VERIFICATION` block.
2. Attempt an auto-fix ONLY if the issue belongs to this permitted category:
   - **Banned font found:** Replace font name references with `var(--font-display)` or `var(--font-mono)`.
   - **Rounded border-radius found:** Force override element target class rule to `border-radius: 0;`.
   - **Hardcoded hex value found:** Replace color declaration with the nearest matching `:root` semantic token.
   - **!important tag misused:** Strip the `!important` tag and correct specificity using nesting selectors.
3. Re-run the verification pipeline check once after making the correction.
4. IF the check continues to fail after the auto-fix attempt:
   - Immediately document the issue under the `CANNOT FIX` escalation log.
   - Proceed to run any remaining verification tests.
   - DO NOT loop on auto-fix adjustments. Run exactly **ONE** attempt, and then escalate to the human log.

---

## 12. HARD ESCALATION LIST (ZABRANJENE AUTONOMNE POPRAVKE)
You MUST NOT attempt to repair or resolve these visual layout issues yourself. Document the problem under `CANNOT FIX` in the agent log and stop execution:

- **WCAG contrast ratio failures:** Requires custom human/client aesthetic color judgment.
- **Third-party CSS library conflicts:** High risk of breaking core dependencies.
- **Missing font files:** Font files missing from `/public` or stylesheet references cannot be resolved autonomously.
- **Missing assets/logos:** Logo files referenced in assets but not physically located inside the repo.
- **Conflicting author media queries:** Layout breaking because multiple authors implemented clashing mobile bounds in parallel stylesheets.
- **Missing animation instructions:** Do not invent scroll motion paths or micro-interactions unless explicitly documented.
- **Database/API integrations affecting render blocks:** Keep adjustments within design layouts.

---

## 13. AGENT LOGGING PROTOCOL
You MUST maintain `AGENT_LOG.md` inside `/tmp/` throughout execution. Insert timestamps and state shifts programmatically:

```bash
# Append timestamp log block
echo "### $(date '+%Y-%m-%d %H:%M:%S') [PHASE-NAME]" >> /tmp/AGENT_LOG.md
```

### Log File Format:
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
