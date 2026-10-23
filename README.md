# duzhuo.icu

Personal site for Junxiang Shao. Plain static HTML — no dependencies, no build step. Edit and deploy.

## Structure

```
.
├── index.html            # Home page (hero + about)
├── 404.html              # Standalone 404 page
├── robots.txt            # Crawler policy
├── favicon.ico           # Site icon
├── css/style.css         # Styles
├── assets/my_avatar.webp # Avatar
├── .nojekyll             # Keeps GitHub Pages from running Jekyll
├── .gitignore
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
| Change name / tagline / meta line | `<h1 class="hero-name">`, `.hero-tagline`, `.hero-meta` in `index.html` |
| Change the avatar | Replace `src` on `<img class="avatar">` in `index.html` (keep `width` / `height` to avoid layout shift) |
| Change the social links | `<nav class="links">` in `index.html` |
| Change the email | Re-encode with `printf '%s' 'you@example.com' | base64` and update `data-b64` |
| Fill in the About text | `<div class="prose">` in `index.html` |
| Change accent colors | `:root` variables in `css/style.css` |

## Layout decisions

- Single `.container` capped at `720px`; the hero, divider, prose and footer
  all share that edge, so nothing looks ragged on wide screens.
- `.prose` deliberately has **no** `max-width`. A `60ch` cap reads better per
  line but breaks the shared left/right edge. To trade alignment for
  readability, add `max-width: 68ch` to `.prose`.
- Body text is capped by the container rather than `ch`, so line length grows
  with the viewport until `720px`. Below that it reflows freely.
- `html { scrollbar-gutter: stable }` keeps pages with and without a scrollbar
  from shifting horizontally.

## Notes

- `404.html` is served automatically on Cloudflare Pages / GitHub Pages for
  unknown paths — there is no SPA fallback (`_redirects` was removed).
- `.nojekyll` must stay committed for GitHub Pages; it is intentionally not
  ignored by `.gitignore`.

## Git

The repo is already initialised; `master` tracks `origin/master`.

After editing:

```bash
git status
git add -A
git commit -m "style: shorten the about section"
git push
```

Commit subjects follow [Conventional Commits](https://www.conventionalcommits.org/)
(`feat`, `fix`, `style`, `chore`, `docs`), which keeps `git log --oneline`
readable as the site grows.
