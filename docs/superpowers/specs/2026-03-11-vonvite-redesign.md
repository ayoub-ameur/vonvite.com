# Vonvite Website Redesign Spec

**Date:** 2026-03-11
**Goal:** Convert freelance leads — Moroccan businesses discovering Vonvite online
**Language:** French
**Tech stack:** Pure HTML / CSS / JS (no framework), Formspree for contact form

---

## Design Direction

- **Background:** Near-black `#0a0010`
- **Primary accent:** Electric purple `#9d4edd`
- **Secondary accent:** Hot purple-pink `#c77dff`
- **Text:** White `#ffffff` / muted `#aaaaaa`
- **Vibe:** Dark, flashy, neon-purple, dynamic — tech agency energy
- **Full color replacement:** All existing cyan (`#00f2ff`) and gold (`#ffd700`) tokens are replaced with purple. Every inline style and CSS variable referencing cyan must be updated.

---

## What Gets Removed

- `<div class="hero-bg">` and its `<img>` child — broken local path, replaced by CSS aurora
- Entire `#automation` section (`/02 WORKFLOW`) — replaced by new `/02 PROCESS` section
- `.hero-bg` and `.hero-bg img` CSS rules
- `.automation-section` and `.automation-image` CSS rules
- `.socials` div in sidebar — removed entirely (no social links, no phone, no chat widget)
- All `--accent-cyan` and `--accent-gold` CSS variable usages

---

## Page Structure

### 1. Sidebar (fixed, vertical)
- Logo "Vonvite" in Dancing Script, purple (`#9d4edd`)
- Nav links: `SERVICES` / `PROCESS` / `MAROC` / `CONTACT` — vertical writing mode, JetBrains Mono
- Active section highlighted: `color: #9d4edd` on the `<a>` tag, driven by scroll-spy JS
- Hover color: `#c77dff`
- Mobile (≤768px): collapses to 60px horizontal top bar. If all 4 labels + logo overflow, reduce font-size to `0.6rem` and clip logo to just "V" or hide it behind an abbreviated mark.

### 2. Hero (100vh)
- **Background:** CSS `@keyframes aurora` — 3–4 overlapping `radial-gradient` blobs in purple/dark-purple, animating `background-position` or `opacity` on a 6–10s loop. No `<img>` elements.
- **Headline:** `L'IA qui` (Dancing Script, purple, block) + `VEND VITE.` (Outfit 900, white, huge — `clamp(3rem, 10vw, 8rem)`)
- **Subheadline:** "Nous fusionnons l'automation n8n de pointe avec l'intelligence artificielle pour transformer vos processus business au Maroc." (Outfit 300, `#aaaaaa`, max-width 600px)
- **Primary CTA:** "DÉCOLLER MAINTENANT" — transparent button, `1px solid #9d4edd`, JetBrains Mono. CSS `@keyframes glow-pulse` animates `box-shadow` from `0 0 10px rgba(157,78,221,0.4)` to `0 0 30px rgba(157,78,221,0.9)` on a 2s infinite loop. On hover: background fills purple, text goes dark.
- **Secondary CTA:** "Voir nos services ↓" — plain text link, `#aaaaaa`, no border, smooth scrolls to `#services`

### 3. Stats Bar
- Standalone `<section>` between hero and services — NOT inside the Morocco section
- Full-width dark strip: `background: rgba(157,78,221,0.05)`, `border-top: 1px solid rgba(157,78,221,0.2)`, `border-bottom: 1px solid rgba(157,78,221,0.2)`
- 3 counters in a flex row, centered:
  - `90` + `%` suffix → label "Réduction des erreurs"
  - `4` + `x` suffix → label "Vitesse de vente"
  - `100` + `%` suffix → label "Support Local"
- Counter animation: `IntersectionObserver` on the stats bar. When visible, JS increments each number from 0 to target over ~1.5s using `requestAnimationFrame`. Runs once.
- Counter text: Outfit 900, `3rem`, `#9d4edd`. Labels: JetBrains Mono, `0.8rem`, `#aaaaaa`.

### 4. Services (3 cards)
- Section tag: `/01 EXPERTISE` (JetBrains Mono, `#9d4edd`)
- Section title: "Nos services propulsés par l'IA"
- 3-column grid (`repeat(auto-fit, minmax(300px, 1fr))`), gap 40px
- Each card: `background: rgba(157,78,221,0.04)`, `border: 1px solid rgba(157,78,221,0.15)`, padding 60px 40px
- Card hover: `border-color: #9d4edd`, `box-shadow: 0 0 30px rgba(157,78,221,0.2)`, `transform: translateY(-10px)`
- Ghost number (01/02/03): `position: absolute; top: 20px; right: 20px` **inside** the card element, Outfit 900, `4rem`, `rgba(255,255,255,0.04)`
- Lucide icons: `color: #9d4edd`, 32×32px
- Cards: n8n Automation (zap icon) / Agents IA Dédiés (bot icon) / Extraction de Données (database icon)
- All cards have `.reveal` class for scroll animation

### 5. How It Works — PROCESS
- Section tag: `/02 PROCESS`
- Section title: "Comment ça marche"
- 3 steps in a horizontal flex row on desktop, vertical stack on mobile:
  1. **Audit** — "On analyse vos processus et identifie les gains rapides."
  2. **Build** — "On construit vos workflows n8n et agents IA sur mesure."
  3. **Déploiement** — "On déploie, on forme, on reste disponibles."
- Connecting line (desktop): CSS `::before` pseudo-element on the flex container. `content: ''`, `position: absolute`, `top: 50%` of the step number circles, `left: 0`, `width: 100%`, `height: 1px`, `background: linear-gradient(to right, transparent, #9d4edd, transparent)`, `z-index: 0`. Step circles are `z-index: 1` to appear above.
- Step number: circle `40px × 40px`, `border: 1px solid #9d4edd`, centered number in JetBrains Mono
- `.reveal` on each step

### 6. Morocco Focus
- Section tag: `/03 LOCALISATION`
- `background: linear-gradient(to bottom, transparent, rgba(157,78,221,0.05), transparent)`
- Centered layout, max-width 900px, margin auto
- Headline: "Accélérez la transformation digitale au Maroc"
- Paragraph (Outfit 300, 1.5rem, `#aaaaaa`): "Vonvite comprend les spécificités du marché marocain. Nous adaptons nos solutions d'IA pour répondre aux besoins locaux."
- 3 bullet points (JetBrains Mono, `#9d4edd`):
  - `> Gestion de stock automatisée`
  - `> E-commerce & prospection IA`
  - `> Services financiers & conformité`
- `.reveal` on the container

### 7. Contact
- Section tag: `/04 START`
- Headline: "Prêt à Vonvite ?"
- Form: `action="https://formspree.io/f/YOUR_FORM_ID"` `method="POST"`
- Fields (all with `name` attributes):
  - `<input type="text" name="name" placeholder="Nom complet">`
  - `<input type="email" name="email" placeholder="Email professionnel">`
  - `<textarea name="message" rows="4" placeholder="Parlez-nous de votre projet...">`
- Styling: transparent bg, no border except bottom `1px solid rgba(157,78,221,0.3)`, white text, `::placeholder` color `#aaaaaa`
- Submit button: same `.cta-button` style with glow-pulse animation
- **Note to user:** Replace `YOUR_FORM_ID` with your actual Formspree form ID (free at formspree.io)

### 8. Footer
- `border-top: 1px solid rgba(157,78,221,0.2)`
- Flex row: logo left (Dancing Script, 3rem, `#9d4edd`) + copyright right (JetBrains Mono, 0.8rem, `#aaaaaa`)
- Copyright: "© 2026 Vonvite Maroc. Propulsé par n8n & IA."

---

## Technical Requirements

### CSS Variables (replacing all existing)
```css
:root {
  --bg-color: #0a0010;
  --accent-purple: #9d4edd;
  --accent-purple-light: #c77dff;
  --text-primary: #ffffff;
  --text-secondary: #aaaaaa;
  --sidebar-width: 80px;
  --glass-bg: rgba(157, 78, 221, 0.04);
  --glass-border: rgba(157, 78, 221, 0.15);
}
```

### Animations
- `@keyframes aurora` — animates hero background gradients
- `@keyframes glow-pulse` — pulses CTA button box-shadow (2s infinite)
- `.reveal` / `.reveal.active` — opacity 0→1, translateY 30px→0 (0.8s ease-out)

### JavaScript
1. **Lucide init:** `lucide.createIcons()`
2. **Reveal on scroll:** `IntersectionObserver` or scroll event, adds `.active` to `.reveal` elements when in viewport
3. **Animated counters:** `IntersectionObserver` on `.stats-bar`. On entry, animate each counter from 0 to target value over 1500ms using `requestAnimationFrame`. Runs once (disconnect observer after trigger).
4. **Scroll-spy:** `IntersectionObserver` on each `<section>` with an `id`. When a section is ≥50% visible, find the matching sidebar `<a href="#sectionId">` and add class `active` (color `#9d4edd`). Remove `active` from all others.

### External Dependencies
- Google Fonts: Dancing Script 700, Outfit 300/400/700/900, JetBrains Mono 400/700
- Lucide icons: pinned to `https://unpkg.com/lucide@0.363.0` (not `@latest`)

### Responsive Breakpoint (≤768px)
- Sidebar → 60px horizontal top bar, `flex-direction: row`
- Logo and nav links switch to `writing-mode: horizontal-tb`, `transform: rotate(0)`
- Nav font-size: `0.6rem` if needed to fit
- Main content: `margin-left: 0`, `padding-top: 60px`
- Automation grid → single column (already handled)
- Process steps → vertical stack, no connecting line

---

## Out of Scope
- CMS or dynamic content
- Multi-page routing
- Analytics / tracking
- WhatsApp button
- Phone number
- Social media links
- Chat widget
