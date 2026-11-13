# HabitTrail design system

This documents what exists in the code today: `index.html`, `progress.html`, `support.html`, `privacy.html`, `css/styles.css` and `js/script.js`.

## Tokens and theme

Design tokens are defined as CSS custom properties on `:root` in `css/styles.css`, and every color, spacing, radius and type size value in the stylesheet uses them. Body text has a 1.6 line height (`--line-height-body`).

Theme: a cheerful Pacific Northwest day hike, with evergreen trees, a sunny trail and soft morning fog.

### Colors

| Token | Name | Value | Use |
|---|---|---|---|
| `--color-primary` | Cedar | `#225C3A` | `<h1>`, buttons, links on Fog and Sunrise. An alias of `--cedar-700` |
| `--color-secondary` | Sunrise | `#F6BF60` | Content section cards (except the hero and features sections) and the feature cards |
| `--color-tertiary` | Sky | `#A8D8F0` | Header band |
| `--color-bg` | Fog | `#F5F3EC` | Page background |
| `--color-text` | Bark | `#1F2D26` | Body text, borders, focus outlines, links on Sky. Never a button fill |
| `--color-white` | White | `#FFFFFF` | Text on Cedar and its shades |

#### Cedar scale

Shades of Cedar, ordered by lightness. The state tokens are aliases, so a button rule says what the color is for and the scale says what it is.

| Scale token | Value | State token | Use |
|---|---|---|---|
| `--cedar-900` | `#143523` | `--color-primary-pressed` | Button fill while pressed |
| `--cedar-700` | `#225C3A` | `--color-primary` | Cedar at rest |
| `--cedar-600` | `#2F7A4F` | `--color-primary-hover` | Button fill on hover |
| `--cedar-500` | `#5A7D69` | `--color-primary-disabled` | Disabled button fill. Desaturated (about 16% against Cedar's 46%), so it reads as inactive, not just lighter |

Where each color is used:
- Cedar: the `<h1>` (on Fog on every page), the call-to-action button fill, links on Fog and Sunrise ("On this page", links inside cards), the skip link. On the Progress page it is also the fill that means progress: done days in the streak calendar, the filled part of a progress bar, and the bars in the weekly chart.
- Sunrise: the background of every `<section>` in `<main>` except the hero and features sections, which sit on Fog. The four feature cards on the home page are Sunrise too.
- Sky: the background of `<header>`, as a full-width band. The footer has no band and sits on Fog. Links on Sky are Bark, not Cedar, because Cedar read as hard to see there.

Contrast (ratios computed with the WCAG formula; text needs 4.5:1, borders and button edges need 3:1):

| Pair | Ratio |
|---|---|
| Bark on Fog (body text) | 12.94 |
| Bark on Sunrise (card text) | 8.58 |
| Bark on Sky (header links) | 9.40 |
| Cedar on Fog (`<h1>` on every page, jump links, footer links) | 7.11 |
| Cedar on Sunrise (links in cards, button edge) | 4.71 |
| Cedar on Sky (skip link chip against the header band) | 5.17 |
| White on Cedar (button, skip link) | 7.89 |
| White on `--cedar-600` (button hover) | 5.23 |
| White on `--cedar-900` (button pressed) | 13.43 |
| White on `--cedar-500` (button disabled) | 4.59 |
| `--cedar-600` on Fog (hover button edge) | 4.71 |
| `--cedar-600` on Sunrise (hover button edge) | 3.12 |
| `--cedar-500` on Fog (disabled button edge) | 4.14 |
| `--cedar-500` on Sunrise (disabled button edge) | 2.74 |

The disabled button on Sunrise is below 3:1 on purpose: WCAG exempts inactive controls, and the muted look is the signal. Its white label still passes 4.5:1.

Rules that follow from this:
- Cedar and Bark are only 1.82:1 apart, so color alone cannot tell a link from body text. Keep links underlined (the browser default).
- Bark is for text, borders and focus outlines. Button fills, in every state, are shades of Cedar.
- Never put white text on Sunrise or Sky; it fails (about 1.5 to 1.9:1). Use Bark.
- The Sunrise cards and Sky bands sit close to Fog in brightness (1.51:1 and 1.38:1) and are told apart by hue, so do not rely on them alone to mark an edge that carries meaning. Use a Bark border, as the table does.
- Cedar links on Sunrise at 4.71:1 pass with a thin margin. Do not make Sunrise any darker or Cedar any lighter without recomputing.

### Fonts

| Token | Value | Use |
|---|---|---|
| `--font-body` | `"Lexend", sans-serif` | Body text |
| `--font-heading` | `"Lexend", sans-serif` | `<h1>`, `<h2>`, `<h3>` |
| `--font-weight-heading` | `600` | Weight of `<h1>`, `<h2>`, `<h3>` |

Lexend is a geometric, open typeface that comes from Google Fonts in weights 400, 600 and 700 only, so use only those. Regular (400) is body text. Semibold (600) is for headings: bold looks cramped at small heading sizes. Bold (700) is for emphasis inside text and controls: `<strong>`, the button label, table captions and the current page's nav link. Numerals were a deciding factor: 1 doesn't look like l or 7, 4 is closed, and 9 has an angled tail so it can't be mistaken for 8, which matters for streak counts and percentages. The fallback is the browser's default sans-serif. `<code>` keeps the browser's default monospace.

#### Type scale

Sized for Lexend, which has a large x-height and wide letterforms. Only sizes something uses are defined. The two largest are fluid: they follow the window width between a minimum and a maximum, and both ends are in `rem`, so they still respect the user's own text size setting.

| Token | Value | Use |
|---|---|---|
| `--font-size-s` | `0.875rem` (14px) | Weekday labels, chart labels, the Privacy table at 40rem and below |
| `--font-size-m` | `1rem` (16px) | Body text, button label, nav links |
| `--font-size-l` | `1.125rem` (18px) | `<h3>` |
| `--font-size-2xl` | `clamp(1.75rem, 3vw, 2.25rem)` (28 to 36px) | `<h2>`, stat values |
| `--font-size-3xl` | `clamp(2.25rem, 5vw, 3.25rem)` (36 to 52px) | `<h1>` |

| Token | Value | Use |
|---|---|---|
| `--line-height-body` | `1.6` | Body text and everything that inherits it |
| `--line-height-heading` | `1.25` | `<h2>`, `<h3>`, stat values |
| `--line-height-display` | `1.15` | `<h1>`, tighter so a title that wraps still reads as one block |
| `--letter-spacing-heading` | `-0.02em` | `<h1>` and `<h2>`. Lexend is wide, so large sizes look loose without it. Not used on `<h3>` or below |

`--font-size-s` (14px) is used for supporting labels only. Nav links and the button stay at `--font-size-m`, because they are the most-tapped text on the site. There is no `xs` or `xl` size yet: add one only when something uses it. Weights follow the Fonts table above: 400 for body, 600 for headings, 700 only at `--font-size-m` or smaller. Do not put 700 on a heading.

### Spacing

Each size doubles the last.

| Token | Value | Use |
|---|---|---|
| `--space-s` | 0.5rem (8px) | List items, nav row gap, table cell padding, button vertical padding |
| `--space-m` | 1rem (16px) | Gap between blocks inside a section, page gutter, button horizontal padding |
| `--space-l` | 2rem (32px) | Section card padding, space before a new question (`<h3>`), header padding, footer bottom padding |
| `--space-xl` | 4rem (64px) | Space between sections |

Section cards drop to `--space-m` padding at 40rem wide and below. The hero and features sections have no card, so no padding of their own beside the hero's block padding. Page content is centered at 60rem with at least a `--space-m` gutter on each side, set as padding on `header`, `main` and `footer`.

### Corner radius

`--radius: 12px`, for both buttons and cards.

### Tap target

`--tap-target: 2.75rem` (44px), the minimum height for anything tappable. Nav links and `.button` use it as `min-height`, so they are easy to hit with a thumb. Any new link-like control should do the same, and should define the same states as `.button` (enabled, hover, focus, pressed, disabled).

### Component sizes

Sizes that belong to one component, not to the spacing scale. Defined on `:root` so they are easy to find and change.

| Token | Value | Use |
|---|---|---|
| `--chart-height` | `10rem` | Height of the tallest bar in the weekly bar chart |
| `--calendar-max-width` | `28rem` | Widest the streak calendar grows. It is centered, so on a wide card it does not stretch into big gaps |

### Dark mode

Only light values are defined. The only other theme-related code is `<meta name="color-scheme" content="light dark">` in the `<head>` of `index.html`.

## Page-level rules

- `<html lang="en">`, `<meta charset="UTF-8">`, and the viewport meta tag in every page.
- In `<head>`, before the stylesheet: the two Google Fonts `preconnect` links and the Lexend stylesheet link (`https://fonts.googleapis.com/css2?family=Lexend:wght@400;600;700&display=swap`).
- Stylesheet `css/styles.css` in `<head>`; script `js/script.js` at the end of `<body>`.
- Title: `See your progress - HabitTrail`, `Support - HabitTrail` and `Privacy - HabitTrail` on those pages; `HabitTrail` on the home page.
- `<meta name="description">` is present on `index.html`, `progress.html` and `support.html`.
- One `<h1>` per page, then `<h2>` for sections and `<h3>` beneath, with no skipped levels. The home page `<h1>` is the site name; Progress, Support and Privacy use the page title. The Progress page's `<h1>` and title say "See your progress", while its nav label is the shorter "Progress".

## Components

All components are HTML patterns, styled in `css/styles.css`.

### Skip link
```html
<a class="skip-link" href="#main">Skip to main content</a>
```
First element in `<body>` on every page. Targets `<main id="main">`. Hidden off-screen until it receives focus, then shown as a Cedar chip with white text.

### Page shell
Order on every page: skip link, `<header>` containing the Main nav, `<main id="main">`, `<footer>`, script tag. The `<h1>` is the first element in `<main>` on Support and Privacy. On the home page it is the first element of the hero section, which is the first element in `<main>`. The header holds only the Main nav.

`<body>` is a full-height Flexbox column (`min-height: 100vh`, with `100dvh` after it) and `<main>` has `flex: 1`, so on a page shorter than the window the footer sits at the bottom of the window instead of floating under the content. On a longer page the footer follows the content as usual.

`<header>` is a Sky band that runs the full width of the page, with `--space-l` padding above and below. `<footer>` has no background: it sits on Fog with `--space-l` padding below only, since `<main>`'s bottom padding already separates it from the content. Both line up with `<main>`, centered at 60rem.

### Navigation
```html
<nav aria-label="Main">
  <ul>
    <li><a href="index.html">Home</a></li>
    <li><a href="progress.html">Progress</a></li>
    <li><a href="support.html" aria-current="page">Support</a></li>
    <li><a href="privacy.html">Privacy</a></li>
  </ul>
</nav>
```
- Labels in use: `Main` (in `<header>`), `Footer` (in `<footer>`), `On this page` (in-page jump links, `support.html` only).
- Main and Footer navs list Home, Progress, Support, Privacy, in that order, on every page.
- The link for the current page has `aria-current="page"`, in both Main and Footer.
- Styling: an inline row that wraps, with an 8px row gap and a 16px column gap. Each link is at least `--tap-target` (44px) tall. Links are underlined: Bark in the header (on Sky), Cedar in the footer and for "On this page" (on Fog). The current page is bold, not marked by color alone.

### Footer
The Footer nav followed by `<p>&copy; 2026 HabitTrail</p>`.

### Content section
Every page: `<section aria-labelledby="x">` containing `<h2 id="x">`.

Styled as a card: Sunrise background, Bark text, `--radius` corners, `--space-l` padding, and `--space-xl` between cards. Blocks inside the card are `--space-m` apart. In Support, each new question after the first has `--space-l` above it, so question and answer pairs read as groups. The exceptions are the home page hero and features sections, which have no card (see Hero and Features grid).

### Hero
```html
<section class="hero" aria-labelledby="build-habits">
  <h1>HabitTrail</h1>
  <h2 id="build-habits">Build habits that stick</h2>
  <p>…</p>
  <p><a class="button" href="#" aria-label="…">Download HabitTrail</a></p>
</section>
```
The first section on `index.html` only. It has no card: it sits directly on the Fog page background, with no fill or corner radius and no inline padding, so its content lines up with the page gutter. It is laid out as a centered Flexbox column: `<h1>`, `<h2>`, description, then the call-to-action link. Spacing is a progressive scale that groups the content: `<h1>` to `<h2>` is `--space-s`, `<h2>` to description is `--space-m`, and description to button is `--space-xl` (`--space-l` at 40rem wide and below). The largest gaps are above the `<h1>` (`--space-xl` padding, `--space-l` at 40rem and below) and between the description and the button, so the text is one group and the button another. There is no padding below the button, since there is no card edge to clear; the next section's `--space-xl` top margin provides the space. There is `--space-m` above the hero, since it is the first thing in `<main>`. The description is capped at 40rem wide so centered lines stay readable.

### Question and answer
An `<h3>` question followed by a `<p>` answer, or an `<ol>` for step-by-step answers. Used in `support.html`, grouped under topic `<h2>` sections.

### Call-to-action link
```html
<p><a class="button" href="#" aria-label="Download HabitTrail from the App Store">Download HabitTrail</a></p>
```
Used twice in `index.html`. Each has a descriptive `aria-label`. `href="#"` is a placeholder.

The `.button` class gives a Cedar fill, white bold text and `--radius` corners, with no border. It is at least `--tap-target` (44px) tall, with the text centered vertically. It stands out from the Sunrise card at 4.71:1, and the fill alone (no shadow) is what marks it as the primary call to action. Use it for call-to-action links.

States. Fills come from the Cedar scale (see Colors); the label is always white.

| State | Fill | Also |
|---|---|---|
| Enabled | `--color-primary` (Cedar) | Bold white label |
| Hover | `--color-primary-hover`, a lighter Cedar | Only on devices that can hover (`@media (hover: hover)`), so a tap on a phone doesn't leave it stuck |
| Focus | Unchanged | The global `:focus-visible` outline (3px Bark, 2px offset), a shape change that doesn't rely on color. There is no separate `.button` focus rule |
| Pressed (`:active`) | `--color-primary-pressed`, the darkest Cedar | Shrinks slightly (`scale(0.98)`), which also gives feedback on touch |
| Disabled | `--color-primary-disabled`, a muted gray-green | `cursor: not-allowed`, and no hover or pressed change. Label contrast drops from 7.89:1 to 4.59:1 but stays readable |

Hover goes lighter and pressed goes darker, on opposite sides of the resting color, because Cedar is already dark and two darker steps would look alike. There are no shadows. `--cedar-600` is about as light as hover can go: lighter and the hover button edge drops below 3:1 against Sunrise, so recompute before nudging it further.

Disabled applies to `.button:disabled` and `.button[aria-disabled="true"]`. A link can't be natively disabled, so on an `<a class="button">` set `aria-disabled="true"`. No page uses a disabled button yet. When one does, also make the link inert (remove its `href`, or add `tabindex="-1"`), since the styling alone does not stop a click.

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
The "Features" section on `index.html` (`<section class="features-section">`, which like the hero has no card and sits on Fog). A `<ul class="features">` laid out as a CSS Grid of four equal columns with a `--space-m` gap. Each `<li>` is a compact Sunrise card (Bark text, `--radius` corners, `--space-m` padding) holding one feature's `<h3>` and a short description (`--space-s` between them). No bullets. It drops to two columns at 48rem wide and below, and one column at 40rem and below.

### Stat tiles
```html
<section class="stats-section" aria-labelledby="streaks">
  <h2 id="streaks">Your streaks</h2>
  <dl class="stats">
    <div>
      <dt>Current streak</dt>
      <dd>12 days</dd>
    </div>
    …
  </dl>
</section>
```
Used on `progress.html`. Like the hero and features sections, `.stats-section` has no card and sits on Fog. A `<dl class="stats">` laid out as a CSS Grid of three equal columns with a `--space-m` gap, one column at 40rem wide and below. Each `<div>` is a compact Sunrise card (`--radius` corners, `--space-m` padding) with the label (`<dt>`, regular text) above the value (`<dd>`, `--font-heading` at `--font-size-2xl`, `--line-height-heading` and `--font-weight-heading`), `--space-s` apart. Label first in the source, so a screen reader says "Current streak, 12 days". Bark on Sunrise is 8.58:1.

### Streak calendar
```html
<figure class="calendar">
  <figcaption>Morning walk, oldest week first</figcaption>
  <ul class="calendar-weekdays" aria-hidden="true">
    <li>Mon</li> …
  </ul>
  <ol class="calendar-days">
    <li class="day done"><span class="visually-hidden">Mon: done</span></li>
    <li class="day"><span class="visually-hidden">Tue: missed</span></li>
    …
    <li class="day done today"><span class="visually-hidden">Sun: done, today</span></li>
  </ol>
</figure>
```
Used on `progress.html`, inside a Sunrise content card. A grid of equal squares, seven to a row, one row per week, oldest first. The weekday labels and the days are two lists that share the same seven-column grid (`repeat(7, minmax(0, 1fr))`, `--space-s` gap), so they line up. The figure is centered and no wider than `--calendar-max-width`. Each `.day` is a square (`aspect-ratio: 1`, `--radius` corners, 1px Bark border):

| State | Look | Also |
|---|---|---|
| Done (`.day.done`) | Cedar fill | Hidden text "Mon: done" |
| Missed (`.day`) | Fog fill, empty | Hidden text "Tue: missed" |
| Today (`.day.today`) | Adds a 3px Bark outline, 2px offset. Combine with Done or Missed | Hidden text ", today" |

Why a grid and not a table: a table sizes each column to its content, so the squares came out different widths on a wide screen. Equal grid columns keep every square the same size at any width, and showing more than four weeks later just adds rows.

The weekday labels are `aria-hidden`, because a plain list has no column headers. A screen reader gets the weekday from each day's hidden text instead. A sentence above the grid says what the shading means and gives the total ("22 of 28 days done").

Done and missed differ in lightness, not only hue (Cedar against Fog is 7.11:1), and today has an outline, so the states hold up in grayscale. Contrast against the Sunrise card: the Cedar fill is 4.71:1 and the Bark border is 8.58:1 (both need 3:1). A Fog missed box has its Bark border to stand out, since Fog and Sunrise are only 1.51:1. Days are not interactive, so the tap target rule does not apply.

**Planned:** icons or another visual indicator for done and missed days. The shading is deliberately plain until then. The expectation is SF Symbols on iOS and Material on Android.

### Progress bar
```html
<ul class="habit-list">
  <li>
    <div class="habit-heading">
      <label for="habit-walk">Morning walk</label>
      <span>7 of 7 days</span>
    </div>
    <progress id="habit-walk" max="7" value="7">7 of 7 days</progress>
  </li>
  …
</ul>
```
Used on `progress.html`, inside a Sunrise content card. A native `<progress>` with a visible `<label>` (bold) and the value written out as text on the same line, so the number never depends on the bar. The bar is full width, `--space-m` tall, with `--radius` corners, a 1px Bark border and a Fog track; the fill is Cedar. The styles reset the browser's default look (`appearance: none`) and set the fill separately for Safari/Chrome (`::-webkit-progress-value`) and Firefox (`::-moz-progress-bar`). Items are `--space-m` apart. Cedar fill on the Fog track is 7.11:1, and the Bark border on Sunrise is 8.58:1.

### Weekly bar chart
```html
<ol class="chart" style="--max: 3">
  <li style="--value: 3">
    <span class="chart-label">Mon</span>
    <span class="chart-bar"></span>
    <span class="chart-value">3<span class="visually-hidden"> of 3 check-ins</span></span>
  </li>
  …
</ol>
```
Used on `progress.html`, inside a Sunrise content card. Seven equal columns (`--space-s` gap) bottom-aligned, so every bar sits on the same baseline. Each bar is Cedar with `--radius` rounding on its top corners, at most `--space-xl` wide, and `--chart-height` times `--value` divided by `--max` tall. `--max` is set on the list and `--value` on each item, inline, because they are the data. In the source each item reads label, bar, value, so a screen reader says "Mon, 3 of 3 check-ins"; `flex-direction: column-reverse` draws the label at the bottom, the bar above it and the number on top of the bar. The number is always shown, so a bar's height is never the only way to read it. Cedar bars against the Sunrise card are 4.71:1 (need 3:1).

### Visually hidden text
```html
<span class="visually-hidden">Done</span>
```
`.visually-hidden` removes text from the screen but leaves it for screen readers. Use it only where a symbol or position already says the same thing to sighted users (a filled or empty calendar box, "of 3 check-ins"). Do not use it to hide content sighted users need.

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
