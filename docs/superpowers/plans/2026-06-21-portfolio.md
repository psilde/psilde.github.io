# Portfolio Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file static portfolio site for Paul Silver, deployable to GitHub Pages at psilde.github.io.

**Architecture:** Single `index.html` with all CSS inlined in `<style>` and minimal JS inlined in `<script>`. No build step. Google Fonts (Inter) loaded via `<link>`. Five sections: nav, hero, about, skills, projects, contact/footer.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox), vanilla JS (IntersectionObserver for scroll animations, active nav highlighting)

## Global Constraints

- No emoji in copy
- Font: Inter from Google Fonts, weights 300/400/500/600/700
- Background: #0d1117 throughout
- Accent blue: #388bfd, accent green: #3fb950
- Text primary: #e6edf3, text muted: #7d8590
- Border colour: #21262d
- Smooth scroll: scroll-behavior: smooth on html element
- Responsive: single column below 768px
- No placeholder content — all text is final copy from spec
- No commits — user handles git

---

### Task 1: Scaffold, nav, and hero

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html with head, CSS variables, reset, font, and nav**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Paul Silver — Software Engineer</title>
  <meta name="description" content="Software engineering student at Curtin University. Building desktop apps, backend pipelines, and API integrations." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:           #0d1117;
      --surface:      #0a0e17;
      --border:       #21262d;
      --border-mid:   #30363d;
      --text:         #e6edf3;
      --text-muted:   #7d8590;
      --text-dim:     #8b949e;
      --accent:       #388bfd;
      --accent-green: #3fb950;
      --cyan:         #39d353;
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* Dot grid */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: radial-gradient(circle, #1c2128 1px, transparent 1px);
      background-size: 24px 24px;
      pointer-events: none;
      z-index: 0;
      opacity: 0.6;
    }

    /* Layout helpers */
    .container {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 32px;
      position: relative;
      z-index: 1;
    }
    section { position: relative; z-index: 1; }

    /* Animations */
    @keyframes pulse-dot {
      0%, 100% { opacity: 1; transform: scale(1); }
      50%       { opacity: 0.4; transform: scale(0.7); }
    }
    @keyframes fade-up {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes bounce-y {
      0%, 100% { transform: translateY(0); }
      50%       { transform: translateY(6px); }
    }
    .fade-up { animation: fade-up 0.6s ease both; }
    .fade-up-1 { animation-delay: 0.1s; }
    .fade-up-2 { animation-delay: 0.2s; }
    .fade-up-3 { animation-delay: 0.3s; }
    .fade-up-4 { animation-delay: 0.4s; }

    /* Section eyebrow */
    .eyebrow {
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 11px;
      font-weight: 500;
      letter-spacing: 0.12em;
      color: var(--accent);
      margin-bottom: 40px;
    }
    .eyebrow-num { opacity: 0.5; }
    .eyebrow-line {
      flex: 1;
      height: 1px;
      background: linear-gradient(90deg, var(--border-mid), transparent);
    }

    /* ── NAV ── */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 100;
      height: 56px;
      background: rgba(13,17,23,0.85);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border);
    }
    .nav-inner {
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 100%;
    }
    .nav-wordmark {
      font-size: 14px;
      font-weight: 600;
      color: var(--text);
      text-decoration: none;
      letter-spacing: -0.01em;
    }
    .nav-links {
      display: flex;
      gap: 32px;
      list-style: none;
    }
    .nav-links a {
      font-size: 13px;
      color: var(--text-muted);
      text-decoration: none;
      transition: color 0.2s;
    }
    .nav-links a:hover,
    .nav-links a.active { color: var(--text); }
  </style>
</head>
<body>

<nav>
  <div class="container">
    <div class="nav-inner">
      <a class="nav-wordmark" href="#hero">Paul Silver</a>
      <ul class="nav-links">
        <li><a href="#about">About</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </div>
  </div>
</nav>
```

- [ ] **Step 2: Add hero section CSS and HTML**

Add to `<style>`:
```css
    /* ── HERO ── */
    .hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 80px 0 60px;
      position: relative;
      overflow: hidden;
    }
    .hero-accent-line {
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), var(--accent-green), transparent);
    }
    .hero-glow {
      position: absolute;
      top: -100px; left: 50%;
      transform: translateX(-50%);
      width: 700px; height: 500px;
      background: radial-gradient(ellipse at center, rgba(56,139,253,0.08) 0%, transparent 70%);
      pointer-events: none;
    }
    .hero-eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      font-size: 10px;
      font-weight: 500;
      letter-spacing: 0.2em;
      color: var(--text-dim);
      border: 1px solid var(--border-mid);
      padding: 5px 14px;
      margin-bottom: 32px;
      background: rgba(33,38,45,0.4);
    }
    .hero-dot {
      width: 5px; height: 5px;
      background: var(--accent-green);
      border-radius: 50%;
      animation: pulse-dot 2s ease-in-out infinite;
    }
    .hero-name {
      font-size: clamp(48px, 8vw, 80px);
      font-weight: 700;
      letter-spacing: -2px;
      line-height: 1;
      margin-bottom: 20px;
      color: var(--text);
    }
    .hero-tagline {
      font-size: clamp(16px, 2.5vw, 20px);
      font-weight: 400;
      color: var(--text-muted);
      margin-bottom: 10px;
    }
    .hero-sub {
      font-size: 13px;
      color: var(--text-dim);
      letter-spacing: 0.04em;
      margin-bottom: 44px;
    }
    .hero-actions {
      display: flex;
      gap: 12px;
      justify-content: center;
      flex-wrap: wrap;
    }
    .btn-ghost {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      font-family: inherit;
      font-size: 13px;
      font-weight: 500;
      color: var(--text-muted);
      border: 1px solid var(--border-mid);
      padding: 9px 20px;
      text-decoration: none;
      background: transparent;
      cursor: pointer;
      transition: color 0.2s, border-color 0.2s, background 0.2s;
      border-radius: 6px;
    }
    .btn-ghost:hover {
      color: var(--text);
      border-color: var(--text-dim);
      background: rgba(33,38,45,0.4);
    }
    .btn-ghost svg { flex-shrink: 0; }
    .hero-scroll {
      position: absolute;
      bottom: 32px;
      left: 50%;
      transform: translateX(-50%);
      color: var(--border-mid);
      animation: bounce-y 2s ease-in-out infinite;
    }
```

Add to `<body>` after `</nav>`:
```html
<!-- HERO -->
<section class="hero" id="hero">
  <div class="hero-accent-line"></div>
  <div class="hero-glow"></div>
  <div class="container" style="display:flex;flex-direction:column;align-items:center;">
    <div class="hero-eyebrow fade-up">
      <span class="hero-dot"></span>
      AVAILABLE FOR OPPORTUNITIES
    </div>
    <h1 class="hero-name fade-up fade-up-1">Paul Silver</h1>
    <p class="hero-tagline fade-up fade-up-2">Building tools that solve real problems.</p>
    <p class="hero-sub fade-up fade-up-3">Software Engineering Student &middot; Curtin University</p>
    <div class="hero-actions fade-up fade-up-4">
      <a class="btn-ghost" href="https://github.com/psilde" target="_blank" rel="noopener">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
        GitHub
      </a>
      <a class="btn-ghost" href="https://www.linkedin.com/in/psilv/" target="_blank" rel="noopener">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
        LinkedIn
      </a>
      <a class="btn-ghost" href="mailto:psilver@outlook.com.au">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
        Email
      </a>
    </div>
  </div>
  <div class="hero-scroll">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m6 9 6 6 6-6"/></svg>
  </div>
</section>
```

- [ ] **Step 3: Open index.html in a browser and verify**

- Nav is fixed, blurred, shows "Paul Silver" wordmark and three anchor links
- Hero fills the viewport, name is large and centered
- Three ghost buttons render with icons
- Dot grid texture is visible in the background

---

### Task 2: About + Skills sections

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add About + Skills CSS**

Add to `<style>`:
```css
    /* ── ABOUT ── */
    .about { padding: 120px 0; }
    .about-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 64px;
      align-items: start;
    }
    .about-bio {
      font-size: 16px;
      line-height: 1.8;
      color: var(--text-muted);
    }
    .currently-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 28px;
    }
    .currently-title {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.12em;
      color: var(--text-dim);
      margin-bottom: 20px;
    }
    .currently-item {
      display: flex;
      gap: 12px;
      align-items: baseline;
      padding: 10px 0;
      border-bottom: 1px solid var(--border);
      font-size: 13px;
    }
    .currently-item:last-child { border-bottom: none; }
    .currently-label {
      color: var(--text-dim);
      min-width: 80px;
      flex-shrink: 0;
    }
    .currently-value { color: var(--text); }

    /* ── SKILLS ── */
    .skills { padding: 0 0 120px; }
    .skills-grid {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }
    .skill-pill {
      font-size: 13px;
      color: var(--text-dim);
      border: 1px solid var(--border);
      background: rgba(33,38,45,0.4);
      padding: 5px 14px;
      border-radius: 20px;
      transition: border-color 0.2s, color 0.2s;
    }
    .skill-pill:hover {
      border-color: var(--border-mid);
      color: var(--text);
    }
    .skill-divider {
      width: 1px;
      background: var(--border);
      margin: 0 4px;
      align-self: stretch;
    }
```

- [ ] **Step 2: Add About + Skills HTML after the hero section**

```html
<!-- ABOUT -->
<section class="about" id="about">
  <div class="container">
    <div class="eyebrow">
      <span class="eyebrow-num">01</span>
      ABOUT
      <span class="eyebrow-line"></span>
    </div>
    <div class="about-grid">
      <p class="about-bio">
        I'm a software engineering student at Curtin University who builds things outside the classroom.
        My projects span desktop apps, backend pipelines, and API integrations —
        usually tools I actually wanted to exist.
      </p>
      <div class="currently-card">
        <div class="currently-title">CURRENTLY</div>
        <div class="currently-item">
          <span class="currently-label">Studying</span>
          <span class="currently-value">Curtin University</span>
        </div>
        <div class="currently-item">
          <span class="currently-label">Building</span>
          <span class="currently-value">Slackbuilder, Depth</span>
        </div>
        <div class="currently-item">
          <span class="currently-label">Competing</span>
          <span class="currently-value">Valorant</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- SKILLS -->
<section class="skills">
  <div class="container">
    <div class="eyebrow">
      <span class="eyebrow-num">02</span>
      SKILLS
      <span class="eyebrow-line"></span>
    </div>
    <div class="skills-grid">
      <span class="skill-pill">Java</span>
      <span class="skill-pill">TypeScript</span>
      <span class="skill-pill">Rust</span>
      <div class="skill-divider"></div>
      <span class="skill-pill">Spring Boot</span>
      <span class="skill-pill">React</span>
      <span class="skill-pill">Tauri</span>
      <div class="skill-divider"></div>
      <span class="skill-pill">PostgreSQL</span>
      <span class="skill-pill">Spring Data JPA</span>
      <span class="skill-pill">Flyway</span>
      <div class="skill-divider"></div>
      <span class="skill-pill">Docker</span>
      <span class="skill-pill">Playwright</span>
      <span class="skill-pill">Maven</span>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

- About section shows bio on left, Currently card on right
- Skills render as pill tags with dividers between groups

---

### Task 3: Projects section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add Projects CSS**

Add to `<style>`:
```css
    /* ── PROJECTS ── */
    .projects { padding: 0 0 120px; }

    /* Featured card */
    .project-featured {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 40px;
      margin-bottom: 20px;
      display: grid;
      grid-template-columns: 1fr auto;
      gap: 40px;
      align-items: start;
      transition: border-color 0.2s;
    }
    .project-featured:hover { border-color: var(--accent); }

    .project-featured-accent {
      width: 120px;
      height: 120px;
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      position: relative;
      overflow: hidden;
    }
    .project-featured-accent::before {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(ellipse at center, rgba(56,139,253,0.12) 0%, transparent 70%);
    }
    .project-featured-accent svg { position: relative; z-index: 1; }

    .project-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      font-size: 9px;
      font-weight: 600;
      letter-spacing: 0.14em;
      color: var(--text-dim);
      border: 1px solid var(--border);
      padding: 3px 8px;
      margin-bottom: 16px;
    }

    /* Regular card grid */
    .projects-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
    }
    .project-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 28px;
      display: flex;
      flex-direction: column;
      gap: 12px;
      transition: border-color 0.2s;
    }
    .project-card:hover { border-color: var(--accent); }

    /* Shared project elements */
    .project-name {
      font-size: 17px;
      font-weight: 600;
      color: var(--text);
      letter-spacing: -0.01em;
    }
    .project-featured .project-name { font-size: 22px; }
    .project-desc {
      font-size: 14px;
      line-height: 1.7;
      color: var(--text-muted);
      flex: 1;
    }
    .project-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-top: 4px;
    }
    .project-tag {
      font-size: 11px;
      color: var(--text-dim);
      border: 1px solid var(--border);
      padding: 2px 8px;
      border-radius: 4px;
      background: rgba(33,38,45,0.3);
    }
    .project-links {
      display: flex;
      gap: 16px;
      margin-top: 4px;
    }
    .project-link {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      font-size: 12px;
      font-weight: 500;
      color: var(--text-dim);
      text-decoration: none;
      transition: color 0.2s;
    }
    .project-link:hover { color: var(--accent); }
```

- [ ] **Step 2: Add Projects HTML**

```html
<!-- PROJECTS -->
<section class="projects" id="projects">
  <div class="container">
    <div class="eyebrow">
      <span class="eyebrow-num">03</span>
      PROJECTS
      <span class="eyebrow-line"></span>
    </div>

    <!-- Featured: Slackbuilder -->
    <div class="project-featured">
      <div>
        <div class="project-badge">COLLABORATIVE PROJECT</div>
        <div class="project-name">Slackbuilder</div>
        <p class="project-desc" style="margin-top:10px;">
          AI-powered WYSIWYG desktop app for writing Slack messages. Drafts in a Slack-faithful editor;
          copies native clipboard data via Rust so formatting survives the paste.
          Supports OpenAI, Anthropic, and OpenRouter with streaming, vision, and web search.
        </p>
        <div class="project-tags" style="margin-top:16px;">
          <span class="project-tag">Tauri</span>
          <span class="project-tag">React</span>
          <span class="project-tag">TypeScript</span>
          <span class="project-tag">TipTap</span>
          <span class="project-tag">Rust</span>
          <span class="project-tag">OpenAI</span>
          <span class="project-tag">Anthropic</span>
        </div>
        <div class="project-links" style="margin-top:20px;">
          <a class="project-link" href="https://github.com/psilde/slackbuilder" target="_blank" rel="noopener">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
            GitHub
          </a>
        </div>
      </div>
      <div class="project-featured-accent">
        <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#388bfd" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
          <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>
        </svg>
      </div>
    </div>

    <!-- 3-col grid -->
    <div class="projects-grid">

      <!-- Depth -->
      <div class="project-card">
        <div class="project-name">Depth</div>
        <p class="project-desc">Valorant companion app that surfaces rank and stats for every player in your lobby before the match starts.</p>
        <div class="project-tags">
          <span class="project-tag">Tauri</span>
          <span class="project-tag">React</span>
          <span class="project-tag">TypeScript</span>
        </div>
        <div class="project-links">
          <a class="project-link" href="https://github.com/psilde/depth-public" target="_blank" rel="noopener">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
            GitHub
          </a>
        </div>
      </div>

      <!-- Deal Detection Pipeline -->
      <div class="project-card">
        <div class="project-name">Deal Detection Pipeline</div>
        <p class="project-desc">Two-service system that scrapes Facebook Marketplace with a headless browser and surfaces underpriced listings against pricing baselines.</p>
        <div class="project-tags">
          <span class="project-tag">Java</span>
          <span class="project-tag">Spring Boot</span>
          <span class="project-tag">Playwright</span>
          <span class="project-tag">PostgreSQL</span>
        </div>
        <div class="project-links">
          <a class="project-link" href="https://github.com/psilde/deal-detector" target="_blank" rel="noopener">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
            GitHub
          </a>
        </div>
      </div>

      <!-- Cover Art -->
      <div class="project-card">
        <div class="project-name">Cover Art</div>
        <p class="project-desc">Spring Boot app that extracts every available resolution of cover art from a Spotify track or album URL.</p>
        <div class="project-tags">
          <span class="project-tag">Java</span>
          <span class="project-tag">Spring Boot</span>
          <span class="project-tag">Spotify API</span>
        </div>
        <div class="project-links">
          <a class="project-link" href="https://github.com/psilde/cover-art" target="_blank" rel="noopener">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
            GitHub
          </a>
        </div>
      </div>

    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

- Featured Slackbuilder card is full width with accent icon panel on the right
- Three cards render in a row below
- Hover state shows blue border on all cards

---

### Task 4: Contact, footer, responsive, and JS

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add Contact + Footer CSS**

Add to `<style>`:
```css
    /* ── CONTACT ── */
    .contact {
      padding: 0 0 120px;
      text-align: center;
    }
    .contact-heading {
      font-size: clamp(28px, 4vw, 40px);
      font-weight: 700;
      letter-spacing: -0.5px;
      margin-bottom: 12px;
    }
    .contact-sub {
      font-size: 15px;
      color: var(--text-muted);
      margin-bottom: 40px;
    }
    .contact-actions {
      display: flex;
      gap: 12px;
      justify-content: center;
      flex-wrap: wrap;
    }

    /* ── FOOTER ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 32px 0;
      position: relative;
      z-index: 1;
    }
    .footer-inner {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .footer-copy {
      font-size: 13px;
      color: var(--text-dim);
    }
    .footer-links {
      display: flex;
      gap: 20px;
      align-items: center;
    }
    .footer-link {
      color: var(--text-dim);
      text-decoration: none;
      transition: color 0.2s;
      display: flex;
      align-items: center;
    }
    .footer-link:hover { color: var(--text-muted); }

    /* ── RESPONSIVE ── */
    @media (max-width: 768px) {
      .container { padding: 0 20px; }
      .about-grid { grid-template-columns: 1fr; gap: 32px; }
      .projects-grid { grid-template-columns: 1fr; }
      .project-featured { grid-template-columns: 1fr; }
      .project-featured-accent { display: none; }
      .footer-inner { flex-direction: column; gap: 16px; text-align: center; }
      .nav-links { gap: 20px; }
    }
    @media (max-width: 480px) {
      .hero-actions { flex-direction: column; align-items: center; }
      .contact-actions { flex-direction: column; align-items: center; }
    }
```

- [ ] **Step 2: Add Contact + Footer HTML**

```html
<!-- CONTACT -->
<section class="contact" id="contact">
  <div class="container">
    <div class="eyebrow" style="justify-content:center;">
      <span class="eyebrow-num">04</span>
      CONTACT
      <span class="eyebrow-line" style="flex:unset;width:60px;"></span>
    </div>
    <h2 class="contact-heading">Want to work together or just say hi?</h2>
    <p class="contact-sub">I'm open to internships, grad roles, and interesting projects.</p>
    <div class="contact-actions">
      <a class="btn-ghost" href="https://github.com/psilde" target="_blank" rel="noopener">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
        GitHub
      </a>
      <a class="btn-ghost" href="https://www.linkedin.com/in/psilv/" target="_blank" rel="noopener">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
        LinkedIn
      </a>
      <a class="btn-ghost" href="mailto:psilver@outlook.com.au">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
        Email
      </a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="container">
    <div class="footer-inner">
      <span class="footer-copy">&copy; 2026 Paul Silver</span>
      <div class="footer-links">
        <a class="footer-link" href="https://github.com/psilde" target="_blank" rel="noopener" aria-label="GitHub">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C6.477 2 2 6.477 2 12c0 4.42 2.865 8.17 6.839 9.49.5.092.682-.217.682-.482 0-.237-.008-.866-.013-1.7-2.782.603-3.369-1.342-3.369-1.342-.454-1.155-1.11-1.462-1.11-1.462-.908-.62.069-.608.069-.608 1.003.07 1.531 1.03 1.531 1.03.892 1.529 2.341 1.087 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.11-4.555-4.943 0-1.091.39-1.984 1.029-2.683-.103-.253-.446-1.27.098-2.647 0 0 .84-.269 2.75 1.025A9.578 9.578 0 0 1 12 6.836a9.59 9.59 0 0 1 2.504.337c1.909-1.294 2.747-1.025 2.747-1.025.546 1.377.202 2.394.1 2.647.64.699 1.028 1.592 1.028 2.683 0 3.842-2.339 4.687-4.566 4.935.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.741 0 .267.18.578.688.48C19.138 20.167 22 16.418 22 12c0-5.523-4.477-10-10-10z"/></svg>
        </a>
        <a class="footer-link" href="https://www.linkedin.com/in/psilv/" target="_blank" rel="noopener" aria-label="LinkedIn">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
        </a>
        <a class="footer-link" href="mailto:psilver@outlook.com.au" aria-label="Email">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
        </a>
      </div>
    </div>
  </div>
</footer>
```

- [ ] **Step 3: Add closing JS before </body>**

```html
<script>
  // Active nav link on scroll
  const sections = document.querySelectorAll('section[id]');
  const navLinks = document.querySelectorAll('.nav-links a');

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        navLinks.forEach(link => {
          link.classList.toggle('active', link.getAttribute('href') === '#' + entry.target.id);
        });
      }
    });
  }, { rootMargin: '-40% 0px -55% 0px' });

  sections.forEach(s => observer.observe(s));
</script>
</body>
</html>
```

- [ ] **Step 4: Verify full page in browser**

- All five sections render correctly top to bottom
- Nav link highlights as you scroll past each section
- Resize to mobile width — everything collapses to single column
- All GitHub / LinkedIn / email links are correct
- No horizontal scroll at any viewport width
