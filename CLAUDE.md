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
| `privacy.html` | Privacy Policy (standalone styled page, serif theme). |
| `terms.html` | Terms & Conditions (same theme as privacy). |
| `README.md` | Human-facing overview. *(Still says "Signal Forge" — not yet rebranded.)* |
| `CLAUDE.md` | This file. |
| `.gitignore`, `.claude/launch.json` | Housekeeping + local preview-server config. |

## The landing page (`index.html`)

**One self-contained file** — all CSS in a single `<style>` block, all JS in a single `<script>`
block. No frameworks, no build step, no dependencies, no bundler. Edit the file directly.

Section order: sticky header → hero (dark Unsplash bg + overlay) → "The problem" (3 stat cards) →
image band quote → "How it works" (3 steps) → "Why risk-free" (+ proof card) → FAQ (native
`<details>`) → final CTA → footer. A reviews section was built then **removed** at the user's
request; its CSS (`.reviews`, `.rev`, `.stars`, …) is left dormant for easy re-add.

### Design system
- **Theme:** near-black bg `#0a0c10`, off-white text `#e7e9ee`, muted grey `#98a2b3`.
- **Accent:** blue `#3b82f6` + cyan `#22d3ee`, combined in `--grad` (blue→cyan gradient) used on
  headline highlight, buttons, pill dot, step badges, stat numbers, ticks, FAQ plus-icons.
  Restyle via the `:root` CSS variables, not per-element colors.
- **Fonts:** Sora (display) + Inter (body) from Google Fonts, each with a system fallback stack.
- **Motion:** any element with class `reveal` fades in via IntersectionObserver; disabled under
  `prefers-reduced-motion`. Add `reveal` to new sections for consistency.
- **Responsive:** fluid `clamp()` type; collapses to single column ≤880px; verified to 375px with
  no horizontal scroll.

### Single-sourced values (change in ONE place)
- **Per-appointment price:** do NOT hardcode the dollar amount in copy. The constant
  `var PRICE = "$100";` near the bottom of `index.html` fills every `<span class="price">`. Add new
  price mentions as empty `<span class="price"></span>`.
- **Calendly URL** (`https://calendly.com/signalforge-io/30min`) and **contact email**
  (`support@datathaw.com`) repeat across CTAs / footer — update all occurrences together.
  ⚠️ The Calendly handle is still `signalforge-io` (the actual account); changing that URL text
  breaks booking unless the Calendly account itself is renamed first.

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
- **Live URL (current):** `https://6cwb7sddvz-ai.github.io/Signal-Forge/`
- **To update:** edit → `git add` the changed files → commit → `git push origin main`. Pages
  rebuilds in ~30–60s. Verify with a cache-busted fetch of the live URL (Pages/CDN caches briefly).

### Custom domain — `datathaw.com` (IN PROGRESS, not cut over)
The site is moving to **datathaw.com** (GoDaddy-hosted DNS). As of 2026-07-10 the domain still
points at a GoDaddy parking page, so the switch is **not done**. Sequence (do NOT skip the order):
1. User adds DNS at GoDaddy: four apex `A` records → `185.199.108.153`/`.109.153`/`.110.153`/
   `.111.153`, plus `www` CNAME → `6cwb7sddvz-ai.github.io`, and disables parking/forwarding.
2. Only AFTER DNS resolves to those GitHub IPs: add a `CNAME` file containing `datathaw.com` to the
   repo root and push, then enable **Enforce HTTPS** in repo Settings → Pages.
Pushing the CNAME file before DNS is ready would take the live site offline — that's why it hasn't
been committed yet.

## Hard content constraints (do not violate)
- **No fabricated social proof** — no invented testimonials, fake client logos, or made-up
  first-party stats. The "first 3 appointments free" risk-reversal stands in for social proof. Any
  (re)added reviews section ships with clearly-marked placeholders, never invented quotes.
- **No contact form** — the Calendly link is the only CTA (there's no backend).
- Keep it single-file, framework-free, responsive to 375px, no horizontal scroll.

## Related automation (lives outside this repo)
Data Thaw also has a **client-acquisition cold-email engine** built in the user's **n8n Cloud** +
**Instantly**, tracked in a Google Sheet — two workflows deployed **inactive**, gated on
sending-domain warmup. That work is not part of this website repo; its infrastructure identifiers
(instance URL, workflow/sheet IDs, credentials) live in **private session memory**, not in this
public file. This CLAUDE.md governs the website only.

## Known follow-ups
- `README.md` still says "Signal Forge" — rebrand when convenient.
- Calendly account/link still `signalforge-io` — rename in Calendly, then update the 4 CTA URLs.
- Custom-domain CNAME cutover pending user's GoDaddy DNS change (see above).
