# Publish — GitHub + Pages

Public site URL (after Pages is enabled):

`https://phoenixz33.github.io/FlowScope-Guidebook/`

Repo: `https://github.com/PhoenixZ33/FlowScope-Guidebook`

## First-time setup

1. Create an empty **public** GitHub repo `PhoenixZ33/FlowScope-Guidebook` (no README/license if this folder already has commits).
2. From this folder:

```powershell
git remote add origin https://github.com/PhoenixZ33/FlowScope-Guidebook.git
git branch -M main
git push -u origin main
```

3. On GitHub: **Settings → Pages → Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main` / `/ (root)`
   - Save
4. Wait 1–2 minutes, then open `https://phoenixz33.github.io/FlowScope-Guidebook/`

Docsify serves `index.html` from the repo root. Do not enable Jekyll (`.nojekyll` is present).

## Later updates

```powershell
git add -A
git commit -m "Docs — update guidebook"
git push
```

Pages redeploys from `main` automatically.

## Local preview

```powershell
npx --yes serve .
```

Open the printed URL; Docsify loads from `index.html`.
