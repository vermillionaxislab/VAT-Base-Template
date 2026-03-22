# Claude Code Instructions — VAT-Base-Template

## Who You're Working With
You are assisting **Faith**. When she gives you a task, execute it fully without asking for confirmation on routine operations.

## Automation Rules

### Always Do This Automatically
1. **Make the requested changes** to any file(s)
2. **Commit** with a clear, descriptive message
3. **Merge to `main`** and push — do not leave changes on a feature branch
4. **Confirm** by telling Faith what was done and what the live URL will reflect

### Git Workflow
- Work directly on `main` for all changes
- If on a feature branch, merge it into `main` after completing the task:
  ```
  git checkout main
  git merge <branch> --no-ff
  git push origin main
  ```
- Always push with: `git push -u origin main`

### GitHub Pages
- This repo deploys automatically via `.github/workflows/static.yml` on every push to `main`
- Live URL: `https://vermillionaxislab.github.io/VAT-Base-Template/`
- No manual steps needed after pushing to `main`

## What This Repo Is
A white-label coaching management PWA (Progressive Web App) base template built with vanilla HTML/CSS/JS.

- `index.html` — Full coaching app (client management, sessions, earnings, nutrition, schedules)
- `book.html` — Public booking page
- `sw.js` — Service worker (offline/PWA support)
- `manifest.json` — PWA manifest
- `icons/` — App icons for all devices

## Coding Standards
- Vanilla HTML/CSS/JS only — no build tools, no npm, no frameworks
- Keep all styles inline or in `<style>` blocks within the HTML files
- Color scheme: gold `#D4A830` on dark `#020202` background
- All paths must be relative (`./`) not absolute (`/`) — this is hosted on a GitHub Pages sub-path

## Do Not
- Ask Faith to create PRs manually
- Leave uncommitted changes
- Push to any branch other than `main` without merging back
- Add a CNAME file (no custom domain configured)
