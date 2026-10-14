# CLAUDE.md — logicalmechanism.io-platform

## Project Overview

Static website for Logical Mechanism LLC — a Cardano smart contract and R&D company focused on cryptography and privacy (tagline: "Cryptography, privacy, and decentralized systems"). Makers of the Seedelf Wallet. Pure HTML and CSS, no JavaScript. Optimized for perfect PageSpeed Insights scores (100s across the board).

The company is deliberately anonymous: no people, names, or faces on the site. Client work is under NDA, so no client names or logos either. Trust comes from Logical Mechanism's own products and the infrastructure it runs.

## Tech Stack

- **HTML + CSS only** — no JavaScript, no frameworks, no build tools
- **Deployment:** DigitalOcean App Platform (static site, deploys on push to main)

## Project Structure

```
├── index.html          # The site — single-page landing
├── css/base.css        # Stylesheet source (edit this)
├── css/mini.base.css   # Minified stylesheet (what index.html loads)
├── fonts/              # Self-hosted IBM Plex Mono + Sans (latin, 400) and OFL license
├── img/mark.svg        # LM mark, standalone (for dark backgrounds)
├── img/og.png          # 1200x630 link-preview card (og:image)
├── img/fullLogo.png    # Old logo; referenced by stake pool + dRep metadata, do not delete
├── img/poolLogo.png    # Referenced by stake pool metadata, do not delete
├── fav/                # Favicons (svg + png + ico) and webmanifest
├── docs/               # Aiken-generated documentation (auto-generated, do not hand-edit)
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
- **No third-party requests** — everything is served from this domain
- Fonts are self-hosted WOFF2, preloaded, with `font-display: optional` (zero layout shift)
- Icons and the mark are inline SVG (no extra requests)
- Static assets cached 30 days via `static.json`
- Wrap every email address in `<!--email_off-->...<!--/email_off-->`; otherwise Cloudflare (in front of DigitalOcean) rewrites it and injects an email-decode script

## Color Palette

Dark slate-indigo with a blue-to-violet accent (taken from the 2026 brand art). Defined as CSS variables in `css/base.css`:

- **night** `#0c1020` — background
- **line** `#262f4b` — hairlines, borders
- **paper** `#e7eaf4` — primary text
- **mist** `#a1aac4` — secondary text
- **dim** `#8790ae` — tertiary text (lowest allowed; 5.97:1 on night)
- **blue** `#86aaf6` — accent, buttons, links (hover `#a9c2f9`)
- **violet** `#b9a4f3` — second accent; gradients run blue → violet

All text must stay at or above 4.5:1 contrast on `night`.

## Brand

- **Wordmark:** "LOGICAL MECHANISM" in IBM Plex Mono, uppercase, `letter-spacing: 0.32em`, stacked on two lines
- **Mark:** one continuous line `M34 52H12V12L32 34L52 12V…` (64×64 viewBox): the L's stem is also the M's left stem. Hollow nodes at the three upper joints. Gradient blue → violet. Favicons drop to solid nodes or no nodes at small sizes
- **Type:** IBM Plex Mono for the wordmark and headings, IBM Plex Sans for body text; both 400 only, no bold
- **Section dividers:** a hairline that starts in a small node ring, echoing the mark

## Motion

Two CSS-only effects, both off under `prefers-reduced-motion`:

- **Hero mark** draws itself once on load (`stroke-dashoffset` on a `pathLength="1"` path, then the nodes pop in)
- **Service titles decrypt** from hex into plain text: once on load (staggered, right after the mark draws, done by ~3s), and again as each service scrolls in from the bottom edge (scroll-driven `view-timeline`, range `entry 0% entry 75%`, inside `@supports (animation-timeline: view())`). It must finish before a title is fully on screen; a title must never sit scrambled where someone is reading it. No infinite loops (WCAG 2.2.2). The frames are pre-rendered in `index.html`: 8 rows per title, each the same length as the title, spaces kept in place, hex characters revealed in random order, last row is the plain title. The CSS assumes exactly 8 rows (`steps(8, jump-none)`, `translateY(-9.8em)` = 7 × 1.4em). If a service title changes, regenerate its 8 rows. The `.plain` span holds the real title for screen readers

## Code Style

- **Indentation:** 4 spaces in HTML
- **Semantic HTML:** proper `<header>`, `<main>`, `<footer>`, `<section>`, `<nav>` usage
- **Accessibility:** ARIA labels on links and images, proper alt text
- **Links:** external links use `target="_blank" rel="noopener noreferrer"`

## Key Conventions

- Commit messages: lowercase, short descriptive phrases (e.g., `fall 2025 update`, `site should be good now`); occasionally `chore:` prefix for maintenance
- Docs in `docs/` are auto-generated from the [Assist](https://github.com/logical-mechanism/Assist) Aiken library — do not hand-edit
- Copyright in footer: `© 2024-2026 Logical Mechanism LLC`
- CSS source is `css/base.css`, minified to `css/mini.base.css` via Python (no build tools required)
