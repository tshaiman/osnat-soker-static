# CLAUDE.md — Project Context for Claude Code

## What This Project Is

A static website for **Osnat Soker**, a psychotherapist in Modi'in, Israel.
Converted from her Wix site to a custom Astro site. Single-page, Hebrew RTL, deployed on Cloudflare Pages.

## What Was Done (June 2026)

### Migration
- Original site was on Wix. A vibe-coding session built the Astro equivalent based on PLAN.md.
- All 6 sections implemented on a single `index.astro` page (hero, why-therapy, about, psychotherapy, contact, footer).
- The 3 sub-pages planned in PLAN.md (`about.astro`, `psychotherapy.astro`, `supervision.astro`) were **not built** — all content lives on the single page.

### Contact Form
- Uses **Formspree** (form ID: `mnjkdgdq`, destination: `tomer.shaiman@gmail.com`).
- Implemented as a client-side `fetch()` POST — no server-side backend.
- Form was verified working: submits successfully, shows "תודה! ההודעה נשלחה".
- Free tier: 50 submissions/month. Upgrade path: Resend (3,000/month free).

### Deployment Decision
- **Cloudflare Pages** chosen (free, unlimited bandwidth, no overage bills).
- Build: `npm run build`, output: `dist/`, Node version: `22`.
- Repo needs to be pushed to GitHub first, then connected in Cloudflare Pages dashboard.

### Known Issues / Pending
- Node ≥ 22.12.0 required — default nvm shell may have v20. Run `nvm use 22` before `npm run dev`.
- GitHub repo not yet created (as of June 2026) — needed before Cloudflare Pages deploy.
- No sub-pages — if Osnat wants dedicated pages per service, they need to be built.

## Architecture Notes

- Pure static site — no SSR, no API routes, no backend.
- Tailwind v4 via Vite plugin (no `tailwind.config.mjs` needed).
- All responsiveness is custom CSS media queries (not Tailwind utility classes).
- Scroll-reveal animations via IntersectionObserver in `Layout.astro`.
- RTL enforced via `dir="rtl" lang="he"` on `<html>`.

## Sensitive Values

- Formspree form ID: `mnjkdgdq` (safe to commit, no secret)
- Destination email configured in Formspree dashboard, not in code
