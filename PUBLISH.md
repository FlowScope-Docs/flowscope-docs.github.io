# Publish — FlowScope org + GitHub Pages

Canonical site URL:

`https://flowscope.github.io/`

Repo: `https://github.com/FlowScope/flowscope.github.io`

Local folder name stays `FlowScope-Guidebook`; only the GitHub org/repo name is branded.

## First-time setup (you do this once)

1. On GitHub, create organization **`FlowScope`** (if the name is taken, use `FlowScope-Docs` and replace URLs below with `https://flowscope-docs.github.io/`).
2. In that org, create an empty **public** repository named **`flowscope.github.io`** (no README / license / gitignore if this folder already has commits).
3. From this folder:

```powershell
git branch -M main
git remote remove origin 2>$null
git remote add origin https://github.com/FlowScope/flowscope.github.io.git
git push -u origin main
```

4. On GitHub: **Settings → Pages → Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main` / `/ (root)`
   - Save
5. Wait 1–2 minutes, then open `https://flowscope.github.io/`

Docsify serves `index.html` from the repo root. `.nojekyll` is present (do not enable Jekyll).

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

Open the printed URL; Docsify loads from `index.html` with `theme.css`.

## App link

FlowScope navbar Guidebook icon defaults to `https://flowscope.github.io/` (`NEXT_PUBLIC_GUIDEBOOK_URL` can override).
