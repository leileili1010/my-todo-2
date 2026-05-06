# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

MEMO is a single-file todo app (`index.html`). All HTML, CSS, and JavaScript live in that one file — there is no build step, no package manager, and no framework.

To view the app, open `index.html` directly in a browser.

## Architecture

Everything is in `index.html`, organized in three sections:

**CSS (`<style>`)** — all styling is inline, using CSS custom properties defined in `:root` for the color palette and design tokens. The "Sunny Pop" theme uses warm cream backgrounds (`--bg: #fff8f0`) with coral-orange accents (`--accent: #ff6b35`) and Nunito + Bebas Neue fonts loaded from Google Fonts.

**HTML (`<body>`)** — static shell only. The task list (`#task-list`) is fully rendered by JavaScript; the HTML just provides the boot overlay, input row, filter buttons, and footer scaffold.

**JavaScript (inline `<script>`)** — an IIFE with no external dependencies. Key pieces:

- `tasks` array held in module scope, persisted to `localStorage` under key `memo_tasks`
- `uid` auto-increments from the highest existing task `id` on load
- `render()` rebuilds `#task-list` innerHTML on every state change (no virtual DOM)
- `window.toggle(id)` / `window.remove(id)` are exposed globally because they are called from inline `onclick` attributes in the rendered HTML
- Task deletion waits for the CSS `removing` animation to finish before mutating state (`animationend` listener with `{ once: true }`)
- All user-supplied text is sanitized through `esc()` before being injected into innerHTML

## GitHub

`gh` CLI is not installed system-wide. A working binary lives at `/tmp/gh_2.67.0_linux_amd64/bin/gh` (may not survive reboots). Re-download with:

```
curl -sL https://github.com/cli/cli/releases/download/v2.67.0/gh_2.67.0_linux_amd64.tar.gz -o /tmp/gh.tar.gz && tar xzf /tmp/gh.tar.gz -C /tmp
```

# Rules

- Do NOT run any git operations (git add, commit, push, etc.) unless explicitly instructed by the user.
- Never automatically commit or push changes.
- Only modify files.
