# Signal Forge — Landing Page

Marketing landing page for **Signal Forge**, a done-for-you lead-reactivation service:
it reactivates a local service business's old leads and past customers into booked
appointments through text/email follow-up campaigns. Pricing is pay-per-result — a flat
fee per appointment that shows, with the first 3 free. The page's single goal is to get a
visitor to book a 15-minute Calendly call.

The entire site is **one self-contained file: [`index.html`](index.html)** — all CSS and
JavaScript are inline. No frameworks, no build step, no dependencies. To make changes, edit
that one file.

---

## Project layout

| File | Purpose |
|------|---------|
| `index.html` | The whole landing page (HTML + inline CSS + inline JS). |
| `README.md` | This file — human-facing overview. |
| `CLAUDE.md` | Guidance for Claude Code / AI assistants working on the file. |
| `.claude/launch.json` | Local preview-server config (Python static server, port 4599). |

> **Note:** `privacy.html` and `terms.html` live in the deployed GitHub Pages repo, **not**
> in this working folder. The footer links `./privacy.html` and `./terms.html` resolve on the
> live site, so they will 404 if you open `index.html` locally on its own.

---

## Editing the page

A few things are intentionally single-sourced — change them in one place:

- **Per-appointment price:** do NOT type the dollar amount into the copy. One constant near
  the bottom of `index.html` fills every price on the page:
  ```js
  var PRICE = "$100";   // ← set your flat per-appointment fee here
  ```
  Add new price mentions with `<span class="price"></span>`.
- **Calendly URL:** `https://calendly.com/datathaw/30min` (used on every CTA button).
- **Contact email:** `SignalForge.io@gmail.com` (footer).
- **Colors / theme:** driven by CSS variables in `:root` — `--accent` (blue `#3b82f6`),
  `--accent-2` (cyan `#22d3ee`), and `--grad` (the blue→cyan gradient). Restyle through
  these, not per-element.
- **Scroll fade-ins:** add the class `reveal` to any new section to get the fade-in-on-scroll
  animation (auto-disabled for users who prefer reduced motion).

### Content rules (kept on purpose)
- **No fabricated social proof** — no fake testimonials, client logos, or invented stats.
  The "first 3 appointments free" offer is the proof. If you add a reviews section, fill it
  with **real** client quotes only.
- **No contact form** — there's no backend; the Calendly link is the only call to action.
- Keep it **single-file, framework-free, and responsive to 375px** with no horizontal scroll.

---

## Preview locally

Any static file server works. The included config uses Python:

```bash
# from this folder
python -m http.server 4599
# then open http://localhost:4599
```

(Opening `index.html` directly via `file://` also works, except the footer legal links.)

---

## Deploy to GitHub Pages

This file becomes the homepage of an existing GitHub Pages repo (the one that already holds
`privacy.html` and `terms.html`). GitHub Pages automatically serves a root `index.html` as
the site homepage.

**Option A — GitHub web upload (no terminal):**
1. Open the repo on github.com → **Add file → Upload files**.
2. Drag in `index.html`, add a commit message, **Commit changes**.
3. **Settings → Pages** → confirm Pages is enabled (branch usually `main`, folder `/root`).
4. Wait ~1 minute, then visit `https://<username>.github.io/`.

**Option B — Git command line:** from a local clone of that repo:
```bash
cp "index.html" .
git add index.html
git commit -m "Update landing page"
git push
```

Because `privacy.html` and `terms.html` sit beside `index.html` in that repo, the footer
links resolve correctly once live.
