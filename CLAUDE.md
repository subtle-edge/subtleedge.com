# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Marketing and legal site for Subtle Edge LLC and its product Paper Tiger (a conversational assessment platform). Hosted on GitHub Pages at www.subtleedge.com.

## Development

```bash
bundle install
bundle exec jekyll serve     # Local server at http://localhost:4000
bundle exec jekyll build     # Build to _site/
```

Changes to `_config.yml` require restarting the server.

## Deployment

Push to `main` triggers the GitHub Actions workflow (`.github/workflows/jekyll.yml`) which builds with Jekyll and deploys to GitHub Pages. There is no staging environment.

## Architecture

Jekyll site using the Minima theme with `github-pages` gem. Most pages bypass the theme entirely with `layout: null` and contain self-contained HTML with inline styles.

**Two design systems coexist:**

- **Homepage** (`index.markdown` + `_layouts/default.html`): Uses `assets/css/style.css` with CSS custom properties (`--primary-color`, etc.), background image, email obfuscation via JS, and the Minima theme layout chain.
- **Subpages** (`about.html`, `paper-tiger.html`, `privacy.html`, `sms-consent.html`): Standalone HTML files with `layout: null`. Each has its own inline `<style>` block. Paper Tiger pages use Sora/Manrope from Google Fonts and a distinct teal/brown palette.

**Email obfuscation**: The homepage assembles the email address from `.email-part` spans via JavaScript to deter scrapers. Subpages use plain `mailto:` links.

**Paper Tiger SVG logo**: Duplicated inline across `paper-tiger.html`, `privacy.html`, and `sms-consent.html` (each with a unique clipPath ID to avoid SVG conflicts).

## Pages and routes

| File | Route | Purpose |
|------|-------|---------|
| `index.markdown` | `/` | Company landing page |
| `about.html` | `/about` | Company info and Paper Tiger summary |
| `paper-tiger.html` | `/paper-tiger` | Product page |
| `privacy.html` | `/privacy` | Privacy policy |
| `sms-consent.html` | `/sms-consent` | SMS consent policy (Twilio/carrier compliance) |
| `404.html` | `/404.html` | Error page |

## Git Commit Standards

Follow Tim Pope's commit message guide (https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html):

- Subject line: Capitalized, imperative mood, 50 chars or less
- Blank line after subject
- Body: Wrap at 72 chars, explain what and why

**No "and" in commit subjects.** If the subject needs "and", it is two commits.

Attribute commits to the human developer. No Co-Authored-By tags, no tool names in commit messages.

Omit implementation details from commit bodies: no file paths, no variable names, no counts. The reader has `git show` for the code; the body explains what changed and why.

Stage files and create commits only when explicitly requested. Push to remote only when explicitly approved.

## Things to watch

- The `_includes/` directory contains Minimal Mistakes theme partials carried over from a previous theme. Most are unused. The active include chain is `default.html` layout only.
- `erl_crash.dump` and `elixir.list` are artifacts in the repo root that should not be modified.
- Inline styles on subpages mean style changes must be made per-file. There is no shared stylesheet for Paper Tiger pages.
