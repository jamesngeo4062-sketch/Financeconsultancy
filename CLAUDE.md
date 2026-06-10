# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A single-file, static one-page investment strategy / financial advisory landing page. Everything (HTML, CSS, JavaScript) lives in `index.html`. There is no build step, package manager, framework, or backend — the file is served as-is.

## Running locally

The contact form submits via `fetch()` to FormSubmit's AJAX endpoint, which requires the page to be served over `http://`/`https://` (not opened directly as a `file://` URL, due to CORS). Serve it with any static server, e.g.:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

There is no test suite, linter, or build/deploy command — verify changes by reloading the page in a browser.

## Architecture

`index.html` is organized top-to-bottom as: `<head>` (meta/SEO/fonts) → embedded `<style>` → page sections in `<body>` → embedded `<script>` at the end.

### CSS
- All theme colors are CSS custom properties defined in `:root` (`--primary`, `--secondary`, `--accent`, `--background`, `--text`, `--light-bg`, plus derived tokens like `--border`, `--shadow`, `--radius`). Change the palette by editing these variables only.
- Mobile-first layout using Flexbox/Grid with breakpoints at ~640px, ~900px/1024px.
- `.reveal` elements are animated in via JS (see below); `prefers-reduced-motion` disables all transitions/animations and forces `.reveal` elements visible.

### Page sections (in document order)
1. Sticky `nav` (`#navbar`) — toggles `.scrolled` style on scroll, mobile menu via `#menuToggle`/`#navLinks`.
2. Hero (`#top`) — full-viewport intro with Unsplash background image and CTAs.
3. Why Choose Us (`#why-us`) — 6 benefit cards.
4. Investment Process (`#process`) — 4-step timeline (horizontal on desktop, vertical stacked on mobile).
5. Testimonials (`#testimonials`).
6. Lead Magnet (`#lead-magnet`) — free checklist offer, CTA scrolls to the form.
7. Enquiry Form (`#enquiry`) — `#enquiryForm`.
8. FAQ (`#faq`) — accordion.
9. Final CTA banner.
10. Footer — contact info, social links, disclaimer.

### JavaScript (single IIFE at bottom of `index.html`)
- Sticky navbar scroll handler, mobile menu toggle, smooth-scroll for all `a[href^="#"]` anchors (offsets by navbar height).
- Scroll-reveal via `IntersectionObserver` on `.reveal` elements.
- FAQ accordion (one open at a time, animated via `max-height`).
- Form validation (name/email/phone regex checks) and submission via `fetch()` to `https://formsubmit.co/ajax/<email>`, with inline success/error messaging in `#formMessage`.

### Form submission target
The recipient email is hardcoded in two places and must stay in sync:
- `<form action="https://formsubmit.co/...">` (HTML fallback)
- the `fetch('https://formsubmit.co/ajax/...')` URL in the script

Currently set to `mandyc@singnet.com.sg`.

### Images
Hero/testimonial images are loaded directly from `images.unsplash.com` via query-param-sized URLs (`?auto=format&fit=crop&w=...&q=...`) — no local image assets or build pipeline.
