# אסנת סוקר — אתר פסיכותרפיה

A custom static website for **Osnat Soker**, a psychotherapist based in Modi'in, Israel.
Converted from a Wix site to a self-hosted Astro site.

## Stack

- [Astro 6.4](https://astro.build) — static site generator (`output: 'static'`)
- [Tailwind CSS v4](https://tailwindcss.com) — via `@tailwindcss/vite`
- Hebrew RTL layout (`dir="rtl" lang="he"`)
- [Formspree](https://formspree.io) — contact form email delivery (form ID: `mnjkdgdq`)

## Requirements

Node.js **≥ 22.12.0** is required. If you're using nvm and get an error, run:

```sh
nvm install 22 && nvm use 22
```

## Commands

| Command           | Action                                      |
|:------------------|:--------------------------------------------|
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start dev server at `localhost:4321`        |
| `npm run build`   | Build static output to `./dist/`            |
| `npm run preview` | Preview production build locally            |

## Project Structure

```
src/
├── components/
│   ├── ContactForm.astro   # Formspree-powered contact form
│   ├── Footer.astro        # Footer with Facebook link + scroll-to-top
│   ├── Nav.astro           # Sticky two-tier nav + mobile hamburger
│   └── Section.astro       # Reusable section wrapper
├── layouts/
│   └── Layout.astro        # HTML shell, RTL, OG meta, scroll-reveal
├── pages/
│   └── index.astro         # Single-page site (all 6 sections)
└── styles/
    └── global.css          # CSS variables, RTL reset, animations
```

## Contact Form

The form POSTs to Formspree (`https://formspree.io/f/mnjkdgdq`).
To change the destination email, log in to [formspree.io](https://formspree.io) and update the form settings.
Free tier allows 50 submissions/month.

## Deployment

Hosted on **Cloudflare Pages**.

- Build command: `npm run build`
- Output directory: `dist`
- Node version: `22` (set in Cloudflare Pages environment variables: `NODE_VERSION=22`)

To deploy: push to GitHub, connect the repo in Cloudflare Pages dashboard.
