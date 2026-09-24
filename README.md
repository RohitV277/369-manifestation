# 369 Manifestation Journal

A self-contained web app: a full calendar, per-day 369 affirmation worksheets, and a
Settings drawer (theme, background, fonts, watermark). No build step, no subfolders —
just 8 files, ready to upload anywhere.

## Files

```
369-pwa/
├── index.html          the whole app (markup, styles, and logic in one file)
├── manifest.json        web app manifest (name, icons, standalone display mode)
├── service-worker.js    caches the app so it opens instantly offline after first load
├── icon-180.png          iOS home-screen icon
├── icon-192.png          Android/Chrome icon
├── icon-512.png          Android/Chrome splash icon
├── favicon-32.png        browser tab icon
├── favicon-16.png        browser tab icon (small)
└── README.md            this file
```

Everything sits in one flat folder — no subfolders to recreate, so uploading through
a mobile browser (e.g. GitHub's "upload files" screen) is just: select all 8 files,
drop them in, commit. Nothing needs renaming or path-prefixing.

## Why this fixes the "opens inside Claude" problem

Right now, opening the app through a claude.ai link means Safari is really adding
*claude.ai's* page to your Home Screen, with Claude's own toolbar wrapped around
the app. Deploying these files to your own free address removes that wrapper
entirely — Safari will see *only* this app's own files, so "Add to Home Screen"
will launch it full-screen with no browser bar and no Claude UI, the same way
PDF Chef does.

## Deploy it (free, no domain purchase required)

### Option A — GitHub Pages
1. Create a free account at [github.com](https://github.com) if you don't have one.
2. New repository → any name → Public.
3. "Upload files" → select all 8 files from this folder → Commit.
4. Repo → Settings → Pages → Source: "Deploy from a branch" → `main` → `/ (root)` → Save.
5. After a minute, your live link appears: `https://yourusername.github.io/repo-name/`.

### Option B — Netlify Drop (no account needed to try it)
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag this whole folder onto the page.
3. You get a live link instantly, like `random-name.netlify.app`.

### Option C — Cloudflare Pages (what PDF Chef itself uses)
1. Go to [pages.cloudflare.com](https://pages.cloudflare.com), free sign-up.
2. Create a project → Upload assets → drag this folder in.
3. You get a live link like `369-manifest.pages.dev`.

## After deploying

1. Open your new URL in **Safari** on your iPhone/iPad (not the claude.ai link).
2. Tap **Share → Add to Home Screen**.
3. Launch it from that new icon — it opens standalone, no browser bar.

## Two honest trade-offs of self-hosting

- **Cross-device sync won't work here.** The "Synced across devices" feature in
  Settings relies on Claude's backend, which only exists inside claude.ai. On your
  own address, the app automatically falls back to saving locally on each device
  (nothing breaks — entries just won't sync between devices anymore).
- **Fonts still need internet the first time.** The custom fonts load from Google
  Fonts; if that request fails, the app falls back to your device's built-in
  fonts automatically. After the first successful load, the service worker keeps
  the app itself working fully offline regardless.

## Making changes later

`index.html` is the entire app — open it in any text editor to make changes.
If you ever update it, also bump the version number in `service-worker.js`
(change `369-manifest-cache-v1` to `-v2`, etc.) so visitors get the new version
instead of a cached old one.
