# ToolFactory — Release Runner

This repository hosts the **built site** for [ToolFactory](https://toolfactory.pages.dev)
and the scheduled job that ships each weekly batch of new tools.

- `site/dist/` — the complete static site (deployed via Cloudflare Pages)
- `.github/workflows/weekly-release.yml` — runs **every Monday 07:00 IST**,
  promotes the next batch of new tools and pushes a fresh build
- `data/live-tools.json` — public index of the tools currently live

The tool sources, generation logic and analytics live in a private repository.
This runner is intentionally minimal: schedule in, static site out.

## Manual run

Actions → **weekly-release** → *Run workflow* (first run is manual).

## Hosting

Cloudflare Pages is git-connected to this repo: framework **None**,
build command *(empty)*, output directory **`site/dist`**.
Every push auto-deploys.
