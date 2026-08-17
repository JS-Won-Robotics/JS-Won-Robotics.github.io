# JS-Won-Robotics.github.io

Academic personal website of Jaeseog Won, built with [Jekyll](https://jekyllrb.com/) and the [Academic Pages](https://github.com/academicpages/academicpages.github.io) template (a fork of [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)), deployed to GitHub Pages at <https://js-won-robotics.github.io>.

## Local development

Requires Ruby 3.x and Bundler.

```bash
bundle install
bundle exec jekyll serve -l -H 0.0.0.0   # http://127.0.0.1:4000
```

Or with Docker (no local Ruby needed):

```bash
docker compose up
```

## Adding content

Each kind of content is one Markdown file in its own directory:

| Directory        | Shows up on          |
| ---------------- | -------------------- |
| `_publications/` | `/publications/`     |
| `_talks/`        | `/talks/`            |
| `_teaching/`     | `/teaching/`         |
| `_portfolio/`    | `/portfolio/`        |
| `_posts/`        | `/year-archive/`     |
| `_pages/`        | standalone pages     |

Files (PDFs, slides, etc.) go in `files/` and are served at `/files/<name>`. Images go in `images/`.

Site-wide settings live in `_config.yml`; the top navigation is `_data/navigation.yml`. `_pages/markdown.md` is the template's own syntax guide, viewable at `/markdown/`.

## Deployment

Pushing to `main` runs `.github/workflows/pages-deploy.yml`, which builds the site and deploys it to GitHub Pages. The repository's Pages source must be set to **GitHub Actions**.

## License

MIT — see [LICENSE](LICENSE). Template © Michael Rose and the Academic Pages contributors.
