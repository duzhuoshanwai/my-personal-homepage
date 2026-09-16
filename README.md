# duzhuo.icu

Personal placeholder page ("under construction"). Plain static HTML — no dependencies, no build step. Edit and deploy.

## Structure

```
.
├── index.html            # Home page (the placeholder itself)
├── 404.html              # Standalone 404 page
├── robots.txt            # Blocks crawlers while under construction
├── _redirects            # Catch-all: unknown paths → /index.html (200)
├── favicon.ico           # Site icon
├── favicon.svg           # Site icon (vector)
├── css/style.css         # Styles
├── assets/construction.webp
├── .nojekyll             # Keeps GitHub Pages from running Jekyll
└── README.md
```

## Preview locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Opening `index.html` directly also works, but the absolute `/css/style.css` path may break, so use a local server.

## Deploy

**Cloudflare Pages**

- Option A: push to GitHub, then Pages → Connect to Git. Leave the build command empty and set the output directory to `/`.
- Option B: deploy straight from the CLI:
  ```bash
  npx wrangler pages deploy . --project-name=duzhuo-icu
  ```

**GitHub Pages**: Settings → Pages → pick the branch root (`.nojekyll` is already in place).

## Common edits

| Goal | File |
| --- | --- |
| Change the text | `<div class="text">` in `index.html` |
| Change accent/button colors | `:root` variables in `css/style.css` |
| Change the icon | Replace `assets/construction.webp`, or edit the `<img>` `src` |
| Change the button link | `<a class="button" href="...">` in `index.html` |
| Go live | Replace `index.html`, then remove `Disallow: /` from `robots.txt` and the `<meta name="robots">` tag |

## Notes

- `_redirects` sends every unknown path to `/index.html` with a `200`, so no real 404s are returned. Fine for SPAs, but search engines may treat it as a soft 404. Remove it once the real site is up so `404.html` can do its job.
- The image used to be hosted on `images.duzhuo.icu`. A copy now lives in `assets/` so the site is self-contained and works offline.

## Git

```bash
git init
git add -A
git commit -m "init: duzhuo.icu placeholder site"
git branch -M main
git remote add origin git@github.com:<your-account>/<repo>.git
git push -u origin main
```
