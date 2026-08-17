# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Academic personal website for GitHub Pages (https://js-won-robotics.github.io), built with Jekyll and the [Academic Pages](https://github.com/academicpages/academicpages.github.io) template — itself a fork of [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) extended with academic collections.

Unlike a gem-based theme, **the entire theme source is vendored in this repo**: `_layouts/`, `_includes/`, `_sass/`, `assets/`. Editing them is normal and expected, but it also means upstream template fixes must be merged in by hand. Before changing presentation, check whether `_config.yml` (`site_theme`, `author:` block) or `_data/` already exposes the knob.

## Commands

```bash
bundle install                            # requires Ruby 3.x
bundle exec jekyll serve -l -H 0.0.0.0    # local preview at http://127.0.0.1:4000
JEKYLL_ENV=production bundle exec jekyll build
docker compose up                         # same preview without a local Ruby
```

There are no tests. `bundle exec jekyll build` succeeding is the check — CI does exactly that.

Note: this machine has no local Ruby or running Docker daemon, so builds cannot be verified here. Changes are validated by the GitHub Actions build on push.

## Deployment

Pushing to `main` triggers `.github/workflows/pages-deploy.yml` (Ruby 3.3 → `jekyll build` → `deploy-pages`). The repo's Pages source must be "GitHub Actions". Changes limited to `README.md`, `LICENSE`, `CLAUDE.md`, or `.gitignore` do not trigger a deploy.

The workflow deliberately does *not* use `actions/configure-pages`: this is a user site at the domain root, so `baseurl` in `_config.yml` is authoritative, and running a Pages API call before the build would report build success/failure as a Pages error. If `deploy-pages` fails with "Creating Pages deployment failed", Pages is not enabled for the repo — on a free plan that requires the repo to be public.

This workflow was written for this repo; the template's own upstream workflows (`bad-pr`, `close-tests`, `jekyll-build`, `scrape_talks`) were deleted because they exist to maintain the template repository, not sites built from it.

## Content model

The template's whole design is **structured data over prose**: each item of academic content is one Markdown file whose front matter drives several rendered surfaces at once. A `_talks/` entry, for example, feeds the talks index, its own permalink page, and the talks section of `/cv/` — so front matter fields are load-bearing, not decoration.

| Directory        | Collection | URL                  | Layout   |
| ---------------- | ---------- | -------------------- | -------- |
| `_publications/` | yes        | `/publications/<slug>/` | `single` |
| `_talks/`        | yes        | `/talks/<slug>/`     | `talk`   |
| `_teaching/`     | yes        | `/teaching/<slug>/`  | `single` |
| `_portfolio/`    | yes        | `/portfolio/<slug>/` | `single` |
| `_posts/`        | built-in   | `/year-archive/`     | `single` |
| `_pages/`        | pages      | per-page `permalink` | `single` |

- Filenames in all collections start `YYYY-MM-DD-`; the date drives sort order on index pages.
- `_publications/` entries use `category:` matching a key under `publication_category:` in `_config.yml` (`books`, `manuscripts`, `conferences`) to land in the right section of `/publications/`. A category not listed there silently vanishes from the page.
- One sample file is kept in each collection as a format reference — read it before authoring a new entry rather than guessing the front-matter keys.
- `_pages/markdown.md` (served at `/markdown/`) is the template's own authoring guide, covering MathJax, Mermaid, and Plotly support. Consult it for syntax questions.
- Uploads: PDFs and slides in `files/` → `/files/<name>`; images in `images/`.
- `_pages/about.md` has `permalink: /` — it is the homepage, not a subpage.

## Config

- `_config.yml` — site identity plus the `author:` block that renders the entire left sidebar. Blank fields hide their icon, so leave unused social/academic links empty rather than filling in placeholders.
- `_data/navigation.yml` — top menu. Removing an entry hides the link but the page still builds and remains reachable by URL.
- `_data/ui-text.yml` — UI string translations; `_data/authors.yml` — multi-author post bylines.
- `_config_docker.yml` — overlay applied only by `docker compose up`.

## Tooling carried over from the template

`markdown_generator/` (Jupyter notebooks + scripts that turn a TSV/BibTeX of publications and talks into collection Markdown files) and `talkmap.py` / `talkmap.ipynb` (generates `/talkmap.html` from `_talks/` locations, requires `geopy`) are optional and unused so far. The workflow that auto-ran talkmap was removed, so run it locally if wanted.

## History

This repo previously used the Chirpy theme; the switch to Academic Pages replaced all site content and config. Earlier Chirpy posts and the Korean post-authoring template live only in git history before that commit — Chirpy-specific syntax (`{: .prompt-tip }`, `{: .filepath}`) does not render here.
