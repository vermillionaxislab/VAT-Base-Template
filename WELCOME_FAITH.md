# Welcome to Coaching Lab — Your AI-Powered Development System

Hey Faith! Here's how the coaching app system works — David built this so you don't need to know any code.

---

## The Big Picture

David has set up an AI-powered development system where you literally just talk. You open the app (Claude Code), tap your microphone, and speak naturally — as long-winded as you want. The AI (me, Claude) listens to everything you say, understands what you want, and builds it. No coding. No technical knowledge. David engineered this entire system specifically so that you can operate it by just having a conversation.

---

## How to Access Your Portal (READ THIS FIRST)

**You access everything through the internet — not through a local app on your computer.**

### Step 1 — Open the Live Site
Your portal is live at:
**https://vermillionaxislab.github.io/VAT-Base-Template/**

Open that URL in any browser. That is the real, internet-hosted version of the app. Bookmark it.

### Step 2 — Sign In
Use the account credentials David provided you. The app uses a PIN-based unlock system — enter your PIN on the lock screen to get in.

### Step 3 — You're In
Everything you need is inside the portal. You do NOT run anything locally. You do NOT open files on your computer. You just go to that URL like any other website.

> **Important:** If someone tells you to "run the app" or "open index.html" on your computer — that is NOT how this works. Always use the live URL above.

---

## How a Typical Workflow Goes

**Example: A new client buys the software and wants customizations**

### Step 1 — Plan Mode (talk it out first)

When you first start, you'll be in what's called "Plan Mode." This is your brainstorming/planning phase. You just talk and tell me what the client wants:

*"Hey, so this client is a gym called Iron Paradise, they want their logo colors to be black and gold, they want to track 50 trainers, each trainer needs to be able to log their clients' workouts and meal plans..."*

I'll take everything you say, organize it into a clear step-by-step plan, and show it back to you. You review it. If something's off, just say *"no, change this part to..."* and I'll adjust. Once you're happy with the plan, you approve it.

**The key thing about Plan Mode:** I cannot make any changes to the actual app. I can only research, read files, and build the plan. This is a safety net David built in — nothing gets touched until you're confident the plan is right.

### Step 2 — Code Mode (I build it)

Once you approve the plan, you switch to Code Mode (just say "go ahead" or "build it" or type `/code`). Now I execute everything from the plan. I make all the changes, test them, and verify everything works. You don't need to understand what's happening technically — David built the system so that I handle all of that.

---

## Security & Repository Policy (David's Rules — Do Not Change)

David has set specific security policies for this repository. These are non-negotiable:

1. **Branch protection is ON** — The `main` branch is protected. This means changes cannot be pushed directly to main without going through a review process. This is intentional security. Do not ask to remove it.
2. **All changes go through a feature branch** — When I make changes, they go to a `claude/` branch first, then get merged to `main` through GitHub's process.
3. **GitHub Pages auto-deploys from `main`** — Once changes reach `main`, the live site updates automatically within ~2 minutes.
4. **You do not touch GitHub settings** — Repository settings, branch rules, Actions workflows, and Pages configuration are David's domain. If something isn't working on the repo level, message David.

---

## Trigger Words — Your Automation Protocol

David has programmed specific trigger phrases that tell me to take immediate automated action. Use these exactly:

### Save & Deploy Triggers
| Say This | What Happens |
|----------|-------------|
| `"commit and push"` | I save all changes and upload them to GitHub |
| `"save everything"` | Same as above — full save and upload |
| `"ship it"` | Commit, push, and confirm deploy is triggered |
| `"go live"` | Commit, push to feature branch, ready for merge to main |
| `"deploy"` | Full commit + push sequence |

### Build Triggers
| Say This | What Happens |
|----------|-------------|
| `"go ahead"` | I execute the approved plan immediately |
| `"build it"` | Start building what we just planned |
| `"make it happen"` | Execute all pending tasks |
| `"run it"` | Execute and test |

### Review Triggers
| Say This | What Happens |
|----------|-------------|
| `"audit"` | I run the full system audit (see AUDIT.md) |
| `"check everything"` | Full validation sweep — all specs tested |
| `"run tests"` | Execute the complete testing checklist |
| `"debug"` | Find and fix all errors in the current file |
| `"full debug"` | Multi-angle diagnostic across all files |

### Mode Switches
| Say This | What Happens |
|----------|-------------|
| `"/plan"` | Switch to Plan Mode (read-only, planning only) |
| `"/code"` | Switch to Code Mode (building) |
| `"what's broken"` | I diagnose the current state and report issues |
| `"status"` | I tell you what's been done and what's pending |

### Undo / Revert Triggers
| Say This | What Happens |
|----------|-------------|
| `"undo that"` | Revert the last change |
| `"go back"` | Same — roll back to previous state |
| `"start over"` | Reset to last clean commit |

---

## What David Built Behind the Scenes

- A detailed instruction file (`CLAUDE.md`) that tells me exactly how this app is built, what every piece does, where everything lives, and what rules to follow
- A full audit and testing system (`AUDIT.md`) — every change gets validated automatically before anything goes live
- Single-file architecture — the entire app is `index.html` (~3,800 lines). No complex web of files to manage
- GitHub Actions auto-deploy — every merge to `main` triggers an automatic publish to the live URL
- Branch protection — security layer ensuring nothing goes live without being properly routed

---

## How "Saving Your Work" Works

When I make changes, they need to be saved in two steps:

- **Commit** = clicking "Save." Snapshots everything I changed with a note describing what was done
- **Push** = clicking "Upload." Sends that save to GitHub so it's backed up and ready to go live

**You won't do this manually.** Just say:
- *"Commit and push"*
- *"Save everything and push it up"*
- *"Ship it"*

I handle all of it. You'll see a confirmation when it's done.

---

## Quick Reference Card

| Action | What to Say |
|--------|-------------|
| Start planning | Just describe what you want |
| Approve plan & build | "Go ahead" / "Build it" |
| Save & upload | "Commit and push" / "Ship it" |
| Run full audit | "Audit" / "Check everything" |
| Fix all bugs | "Full debug" |
| Check status | "Status" / "What's done" |
| Switch to planning mode | "/plan" |
| Switch to building mode | "/code" |
| Undo last change | "Undo that" / "Go back" |

---

## In Simple Terms

1. Go to the URL → sign in with your PIN
2. Open Claude Code → start talking
3. Tell me what you or your client needs
4. I plan it → you approve → I build and test it
5. Say "ship it" → it goes live automatically
6. Zero coding required from you — ever

---

## If Something Looks Wrong

- **Site not loading?** Check the URL: `https://vermillionaxislab.github.io/VAT-Base-Template/`
- **Changes not showing?** Give it 2 minutes — GitHub Pages takes a moment to update after a deploy
- **Something broken in the app?** Say "full debug" and I'll find and fix it
- **Repository issue?** Message David — he owns the settings layer

---

*There's no such thing as a dumb question here. I'm built to help. Just talk.*
