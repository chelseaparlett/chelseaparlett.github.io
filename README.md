# Quarto blog starter

A minimal Quarto website, set up for blogging and ready to host on GitHub Pages.

## What's here

- `_quarto.yml` -- site configuration: navbar, theme, search, and `freeze` settings
- `index.qmd` -- the post listing page (built automatically from `posts/`)
- `about.qmd` -- an about page (uses Quarto's built-in "jolla" template)
- `profile.svg` -- placeholder photo for the about page; replace it
- `posts/` -- each post is its own folder containing an `index.qmd`
- `posts/_metadata.yml` -- shared defaults (author, table of contents) for all posts
- `styles.css` -- a handful of small tweaks on top of the Cosmo/Darkly themes
- `.github/workflows/publish.yml` -- optional GitHub Actions workflow for automated deploys

## 1. Prerequisites

- [Quarto CLI](https://quarto.org/docs/get-started/) installed locally
- R (or Python) with whatever packages your posts actually use -- the sample
  post only needs `ggplot2`

## 2. Local preview

From the project root:

```bash
quarto preview
```

This opens a live-reloading preview in your browser. Edit `.qmd` files and it updates automatically.

## 3. Writing posts

Create a new folder under `posts/`, e.g. `posts/2026-07-01-my-new-post/`, with an `index.qmd` inside it. Give it frontmatter like:

```yaml
---
title: "My New Post"
description: "One sentence describing it"
date: 2026-07-01
categories: [topic-a, topic-b]
---
```

The listing page (`index.qmd` at the project root) picks these up automatically, sorted newest-first, with an RSS feed generated for free.

## 4. Putting it on GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
```

Then update `site-url` in `_quarto.yml` to `https://your-username.github.io/your-repo/` (or your custom domain, if you set one up).

## 5. Publishing to GitHub Pages

There are two reasonable ways to do this. Pick one.

### Option A -- render locally, serve from `/docs` (simplest)

1. `quarto render` -- this writes the built site into `docs/` (per `output-dir: docs` in `_quarto.yml`)
2. Commit and push, including the `docs/` folder
3. In your repo: **Settings > Pages > Build and deployment > Source**, choose **"Deploy from a branch"**, then branch `main`, folder `/docs`

Every time you want to update the live site, re-run `quarto render` and push.

### Option B -- GitHub Actions (automated)

The included `.github/workflows/publish.yml` renders the site on every push to `main` and publishes it to a `gh-pages` branch.

1. In your repo: **Settings > Pages > Build and deployment > Source**, choose **"Deploy from a branch"**, then branch `gh-pages`, folder `/ (root)` -- this branch is created automatically the first time the workflow runs
2. Push to `main` and check the **Actions** tab for progress

#### A note on R/Python code and CI

`_quarto.yml` sets `execute: freeze: auto`. The first time you render a post locally (`quarto render` or `quarto preview`), Quarto caches the executed code output in a `_freeze/` folder. If you commit `_freeze/`, the GitHub Actions runner can reuse those cached results and doesn't need R or Python installed at all.

If you add a brand-new post with code and push it *without* rendering locally first, the Actions build will need R/Python (and any system libraries, e.g. for `gganimate` or `magick`) -- in that case, uncomment and fill in the R/Python setup steps in `publish.yml`. The simplest workflow, though, is: render locally once so `_freeze/` is populated, commit it, then let CI just reuse it.

## 6. Customizing

- **Title, navbar links, theme**: `_quarto.yml`
- **About page content, photo, social links**: `about.qmd` and `profile.svg`
- **Colors/fonts**: `styles.css` (currently a slate-blue accent with a serif/sans pairing -- change `--accent` and the font imports to taste)
- **Comments**: Quarto has built-in support for giscus and Utterances if you'd like reader comments on posts
