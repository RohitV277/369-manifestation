# 369 Manifestation Journal

A self-contained web app: a full calendar, per-day 369 affirmation worksheets, and a
Settings drawer (theme, background, fonts, watermark). No build step — just static
files, ready to host anywhere.

## Files

```
369-pwa/
├── index.html          the whole app (markup, styles, and logic in one file)
├── manifest.json        web app manifest (name, icons, standalone display mode)
├── service-worker.js    caches the app so it opens instantly offline after first load
├── icons/
│   ├── icon-180.png      iOS home-screen icon
│   ├── icon-192.png      Android/Chrome icon
│   ├── icon-512.png      Android/Chrome splash icon
│   ├── favicon-32.png    browser tab icon
│   └── favicon-16.png    browser tab icon (small)
└── README.md            this file
```

## Why this fixes the "opens inside Claude" problem

Right now, opening the app through a claude.ai link means Safari is really adding
*claude.ai's* page to your Home Screen, with Claude's own toolbar wrapped around
the app. Deploying these files to your own domain removes that wrapper entirely —
Safari will see *only* this app's `index.html`, `manifest.json`, and icons, so
"Add to Home Screen" will launch it full-screen with no browser bar and no Claude
UI, the same way PDF Chef does.

## Deploy it (free, no coding required)

Any static host works. Two easy options:

### Option A — Cloudflare Pages (what PDF Chef itself uses)
1. Go to [pages.cloudflare.com](https://pages.cloudflare.com) and sign up (free).
2. Click **Create a project → Upload assets** (direct upload, no Git needed).
3. Drag the whole `369-pwa` folder in.
4. Cloudflare gives you a URL like `369-manifest.pages.dev` — that's a real domain.

### Option B — Netlify Drop
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the `369-pwa` folder onto the page.
3. It deploys in seconds and gives you a URL like `random-name.netlify.app`.
   You can rename it for free in the site settings.

### Option C — GitHub Pages
1. Create a new GitHub repository and upload these files to it.
2. In the repo's **Settings → Pages**, set the source to the `main` branch, root folder.
3. GitHub gives you a URL like `yourname.github.io/repo-name`.

## After deploying

1. Open your new URL in **Safari** on your iPhone/iPad (not the claude.ai link).
2. Tap **Share → Add to Home Screen**.
3. Launch it from that new icon — it will open standalone, no browser bar.

## Two honest trade-offs of self-hosting

- **Cross-device sync won't work here.** The "Synced across devices" feature in
  Settings relies on Claude's backend, which only exists inside claude.ai. On your
  own domain, the app automatically falls back to saving locally on each device
  (it already does this gracefully — nothing will break, entries just won't sync
  between devices anymore).
- **Fonts still need internet the first time.** The custom fonts load from Google
  Fonts; if that request fails, the app falls back to your device's built-in
  fonts automatically. After the first successful load, the service worker keeps
  the app itself working fully offline regardless.

## Making changes later

`index.html` is the entire app — open it in any text editor to make changes.
If you ever update it, also bump the version number in `service-worker.js`
(change `369-manifest-cache-v1` to `-v2`, etc.) so visitors get the new version
instead of a cached old one.
