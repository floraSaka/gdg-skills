# Deploying GDG Skills to GitHub Pages

Everything is flat files at the repo root — no build step. All paths in the app are
relative (`./…`), so it works from a project subpath like
`https://<you>.github.io/gdg-skills/` as well as from a root domain. The service
worker is registered as `./sw.js`, which scopes it to the repo subpath automatically.

## One-time setup

1. Create a new GitHub repository (e.g. `gdg-skills`). Public is simplest for Pages.
2. From this folder:

   ```
   git init
   git add .
   git commit -m "GDG Skills PWA"
   git branch -M main
   git remote add origin https://github.com/<you>/gdg-skills.git
   git push -u origin main
   ```

3. On GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root)** → Save.
4. Wait a minute, then open `https://<you>.github.io/gdg-skills/`.

The `.nojekyll` file is required — it stops GitHub running Jekyll over the files.

## Installing on the iPhone

1. Open the Pages URL in **Safari** (must be Safari for Add to Home Screen).
2. Share button → **Add to Home Screen** → Add. The tile is the navy GDG icon.
3. Open it from the home screen: full-screen, no browser chrome, portrait.
4. It works offline after the first open — the service worker caches the app shell.

## Updating

Edit the files, then:

```
git add . && git commit -m "update" && git push
```

If you changed `index.html`, `manifest.json` or the icons, **bump the version in
`sw.js`** (`gdg-skills-v1` → `-v2`) in the same commit — that is what makes installed
phones fetch the new shell instead of serving the old cache forever. Old caches are
cleaned automatically on activate. An installed app picks the update up on the second
open after the push.

## Notes

- Logging data lives in the phone's own localStorage, per dog — deploys never touch it.
- To merge a phone session back into the desktop matrix file: **Data → Copy JSON** on
  the phone, then paste over the contents of `<script id="state">…</script>` in the
  desktop file.
