---
name: brutalist_frontend
description: Autonomous execution protocol, decision trees, code recipes, and bash verification scripts for building high-performance, controlled-tension brutalist web interfaces.
---

# Brutalist & Controlled-Tension Frontend Design Protocol

This document is a strict, autonomous execution protocol for building, modifying, and verifying responsive, high-performance, and visually distinctive "Brutalist" or "Controlled-Tension" frontend layouts.

---

## 0. BOOTSTRAP MODE (READ BEFORE PROCEEDING)

### DETECT REPO STATE & PROJECT ROOT
Before analyzing the workspace, execute this check:
```bash
PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
echo "PROJECT_ROOT=\"$PROJECT_ROOT\"" > /tmp/AGENT_SESSION.env
ls -la | grep -E "DESIGN_BRIEF|SITEMAP|COPY|ASSETS"
```
- **IF output is empty (no foundation files):** Stop layout work. Guide the user through the 4-phase interview below (max 3 questions at once). Write outputs to `$PROJECT_ROOT`.
- **IF all 4 files exist:** Read them in order: `DESIGN_BRIEF.md` → `SITEMAP.md` → `COPY.md` → `ASSETS.md`. Proceed to Section 1.
- **IF partial files exist:** Interview only for missing files.

### FOUNDATION INTERVIEW SCRIPT
- **Phase 1: Project Identity** -> Ask: 1. Client name/industry description? 2. Target audience? 3. 3 words describing visual tone & 3 words describing what it DEFINITELY MUST NOT be? (Save to `DESIGN_BRIEF.md`).
- **Phase 2: Visual Identity** -> Ask: 1. Hex brand colors or references? 2. Logo format? 3. Font preferences? (Save to `DESIGN_BRIEF.md`).
- **Phase 3: Structure** -> Ask: 1. Pages needed? 2. Homepage sections order? 3. Primary CTA action? (Save to `SITEMAP.md`).
- **Phase 4: Copy & Assets** -> Ask: 1. Raw copy? 2. Visual assets paths? (Save to `COPY.md` & `ASSETS.md` using `<!-- PLACEHOLDER -->` for missing parts).

**TBD Fallback Check:** Before Section 1, run `grep -n "TBD" DESIGN_BRIEF.md | wc -l`. If > 0, announce fallback placeholders to user and proceed:
- `--color-bg: #0f0e0d /* TBD-PLACEHOLDER */`
- `--color-text: #f0ece4 /* TBD-PLACEHOLDER */`
- `--color-muted: #6b6560 /* TBD-PLACEHOLDER */`
- `--color-accent: #d4391a /* TBD-PLACEHOLDER */`

---

## 1. PRE-FLIGHT DISCOVERY & CHANGE ISOLATION

### Discovery Commands
Execute this block to detect framework type and active hex colors:
```bash
cat package.json | grep -E "tailwind|vite|next|astro|svelte|react" || echo "Framework: Vanilla"
grep -r "tailwind\|@apply\|className=" src/ --include="*.{js,jsx,ts,tsx,astro,html}" -l | head -n 3
find src/ -name "*.module.css"
grep -r "color\|background\|#[0-9a-fA-F]" src/ --include="*.css" -h 2>/dev/null | sort | uniq | head -n 10
find . -name "*.css" | xargs grep -l ":root" 2>/dev/null
grep -rn "@media" src/ --include="*.css" --include="*.module.css" | head -n 10
```
Write all results to `/tmp/agent_preflight.md` before coding.

### Change Isolation Rules
1. **Backup:** Always run `cp src/styles/global.css /tmp/global.css.bak`.
2. **Isolation:** NEVER edit CSS outside `@media` queries unless creating global `:root` variables or baseline resets (e.g. `box-sizing: border-box`). Mark resets with `/* AGENT: reset */`.
3. **Verification:** Run `diff /tmp/global.css.bak src/styles/global.css || true`. Revert changes immediately if layout regression occurs outside mobile queries.

---

## 2. STRICT RULES & PROHIBITIONS

### Assumption Limits
- **NEVER** assume animations are wanted (verify first).
- **NEVER** assume fonts are pre-loaded (always include `@font-face` or Google `@import`).
- **NEVER** assume a global reset exists (always check for box-sizing resets).
- **NEVER** assume colors can be hardcoded (always use `:root` CSS variables).

### Banned Features
- **Banned Typefaces:** `Inter`, `Roboto`, `Poppins`, `Open Sans`. Use serifs for headings (`Didot`, `Times`) and monospaces for UI (`JetBrains Mono`, `Courier`).
- **Banned Hero Layout:** Centered hero text with subtitle and two adjacent pill buttons.
- **Banned Cards:** 3-column generic grid cards with thin borders and circular icons.
- **Banned Styling:** Purple/blue gradients, glassmorphism, soft blur shadows, and rounded buttons (`border-radius: 0 !important`).
- **Banned Touch Targets:** Clickable areas under `44x44px` or touch spacing under `8px`.

---

## 3. LAYOUT DECISION TREE

Execute `DETECT FIRST` command for the section you are modifying:

| Section | Detection Command | Layout Rules & Fallbacks |
| :--- | :--- | :--- |
| **Hero** | `grep -rn "hero" src/` | No centering. Use offset asymmetrical grids, multiline headings with custom indents (`7vw`), and heavy vertical line separators. |
| **Features** | `grep -rn "feature\|service" src/` | No card grids. Use numbered indexes (`01`, `02`), horizontal border lines, and asymmetric lists where copies bleed into columns. |
| **Form** | `grep -rn "form\|contact" src/` | Labels strictly above inputs, never placeholder as label, submit buttons full-width on mobile. |
| **Navigation** | `grep -n "nav\|header\|menu" src/` | **IF found:** Inspect transition. If using `display:none/block` replace with translate drawer. If transform exists, add focus trap and Escape key overrides. **IF not found:** Scaffold navigation from drawer recipe. **IF third-party:** Wrapper CSS overrides only. |
| **Social Proof** | `grep -rn "testimonial\|review" src/` | No autoplay sliders/carousels. Use single vertical stack on mobile and raw blockquotes. |
| **Pricing** | `grep -rn "price\|pricing" src/` | No 3-column rows on mobile. Use vertical stack or mobile accordion with sticky bottom CTA. |
| **Modal** | `grep -rn "modal\|dialog\|drawer" src/` | Enforce `overscroll-behavior: contain` on containers. Add `aria-modal="true"` and Focus Trap. |

---

## 4. BRUTALIST TOKEN SKELETON

Use this variables schema in `:root`. Consult the user to replace placeholders with custom values:

```css
:root {
  /* Typography — DEFINE IN COORDINATION WITH THE CLIENT / USER */
  --font-display: <client-display-serif-font>, Georgia, serif;
  --font-mono:    <client-monospace-font>, monospace;
  
  /* Colors — Max 4. DEFINE IN COORDINATION WITH THE CLIENT / USER */
  --color-bg:     <primary-bg-color>;      /* dominant background */
  --color-text:   <primary-text-color>;    /* highly legible text */
  --color-muted:  <muted-label-color>;     /* secondary text, labels */
  --color-accent: <brand-accent-color>;    /* single accent color */
  
  /* Borders & Spacing */
  --line: 1px; --line-heavy: 2px;
  --space-xs: clamp(0.5rem, 1vw, 0.75rem);
  --space-sm: clamp(0.75rem, 2vw, 1rem);
  --space-md: clamp(1rem, 3vw, 1.5rem);
  --space-lg: clamp(2rem, 5vw, 3rem);
  --space-xl: clamp(3rem, 8vw, 6rem);
}
```
*Note: As an agent, you MUST NOT assume the client's colors. Propose a visual direction and get confirmation before writing styling.*

---

## 5. CODE RECIPES & EXCLUSION PROCEDURES

### Fluid Typography
- **USE ON:** Headings. **DO NOT USE ON:** Body text, menus, buttons, forms, or labels.
- **Code:** `font-size: clamp(2rem, 8vw, 4.5rem); line-height: 1.05;`

### Safe Area Notch
- **USE ON:** Sticky headers, drawers, fixed bottom items. **DO NOT USE ON:** Inline elements, block grid columns.
- **Code:** `padding-bottom: calc(24px + env(safe-area-inset-bottom)); padding-top: calc(8px + env(safe-area-inset-top));`

### iOS Zoom Prevention
- **USE ON:** Mobile form input fields. **DO NOT USE ON:** Text layout wraps, desktop forms.
- **Code:** `input, select, textarea { font-size: 16px !important; }` *(Note: !important is allowed here for normalization)*.

### 100vh Fix
- **USE ON:** Full-screen views. **DO NOT USE ON:** Standard content containers, forms, lists.
- **Code:** `min-height: 100dvh;`

### Touch Feedback
- **USE ON:** Custom anchors, buttons. **DO NOT USE ON:** Native text inputs, non-clickable cards.
- **Code:**
  ```css
  a, button, [role="button"] { -webkit-tap-highlight-color: transparent !important; }
  a:active, button:active, [role="button"]:active { transform: scale(0.98); opacity: 0.85; transition: transform 0.1s ease; }
  ```

### Hamburger Menu Drawer Script (Astro -> save to `src/scripts/nav.ts` / HTML -> append before `</body>`)
```javascript
const nav = document.querySelector('[data-nav]');
const toggle = document.querySelector('[data-nav-toggle]');
const focusable = nav.querySelectorAll('a, button, [tabindex]');
const firstFocus = focusable[0];
const lastFocus = focusable[focusable.length - 1];

const focusTrap = (e) => {
  if (e.key === 'Tab') {
    if (e.shiftKey && document.activeElement === firstFocus) { lastFocus.focus(); e.preventDefault(); }
    else if (!e.shiftKey && document.activeElement === lastFocus) { firstFocus.focus(); e.preventDefault(); }
  }
};
toggle.addEventListener('click', () => {
  const isOpen = toggle.getAttribute('aria-expanded') === 'true';
  toggle.setAttribute('aria-expanded', String(!isOpen));
  nav.setAttribute('data-open', String(!isOpen));
  document.body.style.overflow = isOpen ? '' : 'hidden';
  if (!isOpen) { document.addEventListener('keydown', focusTrap); firstFocus.focus(); }
  else { document.removeEventListener('keydown', focusTrap); }
});
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && toggle.getAttribute('aria-expanded') === 'true') { toggle.click(); }
});
```

### Intersection Observer Script
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.setAttribute('data-visible', 'true'); observer.unobserve(e.target); }
  });
}, { threshold: 0.15 });
document.querySelectorAll('[data-reveal]').forEach(el => observer.observe(el));
```

### Anti-Flicker Image Reveal (Head script block -> CSS class hidden state -> JS var animation override)
- **Head:** `<script is:inline>if(!window.matchMedia('(prefers-reduced-motion: reduce)').matches){document.documentElement.classList.add('js');}</script>`
- **CSS:** `.hero { --hero-cut: 44%; } .hero-image { clip-path: inset(0 0 0 var(--hero-cut)); } html.js .hero { --hero-cut: 100%; }`
- **JS:** `const setHeroCut = (v) => heroElement.style.setProperty('--hero-cut', `${v}%`);`

---

## 6. !important PROTOCOL
- **NEVER** use `!important` as a shortcut for specificity problems in custom layout or grid CSS rules.
- **USE ONLY** for browser normalizations (iOS input zoom overrides, disabling default tap highlight) or to override third-party library assets (e.g. Google Maps, wrapper integrations).

---

## 7. COMPARATIVE CODE EXAMPLES

### Scenario: Service List & Pricing columns stack on Mobile viewports

#### ❌ BAD (Generic, horizontal overflow, too small typography):
```css
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Squishes columns horizontally on mobile */
}
.service-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
}
.service-desc {
  font-size: 11px; /* Illegible on screens */
  letter-spacing: 0.2em; /* Bad legibility density */
}
```

#### ✅ GOOD (Responsive vertical column stacks, legible clamps, no borders-radius):
```css
@media (max-width: 768px) {
  .pricing-grid {
    grid-template-columns: 1fr; /* Single column stack */
    gap: var(--space-md);
  }
  .pricing-card {
    border: var(--line) solid var(--color-text);
    border-radius: 0; /* Strict brutalist sharp corners */
  }
  .service-item {
    display: grid;
    grid-template-columns: 1fr;
    padding: 16px 0;
    border-bottom: var(--line) solid var(--color-bg);
  }
  .service-title {
    font-size: clamp(1.8rem, 8vw, 2.5rem);
    font-family: var(--font-display);
    word-break: break-word;
    overflow-wrap: break-word;
  }
  .service-desc {
    font-size: 16px; /* High legibility standard */
    line-height: 1.55;
    letter-spacing: 0.02em;
  }
}
```

---

## 8. QA AUDIT & ESCALATION SEQUENCE

### Automated Verification Script
Run these bash queries before finishing the task. Treat all non-empty outputs as failures:
```bash
# 1. Overflow & width checks
grep -rn "overflow-x\|min-width" src/ --include="*.css"
# 2. Raw px sizes
grep -rn "font-size" src/ --include="*.css" | grep -v "clamp\|rem\|em\|16px\|var(" || true
# 3. Mobile Queries isolation
grep -n "768\|480\|mobile" src/styles/*.css
# 4. Banned fonts usage
grep -rn "Inter\|Roboto\|Poppins\|Open Sans" src/ --include="*.css" --include="*.html" --include="*.astro" || true
# 5. Border-radius overrides
grep -rn "border-radius" src/ --include="*.css" | grep -v "border-radius: 0\|var(" || true
# 6. Misused !important tags
grep -rn "!important" src/ --include="*.css" | grep -v "tap-highlight\|font-size.*16px\|text-size-adjust" || true
# 7. Hardcoded Hexes
grep -rn "#[0-9a-fA-F]\{3,6\}" src/styles/ --include="*.css" | grep -v ":root\|\/\*" || true
```

### Auto-Fix & Escalation Rules
- **Permitted Auto-Fixes (Max 1 attempt, do NOT loop):**
  - *Banned font:* Swap with `var(--font-display)` or `var(--font-mono)`.
  - *Border radius:* Override to `border-radius: 0;`.
  - *Hardcoded hex:* Swap with nearest `:root` color token.
  - *Misused !important:* Remove and correct selector specificity.
- **Escalate immediately (Mark under CANNOT FIX and stop):**
  - WCAG contrast failures, missing public font/logo files, clashing media query breakpoints from multiple authors, API/database layout impacts, and undefined animation specifications.

---

## 9. AGENT LOGGING PROTOCOL
Maintain a log at `/tmp/AGENT_LOG.md`. Insert log phase entries programmatically:
```bash
echo "### $(date '+%Y-%m-%d %H:%M:%S') [PHASE]" >> /tmp/AGENT_LOG.md
```
Log structure details MUST match:
- **PRE-FLIGHT:** Framework, token files found, existing breakpoints, active assumptions.
- **CHANGES MADE:** File path, change description, change scope.
- **VERIFICATION:** Command run, output result, pass/fail status.
- **CANNOT FIX:** Description of issue, reason for failure, recommended manual actions.

---

## 10. SESSION HANDOFF
At the end of every execution session, write to `$PROJECT_ROOT/AGENT_LOG.md`:

### HANDOFF NOTE
- Last completed task: [description]
- Next recommended task: [from SITEMAP.md]  
- Open TBD items: [list from DESIGN_BRIEF.md]
- Files modified this session: [list]
- Verification status: [PASS / PARTIAL / NEEDS REVIEW]
