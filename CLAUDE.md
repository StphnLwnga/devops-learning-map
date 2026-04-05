# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML/CSS/JS educational site called **DevOps Easy Learning Hub** — a three-tier concept map for learning DevOps. There is no build system, bundler, or framework; files are plain HTML opened directly in a browser.

## Architecture

- **`index.html`** — Main hub/landing page. Contains a three-tier interactive concept map (Foundations → Core Practices → Advanced) with a slide-up detail panel. All topic data lives in a `DATA` JavaScript object embedded in the page.
- **`topics/`** — Individual deep-dive lesson pages (e.g., `git-and-vcs.html`, `what-is-devops.html`). Each is a standalone HTML file with its own inline `<style>` block for page-specific styles, plus embedded quiz logic in `<script>`.
- **`css/shared.css`** — Shared design system: dark theme variables, typography (DM Sans + JetBrains Mono), reusable component styles (callouts, code blocks, cards, tables, quizzes, navigation).
- **`git history.txt`** — Shell history log used as a teaching reference for Git commands.
- **`git-process.pptx.pdf`**, **`What is DevOps.pptx`** — Supplementary presentation materials.

## Design Conventions

- Dark theme with CSS custom properties (`--bg`, `--surface`, `--text`, etc.) defined in both `shared.css` and per-page `:root` blocks.
- Color-coded tiers: amber/gold for Foundations, indigo for Core, emerald for Advanced — each with matching `--tier-*` and `--c-*` variables.
- Fonts loaded from Google Fonts: `DM Sans` for body text, `JetBrains Mono` for code/labels/badges.
- Topic pages use a sidebar TOC layout on desktop that collapses on mobile. The hub page uses a grid layout.
- Interactive quizzes are embedded at the bottom of topic pages with self-contained JS (no external dependencies).

## Adding a New Topic Page

1. Create `topics/<slug>.html` following the pattern of existing pages: import `../css/shared.css`, add page-specific styles inline, include a `.top-nav` with back link to `../index.html`.
2. In `index.html`, add the topic entry to the appropriate tier array in the `DATA` object, setting `hasContent: true` and `contentUrl: 'topics/<slug>.html'`.

## Quiz Generation Pattern

Each topic page includes an embedded quiz at the bottom:

- **Pool**: 50-60 questions covering all sub-topics, distributed roughly evenly.
- **Per session**: 10 random questions via Fisher-Yates shuffle (`shuffle(QUESTIONS).slice(0, 10)`).
- **Pass threshold**: 7/10.
- **Question format**: `{ q: "...", options: ["A", "B", "C", "D"], correct: <0-3>, explanation: "..." }`
- **Explanations**: Every question includes an `explanation` string shown after the user answers.
- Quiz engine JS is duplicated inline in each page's `<script>` block (no shared JS file).

## Tooltip / Hover Definition Component

For inline technical term definitions, use the `.term` + `.tip` pattern (styles in `css/shared.css`):

```html
<span class="term">cipher suite<span class="tip">A set of algorithms for encryption, authentication, and key exchange in TLS.</span></span>
```

- Terms render with a dashed underline and `cursor: help`
- Tooltip appears above on hover with a 0.2s fade-in animation
- Max width 300px, dark surface background, 8px gap from trigger
- Use for terms that benefit from a quick definition without breaking reading flow
- Works on mobile via CSS `:focus` fallback
