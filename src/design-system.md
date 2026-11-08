# HabitTrail design system

This documents what exists in the code today: `index.html`, `support.html`, `privacy.html`, `css/styles.css` and `js/script.js`.

## Tokens and theme

Design tokens are defined as CSS custom properties on `:root` in `css/styles.css`, and every color, spacing and radius value in the stylesheet uses them. Body text has a 1.6 line height.

Theme: a cheerful Pacific Northwest day hike, with evergreen trees, a sunny trail and soft morning fog.

### Colors

| Token | Name | Value | Use |
|---|---|---|---|
| `--color-primary` | Cedar | `#225C3A` | `<h1>`, buttons, links on Fog and Sunrise |
| `--color-secondary` | Sunrise | `#F6BF60` | Section card backgrounds |
| `--color-tertiary` | Sky | `#A8D8F0` | Header and footer bands |
| `--color-bg` | Fog | `#F5F3EC` | Page background |
| `--color-text` | Bark | `#1F2D26` | Body text, borders, focus outlines, links on Sky |
| `--color-white` | White | `#FFFFFF` | Text on Cedar |

Where each color is used:
- Cedar: the `<h1>` (on Fog on Support and Privacy, on Sunrise in the home page hero), the call-to-action button fill, links on Fog and Sunrise ("On this page", links inside cards), the skip link.
- Sunrise: the background of every `<section>` in `<main>`.
- Sky: the background of `<header>` and `<footer>`, as full-width bands. Links on Sky are Bark, not Cedar, because Cedar read as hard to see there.

Contrast (ratios computed with the WCAG formula; text needs 4.5:1, borders and button edges need 3:1):

| Pair | Ratio |
|---|---|
| Bark on Fog (body text) | 12.94 |
| Bark on Sunrise (card text) | 8.58 |
| Bark on Sky (header and footer links, footer text) | 9.40 |
| Cedar on Fog (`<h1>` on Support and Privacy, jump links) | 7.11 |
| Cedar on Sunrise (links in cards, button edge, home page `<h1>`) | 4.71 |
| Cedar on Sky (skip link chip against the header band) | 5.17 |
| White on Cedar (button, skip link) | 7.89 |
| White on Bark (button hover) | 14.37 |

Rules that follow from this:
- Cedar and Bark are only 1.82:1 apart, so color alone cannot tell a link from body text. Keep links underlined (the browser default).
- Never put white text on Sunrise or Sky; it fails (about 1.5 to 1.9:1). Use Bark.
- The Sunrise cards and Sky bands sit close to Fog in brightness (1.51:1 and 1.38:1) and are told apart by hue, so do not rely on them alone to mark an edge that carries meaning. Use a Bark border, as the table does.
- Cedar links on Sunrise at 4.71:1 pass with a thin margin. Do not make Sunrise any darker or Cedar any lighter without recomputing.

### Fonts

| Token | Value | Use |
|---|---|---|
| `--font-body` | `"Work Sans", sans-serif` | Body text |
| `--font-heading` | `"Work Sans", sans-serif` | `<h1>`, `<h2>`, `<h3>` |

Work Sans comes from Google Fonts, in weights 400 and 700 only, so use only regular and bold. The fallback is the browser's default sans-serif. `<code>` keeps the browser's default monospace.

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
- In `<head>`, before the stylesheet: the two Google Fonts `preconnect` links and the Work Sans stylesheet link (`https://fonts.googleapis.com/css2?family=Work+Sans:wght@400;700&display=swap`).
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
Order on every page: skip link, `<header>` containing the Main nav, `<main id="main">`, `<footer>`, script tag. The `<h1>` is the first element in `<main>` on Support and Privacy. On the home page it is the first element of the hero section, which is the first element in `<main>`. The header holds only the Main nav.

`<header>` and `<footer>` are Sky bands that run the full width of the page, with `--space-l` padding above and below. Their content lines up with `<main>`, centered at 60rem.

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
- Styling: an inline row that wraps, with an 8px row gap and a 16px column gap. Links are underlined: Bark in the header and footer (on Sky), Cedar for "On this page" (on Fog). The current page is bold, not marked by color alone.

### Footer
The Footer nav followed by `<p>&copy; 2026 HabitTrail</p>`.

### Content section
Every page: `<section aria-labelledby="x">` containing `<h2 id="x">`.

Styled as a card: Sunrise background, Bark text, `--radius` corners, `--space-l` padding, and `--space-xl` between cards. Blocks inside the card are `--space-m` apart. In Support, each new question after the first has `--space-l` above it, so question and answer pairs read as groups.

### Hero
```html
<section class="hero" aria-labelledby="build-habits">
  <h1>HabitTrail</h1>
  <h2 id="build-habits">Build habits that stick</h2>
  <p>…</p>
  <p><a class="button" href="#" aria-label="…">Download HabitTrail</a></p>
</section>
```
The first section on `index.html` only. It is a Content section (Sunrise card) laid out as a centered Flexbox column: `<h1>`, `<h2>`, description, then the call-to-action link. Spacing is a progressive scale that groups the content: `<h1>` to `<h2>` is `--space-s`, `<h2>` to description is `--space-m`, and description to button is `--space-xl` (`--space-l` at 40rem wide and below). The largest gaps are above the `<h1>` (`--space-xl` block padding, `--space-l` at 40rem and below) and between the description and the button, so the text is one group and the button another. The same block padding sits below the button, and there is `--space-m` above the card, since it is the first thing in `<main>`. The description is capped at 40rem wide so centered lines stay readable.

### Question and answer
An `<h3>` question followed by a `<p>` answer, or an `<ol>` for step-by-step answers. Used in `support.html`, grouped under topic `<h2>` sections.

### Call-to-action link
```html
<p><a class="button" href="#" aria-label="Download HabitTrail from the App Store">Download HabitTrail</a></p>
```
Used twice in `index.html`. Each has a descriptive `aria-label`. `href="#"` is a placeholder.

The `.button` class gives a Cedar fill, white bold text and `--radius` corners, with no border. It stands out from the Sunrise card at 4.71:1. Hover turns the fill Bark. Use it for call-to-action links.

### Features grid
```html
<ul class="features">
  <li>
    <h3>Daily check-ins</h3>
    <p>Mark a habit done in one tap.</p>
  </li>
  …
</ul>
```
The "Features" section on `index.html`. A `<ul class="features">` laid out as a CSS Grid of four equal columns with a `--space-l` gap, each `<li>` holding one feature's `<h3>` and a short description (`--space-s` between them). No bullets. It drops to two columns at 48rem wide and below, and one column at 40rem and below.

### Feature list
A `<ul>` whose items start with a bold label: `<li><strong>Label:</strong> text</li>`. Used, with a nested `<ul>`, in `privacy.html`.

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
- Check the contrast table before pairing colors in a new way. Text needs 4.5:1 and borders and button edges need 3:1.
