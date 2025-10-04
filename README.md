# Stars and Planets — Unity WebGL build

This repository contains a Unity WebGL build of "Stars and Planets" ready to be deployed as a static site.

Contents
- `index.html` — entry page
- `Build/`, `TemplateData/`, `StreamingAssets/` — Unity build output
- `vercel.json` — headers to ensure correct MIME types and caching on Vercel
- `.gitattributes` — Git LFS tracking for large build files

Quick status
- Large Unity build files are tracked with Git LFS (see `.gitattributes`).
- `vercel.json` maps `.wasm` and `.unityweb` to proper Content-Type and caching headers.

Prerequisites
- git
- git-lfs (installed and configured)
- (optional) Vercel account and CLI (`npm i -g vercel`)

Deploy options

1) Deploy via Vercel dashboard (recommended, easiest)

- Go to https://vercel.com and create/connect your account.
- Import this repository (`Romio1310/StarsAndPlanets`) as a New Project.
- Framework Preset: Other.
- Root Directory: (leave empty / root of repo).
- Vercel will build and deploy automatically on pushes to `main`.

2) Deploy via Vercel CLI

Install and login:

```bash
npm i -g vercel
vercel login
```

From the repo root:

```bash
vercel --prod
```

This will deploy the current contents. For automated CI deploys, see GitHub Actions below.

GitHub Actions (automated deploy)

This repo includes a workflow `.github/workflows/deploy-to-vercel.yml` that will deploy to Vercel on pushes to `main` using the Vercel GitHub Action. To enable it, add the following repository secrets in GitHub:

- `VERCEL_TOKEN` — a personal token from Vercel (Account Settings -> Tokens)
- `VERCEL_ORG_ID` — your Vercel team/org ID (if applicable)
- `VERCEL_PROJECT_ID` — Vercel project ID (optional if the project is linked)

If you prefer to let Vercel handle deployments from the dashboard (via GitHub integration), you don't need the workflow — Vercel will deploy automatically when the repo receives pushes.

Local test (quick)

Run a static server from the repo root and open http://localhost:8000:

```bash
# python built-in
python3 -m http.server 8000

# or using http-server (recommended for better MIME handling)
npm install -g http-server
http-server -p 8000 -c-1
```

Verify in browser devtools -> Network that the `.wasm` files have `Content-Type: application/wasm` and your `.unityweb` files have `application/octet-stream` (or similar). The provided `vercel.json` ensures these headers on Vercel.

About Git LFS migration (already done)

This repository migrated existing large build files into Git LFS and forced the rewritten history to the remote. A backup branch named `backup-before-lfs-migration` was created and a bundle `StarsAndPlanets-backup.bundle` was written to the parent directory before migration.

If you're a collaborator and see issues after the migration, re-clone the repo:

```bash
git clone https://github.com/Romio1310/StarsAndPlanets.git
```

Or reset your local branch:

```bash
git fetch origin
git checkout main
git reset --hard origin/main
git clean -fdx
```

Monitoring & troubleshooting deploys

- On Vercel, open the project dashboard → Deployments to view logs and recent builds.
- If you see MIME or caching issues, double-check `vercel.json` (already included) and re-deploy.
- If Vercel doesn't fetch LFS objects, ensure the Vercel build environment supports LFS (GitHub integration generally does). The GitHub Action included below uses a Vercel token and will fetch LFS objects when running.

If you want, I can add a GitHub Actions status badge or further CI steps to run tests or smoke-check the site after deploy.
