# DESIGN.md — cPanel Jupiter

Reusable design system for **tools that live inside a hosting panel**: database browsers, file managers, cron and mail editors — anything opened from a cPanel or WHM menu. The goal is not to be liked on its own. It is to look like it was always there. Swap the product; keep every token, rule, and guardrail.

**Posture.** This is cPanel 11's Jupiter theme, captured as tokens. Light-gray page, white cards ringed by hairlines, one deep-blue link/primary, one orange accent reserved for graphics, Open Sans. A tool that ships its own palette next to cPanel looks bolted on — even a good one.

- **Audience:** the account owner or sysadmin who arrived from a panel menu and expects cPanel, not a product.
- **Emotion:** continuity — _this belongs here_.
- **Leading words:** _card_ (the white surface) · _cap_ (a card's gray header bar) · _strip_ (the top bar the breadcrumb and logout sit in) · _primary_ (the deep blue) · _accent_ (the orange, graphics and hover highlight only).
- **Principles, in order:** look native over look distinctive · cards over full-bleed panels · one blue for action, one orange for graphics · reuse the host's type and spacing · legible defaults, no theming.

## 1. Brand Philosophy

**Look:** the panel's own chrome, extended — a gray canvas, white cards, hairline borders, a deep-blue link that never stops reading as cPanel. Nothing here is a design statement. If a visitor can tell your tool apart from the pages around it, you have added friction.

The whole system is one `:root` block. It exists so a second palette never enters the build. Derive every color, radius, and shadow from it.

## 2. Colors

```css
:root {
  /* page + surfaces */
  --cp-page: #fafafa; /* --cp-body-bg: canvas behind the cards */
  --cp-surface: #fff; /* --cp-component-bg: cards, inputs, popovers */
  --cp-cap: #f2f3f4; /* --cp-card-cap-bg: card headers, zebra rows */

  /* text */
  --cp-fg: #243746; /* --cp-body-color / --cp-gray-900 */
  --cp-muted: #6d7983; /* --cp-gray-600: captions, version, breadcrumb */

  /* lines */
  --cp-border: #c3c7cc; /* --cp-border-color / --cp-gray-300: tables, inputs */
  --cp-border-light: #e5e7e9; /* --cp-gray-200: card edges, dividers */

  /* action + accent */
  --cp-link: #003da6; /* --cp-link-color / --cp-primary */
  --cp-link-hover: #003185; /* --cp-link-hover-color */
  --cp-accent: #de5c2e; /* --cp-accent / --cp-graphic-color: graphics + hover highlight, never a primary action */
  --cp-danger: #dc3545; /* --cp-danger */

  /* alerts */
  --cp-alert-error: #d03f00;
  --cp-alert-error-bg: #f2dede;
  --cp-alert-success: #83b655;
  --cp-alert-success-bg: #dff2d5;

  /* shape + depth */
  --cp-radius: 0.25rem; /* --cp-border-radius: controls */
  --cp-radius-card: 0.125rem; /* cards sit tighter than controls */
  --cp-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075); /* .card-raised */
  --cp-shadow-float: 0 0 20px -3px rgba(0, 0, 0, 0.3); /* the floating menu under 800px */
  --cp-focus: 0 0 0 0.25rem rgba(0, 61, 166, 0.25); /* --cp-focus-ring-color */

  /* layout */
  --cp-strip: calc(
    2em + 10px
  ); /* top bar height, so card headers and the breadcrumb line up */
}
```

| Role          | Token                        | Applied to                                                              |
| ------------- | ---------------------------- | ----------------------------------------------------------------------- |
| Canvas        | `--cp-page`                  | `body` behind everything                                                |
| Surface       | `--cp-surface`               | cards, inputs, editor background, `#help`                               |
| Cap           | `--cp-cap`                   | card headers, alternating/odd table rows, disabled fields               |
| Text          | `--cp-fg`                    | body copy, headings, table cells, alert text                            |
| Muted         | `--cp-muted`                 | version, breadcrumb, disabled button text                               |
| Line          | `--cp-border`                | table and figure borders, input borders, `#help`, `#schema`             |
| Line-light    | `--cp-border-light`          | card edges, card-header underlines, section dividers, disabled field bg |
| Primary       | `--cp-link`                  | links, button borders/text, solid primary, focus ring, icon buttons     |
| Primary-hover | `--cp-link-hover`            | link hover, solid-primary hover                                         |
| Accent        | `--cp-accent`                | icon-button hover; graphic highlights only                              |
| Danger        | `--cp-danger`                | version label, plugin update flags                                      |
| Error         | `--cp-alert-error` / `-bg`   | `.error` / `.alert-danger` boxes                                        |
| Success       | `--cp-alert-success` / `-bg` | `.message` / `.alert-success` boxes                                     |

**One accent for action, one for graphics.** The orange (`--cp-accent`) is never a primary button, link, or selected state — it highlights graphics and the hover of an icon control. Every interactive blue is `--cp-link`. No arbitrary hexes (`#...`) outside this block.

**Tailwind bridge.** For a Tailwind build, alias each token once in `@theme` (`--color-page: var(--cp-page); --color-surface: …`) and let utilities read the custom properties. Do not re-declare literal hexes in the theme.

## 3. Typography

```css
--font-sans: "Open Sans", system-ui, -apple-system, "Segoe UI", sans-serif;
body {
  font: 95%/1.4 var(--font-sans);
}
```

| Element                  | Size / weight           | Notes                                |
| ------------------------ | ----------------------- | ------------------------------------ |
| Card header (`h1`, `h2`) | `1.25rem` / `600`       | on `--cp-cap`, padding `.5rem 1rem`  |
| Section (`h3`)           | `1.125rem` / `600`      | in-card headings                     |
| Body                     | `95%` (≈15.2px) / `400` | `line-height: 1.4`                   |
| Muted / version          | inherit, `--cp-muted`   | captions, breadcrumb, version string |
| Danger flags             | inherit, `--cp-danger`  | `#version`, plugin update counts     |

Open Sans only, and the 95% base with 1.4 leading — not 100%/1.5. Weights `400` and `600`; no `700`, no italics for emphasis. Monospace is reserved for values that are compared character-by-character (query text, IDs), never for prose.

## 4. Layout

**The card skeleton, always.** A gray page holds white cards separated by gutters — never a full-bleed panel.

- **Sidebar card** (`#menu`): fixed left, `margin: 10px 0 0 10px`, ~`20em` wide, grouped nav (a domain, then its tables/records). Dividers are `1px` `--cp-border-light`.
- **Content card** (`#content`): `margin: var(--cp-strip) 20px 20px 21em` — left margin clears the `20em` sidebar plus its gutter; `padding: 0 1rem 1rem`.
- **Top strip:** height `--cp-strip` (`2em` bar + the menu's `10px` offset). Breadcrumb, logout, and card headers all sit in this band so the two card headers line up. The breadcrumb has no background and is `--cp-muted`.
- **Inside a card:** paragraphs, tables, alerts, and the footer reset their right margin and re-bleed to the card padding.

**Density:** default body `95%`; inputs `.25rem .5rem`; buttons `.25rem .75rem`. Don't pad to breathe.

**RTL:** mirror every gutter and offset (`.rtl #menu` → `10px 10px 0 0`; `.rtl #content` → `var(--cp-strip) 21em 20px 20px`; `.rtl .logout` → `left: 20px`). The rules are directional, not symmetric.

**Breakpoints:** below `800px` the sidebar stops being a column and floats above the content (`--cp-shadow-float`, no offset). A `#menuopen` control takes the full `--cp-strip` height so it centres next to the breadcrumb, and content gutters collapse to `10px`. RTL keeps its gutter correction here too.

## 5. Elevation & Depth

**One shadow, and only on cards.** Panels are defined by border + surface first; the raised shadow (`--cp-shadow`) is the subtle separation Jupiter uses for `.card-raised`. It is not decoration.

- `--cp-shadow` — every card (`.card-raised`).
- `--cp-shadow-float` — **only** the sidebar once it floats over content under `800px`, where a hairline is too weak.
- `--cp-focus` — focus rings; the one other "shadow" in the system.

No drop shadows on buttons, inputs, or table rows. Hover is a color change, never a lift.

## 6. Shapes

```css
--cp-radius: 0.25rem; /* controls: buttons, inputs, selects, fieldsets */
--cp-radius-card: 0.125rem; /* cards, card headers, alerts — squared, panel-like */
```

Controls round to `.25rem`; cards to `.125rem`. No `rounded-full`, no pills — Jupiter is a panel, not a product. Card headers round only their top corners (`.125rem .125rem 0 0`). Checkboxes, radios, and file inputs carry no border and no padding.

## 7. Components

Everything below reads the tokens; a component that hardcodes a color is a bug.

```css
/* .card-raised */
.card {
  background: var(--cp-surface);
  border: 1px solid var(--cp-border-light);
  border-radius: var(--cp-radius-card);
  box-shadow: var(--cp-shadow);
}

/* .card-header — the cap */
.card-header {
  color: var(--cp-fg);
  font-size: 1.25rem;
  font-weight: 600;
  padding: 0.5rem 1rem;
  background: var(--cp-cap);
  border-bottom: 1px solid var(--cp-border-light);
  border-radius: var(--cp-radius-card) var(--cp-radius-card) 0 0;
}

/* .btn — outline control by default */
.btn {
  padding: 0.25rem 0.75rem;
  border: 1px solid var(--cp-link);
  border-radius: var(--cp-radius);
  background: var(--cp-surface);
  color: var(--cp-link);
  cursor: pointer;
}
.btn:hover {
  background: var(--cp-link);
  color: var(--cp-surface);
}

/* .btn-primary — one per view */
.btn-primary {
  background: var(--cp-link);
  color: var(--cp-surface);
}
.btn-primary:hover {
  background: var(--cp-link-hover);
  border-color: var(--cp-link-hover);
}

.btn:disabled {
  border-color: var(--cp-border);
  background: var(--cp-border-light);
  color: var(--cp-muted);
  cursor: not-allowed;
}

/* icon button — the only place accent appears on a control */
.icon {
  background: var(--cp-link);
}
.icon:hover {
  background: var(--cp-accent);
}
.icon:disabled,
.icon:disabled:hover {
  background: var(--cp-muted);
  cursor: not-allowed;
}

/* .form-control */
.field {
  border: 1px solid var(--cp-border);
  border-radius: var(--cp-radius);
  padding: 0.25rem 0.5rem;
}
.field:focus {
  border-color: var(--cp-link);
  outline: 0;
  box-shadow: var(--cp-focus);
}
.field:disabled {
  background: var(--cp-border-light);
}

/* .alert — boxed, bordered, no icon */
.alert-danger {
  border-color: var(--cp-alert-error);
  background: var(--cp-alert-error-bg);
  color: var(--cp-fg);
}
.alert-success {
  border-color: var(--cp-alert-success);
  background: var(--cp-alert-success-bg);
  color: var(--cp-fg);
}
.alert-danger,
.alert-success {
  border: 1px solid;
  border-radius: var(--cp-radius-card);
}
```

- **Cards.** Two per page, at most: the sidebar and the content. Both `.card`. Headers are the cap. The content card's `h2` bleeds to the edges (`margin: 0 -1rem 1rem`) so the cap runs the full width.
- **Buttons.** Outline (border + text in `--cp-link`, fill to `--cp-link` on hover) is the default. `.btn-primary` is solid `--cp-link` and appears **once** per screen — the highlighted default action. Hover darkens to `--cp-link-hover`. The disabled state is declared **after** hover so a disabled button never lights up. Icon buttons are the sole accent-bearing control: blue at rest, orange on hover, muted when disabled.
- **Fields.** Inputs, selects, textareas: `--cp-border`, `.25rem` radius, `.25rem .5rem`. Focus swaps the border to `--cp-link` and adds the ring. Disabled fills `--cp-border-light`. Checkbox/radio/file drop the border and padding. Fixed-width helpers (`size`, `type`, `options`) are `border-box`, so grow their `ex` width by `.75rem` to account for the padding.
- **Alerts.** `.error`/`.message` are bordered boxes at `--cp-fg` text with the error or success tone. Inside a card header-side list, restore `margin: .8em 1em; padding: .5em .8em` so the container's divider rule doesn't strip the box's bottom edge.
- **Tables.** Borders `--cp-border`; the second column of odd rows (`nth-child(2n)`) fills `--cp-cap`. Cells, headers, and `.column` all share the line token.
- **Chrome.** Breadcrumb and logout sit on `--cp-page` with no card — `--cp-muted` text, height `--cp-strip`, padding aligned to the content card. Links are `--cp-link`, hover `--cp-link-hover`; this includes `button.link`.

## 8. Do's and Don'ts

**Do** derive every value from the `:root` block · build pages from two cards (sidebar + content) on the gray canvas · keep one solid-primary action per screen · use outline buttons for everything else · put `--cp-link` on links, borders, focus, and selection · reserve `--cp-accent` for graphics and icon hover · keep `--cp-cap` for card headers and zebra rows · match card and breadcrumb to `--cp-strip` so headers align · use Open Sans at 95%/1.4 · declare disabled states after hover/active · mirror all gutters for RTL · test below `800px` where the sidebar floats and its shadow strengthens · alias the tokens once into your theme.

**Don't** ship a second chromatic palette, a second blue, or a dark mode — Jupiter is light and single-accent · hardcode a hex or px where a token exists · use `--cp-accent` for a primary button, link, or selected state · put more than one solid-primary button on a screen · add shadows to buttons, inputs, or rows · round controls with a pill/`rounded-full` · use `700`/italics for emphasis · give a card a full-bleed background · let the card header's `h2` sit inset from the cap · leave the breadcrumb or logout on a card · forget to re-bleed `p`/`table`/`.error`/`.message` inside `#content` · skip the RTL and `800px` variants.

**Write the positive, not the ban:** "reuse the host's tokens" (not "don't pick your own colors") · "cut the interface into two cards" (not "don't use full-bleed layouts") · "reserve orange for graphics" (not "don't use orange on buttons") · "align the headers to the strip" (not "don't let headers drift") · "declare disabled last" (not "don't light up disabled controls").

**One-line summary:** if a user can tell your tool from the cPanel pages around it, you have added friction — look native, not novel.
