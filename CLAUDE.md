# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A documentation site (IT notes, snippets, cheat sheets, tutorials) published at https://docs.surikata3k.com/. It is pure Markdown built with [Zensical](https://zensical.org) (a Material-for-MkDocs-style static site generator). There is no application code or test suite.

## Commands

```bash
pip install -r requirements.txt   # Zensical version is pinned here
zensical serve                    # live preview at http://localhost:8000
zensical build --clean            # build to site/ (what CI runs)
```

If `zensical` isn't on PATH, use `python -m zensical serve`.

## Architecture

- `zensical.toml` is the only config: site metadata, theme features, palette, and the Markdown extensions (admonitions, tabs, mermaid fences, snippets, etc.). The `nav` key is commented out on purpose.
- **Navigation is auto-generated from the `docs/` folder structure.** Each top-level folder (`guides/`, `linux/`, `windows/`, `snippets/`) is a nav section/tab; adding a `.md` file makes it appear without editing config. Don't add a manual `nav` unless asked.
- `docs/stylesheets/extra.css` is the only custom asset (loaded via `extra_css`).
- `pymdownx.snippets` base path is `docs`, with `check_paths = true`, so a bad `--8<--` include fails the build.
- Deploy: pushes to `main` trigger `.github/workflows/docs.yml`, which builds and publishes to GitHub Pages. Output dir `site/` is gitignored.
- Docker material is intentionally *not* here; `docs/guides/docker.md` only links to the separate `surikata3k/docker` repo.
