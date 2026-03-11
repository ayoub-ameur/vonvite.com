# Vonvite Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild vonvite.com as a flashy purple-themed, conversion-focused freelance landing page targeting Moroccan businesses, with a working contact form and all broken assets replaced by CSS visuals.

**Architecture:** Single-page static site. `style.css` holds all styles. `index.html` holds all markup and inline JS. No build step, no framework. Formspree handles form submission.

**Tech Stack:** HTML5, CSS3 (custom properties, keyframe animations, grid/flexbox), vanilla JS (IntersectionObserver, requestAnimationFrame), Lucide icons CDN, Google Fonts CDN, Formspree.

**Spec:** `docs/superpowers/specs/2026-03-11-vonvite-redesign.md`

---

## Files

| Action | Path | Responsibility |
|--------|------|----------------|
| Modify (full rewrite) | `vonvite.com/style.css` | All styles: variables, layout, components, animations, responsive |
| Modify (full rewrite) | `vonvite.com/index.html` | All markup + inline JS (Lucide init, reveal, counters, scroll-spy) |

---

## Chunk 1: CSS Foundation — Variables, Animations, Sidebar, Hero

### Task 1: Replace CSS variables and base styles

**Files:**
- Modify: `vonvite.com/style.css` (lines 1–30)

- [ ] **Step 1: Replace the `:root` block and base styles**

Open `vonvite.com/style.css`. Replace everything from `@import` through the `body {}` rule with:

```css
@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Outfit:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&display=swap');

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

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: var(--bg-color);
    color: var(--text-primary);
    font-family: 'Outfit', sans-serif;
    overflow-x: hidden;
    line-height: 1.6;
}
```

- [ ] **Step 2: Verify the file saved correctly**

Open `vonvite.com/style.css` and confirm `--accent-purple: #9d4edd` is present and `--accent-cyan` is gone.

- [ ] **Step 3: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: replace CSS variables with purple theme"
```

---

### Task 2: Add aurora and glow-pulse keyframe animations

**Files:**
- Modify: `vonvite.com/style.css` (append after base styles)

- [ ] **Step 1: Append the animation keyframes to style.css**

Add after the `body {}` rule:

```css
/* ── Animations ── */
@keyframes aurora {
    0%   { background-position: 0% 50%, 100% 50%, 50% 0%; }
    33%  { background-position: 100% 50%, 0% 50%, 50% 100%; }
    66%  { background-position: 50% 0%, 50% 100%, 0% 50%; }
    100% { background-position: 0% 50%, 100% 50%, 50% 0%; }
}

@keyframes glow-pulse {
    0%, 100% { box-shadow: 0 0 10px rgba(157, 78, 221, 0.4), 0 0 20px rgba(157, 78, 221, 0.2); }
    50%       { box-shadow: 0 0 25px rgba(157, 78, 221, 0.9), 0 0 50px rgba(157, 78, 221, 0.4); }
}

.reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.8s ease-out;
}

.reveal.active {
    opacity: 1;
    transform: translateY(0);
}
```

- [ ] **Step 2: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: add aurora and glow-pulse animations"
```

---

### Task 3: Sidebar CSS

**Files:**
- Modify: `vonvite.com/style.css` (replace existing sidebar block)

- [ ] **Step 1: Replace the sidebar CSS block**

Find and replace the existing `/* Sidebar */` section with:

```css
/* ── Sidebar ── */
.sidebar {
    position: fixed;
    top: 0;
    left: 0;
    width: var(--sidebar-width);
    height: 100vh;
    border-right: 1px solid var(--glass-border);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    padding: 40px 0;
    z-index: 100;
    background: var(--bg-color);
}

.logo-vertical {
    writing-mode: vertical-rl;
    transform: rotate(180deg);
    font-family: 'Dancing Script', cursive;
    font-size: 2.5rem;
    color: var(--accent-purple);
    text-decoration: none;
    letter-spacing: 4px;
}

.nav-links {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 30px;
}

.nav-links a {
    color: var(--text-secondary);
    text-decoration: none;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    writing-mode: vertical-rl;
    transform: rotate(180deg);
    transition: color 0.3s ease;
}

.nav-links a:hover,
.nav-links a.active {
    color: var(--accent-purple);
}
```

- [ ] **Step 2: Verify `.socials` div is removed from the HTML**

```bash
grep -n "socials" vonvite.com/style.css vonvite.com/index.html
```

Expected: no output (the `.socials` div should already be absent since Task 7 rewrites the HTML — this is a forward check; run it after Task 7 if you prefer).

- [ ] **Step 3: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: update sidebar to purple theme"
```

---

### Task 4: Hero CSS with aurora background

**Files:**
- Modify: `vonvite.com/style.css` (replace existing hero + main block)

- [ ] **Step 1: Replace the main and hero CSS blocks**

Find and replace from `/* Main Content */` through the hero section CSS with:

```css
/* ── Main Content ── */
main {
    margin-left: var(--sidebar-width);
    padding: 0 5%;
}

/* ── Hero ── */
.hero {
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    position: relative;
    overflow: hidden;
}

.hero::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
        radial-gradient(ellipse 80% 60% at 20% 40%, rgba(157, 78, 221, 0.35) 0%, transparent 60%),
        radial-gradient(ellipse 60% 80% at 80% 20%, rgba(199, 125, 255, 0.2) 0%, transparent 60%),
        radial-gradient(ellipse 70% 50% at 60% 80%, rgba(157, 78, 221, 0.15) 0%, transparent 60%);
    background-size: 200% 200%, 200% 200%, 200% 200%;
    animation: aurora 8s ease-in-out infinite;
    z-index: 0;
}

.hero-content {
    position: relative;
    z-index: 1;
}

.hero h1 {
    font-size: clamp(3rem, 10vw, 8rem);
    font-weight: 900;
    line-height: 0.9;
    margin-bottom: 20px;
    letter-spacing: -2px;
}

.hero h1 span.script {
    font-family: 'Dancing Script', cursive;
    color: var(--accent-purple);
    font-weight: 700;
    display: block;
    font-size: 0.5em;
    margin-bottom: -0.2em;
}

.hero p {
    font-size: 1.25rem;
    color: var(--text-secondary);
    max-width: 600px;
    margin-bottom: 40px;
    font-weight: 300;
}

.hero-ctas {
    display: flex;
    align-items: center;
    gap: 30px;
}

.secondary-cta {
    color: var(--text-secondary);
    text-decoration: none;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    transition: color 0.3s ease;
}

.secondary-cta:hover {
    color: var(--accent-purple-light);
}

/* ── CTA Button ── */
.cta-button {
    display: inline-flex;
    align-items: center;
    padding: 15px 40px;
    background: transparent;
    border: 1px solid var(--accent-purple);
    color: var(--text-primary);
    text-decoration: none;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    cursor: pointer;
    width: fit-content;
    animation: glow-pulse 2s ease-in-out infinite;
    transition: background 0.3s ease, color 0.3s ease;
}

.cta-button:hover {
    background: var(--accent-purple);
    color: var(--bg-color);
    animation: none;
    box-shadow: 0 0 40px rgba(157, 78, 221, 0.6);
}
```

- [ ] **Step 2: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: hero with CSS aurora background"
```

---

### Task 5: Stats bar, sections, services, process CSS

**Files:**
- Modify: `vonvite.com/style.css` (append / replace remaining blocks)

- [ ] **Step 1: Replace section, services, and add stats + process CSS**

Find and replace everything from `/* Sections */` to the end of the services grid block, then append the stats and process blocks. The full replacement:

```css
/* ── Sections ── */
section {
    padding: 150px 0;
}

.section-tag {
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent-purple);
    font-size: 0.9rem;
    margin-bottom: 20px;
    display: block;
}

.section-title {
    font-size: clamp(2rem, 5vw, 3.5rem);
    font-weight: 700;
    margin-bottom: 60px;
    max-width: 800px;
}

/* ── Stats Bar ── */
.stats-bar {
    padding: 60px 0;
    background: rgba(157, 78, 221, 0.05);
    border-top: 1px solid rgba(157, 78, 221, 0.2);
    border-bottom: 1px solid rgba(157, 78, 221, 0.2);
    margin-left: calc(-5%);
    margin-right: calc(-5%);
    padding-left: 5%;
    padding-right: 5%;
}

.stats-grid {
    display: flex;
    justify-content: center;
    gap: 80px;
    flex-wrap: wrap;
}

.stat-item {
    text-align: center;
}

.stat-value {
    font-family: 'Outfit', sans-serif;
    font-size: 3.5rem;
    font-weight: 900;
    color: var(--accent-purple);
    display: block;
    line-height: 1;
}

.stat-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    color: var(--text-secondary);
    margin-top: 8px;
    display: block;
}

/* ── Services Grid ── */
.services-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 40px;
}

.service-card {
    background: var(--glass-bg);
    border: 1px solid var(--glass-border);
    padding: 60px 40px;
    position: relative;
    transition: transform 0.3s ease, border-color 0.3s ease, box-shadow 0.3s ease;
}

.service-card:hover {
    transform: translateY(-10px);
    border-color: var(--accent-purple);
    box-shadow: 0 0 30px rgba(157, 78, 221, 0.2);
}

.service-number {
    position: absolute;
    top: 20px;
    right: 20px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 4rem;
    font-weight: 900;
    color: rgba(255, 255, 255, 0.04);
    line-height: 1;
}

.service-card h3 {
    font-size: 1.8rem;
    margin-bottom: 20px;
    color: var(--accent-purple-light);
}

.service-card p {
    color: var(--text-secondary);
    font-size: 1rem;
    font-weight: 300;
}

.icon-purple {
    color: var(--accent-purple);
    width: 32px;
    height: 32px;
    margin-bottom: 20px;
}

/* ── Process (How It Works) ── */
.process-grid {
    display: flex;
    gap: 0;
    position: relative;
    align-items: flex-start;
}

.process-grid::before {
    content: '';
    position: absolute;
    top: 20px;
    left: 20px;
    right: 20px;
    height: 1px;
    background: linear-gradient(to right, transparent, var(--accent-purple), transparent);
    z-index: 0;
}

.process-step {
    flex: 1;
    text-align: center;
    padding: 0 30px;
    position: relative;
    z-index: 1;
}

.step-number {
    width: 40px;
    height: 40px;
    border: 1px solid var(--accent-purple);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    color: var(--accent-purple);
    margin: 0 auto 20px;
    background: var(--bg-color);
}

.process-step h3 {
    font-size: 1.4rem;
    color: var(--text-primary);
    margin-bottom: 12px;
}

.process-step p {
    color: var(--text-secondary);
    font-size: 0.95rem;
    font-weight: 300;
}
```

- [ ] **Step 2: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: stats bar, services, and process section CSS"
```

---

### Task 6: Morocco, contact, footer, and responsive CSS

**Files:**
- Modify: `vonvite.com/style.css` (replace remaining blocks + append responsive)

- [ ] **Step 1: Replace the footer CSS and append Morocco, contact, responsive blocks**

Remove the old `/* AI/n8n Special Section */` block (`.automation-section`, `.automation-image`). Replace everything from `/* Footer */` to the end of the file with:

```css
/* ── Morocco Focus ── */
.maroc-section {
    background: linear-gradient(to bottom, transparent, rgba(157, 78, 221, 0.05), transparent);
}

.maroc-inner {
    text-align: center;
    max-width: 900px;
    margin: 0 auto;
}

.maroc-inner .section-title {
    margin: 0 auto 40px;
}

.maroc-inner p {
    font-size: 1.5rem;
    font-weight: 300;
    color: var(--text-secondary);
    margin-bottom: 40px;
}

.maroc-bullets {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 12px;
    align-items: center;
}

.maroc-bullets li {
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent-purple);
    font-size: 1rem;
}

/* ── Contact ── */
.contact-form-wrap {
    background: var(--glass-bg);
    padding: 60px;
    border: 1px solid var(--glass-border);
    max-width: 600px;
}

.contact-form {
    display: flex;
    flex-direction: column;
    gap: 24px;
}

.contact-form input,
.contact-form textarea {
    background: transparent;
    border: none;
    border-bottom: 1px solid rgba(157, 78, 221, 0.3);
    padding: 12px 0;
    color: var(--text-primary);
    font-family: 'Outfit', sans-serif;
    font-size: 1rem;
    outline: none;
    transition: border-color 0.3s ease;
    width: 100%;
    resize: none;
}

.contact-form input::placeholder,
.contact-form textarea::placeholder {
    color: var(--text-secondary);
}

.contact-form input:focus,
.contact-form textarea:focus {
    border-bottom-color: var(--accent-purple);
}

/* ── Footer ── */
footer {
    padding: 60px 0;
    border-top: 1px solid rgba(157, 78, 221, 0.2);
}

.footer-content {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
}

.footer-logo {
    font-family: 'Dancing Script', cursive;
    font-size: 3rem;
    color: var(--accent-purple);
}

.copyright {
    color: var(--text-secondary);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
}

/* ── Responsive ── */
@media (max-width: 768px) {
    .sidebar {
        width: 100%;
        height: 60px;
        flex-direction: row;
        padding: 0 20px;
        border-right: none;
        border-bottom: 1px solid var(--glass-border);
        justify-content: space-between;
    }

    .logo-vertical,
    .nav-links a {
        writing-mode: horizontal-tb;
        transform: rotate(0deg);
    }

    .logo-vertical {
        font-size: 1.6rem;
        letter-spacing: 2px;
    }

    .nav-links {
        flex-direction: row;
        gap: 12px;
    }

    .nav-links a {
        font-size: 0.6rem;
    }

    main {
        margin-left: 0;
        padding-top: 60px;
    }

    .hero h1 {
        font-size: 4rem;
    }

    .hero-ctas {
        flex-direction: column;
        align-items: flex-start;
        gap: 16px;
    }

    .stats-grid {
        gap: 40px;
    }

    .process-grid {
        flex-direction: column;
        gap: 40px;
    }

    .process-grid::before {
        display: none;
    }

    .footer-content {
        flex-direction: column;
        gap: 16px;
        align-items: flex-start;
    }

    .contact-form-wrap {
        padding: 30px 20px;
    }
}
```

- [ ] **Step 2: Verify style.css has no references to removed classes**

Run from the repo root (`vonvite.com/vonvite.com`):

```bash
grep -n "accent-cyan\|accent-gold\|hero-bg\|automation-section\|automation-image" vonvite.com/style.css
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/style.css && git commit -m "style: morocco, contact, footer, responsive — CSS complete"
```

---

## Chunk 2: HTML Structure

### Task 7: Rewrite index.html — head, sidebar, hero

**Files:**
- Modify: `vonvite.com/index.html`

- [ ] **Step 1: Replace the `<head>` and sidebar + hero sections**

Open `vonvite.com/index.html`. Replace from `<!DOCTYPE html>` through the closing `</header>` of the hero section. The replacement below includes the opening `<main>` tag — Tasks 8 and 9 will append their sections inside this `<main>` block before it is closed by Task 9.

```html
<!DOCTYPE html>
<html lang="fr" class="scroll-smooth">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vonvite | Automation n8n & Services IA au Maroc</title>
    <meta name="description" content="Vonvite propulse votre entreprise avec l'automation n8n et des solutions d'intelligence artificielle sur mesure au Maroc. Vendez plus vite, automatisez tout.">
    <link rel="stylesheet" href="style.css">
    <script src="https://unpkg.com/lucide@0.363.0"></script>
</head>
<body>

    <!-- Sidebar -->
    <aside class="sidebar">
        <a href="#" class="logo-vertical">Vonvite</a>
        <ul class="nav-links">
            <li><a href="#services">SERVICES</a></li>
            <li><a href="#process">PROCESS</a></li>
            <li><a href="#maroc">MAROC</a></li>
            <li><a href="#contact">CONTACT</a></li>
        </ul>
    </aside>

    <main>
        <!-- Hero Section -->
        <header class="hero">
            <div class="hero-content">
                <h1>
                    <span class="script">L'IA qui</span>
                    VEND VITE.
                </h1>
                <p>Nous fusionnons l'automation n8n de pointe avec l'intelligence artificielle pour transformer vos processus business au Maroc.</p>
                <div class="hero-ctas">
                    <a href="#contact" class="cta-button">DÉCOLLER MAINTENANT</a>
                    <a href="#services" class="secondary-cta">Voir nos services ↓</a>
                </div>
            </div>
        </header>
```

- [ ] **Step 2: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/index.html && git commit -m "html: head, sidebar, hero section"
```

---

### Task 8: Stats bar and services section HTML

**Files:**
- Modify: `vonvite.com/index.html`

- [ ] **Step 1: Replace the services section and add stats bar before it**

Remove the entire old `<!-- Services Section -->` block. Replace with:

```html
        <!-- Stats Bar -->
        <section class="stats-bar" aria-label="Résultats clés">
            <div class="stats-grid">
                <div class="stat-item reveal">
                    <span class="stat-value" data-target="90" data-suffix="%">0%</span>
                    <span class="stat-label">Réduction des erreurs</span>
                </div>
                <div class="stat-item reveal">
                    <span class="stat-value" data-target="4" data-suffix="x">0x</span>
                    <span class="stat-label">Vitesse de vente</span>
                </div>
                <div class="stat-item reveal">
                    <span class="stat-value" data-target="100" data-suffix="%">0%</span>
                    <span class="stat-label">Support Local</span>
                </div>
            </div>
        </section>

        <!-- Services Section -->
        <section id="services">
            <span class="section-tag">/01 EXPERTISE</span>
            <h2 class="section-title">Nos services propulsés par l'IA</h2>
            <div class="services-grid">
                <div class="service-card reveal">
                    <span class="service-number">01</span>
                    <i data-lucide="zap" class="icon-purple"></i>
                    <h3>n8n Automation</h3>
                    <p>Workflows complexes, intégrations fluides. Nous automatisons vos tâches répétitives pour libérer votre potentiel créatif.</p>
                </div>
                <div class="service-card reveal">
                    <span class="service-number">02</span>
                    <i data-lucide="bot" class="icon-purple"></i>
                    <h3>Agents IA Dédiés</h3>
                    <p>Des assistants intelligents formés sur vos données, disponibles 24/7 pour votre support client ou votre prospection.</p>
                </div>
                <div class="service-card reveal">
                    <span class="service-number">03</span>
                    <i data-lucide="database" class="icon-purple"></i>
                    <h3>Extraction de Données</h3>
                    <p>Scraping éthique et analyse de données par IA pour une veille concurrentielle imbattable au Maroc.</p>
                </div>
            </div>
        </section>
```

- [ ] **Step 2: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/index.html && git commit -m "html: stats bar and services section"
```

---

### Task 9: Process, Morocco, contact, and footer HTML

**Files:**
- Modify: `vonvite.com/index.html`

- [ ] **Step 1: Remove old automation section and replace with process + remaining sections**

Delete the old `<!-- Automation Section -->`, `<!-- Morocco Focus -->`, `<!-- Contact -->`, `<footer>`, and `</main>` blocks. Replace with:

```html
        <!-- Process Section -->
        <section id="process">
            <span class="section-tag">/02 PROCESS</span>
            <h2 class="section-title">Comment ça marche</h2>
            <div class="process-grid">
                <div class="process-step reveal">
                    <div class="step-number">01</div>
                    <h3>Audit</h3>
                    <p>On analyse vos processus et identifie les gains rapides.</p>
                </div>
                <div class="process-step reveal">
                    <div class="step-number">02</div>
                    <h3>Build</h3>
                    <p>On construit vos workflows n8n et agents IA sur mesure.</p>
                </div>
                <div class="process-step reveal">
                    <div class="step-number">03</div>
                    <h3>Déploiement</h3>
                    <p>On déploie, on forme, on reste disponibles.</p>
                </div>
            </div>
        </section>

        <!-- Morocco Focus -->
        <section id="maroc" class="maroc-section">
            <div class="maroc-inner reveal">
                <span class="section-tag">/03 LOCALISATION</span>
                <h2 class="section-title">Accélérez la transformation digitale au Maroc</h2>
                <p>Vonvite comprend les spécificités du marché marocain. Nous adaptons nos solutions d'IA pour répondre aux besoins locaux.</p>
                <ul class="maroc-bullets">
                    <li>> Gestion de stock automatisée</li>
                    <li>> E-commerce & prospection IA</li>
                    <li>> Services financiers & conformité</li>
                </ul>
            </div>
        </section>

        <!-- Contact -->
        <section id="contact">
            <span class="section-tag">/04 START</span>
            <h2 class="section-title">Prêt à Vonvite ?</h2>
            <div class="contact-form-wrap reveal">
                <form class="contact-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
                    <input type="text" name="name" placeholder="Nom complet" required />
                    <input type="email" name="email" placeholder="Email professionnel" required />
                    <textarea name="message" rows="4" placeholder="Parlez-nous de votre projet..."></textarea>
                    <button type="submit" class="cta-button">ENVOYER LE MESSAGE</button>
                </form>
            </div>
        </section>

        <footer>
            <div class="footer-content">
                <div class="footer-logo">Vonvite</div>
                <div class="copyright">&copy; 2026 Vonvite Maroc. Propulsé par n8n & IA.</div>
            </div>
        </footer>
    </main>
```

- [ ] **Step 2: Verify no old broken image paths remain**

Run from the repo root (`vonvite.com/vonvite.com`):

```bash
grep -n "Users/yakeey\|antigravity\|icon-cyan\|automation-section\|socials" vonvite.com/index.html
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/index.html && git commit -m "html: process, morocco, contact, footer sections"
```

---

## Chunk 3: JavaScript

### Task 10: Add all JavaScript (reveal, counters, scroll-spy)

**Files:**
- Modify: `vonvite.com/index.html` (replace closing `<script>` block before `</body>`)

- [ ] **Step 1: Replace the existing inline script with the full JS block**

Remove everything from `<script>` to `</style>` at the bottom of `index.html` (includes old reveal JS and the leftover `<style>` block with `.icon-cyan`). Replace with:

```html
    <script>
        // 1. Initialize Lucide Icons
        lucide.createIcons();

        // 2. Reveal on scroll
        const revealObserver = new IntersectionObserver((entries) => {
            entries.forEach(el => {
                if (el.isIntersecting) {
                    el.target.classList.add('active');
                    revealObserver.unobserve(el.target);
                }
            });
        }, { threshold: 0.15 });

        document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));

        // 3. Animated counters
        function animateCounter(el) {
            const target = parseInt(el.dataset.target, 10);
            const suffix = el.dataset.suffix || '';
            const duration = 1500;
            const start = performance.now();

            function step(now) {
                const elapsed = now - start;
                const progress = Math.min(elapsed / duration, 1);
                const eased = 1 - Math.pow(1 - progress, 3); // ease-out cubic
                el.textContent = Math.floor(eased * target) + suffix;
                if (progress < 1) requestAnimationFrame(step);
            }
            requestAnimationFrame(step);
        }

        const statsObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.querySelectorAll('.stat-value').forEach(animateCounter);
                    statsObserver.unobserve(entry.target);
                }
            });
        }, { threshold: 0.5 });

        const statsBar = document.querySelector('.stats-bar');
        if (statsBar) statsObserver.observe(statsBar);

        // 4. Scroll-spy: highlight active sidebar nav link
        // Only observe <section id="..."> elements — the hero uses <header> with no id
        const sections = document.querySelectorAll('main section[id]');
        const navLinks = document.querySelectorAll('.nav-links a');

        const spyObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    navLinks.forEach(link => link.classList.remove('active'));
                    const activeLink = document.querySelector(`.nav-links a[href="#${entry.target.id}"]`);
                    if (activeLink) activeLink.classList.add('active');
                }
            });
        }, { threshold: 0.5 });

        sections.forEach(section => spyObserver.observe(section));
    </script>
</body>
</html>
```

- [ ] **Step 2: Verify no leftover old code remains**

Run from the repo root (`vonvite.com/vonvite.com`):

```bash
grep -n "icon-cyan\|window.addEventListener.*scroll" vonvite.com/index.html
```

Expected: no output (both patterns belong to the old code that was replaced).

```bash
grep -n "lucide.createIcons" vonvite.com/index.html
```

Expected: exactly one line matching the new `lucide.createIcons()` call.

- [ ] **Step 3: Open index.html in a browser and verify visually**

- Aurora animation plays in hero background (purple blobs shifting)
- Lucide icons render in service cards
- Scroll down: sections reveal with fade-in
- Stats bar counters animate up when scrolled into view
- Sidebar nav link highlights as you scroll through sections
- CTA buttons glow-pulse

- [ ] **Step 4: Commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/index.html && git commit -m "js: reveal, counter animation, scroll-spy"
```

---

### Task 11: Final cleanup and Formspree note

**Files:**
- Modify: `vonvite.com/index.html` (no code change — verification only)

- [ ] **Step 1: Full lint check — no broken references**

Run from the repo root (`vonvite.com/vonvite.com`). The patterns below are structural — `automation-section` and `automation-image` are old CSS class names. Do NOT grep for bare `automation` as that word appears in French body copy and will produce false positives.

```bash
grep -n "Users/yakeey\|accent-cyan\|accent-gold\|icon-cyan\|automation-section\|automation-image\|\.socials\|YOUR_FORM_ID" vonvite.com/index.html vonvite.com/style.css
```

Expected output: exactly one match — `YOUR_FORM_ID` in `index.html` (the Formspree placeholder). All other patterns should produce zero matches.

- [ ] **Step 2: Remind user to set up Formspree**

1. Go to [formspree.io](https://formspree.io) and create a free account
2. Create a new form → copy the form ID (looks like `xabc1234`)
3. In `vonvite.com/index.html`, replace `YOUR_FORM_ID` with your actual ID:
   ```html
   action="https://formspree.io/f/xabc1234"
   ```
4. Test by submitting the contact form — Formspree will forward submissions to your email

- [ ] **Step 3: Final commit**

```bash
cd vonvite.com/vonvite.com && git add vonvite.com/index.html vonvite.com/style.css && git commit -m "feat: vonvite purple redesign complete"
```
