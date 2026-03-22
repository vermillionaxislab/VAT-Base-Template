# Claude Code Instructions — VAT-Base-Template

## Who You're Working With
You are assisting **Faith**. When she gives you a task, execute it fully without asking for confirmation on routine operations. David built this system — Faith operates it. Treat her instructions as authoritative for all app-level changes.

## First Session Protocol
If Faith seems unfamiliar or asks "how does this work" — read her `WELCOME_FAITH.md` in a warm, conversational tone before proceeding. If she jumps straight into a task, skip the intro and get to work.

---

## Trigger Word Automation Protocol

These are pre-programmed trigger phrases. When Faith uses them, execute the mapped action immediately — no confirmation needed.

### Deploy Triggers
| Trigger | Action |
|---------|--------|
| `"commit and push"` | Stage all changes → commit with descriptive message → push to feature branch |
| `"save everything"` | Same as above |
| `"ship it"` | Commit → push → confirm GitHub Actions deploy is queued |
| `"go live"` | Commit → push → report the live URL |
| `"deploy"` | Full commit + push sequence |

### Build Triggers
| Trigger | Action |
|---------|--------|
| `"go ahead"` | Execute the approved plan immediately |
| `"build it"` | Start building what was just planned |
| `"make it happen"` | Execute all pending tasks in sequence |
| `"run it"` | Execute, then run the audit checklist |

### Audit & Debug Triggers
| Trigger | Action |
|---------|--------|
| `"audit"` | Run the full AUDIT.md protocol — report all findings |
| `"check everything"` | Full validation sweep across all specs |
| `"run tests"` | Execute complete testing checklist from AUDIT.md |
| `"debug"` | Find and fix all errors in the current working file |
| `"full debug"` | Multi-angle diagnostic across ALL files — see AUDIT.md |
| `"what's broken"` | Diagnose current state and report all issues found |

### Mode & Status Triggers
| Trigger | Action |
|---------|--------|
| `"/plan"` or `"plan mode"` | Switch to Plan Mode — read only, no changes |
| `"/code"` or `"code mode"` | Switch to Code Mode — execute changes |
| `"status"` | Report what's been done, what's pending, last commit |
| `"what's done"` | Plain-English summary of recent changes |

### Revert Triggers
| Trigger | Action |
|---------|--------|
| `"undo that"` | `git revert HEAD` — roll back last change |
| `"go back"` | Same as above |
| `"start over"` | Reset to last clean commit — confirm with Faith first |

---

## Git Workflow (Security Policy — Do Not Deviate)

David has set specific git policy. **Branch protection on `main` is intentional and must not be removed.**

### Standard Flow
1. All changes go on branch: `claude/setup-base-template-repo-PzW4t` (or a new `claude/` branch per session)
2. Commit with a clear, descriptive message
3. Push to `origin/<branch-name>`
4. GitHub Pages auto-deploys when the PR is merged to `main` (David's security layer)

### Push Command
```
git push -u origin claude/setup-base-template-repo-PzW4t
```

### Never Do This
- Never push directly to `main`
- Never force push (`--force`)
- Never skip hooks (`--no-verify`)
- Never alter `.github/workflows/static.yml` without David's instruction
- Never add a CNAME file (no custom domain configured for this repo)

### GitHub Pages
- Auto-deploys from `main` via `.github/workflows/static.yml`
- Live URL: `https://vermillionaxislab.github.io/VAT-Base-Template/`
- Deploy takes ~2 minutes after merge to `main`

---

## What This Repo Is

A **white-label coaching management PWA** base template. Built as a single-file vanilla JS SPA. Originated from the Big Mike Ely Coaching Lab app, stripped to a clean template for multi-client deployment.

### File Map
| File | Purpose |
|------|---------|
| `index.html` | Entire app (~3,800 lines) — single-file SPA |
| `book.html` | External public booking portal |
| `sw.js` | Service worker — PWA offline support |
| `manifest.json` | PWA manifest — app name, icons, theme |
| `icons/` | App icons (120, 152, 192, 512px + apple-touch) |
| `apple-touch-icon.png` | iOS home screen icon |
| `404.html` | Redirect fallback to index |
| `.nojekyll` | Bypasses GitHub Jekyll processing |
| `WELCOME_FAITH.md` | Faith's onboarding guide |
| `AUDIT.md` | Full system audit and debug protocol |

---

## Architecture
- Single-file vanilla JS SPA: `index.html`
- No framework — pure functions returning HTML strings, swapped via `innerHTML`
- `localStorage` persistence with optional Supabase cloud sync
- Navigation: `go(tab)`, `push(view, data)`, `pop()` with `navStack`
- PWA: `manifest.json` + service worker + canvas-generated theme icons

## Key Data Structures
- `clients` — client objects in `localStorage` as `fm_clients`
- `sessions` — session logs with exercises, sets, rates
- `schedule` — scheduled sessions with reminders
- `mealPlans` — meal plans referencing `FOOD_DB` by index
- `_programs` — program builder data
- `_workouts` — workout templates

## Themes
- Default: Crimson (`LS.get("theme","crimson")`)
- CSS variables switch via `body.theme-crimson` class
- Two built-in themes: Crimson and Gold
- Dynamic canvas icons generated per theme for PWA home screen

## Coding Standards
- Vanilla HTML/CSS/JS only — no build tools, no npm, no frameworks
- All styles inline or in `<style>` blocks within the HTML files
- **All paths must be relative** (`./`) not absolute (`/`) — hosted on GitHub Pages sub-path
- Color scheme: gold `#D4A830` on dark `#020202` — use CSS variables, never hardcode colors
- Never add `max-height` + `overflow-y:auto` inside modals — breaks iOS scroll
- All compound names: `"Pharmaceutical Name / Brand Name"` format

## Customization Points Per Client Deployment
1. App name (title, manifest, header)
2. Logo letter/icon
3. Subtitle text
4. Theme colors
5. PDF cover page branding
6. SMS signature
7. Tutorial text
8. Domain/CNAME (only if client has custom domain — add CNAME file then)
9. Booking page branding (`book.html`)

## Known Patterns & Gotchas
1. **Render priority**: `renderNutrition()` → checks `_editProgram` → `_editWorkout` → `_editMealPlan` in order
2. **Modal scroll**: `showModal` handles all scrolling — do NOT nest `overflow-y:auto` inside modals
3. **Theme colors**: Never hardcode — always use `var(--bg)`, `var(--acc)`, etc.
4. **Cloud sync**: `save()` persists all stores. `savePrograms()` and `saveWorkouts()` are local-only
5. **iOS keyboard**: `focusout` listener resets scroll position after keyboard dismiss
6. **PDF builder**: reads checkboxes into local vars BEFORE calling `closeModal()`

## After Every Change — Minimum Validation
```bash
sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d' > /tmp/app_js.js && node -c /tmp/app_js.js
```
See `AUDIT.md` for the full multi-angle audit protocol. Run it any time Faith says "audit", "check everything", or "full debug".
