# Finance Consultancy

A modern, single-page landing site for an investment strategy / financial advisory consultancy.

**Live site:** https://jamesngeo4062-sketch.github.io/Financeconsultancy/

![Site screenshot](images/screenshot.png)

## Overview

The entire site is a single self-contained `index.html` file (HTML, CSS, and vanilla JavaScript — no frameworks or build step). It includes:

- Hero section with calls-to-action
- "Why Choose Us" benefit highlights
- Investment process timeline
- Client testimonials
- Free strategy checklist lead magnet
- Enquiry/consultation form (powered by [FormSubmit](https://formsubmit.co/))
- FAQ accordion
- Footer with disclaimer

## Running locally

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`. The form's AJAX submission requires the page to be served over HTTP/HTTPS (not opened directly as a `file://` path).

See [CLAUDE.md](CLAUDE.md) for architecture details.

## Deployment

A GitHub Actions workflow ([`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml))
deploys the site to GitHub Pages on every push to `main`. In the repo's
**Settings → Pages**, the source must be set to **GitHub Actions**.
