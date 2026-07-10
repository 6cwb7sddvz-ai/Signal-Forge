# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A marketing landing page for **Signal Forge**, a done-for-you lead-reactivation service (reactivates a local service business's old leads/past customers into booked appointments; pay-per-result — a flat fee per appointment that shows, first 3 free). The page's only conversion goal is booking a 15-minute Calendly call.

The entire site is **one self-contained file: `index.html`** — all CSS in a single `<style>` block, all JS in a single `<script>` block. No frameworks, no build step, no dependencies, no bundler. Edit the file directly; there is nothing to compile.

## Deploy / build / test

- **Build:** none. `index.html` is the artifact.
- **Preview locally:** a static-server config lives in `.claude/launch.json` (Python `http.server` on port 4599). Any static server works; the file also opens directly via `file://`.
- **Deploy:** this file is committed to an existing GitHub Pages repo (root, alongside `privacy.html` and `terms.html`) where it serves as the homepage. The GitHub repo is NOT cloned here — this working directory only holds `index.html`. `privacy.html` and `terms.html` live only in that repo, so the footer's `./privacy.html` / `./terms.html` links resolve on the live site, not locally.

## Editing conventions specific to this file

- **Per-appointment price is single-sourced.** Do NOT hardcode the dollar amount in copy. One JS constant near the bottom (`var PRICE = "$100";`) populates every `<span class="price">` on the page. Change the price there; add new price mentions with `<span class="price"></span>`.
- **Calendly URL** (`https://calendly.com/signalforge-io/30min`) and **contact email** (`SignalForge.io@gmail.com`) are repeated across CTAs/footer — update all occurrences together.
- **Theming** is driven by CSS custom properties in `:root` (`--accent` blue `#3b82f6`, `--accent-2` cyan `#22d3ee`, `--grad` for the blue→cyan gradient used on headline highlights, buttons, step badges, stats, ticks). Restyle via these variables, not per-element colors.
- **Scroll fade-ins:** any element with class `reveal` animates in via IntersectionObserver; it is disabled under `prefers-reduced-motion`. Add `reveal` to new sections for consistency.
- There is dormant CSS for a removed reviews section (`.reviews`, `.rev`, `.stars`, etc.) kept for easy re-add; the markup was deleted.

## Hard content constraints (do not violate)

- **No fabricated social proof.** No invented testimonials, fake client logos, or made-up first-party statistics presented as real results. The "first 3 appointments free" risk-reversal stands in for social proof. If a reviews/testimonials section is (re)added, ship it with clearly-marked placeholders for the user to replace with real quotes — never invent them.
- **No contact form** — the Calendly link is the only CTA (there is no backend to receive form submissions).
- Keep it single-file, framework-free, and responsive down to 375px with no horizontal scroll.
