# HabitTrail design system

This documents what exists in the code today: `index.html`, `support.html`, `privacy.html`, `css/styles.css` and `js/script.js`.

## Tokens and theme

Design tokens are defined as CSS custom properties on `:root` in `css/styles.css`, and every color, spacing and radius value in the stylesheet uses them. There are no typography tokens; the base font is the system font stack at a 1.6 line height.

Theme: a cheerful Pacific Northwest day hike, with evergreen trees, a sunny trail and soft morning fog.

### Colors

| Token | Name | Value | Use |
|---|---|---|---|
| `--color-primary` | Cedar | `#2F7D4F` | Buttons, links, current-page nav |
| `--color-secondary` | Sunrise | `#F4B13E` | Highlights, badges, accents |
| `--color-bg` | Fog | `#F5F3EC` | Page background |
| `--color-text` | Bark | `#1F2D26` | Body text, borders, focus outlines |
| `--color-white` | White | `#FFFFFF` | Text on Cedar |

Where each color is used:
- Cedar: the `<h1>`, the call-to-action button fill, links on the Fog background (navs, "On this page"), the skip link.
- Sunrise: the background of every `<section>` in `<main>`.

Contrast rules (ratios computed with the WCAG formula):
- Cedar on Fog is 4.54:1, so use it for headings and links on Fog only.
- Cedar on Sunrise is about 2.7:1 and fails. Links inside Sunrise sections use Bark with an underline, and the button gets a 2px Bark border (7.67:1 against Sunrise).
- White on Cedar is 5.04:1 and Bark on Sunrise is 7.67:1. Never put white text on Sunrise.

### Spacing

Each size doubles the last.

| Token | Value | Use |
|---|---|---|
| `--space-s` | 0.5rem (8px) | List items, nav row gap, table cell padding, button vertical padding |
| `--space-m` | 1rem (16px) | Gap between blocks inside a section, page gutter, button horizontal padding |
| `--space-l` | 2rem (32px) | Section card padding, space before a new question (`<h3>`), header and footer padding |
| `--space-xl` | 4rem (64px) | Space between sections |

Section cards drop to `--space-m` padding at 40rem wide and below. Page content is centered at 60rem with at least a `--space-m` gutter on each side, set as padding on `header`, `main` and `footer`.

### Corner radius

`--radius: 12px`, for both buttons and cards.

### Dark mode

Only light values are defined. The only other theme-related code is `<meta name="color-scheme" content="light dark">` in the `<head>` of `index.html`.

## Page-level rules

- `<html lang="en">`, `<meta charset="UTF-8">`, and the viewport meta tag in every page.
- Stylesheet `css/styles.css` in `<head>`; script `js/script.js` at the end of `<body>`.
- Title: `Support - HabitTrail` and `Privacy - HabitTrail` on those pages; `HabitTrail` on the home page.
- `<meta name="description">` is present on `index.html` and `support.html`.
- One `<h1>` per page, then `<h2>` for sections and `<h3>` beneath, with no skipped levels. The home page `<h1>` is the site name; Support and Privacy use the page title.

## Components

All components are HTML patterns, styled in `css/styles.css`.

### Skip link
```html
<a class="skip-link" href="#main">Skip to main content</a>
```
First element in `<body>` on every page. Targets `<main id="main">`. Hidden off-screen until it receives focus, then shown as a Cedar chip with white text.

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
- Styling: an inline row that wraps, with an 8px row gap and a 16px column gap. Links are Cedar. The current page is bold and underlined, not marked by color alone.

### Footer
The Footer nav followed by `<p>&copy; 2026 HabitTrail</p>`.

### Content section
Every page: `<section aria-labelledby="x">` containing `<h2 id="x">`.

Styled as a card: Sunrise background, Bark text, `--radius` corners, `--space-l` padding, and `--space-xl` between cards. Blocks inside the card are `--space-m` apart. In Support, each new question after the first has `--space-l` above it, so question and answer pairs read as groups.

### Question and answer
An `<h3>` question followed by a `<p>` answer, or an `<ol>` for step-by-step answers. Used in `support.html`, grouped under topic `<h2>` sections.

### Call-to-action link
```html
<p><a class="button" href="#" aria-label="Download HabitTrail from the App Store">Download HabitTrail</a></p>
```
Used twice in `index.html`. Each has a descriptive `aria-label`. `href="#"` is a placeholder.

The `.button` class gives a Cedar fill, white bold text, a 2px Bark border and `--radius` corners. Hover turns the fill Bark. Use it for call-to-action links.

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

Styled as full width with 1px Bark borders, `--space-s` cell padding, and a Fog header row. At 40rem wide and below it uses equal-width columns and smaller text so it fits inside its card without scrolling sideways.

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
- Use tokens for every color, spacing and radius value in CSS. Do not hardcode hex values or spacing outside `:root`.
- Do not put Cedar text on a Sunrise background. Use Bark.
