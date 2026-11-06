# HabitTrail design system

This documents what exists in the code today: `index.html`, `support.html`, `privacy.html`, `css/styles.css` and `js/script.js`.

## Tokens and theme

No design tokens are defined. `css/styles.css` is empty, so there are no colors, typography, spacing, or other variables.

The only theme-related code is `<meta name="color-scheme" content="light dark">` in the `<head>` of `index.html`.

## Page-level rules

- `<html lang="en">`, `<meta charset="UTF-8">`, and the viewport meta tag in every page.
- Stylesheet `css/styles.css` in `<head>`; script `js/script.js` at the end of `<body>`.
- Title: `Support - HabitTrail` and `Privacy - HabitTrail` on those pages; `HabitTrail` on the home page.
- `<meta name="description">` is present on `index.html` and `support.html`.
- One `<h1>` per page, then `<h2>` for sections and `<h3>` beneath, with no skipped levels. The home page `<h1>` is the site name; Support and Privacy use the page title.

## Components

All components are HTML patterns. None has styling yet.

### Skip link
```html
<a class="skip-link" href="#main">Skip to main content</a>
```
First element in `<body>` on every page. Targets `<main id="main">`. The only CSS class in the codebase; it has no CSS rule.

### Page shell
Order on every page: skip link, `<header>` containing the Main nav, `<main id="main">`, `<footer>`, script tag. On the home page the header also contains the `<h1>` site name before the nav.

### Navigation
```html
<nav aria-label="Main">
  <ul>
    <li><a href="index.html">Home</a></li>
    <li><a href="support.html" aria-current="page">Support</a></li>
    <li><a href="privacy.html">Privacy</a></li>
  </ul>
</nav>
```
- Labels in use: `Main` (in `<header>`), `Footer` (in `<footer>`), `On this page` (in-page jump links, `support.html` only).
- Main and Footer navs list Home, Support, Privacy, in that order, on every page.
- The link for the current page has `aria-current="page"`, in both Main and Footer.

### Footer
The Footer nav followed by `<p>&copy; 2026 HabitTrail</p>`.

### Content section
Every page: `<section aria-labelledby="x">` containing `<h2 id="x">`.

### Question and answer
An `<h3>` question followed by a `<p>` answer, or an `<ol>` for step-by-step answers. Used in `support.html`, grouped under topic `<h2>` sections.

### Call-to-action link
```html
<p><a href="#" aria-label="Download HabitTrail from the App Store">Download HabitTrail</a></p>
```
Used twice in `index.html`. Each has a descriptive `aria-label`. `href="#"` is a placeholder.

### Feature list
A `<ul>` whose items start with a bold label: `<li><strong>Label:</strong> text</li>`. Used in `index.html` and, with a nested `<ul>`, in `privacy.html`.

### Data table
```html
<table>
  <caption>…</caption>
  <thead><tr><th scope="col">…</th></tr></thead>
  <tbody><tr><th scope="row">…</th><td>…</td></tr></tbody>
</table>
```
Used in `privacy.html`. Has a `<caption>`, column headers with `scope="col"`, and row headers with `scope="row"`.

### Inline elements
- `<code>` for literal values, e.g. `YYYY-MM-DD`.
- `<time datetime="2026-11-05">` for dates.
- Both used in `privacy.html`.

## Usage rules

Followed consistently across the pages:

- Keep the Main and Footer nav labels, link order and `aria-current="page"` handling identical on every page.
- Give every landmark that appears more than once (`<nav>`) a distinct `aria-label`.
- Label every `<section>` with `aria-labelledby` pointing at its `<h2>`'s `id`.
- Keep one `<h1>` per page and do not skip heading levels.
- Give links whose visible text is ambiguous out of context a descriptive `aria-label`.
- Write `&` as `&amp;` in HTML text.
