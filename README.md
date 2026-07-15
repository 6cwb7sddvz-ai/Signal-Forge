# Data Thaw — Landing Page

Marketing landing page for **Data Thaw** (formerly *Signal Forge*), a done-for-you
lead-reactivation service: it turns a local service business's old leads and past customers into
booked appointments through text/email follow-up campaigns. Pricing is pay-per-result — a flat
fee per appointment that shows, based on the value of the work, with the first 3 free. The page's
goals are to get a visitor to **submit the intake form** (primary) or **book a 15-minute Calendly
call** (secondary).

The landing page is **one self-contained file: [`index.html`](index.html)** — all CSS and
JavaScript are inline, no frameworks, no build step. To change the page, edit that one file.

> The repo/folder are still named `Signal-Forge` / `Signal Forge Web` — only the *brand* was
> renamed. Don't reintroduce "Signal Forge" into page copy.

---

## Project layout

| File | Purpose |
|------|---------|
| `index.html` | The whole landing page (HTML + inline CSS + inline JS). |
| `logo.svg` | Data Thaw droplet logo (cold→warm); used as the header/footer mark and favicon. |
| `privacy.html`, `terms.html` | Legal pages (serif theme; SMS/CAN-SPAM compliance copy). |
| `README.md` | This file — human-facing overview. |
| `CLAUDE.md` | Detailed guidance for Claude Code / AI assistants. |
| `CNAME` | Custom domain (`datathaw.com`). |
| `.claude/launch.json` | Local preview-server config (Python static server, port 4599). |

---

## Editing the page

A few things are intentionally single-sourced — change them in one place near the bottom of
`index.html`:

- **Per-appointment fee wording:** deliberately **not a dollar amount**. One constant fills every
  fee mention:
  ```js
  var PRICE = "fee based on your product's value";   // reads "a flat ___ for each appointment"
  ```
  Keep it a value/percentage phrase, not a number. Add new mentions with `<span class="price"></span>`.
- **Intake form target:** `var INTAKE_WEBHOOK = "…"` — the n8n webhook the form POSTs to.
- **Calendly URL:** `https://calendly.com/datathaw/30min` (secondary CTA).
- **Contact email:** `support@datathaw.com` (footer + legal pages).
- **Colors / theme:** CSS variables in `:root` — cool `--accent`/`--accent-2` (blue→cyan `--grad`)
  plus a warm `--accent-3`/`--accent-3b` (coral→amber `--grad-warm`). Restyle through these.
- **Scroll fade-ins:** add class `reveal` to any new section (auto-disabled for reduced-motion).

### Intake form + backend
The homepage's About/hero includes an **intake form** (business name, contact, phone, email,
website, which CRM, list size, notes + a spam honeypot). It POSTs to an **n8n webhook**, which
appends the submission to the "Website Intake" tab of a Google Sheet. There's no server in this
repo — the backend is n8n. **Never collect CRM passwords or API keys in the form** — only which CRM
the business uses.

### Content rules (kept on purpose)
- **No fabricated social proof** — no fake testimonials, client logos, or invented stats. The
  "first 3 appointments free" offer is the proof.
- Keep it **single-file, framework-free, and responsive to 375px** with no horizontal scroll.

---

## Preview locally

Any static file server works. The included config uses Python:

```bash
# from this folder
python -m http.server 4599
# then open http://localhost:4599
```

---

## Deploy

Hosted on **GitHub Pages** (repo `6cwb7sddvz-ai/Signal-Forge`, branch `main`), live at
**https://datathaw.com** (custom domain, HTTPS enforced). To update:

```bash
git add index.html            # + any other changed files
git commit -m "Update landing page"
git push origin main          # Pages rebuilds in ~30–60s
```
