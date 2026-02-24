# CLAUDE.md — logicalmechanism.io-platform

## Project Overview

Static website for Logical Mechanism LLC — a Cardano blockchain consulting and R&D company. Pure HTML and CSS, no JavaScript. Optimized for perfect PageSpeed Insights scores (100s across the board).

## Tech Stack

- **HTML + CSS only** — no JavaScript, no frameworks, no build tools
- **Deployment:** DigitalOcean App Platform (static site, deploys on push to main)

## Project Structure

```
├── index.html          # The site — single-page landing
├── css/mini.base.css   # Stylesheet (minified)
├── img/                # Images (avif/svg preferred)
├── fav/                # Favicons and webmanifest
├── docs/               # Aiken-generated documentation (auto-generated, do not hand-edit)
├── app/                # Dead code — legacy Next.js app, not in use
├── webserver.sh        # `python3 -m http.server` for local dev
├── update-docs.sh      # Pulls Assist repo, runs `aiken docs`, copies output to docs/
├── static.json         # Cache headers for static assets (30-day)
├── robots.txt          # SEO
├── sitemap.xml         # SEO
├── pgp.txt             # PGP public key
├── metadata.json       # Cardano metadata
└── .github/dependabot.yaml
```

## Commands

```bash
./webserver.sh          # Serve locally (python3 -m http.server)
./update-docs.sh        # Regenerate Aiken docs from Assist repo
```

## Performance Principles

- **No JavaScript** — the site does not need it, do not add any
- **PageSpeed 100s** — every change must preserve perfect Lighthouse scores
- Body starts `visibility: hidden` with inline dark mode CSS, then CSS loads async — FCP optimization hack
- Fonts (Comfortaa) preloaded async with noscript fallback
- Hero image (`img/world.avif`) preloaded with `fetchpriority="high"`
- Static assets cached 30 days via `static.json`
- Images use `loading="lazy"` and `decoding="async"` except the hero

## Code Style

- **Indentation:** 4 spaces in HTML
- **Semantic HTML:** proper `<header>`, `<main>`, `<footer>`, `<section>`, `<nav>` usage
- **Accessibility:** ARIA labels on links and images, proper alt text
- **Links:** external links use `target="_blank" rel="noopener noreferrer"`

## Key Conventions

- Commit messages: lowercase, short descriptive phrases (e.g., `fall 2025 update`, `site should be good now`); occasionally `chore:` prefix for maintenance
- Docs in `docs/` are auto-generated from the [Assist](https://github.com/logical-mechanism/Assist) Aiken library — do not hand-edit
- `app/` directory is dead code (legacy Next.js dApp) — ignore it
- Copyright in footer: `© 2024-2026 Logical Mechanism LLC`
- CSS source is `css/base.css`, minified to `css/mini.base.css` via Python (no build tools required)
