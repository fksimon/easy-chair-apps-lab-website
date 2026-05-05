# Easy Chair Apps Lab — Website

Marketing and support website for Easy Chair Apps Lab apps. Hosted on GitHub Pages at [easychairappslab.com](https://easychairappslab.com).

## Structure

```
index.html              Home page (app showcase, about)
scorepad.html           ScorePad marketing page
retirementplanner.html  Retirement Planner marketing page
privacy.html            Privacy policy
terms.html              Terms of use
support.html            Support page
styles.css              Shared stylesheet
logo.png                Lab logo
fonts/                  Self-hosted Fraunces font (woff2)
scorepad/               ScorePad assets (icon)
retirementplanner/      Retirement Planner assets (icon)
```

## Testing locally

The site is plain HTML/CSS — no build step required. Serve it with Python's built-in HTTP server so fonts and relative paths resolve correctly:

```bash
cd /path/to/easy-chair-apps-lab-website
python3 -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000) in your browser.

To use a different port:

```bash
python3 -m http.server 3000
```

> **Note:** Opening `index.html` directly as a `file://` URL works for most content, but the self-hosted fonts in `fonts/` may be blocked by the browser's CORS policy. Use the Python server to avoid this.

## Deployment

Pushes to `main` deploy automatically via GitHub Pages. The custom domain is configured in `CNAME`.

## Claude Code skills

### `/update-app-pages`

Rebuilds app marketing pages from their source `MARKETING.md` files. Run from the repo root in Claude Code:

```
/update-app-pages                  # rebuild all app pages
/update-app-pages retirementplanner  # rebuild one page
/update-app-pages scorepad
```

The skill reads each app's `MARKETING.md`, compares its content to the current HTML page, and does a full rebuild of any page where the content has changed. The site's CSS, layout, animations, and icons are preserved — only the text content, feature cards, and sections are updated from the Markdown source.

The skill definition lives at `.claude/commands/update-app-pages.md`.

#### Adding a new app

1. Add a row to the source-to-page mapping table in `.claude/commands/update-app-pages.md`:

   ```markdown
   | `~/Workspace/MyApp/MARKETING.md` | `myapp.html` |
   ```

2. Create the initial `myapp.html` following the same structure as `scorepad.html` or `retirementplanner.html` — same `<header>`, `<footer>`, shared `styles.css`, and page-scoped CSS prefix (e.g. `ma-`).

3. Add the app icon at `myapp/icon.png`.

4. Add a card for the app in `index.html` under the `app-grid` section.

5. Run `/update-app-pages myapp` to do the first content pass from the Markdown source.
