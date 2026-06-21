---
name: portfolio-site
description: Design for Paul Silver's personal portfolio site targeting software engineering recruiters
metadata:
  type: project
---

# Portfolio Site Design

## Goals

- Audience: software engineering recruiters
- Format: single-page scroll, static HTML/CSS/JS
- Aesthetic: dark & technical — consistent with existing README SVG graphics and Depth landing page
- Hosting: GitHub Pages at psilde.github.io
- No build step, no dependencies except Google Fonts (Inter)

## Owner

- Name: Paul Silver
- Role: Software Engineering Student, Curtin University
- GitHub: psilde (https://github.com/psilde)
- LinkedIn: https://www.linkedin.com/in/psilv/
- Email: psilver@outlook.com.au
- Personal: Semi-pro Valorant player (competes for fun)

## Design Language

- Background: #0d1117
- Surface: #0a0e17 → #0d1117 gradient
- Dot grid texture: 24px grid, #1c2128 dots, r=0.75
- Accent primary: #388bfd (blue)
- Accent secondary: #3fb950 (green)
- Accent gradient: blue → green (left to right), fades out
- Text primary: #e6edf3
- Text muted: #7d8590
- Text dim: #8b949e
- Border: #21262d
- Font: Inter (Google Fonts), weights 300/400/500/600/700
- Border radius: 8px for cards
- Transitions: 0.2s ease

## Sections

### 1. Nav

- Fixed, top: 0, full width
- Background: rgba(13,17,23,0.85), backdrop-filter: blur(16px)
- Border-bottom: 1px solid #21262d
- Left: "Paul Silver" wordmark, 14px, 600 weight
- Right: anchor links — About · Projects · Contact — 13px, color #7d8590, hover #e6edf3
- Height: 56px
- z-index: 100

### 2. Hero

- Full viewport height (min-height: 100vh)
- Content centered vertically and horizontally
- Top accent line: blue→green gradient, 2px, fades right (same as README headers)
- Dot grid background texture
- Subtle radial glow: #1f6feb, 12% opacity, top-left origin

Layout (centered column, max-width 640px):
- Eyebrow: small tag — "AVAILABLE FOR OPPORTUNITIES" with a pulsing cyan dot
- Name: "Paul Silver" — 64px, 700 weight, #e6edf3, letter-spacing -2px
- Tagline: "Building tools that solve real problems." — 20px, 400 weight, #7d8590
- Sub-line: "Software Engineering Student · Curtin University" — 14px, #8b949e
- CTA buttons (inline flex, gap 12px):
  - GitHub: ghost button with GitHub icon → https://github.com/psilde
  - LinkedIn: ghost button with LinkedIn icon → https://www.linkedin.com/in/psilv/
  - Email: ghost button with mail icon → mailto:psilver@outlook.com.au
- Scroll hint: small down-arrow at bottom center, subtle pulse animation

### 3. About

Section label: "ABOUT" eyebrow (same style as README section headers — dim number 01, accent color, letter-spacing)

Two-column grid (1fr 1fr), gap 64px, align-items start:

Left — bio paragraph:
> "I'm a software engineering student at Curtin University who builds things outside the classroom. My projects span desktop apps, backend pipelines, and API integrations — usually tools I actually wanted to exist."

Right — "Currently" card (dark surface, border, padding 24px):
- Studying: Curtin University
- Building: Slackbuilder, Depth
- Competing: Valorant

### 4. Skills

Section label: "SKILLS" eyebrow — eyebrow number 02

No progress bars, no icons. Pill tag grid, grouped by implicit category, flex-wrap:

Languages: Java · TypeScript · Rust
Frameworks: Spring Boot · React · Tauri
Data: PostgreSQL · Spring Data JPA · Flyway
Tooling: Docker · Playwright · Maven

Pill style: border 1px solid #21262d, background rgba(33,38,45,0.5), padding 5px 14px, border-radius 20px, font-size 13px, color #8b949e

### 5. Projects

Section label: "PROJECTS" eyebrow — eyebrow number 03

**Featured card** (Slackbuilder) — full width, horizontal layout:
- Left: project name (20px, 700), one-liner, tech tags, GitHub link
- Right: subtle decorative accent panel (dark surface with border)
- Name: Slackbuilder
- One-liner: "AI-powered WYSIWYG desktop app for writing Slack messages. Drafts in a Slack-faithful editor; copies native clipboard data via Rust so formatting survives the paste."
- Tags: Tauri · React · TypeScript · TipTap · OpenAI · Anthropic
- Links: GitHub (https://github.com/psilde/slackbuilder)
- Badge: "COLLABORATIVE" — small dim label

**3-column grid below** (3 cards):

Card 1 — Depth
- One-liner: "Valorant companion app that surfaces rank and stats for every player in your lobby before the match starts."
- Tags: Tauri · React · TypeScript
- Links: GitHub (https://github.com/psilde/depth-public), Live (https://psilde.github.io/depth — the existing landing page)

Card 2 — Deal Detection Pipeline
- One-liner: "Two-service system that scrapes Facebook Marketplace with a headless browser and surfaces underpriced listings against pricing baselines."
- Tags: Java · Spring Boot · Playwright · PostgreSQL
- Links: GitHub (https://github.com/psilde/deal-detector)

Card 3 — Cover Art
- One-liner: "Spring Boot app that extracts every available resolution of cover art from a Spotify track or album URL."
- Tags: Java · Spring Boot · Spotify API
- Links: GitHub (https://github.com/psilde/cover-art)

Card style: background #0a0e17, border 1px solid #21262d, border-radius 8px, padding 28px, hover: border-color #388bfd (transition 0.2s)

### 6. Contact + Footer

Section label: "CONTACT" eyebrow — eyebrow number 04

Centered column:
- Heading: "Want to work together or just say hi?"
- Sub: "I'm open to internships, grad roles, and interesting projects."
- Three large ghost buttons: GitHub · LinkedIn · Email

Footer (border-top: 1px solid #21262d, padding 32px 0):
- Left: "© 2026 Paul Silver"
- Right: three small icon links (GitHub, LinkedIn, email)

## File Structure

- `index.html` — single file, all CSS inlined in `<style>`, all JS inlined in `<script>`
- No external JS dependencies
- Google Fonts loaded via `<link>` (Inter)
- Deploy repo: psilde.github.io

## Constraints

- No emoji in copy
- No placeholder content — all text is final
- All project GitHub links use psilde's actual repos
- Smooth scroll between sections (`scroll-behavior: smooth`)
- Responsive: single column on mobile (<768px), two columns on tablet+
- No placeholder images — purely typographic/geometric design
