---
description: Security-scan, commit, push to GitHub, ensure GitHub Pages deploy workflow exists, and keep README/repo metadata in sync.
---

Run through these steps in order. Stop and ask the user before any destructive or
irreversible action (force-push, history rewrite, deleting files you didn't create).

## 1. Security scan (do this FIRST, before staging/committing anything)
- Run `git status` and `git diff` to see what's changed.
- Scan changed/new files for secrets or credentials: API keys, tokens, passwords,
  private keys, `.env` files, AWS/GCP credentials, FormSubmit tokens, etc.
  Use `grep -riE "api[_-]?key|secret|password|token|private[_-]?key|BEGIN (RSA|OPENSSH|PRIVATE) KEY"`
  over staged/changed files as a first pass.
- Confirm no `.env`, credential files, or `.DS_Store`/local-only files are about to
  be committed. Check `.gitignore` covers common secret file patterns
  (`.env`, `*.pem`, `*.key`, credential JSON files).
- If anything sensitive is found, STOP and tell the user — do not commit or push
  until it's removed/ignored.

## 2. Commit and push to GitHub
- Stage the relevant changed files (avoid `git add -A`/`git add .`; add by name).
- Write a concise commit message describing the change (why, not just what).
- Commit and push to `origin main` (or current branch). Do not force-push.

## 3. GitHub Pages via GitHub Actions
- Check if a Pages deploy workflow already exists at `.github/workflows/`.
- If not, create a minimal workflow (`.github/workflows/deploy-pages.yml`) that
  deploys the static site (this repo is a single `index.html`, no build step)
  using `actions/upload-pages-artifact` + `actions/deploy-pages`, triggered on
  push to `main`.
- Note for the user: the repo's Pages source setting must be switched to
  "GitHub Actions" in repo Settings → Pages (one-time, cannot be done via git push —
  use `gh api` if the user wants this automated, but confirm first since it
  changes repo settings).

## 4. README
- Check if `README.md` exists and is up to date with the current site
  (sections, live URL, run instructions). Create it if missing, or update it
  if it's stale relative to `index.html`/`CLAUDE.md`. Don't duplicate content
  that belongs in `CLAUDE.md`.

## 5. Repo "About" metadata
- Use `gh repo edit` to set/update the repository description and homepage URL
  (the GitHub Pages live URL) so the repo's "About" section matches the project.
  Confirm the description text with the user if it's a meaningful change.

## 6. Final check
- Re-run a quick `git status` / `git log -1` to confirm everything pushed cleanly.
- Summarize what changed for the user.
