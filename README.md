# temhan.dev — source

Source for [temhan.dev](https://temhan.dev), Temur Khan's personal practitioner site. Quarto-based, deploys to Cloudflare Pages.

## Stack

- **[Quarto](https://quarto.org)** — markdown-driven static site generator
- **Theme:** cosmo (light) with custom CSS overrides in `styles.css`
- **Hosting:** Cloudflare Pages (domain registered at Cloudflare)
- **Deploy:** auto on push to `main` branch via Cloudflare Pages GitHub integration

## Project structure

```
temhan-site/
├── _quarto.yml          # project config (nav, theme, metadata)
├── index.qmd            # homepage — Hero + Tools + Recent writing + Contact
├── hire.qmd             # services page — Audits + Custom MCP Builds
├── tools.qmd            # detailed list of 7 MCPs + Aufgaard
├── posts.qmd            # blog index (auto-generated from posts/)
├── about.qmd            # About page
├── posts/
│   ├── _metadata.yml    # default settings for posts
│   └── silent-failure-patterns/
│       └── index.qmd    # first blog post (canonical version)
├── styles.css           # custom CSS overrides on cosmo theme
├── .gitignore
└── README.md
```

## Local preview

```bash
# 1. Install Quarto CLI (one-time)
# Mac:     brew install --cask quarto
# Windows: winget install --id Posit.Quarto
# Linux:   download .deb/.rpm from https://quarto.org/docs/get-started/

# 2. Preview locally with hot reload
cd Build/temhan-site
quarto preview
# → opens http://localhost:8888 with auto-refresh on file save
```

## Build

```bash
quarto render
# → outputs to _site/ (gitignored)
```

## Deploy via Cloudflare Pages (recommended — domain is at Cloudflare)

**One-time setup:**

1. Push this directory to a GitHub repo: `temurkhan13/temhan-site` (or similar — separate repo, NOT under the existing 7 MCP repos)
2. Cloudflare Dashboard → Pages → "Create application" → "Connect to Git"
3. Select the GitHub repo
4. Build settings:
   - **Framework preset:** Quarto (or "None" if not listed)
   - **Build command:** `quarto render`
   - **Build output directory:** `_site`
5. Deploy. First build takes ~2 min.
6. Connect custom domain: Cloudflare Pages → Custom domains → Add `temhan.dev` → Cloudflare auto-configures DNS (CNAME or AAAA) since the domain is in the same Cloudflare account.

**After setup:** every push to `main` triggers a redeploy. Live within ~90 seconds of push.

## Deploy via GitHub Pages (alternative)

If preferring GitHub Pages over Cloudflare Pages:

1. Push to GitHub repo with a `gh-pages` branch
2. Add a GitHub Actions workflow at `.github/workflows/quarto-publish.yml` (Quarto provides a template)
3. Enable Pages in repo settings, source = `gh-pages` branch
4. Add CNAME file with `temhan.dev` content
5. Configure DNS at Cloudflare to point CNAME `temhan.dev` → `temurkhan13.github.io`
6. GitHub Pages auto-provisions SSL via Let's Encrypt

Cloudflare Pages is simpler given the domain is already there. Use GitHub Pages only if Cloudflare Pages has an issue.

## Adding a new blog post

```bash
mkdir posts/your-post-slug
$EDITOR posts/your-post-slug/index.qmd
```

Front matter template:

```yaml
---
title: "Your Post Title"
description: "1-2 line description for OG cards + listing"
date: 2026-MM-DD
categories: [production-ai, mcp]
author: "Temur Khan"
toc: true
toc-depth: 2
reading-time: true
---
```

Then write the post body in markdown. `quarto preview` for hot reload, `git push` to deploy.

## Updating navigation / footer

Edit `_quarto.yml`. Restart `quarto preview` after changes.

## Updating the homepage tools table

Edit `index.qmd` directly. The "Tools I've shipped" table is the authoritative list — when a new MCP ships, add a row here AND in `tools.qmd` (which has the long-form descriptions).

## Notes

- The site is intentionally minimal — no JS frameworks, no client-side routing, no heavy assets. Static HTML + minimal CSS.
- Cosmo theme chosen because Hamel uses it (the closest archetype peer) — see [`Plan/positioning-lock.md`](../../Plan/positioning-lock.md) and [`Analyses/2026-05-10_archetype-channel-tactics.md`](../../Analyses/2026-05-10_archetype-channel-tactics.md) for the strategic reasoning.
- All Pixelette client work (V1, V2, TLH, CMP) is **off-limits as content** per the brand-wall — see [`Rules/NEVER-DO.md`](../../Rules/NEVER-DO.md). Keep posts grounded in publicly-sourced research (HN/Reddit/X threads) + own MCP development, not client-engagement war stories.
- Profile photo placeholder: `profile.jpg` — drop in when ready (referenced from `index.qmd` if `about.template: trestles` is enabled).
