# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Project overview

HabitTrail is a static website for a habit-tracking app. It has three pages:

- `index.html`: Home. Introduces the app, lists its main features, links to download it.
- `support.html`: Support. Answers common questions (getting started, reminders, streaks and progress, data).
- `privacy.html`: Privacy. Explains what data is collected and how it is protected.

Personas, the site map and each page's success criteria are in `docs/BRIEF.md`.

## Commands

None. There is no `package.json`, build step, linter or test suite. The pages are plain HTML; open them directly in a browser.

## Architecture

- `index.html`, `support.html`, `privacy.html`: the pages, at the repo root.
- `css/styles.css`: the single stylesheet, linked from every page. Holds the design tokens (CSS variables on `:root`) and the styles for all three pages.
- `js/script.js`: the single script, loaded at the end of `<body>` on every page. Currently empty.
- `docs/BRIEF.md`: personas, site map, required content and success criteria.
- `docs/sitemap.md`: diagram of how the pages link to each other.
- `src/design-system.md`: the design system reference (see below).

Every page shares the same Main nav (header) and Footer nav, each linking to Home, Support and Privacy.

## Conventions

- Use semantic HTML and keep the accessibility patterns already on every page: skip link, labelled landmarks, `aria-current="page"` on the current page's nav links, one `<h1>` per page.
- Write `&` as `&amp;` in HTML text.
- Placeholder content (support answers, contact method, App Store links with `href="#"`, parts of the privacy copy) is waiting on a collaborator. Do not invent real content to replace it.
- There is no git remote yet. Commit to local history only.

## Browser checks

The user checks all changes in their own browser. Do not launch headless Chrome (or any headless or automated browser) to verify changes, and do not run any command or flag that reduces, or could reduce, browser security (for example `--disable-web-security`, `--allow-file-access-from-files` or `--no-sandbox`). After a change, say what to look at in the browser (page, viewport width, behaviour to confirm) and leave the check to the user.

## Design system

Before any task that touches UI, styling, or components (HTML structure in `index.html`, `support.html` or `privacy.html`, `css/styles.css`, `js/script.js`), read `src/design-system.md` first and follow it before changing code.
