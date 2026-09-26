# Setup — Private Core + Public Runner

One-time, ~15 minutes.

## 1. Repos

1. **Private core** — push the `ToolFactory` project (all code + `data/` + `content/`)
   to `ErBharatMalhotra/ToolFactory` (**Private**).
2. **Public runner** — push the `tf-public/` contents to a new **public** repo,
   e.g. `ErBharatMalhotra/toolfactory-site`.

## 2. Fine-grained token (CORE_TOKEN)

GitHub → Settings → Developer settings → Fine-grained tokens → Generate:

- **Repository access:** only `ToolFactory` (the private core)
- **Permissions:** Contents → **Read and write**
- Set an expiry you'll remember; regenerate before it lapses

Add it in the **public** repo: Settings → Secrets and variables → Actions →
New repository secret → Name: `CORE_TOKEN`.

No other secrets are needed — the weekly release runs fully offline
(deterministic content, no LLM keys).

## 3. Cloudflare Pages

Dashboard → Workers & Pages → Create → **Connect to Git** → pick the **public**
repo:

- Production branch: `main`
- Framework preset: **None**
- Build command: *(empty)*
- Output directory: **`site/dist`**

Every workflow push of `site/dist/` auto-deploys.

## 4. First run

Public repo → Actions → **weekly-release** → **Run workflow**. Watch it:
clone core → gate → tests → publish batch → rebuild → verify → push site.

## 5. Keepalive

GitHub disables scheduled workflows on public repos after **60 days of repo
inactivity**. The weekly workflow commits to the public repo on every run
(new site build), so the repo never goes quiet — the keepalive is built in.

## Troubleshooting

- `Clone private core` fails → `CORE_TOKEN` expired or missing repo scope
- `Privacy gate` fails → something pipeline-flavoured reached the site; the
  gate output names the exact file
- Site not updating → check the "Commit and push" step, then the Cloudflare
  Pages deployment log
