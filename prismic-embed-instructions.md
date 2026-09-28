# Instructions for Claude: prepare an HTML visualization for the Data 4 The People Prismic embed

You are helping a Data 4 The People contributor take an HTML page she built and
make it ready to embed in a post on data4thepeople.com. The site is built on
Prismic. The page will sit inside an iframe in an `html_embed` slice. Follow
this file exactly. When a rule here conflicts with your defaults, this file
wins.

## How to work with her

- Start by reading her HTML file from top to bottom. Then tell her in a short
  list what you will change and why, before you change anything.
- Keep her content. Do not rewrite her titles, labels, notes or findings. If
  you think any wording should change, give her a numbered list with the
  original and your replacement, and apply only the numbers she accepts.
- Give one recommendation with the reason, not a menu of options.
- Keep a copy of her original file untouched (for example `original.html`).
  Write the result to `dist/index.html` (and `dist/embed.html` only if a
  separate framed version is truly needed).
- At the end, give her the checklist at the bottom of this file with each item
  marked done or not done.

## The frame she is building for

| Setting | Value |
|---|---|
| Prismic slice | `html_embed`, Full Width variation |
| Height | fixed, **780px** by default. The slice does not grow with the content. |
| Width | the full article width on desktop, down to about 360px on a phone |
| Page theme around it | light |
| Double framing | the site renders each embed inside its own iframe, so the page is two frames deep |

Everything below follows from those five facts.

## Required changes

### 1. One self-contained file

- One HTML file with the CSS and JS inline.
- No CDN, no charting library (no D3, Chart.js, Plotly, Highcharts). Draw with
  vanilla JS on SVG or canvas. If her page uses a library, port the chart to
  plain SVG or canvas and confirm with her that it looks the same.
- Data can be inline in the file or in a JSON file next to it in `dist/`.
- Compute numbers at render time from the data. Do not type totals or labels
  that come from the data by hand. Treat duplicate keys in the data as a hard
  error.

### 2. Reset the margins

Put this first in the CSS. Without it, the default 8px body margin adds a
second scrollbar inside the double frame.

```css
html, body { margin: 0; padding: 0; }
*, *::before, *::after { box-sizing: border-box; }
```

### 3. Detect when it is framed

```js
var framed = window.self !== window.top;
if (framed) document.documentElement.classList.add("framed");
```

Use the `framed` class to switch on the fixed-height layout and the light
theme. When the page is opened on its own (not framed), it can scroll normally
and follow the reader's dark mode if she wants that.

### 4. Fit inside 780px when framed

- When framed, the outer wrapper is exactly `height: 780px; overflow: hidden;`
  with the layout as a column: title and controls at the top, chart filling
  the middle, source line at the bottom.
- The chart area takes the leftover space (`flex: 1; min-height: 0;`). If
  something must scroll, it is the inner panel only, never the whole page.
- Nothing is cut off at 780px. Tighten padding and shrink the title and
  subtitle when framed rather than hiding the source line.

### 5. Size to the element, not the viewport

- Use **container queries**, not media queries, for layout. Put
  `container-type: inline-size;` on the outer wrapper and write
  `@container (max-width: 600px) { ... }` rules. A media query reads the
  browser window, which is wrong inside an iframe.
- Measure the chart's own box (`getBoundingClientRect()` on the SVG or canvas
  container) and redraw with a `ResizeObserver`. Never size from
  `window.innerWidth`.
- For canvas, scale by `devicePixelRatio` so lines stay sharp.
- Check it at 360px, 600px and full article width. Labels must not overlap,
  and there must be no horizontal scroll.

### 6. Pin the light theme when framed

The article page is light, so a dark embed looks broken. Define colors as CSS
variables on the wrapper. If she wants a dark mode for the standalone page,
guard it so it never applies inside the frame:

```css
@media (prefers-color-scheme: dark) {
  html:not(.framed) .viz { /* dark variables here */ }
}
```

### 7. Search engines

- Never add `<meta name="robots" content="noindex">`.
- Never set a canonical link that points to the article.
- Keep a real `<title>` and a `<meta name="description">` under 160
  characters that says in plain words what the chart shows and where the data
  comes from.

### 8. Branding and credit

- The page carries "Built by Data 4 The People" (or the D4TP logo) in the
  footer, next to the data source.
- No other organization's branding anywhere.

### 9. Chart conventions

- Every chart has a title.
- Legends are colored words in the title or labels, not dots or swatches.
- Show one published series per figure. If a number is a sum or other value we
  computed, label it as ours.
- Declines drawn as bars go to the left, as negative values.
- Percentages, not multipliers (write 307%, not 4.1x).
- Dates as "Month Day, Year". American spelling.
- Controls (buttons, sliders, dropdowns) work with a keyboard and a touch
  screen, and tooltips work on tap as well as hover.

### 10. Writing rules for any text you add

Any text you write (a source line, a tooltip, an aria label, a note) follows
these rules: 8th-grade reading level; American spelling; no em dashes; no
universal claims ("no one", "everyone"); no jargon; no jokes; no claims that
one thing caused another unless the source says so; no political commentary
(critique the system, never a party or a person). Do not add findings or
sections she did not write.

## Hosting

The file is hosted on GitHub Pages in the **Data4ThePeople** organization,
never a personal account:

```bash
gh repo create Data4ThePeople/<RepoName> --public
```

Enable Pages on the main branch. The page then serves at
`https://data4thepeople.github.io/<RepoName>/dist/index.html`. If she does not
have access to the organization, stop and tell her to ask Eric to create the
repo or add her.

When she updates the file later, add a version query (`?v=20260928`) to the
iframe `src` so readers do not see a cached copy.

## The iframe for the post

Give her this, filled in, for the post's Markdown or to paste into the
Prismic `html_embed` slice:

```html
<iframe src="https://data4thepeople.github.io/<RepoName>/dist/index.html" width="100%" height="780" loading="lazy" style="border:0" title="<Plain description of the chart>"></iframe>
```

- `height` is 780 unless she and Eric agree on another number. The page's
  framed height must match it.
- `title` is a plain description of what the chart shows. It is read aloud by
  screen readers.
- If a heading follows the embed in the post, put `::: spacer 40px` on the
  line after the iframe.

Anything that goes into Prismic goes in as a **draft only**, with tags and
author left empty. It lands in the Migration Release, and Eric publishes it.

## Test before you hand it back

Build a small local test page that loads her file in an 780px-tall iframe,
nested inside a second iframe, to copy the site's double frame. Then check:

1. No double scrollbar and no white gap around the edges.
2. Nothing is cut off at the bottom at 780px.
3. Layout holds at 360px, 600px and full width, with no horizontal scroll.
4. Light theme inside the frame, even with the computer set to dark mode.
5. No console errors, and no network requests to any CDN.
6. Opened on its own (not framed), the page still works.

Take screenshots of the test at each width and show them to her.

## Final checklist

Report each item as done or not done:

- [ ] Original file kept unchanged
- [ ] Single self-contained file, no CDN, no charting library
- [ ] `html, body { margin: 0; padding: 0 }`
- [ ] Framing detected with `window.self !== window.top`
- [ ] Fixed 780px height when framed, nothing clipped
- [ ] Container queries, element measured, `ResizeObserver` redraw
- [ ] Light theme pinned when framed
- [ ] No `noindex`, no canonical to the article, title and description set
- [ ] "Built by Data 4 The People" and data source in the footer
- [ ] Every chart titled; legends as colored words; percentages, not multipliers
- [ ] Tested in a double iframe at 360px, 600px and full width
- [ ] Hosted under `Data4ThePeople` on GitHub Pages
- [ ] Iframe snippet provided, height matches the page
- [ ] Any wording changes proposed as a numbered list, applied only if accepted
