# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The marketing website for **Data Thaw** — a done-for-you lead-reactivation service that turns a
local service business's old leads / past customers into booked appointments. Pricing is
pay-per-result: a flat fee per appointment that shows, with the first 3 free. The site's only
conversion goal is booking a 15-minute Calendly call.

> **Rebrand note:** the business was renamed from **"Signal Forge" → "Data Thaw"** on 2026-07-10.
> The repo and local folder are still named `Signal-Forge` / `Signal Forge Web`, and the GitHub
> repo is `6cwb7sddvz-ai/Signal-Forge` — only the *brand* changed, not the repo. Do not
> reintroduce the name "Signal Forge" into user-facing page copy.

## Repository layout

| File | Purpose |
|------|---------|
| `index.html` | The entire landing page — HTML + inline `<style>` + inline `<script>`. The main artifact. |
| `logo.svg` | The Data Thaw droplet logo (cold→warm). Used as the header/footer `.mark` and favicon. |
| `privacy.html` | Privacy Policy (standalone styled page, serif theme). |
| `terms.html` | Terms & Conditions (same theme as privacy). |
| `README.md` | Human-facing overview. |
| `CLAUDE.md` | This file. |
| `.gitignore`, `.claude/launch.json` | Housekeeping + local preview-server config. |

## The landing page (`index.html`)

**One self-contained file** — all CSS in a single `<style>` block, all JS in a single `<script>`
block. No frameworks, no build step, no dependencies, no bundler. Edit the file directly.

Section order: sticky header → hero (**two-column: About-us copy + intake form**, `#start`) →
"The problem" (3 stat cards) → image band quote → "How it works" (3 steps) → "Why risk-free"
(+ proof card) → FAQ (native `<details>`) → final CTA (form + Calendly) → footer. A reviews section
was built then **removed** at the user's request; its CSS (`.reviews`, `.rev`, `.stars`, …) is left
dormant for easy re-add.

**Hero intake form** (`#intakeForm`): captures business name, contact name, phone, email, website,
CRM (dropdown), list size, optional notes — plus a hidden honeypot (`company_url_hp`) for spam. It
posts a `FormData` (CORS-simple request, no custom headers) to the n8n webhook in the
`INTAKE_WEBHOOK` constant; the handler shows inline success/error and falls back to email/Calendly
if the webhook is unset. Calendly is now the *secondary* CTA. It deliberately **never** collects CRM
passwords or API keys — only which CRM they use (secure CRM pull happens later during onboarding).

### Design system
- **Theme:** near-black bg `#0a0c10`, off-white text `#e7e9ee`, muted grey `#98a2b3`.
- **Accent:** blue `#3b82f6` + cyan `#22d3ee`, combined in `--grad` (blue→cyan gradient) used on
  headline highlight, buttons, pill dot, step badges, stat numbers, ticks, FAQ plus-icons.
  A **warm accent** (`--accent-3` coral `#fb7185` + `--accent-3b` amber `#f59e0b`, `--grad-warm`)
  adds color on: the "booked revenue" headline span, form focus rings + required asterisks, the
  "Get started" eyebrow, step-2 badge, proof card, and section glows. Restyle via the `:root` CSS
  variables, not per-element colors.
- **Fonts:** Sora (display) + Inter (body) from Google Fonts, each with a system fallback stack.
- **Motion:** any element with class `reveal` fades in via IntersectionObserver; disabled under
  `prefers-reduced-motion`. Add `reveal` to new sections for consistency.
- **Responsive:** fluid `clamp()` type; collapses to single column ≤880px; verified to 375px with
  no horizontal scroll.

### Single-sourced values (change in ONE place)
- **Per-appointment fee wording:** the copy is deliberately **value-based, not a dollar amount**
  (the user does not want a number shown). The constant `var PRICE = "fee based on your product's
  value";` near the bottom of `index.html` fills every `<span class="price">` and sits right after
  the word "flat" (e.g. "a flat ___ for each appointment"). Keep it a phrase, not a number; add new
  mentions as `<span class="price">…</span>`.
- **Calendly URL** (`https://calendly.com/datathaw/30min`) and **contact email**
  (`support@datathaw.com`) repeat across CTAs / footer — update all occurrences together. (The
  Calendly account was renamed `signalforge-io` → `datathaw` on 2026-07-15; the old link now 404s.)
- **Intake webhook:** the `var INTAKE_WEBHOOK = "…"` constant (next to `PRICE`) is the single place
  the form's POST target lives. It points at the live n8n **"Data Thaw — Website Intake"** workflow
  (webhook → honeypot filter → append to the **"Website Intake"** tab of the leads sheet, via the
  same Google Sheets OAuth cred the Plaid workflow uses). This is a **public endpoint** by nature
  (it's in client JS) — that's expected for any web form, same as a Formspree/Google-Form action.

## Legal pages (`privacy.html`, `terms.html`)

Standalone pages, serif theme (Georgia), blue `#2f5d8a` accent — intentionally different look from
the dark landing page. They carry **SMS/CAN-SPAM compliance language** (STOP/HELP opt-out, "no
mobile info shared with third parties," message-frequency disclosure) because the service sends
SMS/email follow-ups. Business name = **Data Thaw**; contact = `support@datathaw.com`, phone
803-820-0608, Charleston SC; governing law South Carolina. Footer links are alphabetical
(Privacy Policy → Terms & Conditions). If editing compliance copy, keep the STOP/HELP + opt-out
mechanics intact.

## Deploy / build / test

- **Build:** none. The `.html` files are the artifacts.
- **Preview locally:** `.claude/launch.json` runs a Python `http.server` on port 4599. Any static
  server works; files also open via `file://` (except the footer legal links, which are relative).
- **Host:** GitHub Pages, repo **`6cwb7sddvz-ai/Signal-Forge`**, branch `main`, `/root`. This local
  folder is a git clone of that repo (git identity is repo-local: Data Thaw / support@datathaw.com).
- **Live URL:** **`https://datathaw.com`** (custom domain, HTTPS enforced). The Pages default
  `https://6cwb7sddvz-ai.github.io/Signal-Forge/` now 301-redirects to it.
- **To update:** edit → `git add` the changed files → commit → `git push origin main`. Pages
  rebuilds in ~30–60s. ⚠️ **datathaw.com is firewall-blocked from the Claude environment** — verify
  deploys via `gh api repos/6cwb7sddvz-ai/Signal-Forge/pages/builds/latest` + a
  `raw.githubusercontent.com/6cwb7sddvz-ai/Signal-Forge/main/<file>` fetch, NOT by fetching the
  live domain. (See [[project-datathaw-deploy-verify]] in memory.)

### Custom domain — `datathaw.com` (LIVE as of 2026-07-15)
The site is served at **datathaw.com** via GoDaddy-hosted DNS (four apex `A` records →
`185.199.108–111.153`, `www` CNAME → `6cwb7sddvz-ai.github.io`). A `CNAME` file containing
`datathaw.com` is committed at the repo root and **Enforce HTTPS** is enabled (cert approved); the
github.io URL 301-redirects here. If the domain ever changes, update the `CNAME` file and the
GoDaddy DNS together.

## Hard content constraints (do not violate)
- **No fabricated social proof** — no invented testimonials, fake client logos, or made-up
  first-party stats. The "first 3 appointments free" risk-reversal stands in for social proof. Any
  (re)added reviews section ships with clearly-marked placeholders, never invented quotes.
- ~~**No contact form**~~ — *superseded 2026-07-12.* The homepage now has an **intake form** that
  posts to an n8n webhook backend (see Single-sourced values). Still no server in the *repo* — the
  backend is n8n. Never collect CRM passwords/API keys in the form.
- Keep it single-file, framework-free, responsive to 375px, no horizontal scroll.

## Related automation (lives outside this repo)
Data Thaw also has a **client-acquisition cold-email engine** built in the user's **n8n Cloud** +
**Instantly**, tracked in a Google Sheet — two workflows deployed **inactive**, gated on
sending-domain warmup. That work is not part of this website repo; its infrastructure identifiers
(instance URL, workflow/sheet IDs, credentials) live in **private session memory**, not in this
public file. This CLAUDE.md governs the website only.

## Known follow-ups
- Intake form's **optional file upload is not wired** yet — the form field exists, but the n8n
  webhook is text-only (storing the file needs a Google Drive credential connected in n8n).
