# DESIGN.md: Windows 98

Reusable design system for compact admin tools that deliberately use the Windows 98 desktop visual language. Derived from the supplied Adminer stylesheet and inspired by [98.css](https://github.com/jdan/98.css). The CSS tokens and component rules below can be used without Adminer or a specific framework.

**Visual direction.** Teal desktop, silver application panels, square controls, nested inset bevels, and a navy-to-blue title bar. Raised controls look raised; fields look sunken; pressing a button reverses its bevel immediately.

- **Audience:** people using a utility that intentionally presents itself as a classic desktop application.
- **Identity:** tactile, compact, unmistakably late-1990s desktop chrome.
- **Design read:** a compact admin utility for administrators, in the Windows 98 desktop visual language.
- **Dials:** ENERGY 3 / RHYTHM 1 / MOTION 1. The palette and bevels carry the character; screens stay orderly; feedback is immediate rather than animated.
- **Principles:** preserve the light direction of each bevel · reserve the gradient for title bars · keep content on legible silver or white surfaces · show keyboard focus · keep controls square.

## 1. Visual Identity

The desktop is teal. Application panels and the menu are silver with a four-edge inset bevel. Buttons use a raised bevel, fields use a sunken bevel, and table headers use the raised button treatment. A title bar uses a horizontal navy-to-blue gradient with white text.

The bevel is the elevation system. Do not replace it with soft drop shadows. The title bar identifies the active window; the default action gets the deeper bevel. The title-bar gradient does not become a general-purpose page background or button fill.

## 2. Colors and Tokens

Namespaced `--win98-*` tokens are canonical for reuse. The five original Adminer custom-property names remain as aliases at the end of the block. CSS named colors from the source are expanded here so projects can copy the palette consistently.

```css
:root {
  /* Namespaced tokens */
  --win98-text-color: #222222;
  --win98-button-face: #dfdfdf;
  --win98-window-frame: #0a0a0a;
  --win98-dialog-blue-light: #1084d0;
  --win98-link-blue: #0000ff;

  /* Named colors used by the source stylesheet */
  --win98-desktop-teal: #008080;
  --win98-surface-silver: #c0c0c0;
  --win98-bevel-gray: #808080;
  --win98-highlight-white: #ffffff;
  --win98-focus-black: #000000;
  --win98-title-navy: #000080;

  /* AA title-bar endpoint for normal white text */
  --win98-dialog-blue-title-aa: #0f6fae;

  /* Shared bevel recipes */
  --win98-bevel-control-raised:
    inset -1px -1px var(--win98-window-frame),
    inset 1px 1px var(--win98-highlight-white),
    inset -2px -2px var(--win98-bevel-gray),
    inset 2px 2px var(--win98-button-face);
  --win98-bevel-panel-raised:
    inset -1px -1px var(--win98-window-frame),
    inset 1px 1px var(--win98-button-face),
    inset -2px -2px var(--win98-bevel-gray),
    inset 2px 2px var(--win98-highlight-white);
  --win98-bevel-field-sunken:
    inset -1px -1px var(--win98-highlight-white),
    inset 1px 1px var(--win98-bevel-gray),
    inset -2px -2px var(--win98-button-face),
    inset 2px 2px var(--win98-window-frame);
  --win98-bevel-button-default:
    inset -2px -2px var(--win98-window-frame),
    inset 1px 1px var(--win98-window-frame),
    inset 2px 2px var(--win98-highlight-white),
    inset -3px -3px var(--win98-bevel-gray),
    inset 3px 3px var(--win98-button-face);
  --win98-bevel-button-pressed:
    inset -1px -1px var(--win98-highlight-white),
    inset 1px 1px var(--win98-window-frame),
    inset -2px -2px var(--win98-button-face),
    inset 2px 2px var(--win98-bevel-gray);
  --win98-bevel-button-default-pressed:
    inset 2px 2px var(--win98-window-frame),
    inset -1px -1px var(--win98-window-frame),
    inset -2px -2px var(--win98-highlight-white),
    inset 3px 3px var(--win98-bevel-gray),
    inset -3px -3px var(--win98-button-face);

  /* Type and geometry */
  --win98-font-sans: Arial, Helvetica, sans-serif;
  --win98-button-font-size: 12px;
  --win98-button-min-height: 23px;
  --win98-button-min-width: 75px;
  --win98-field-min-height: 21px;
  --win98-panel-padding: 16px;
  --win98-edge-image: url("data:image/svg+xml;charset=utf-8,%3Csvg width='5' height='5' fill='gray' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M0 0h5v5H0V2h2v1h1V2H0' fill='%23fff'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M0 0h4v4H0V1h1v2h2V1H0'/%3E%3C/svg%3E");
  --win98-select-arrow: url("data:image/svg+xml;charset=utf-8,%3Csvg width='16' height='17' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M15 0H0v16h1V1h14V0z' fill='%23DFDFDF'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M2 1H1v14h1V2h12V1H2z' fill='%23fff'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M16 17H0v-1h15V0h1v17z' fill='%23000'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M15 1h-1v14H1v1h14V1z' fill='gray'/%3E%3Cpath fill='silver' d='M2 2h12v13H2z'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M11 6H4v1h1v1h1v1h1v1h1V9h1V8h1V7h1V6z' fill='%23000'/%3E%3C/svg%3E");
  --win98-select-arrow-pressed: url("data:image/svg+xml;charset=utf-8,%3Csvg width='16' height='17' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M0 0h16v17H0V0zm1 16h14V1H1v15z' fill='gray'/%3E%3Cpath fill='silver' d='M1 1h14v15H1z'/%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M12 7H5v1h1v1h1v1h1v1h1v-1h1V9h1V8h1V7z' fill='%23000'/%3E%3C/svg%3E");

  /* Original Adminer aliases */
  --text-color: var(--win98-text-color);
  --button-face: var(--win98-button-face);
  --window-frame: var(--win98-window-frame);
  --dialog-blue-light: var(--win98-dialog-blue-light);
  --link-blue: var(--win98-link-blue);
}
```

| Role | Token | Use |
| --- | --- | --- |
| Desktop | `--win98-desktop-teal` | Workspace behind application panels |
| Panel | `--win98-surface-silver` | Menus, window panels, fieldset legends, disabled fields |
| Button face | `--win98-button-face` | Inner highlight band in raised and sunken edges |
| Text | `--win98-text-color` | Text on silver and white surfaces |
| Raised edge | `--win98-window-frame`, `--win98-bevel-gray`, `--win98-highlight-white` | Control and panel bevels |
| Title bar | `--win98-title-navy`, `--win98-dialog-blue-light` | Original title-bar gradient |
| Accessible title bar | `--win98-dialog-blue-title-aa` | Darker gradient endpoint for normal white title text |
| Link | `--win98-link-blue` | Links on silver or white surfaces |
| Field | `--win98-bevel-field-sunken` | White editable inputs, selects, and textareas |

The source's white title text over `#1084d0` has a 4.01:1 contrast ratio, below 4.5:1 for normal text. Use `--win98-dialog-blue-title-aa` as the gradient endpoint when the title remains normal-sized; its white-text ratio is 5.38:1. Keep `--win98-dialog-blue-light` for exact source reproduction only when the text treatment separately meets the required contrast.

The source also sets `#222222` body text over teal. That pairing is 3.33:1. Keep ordinary text inside silver or white panels. For labels placed directly on teal, use white text; white on `#008080` is 4.77:1. The blue link token is intended for silver or white surfaces, not teal.

## 3. Typography

```css
body {
  background: var(--win98-desktop-teal);
  color: var(--win98-highlight-white);
  font-family: var(--win98-font-sans);
}
button, input[type="reset"], input[type="submit"] { font-size: var(--win98-button-font-size); }
.window-titlebar { font-size: 120%; font-weight: 400; }
```

The source sets the font family but leaves the global font size to Adminer's base styles. Preserve the consuming app's base size. Buttons use `12px`; the window title uses `120%` and normal weight. Keep the default text treatment compact and plain. Do not add a display face or wide-tracked labels.

For standalone reuse, set the desktop text to white and reset panel text to `--win98-text-color`. This keeps labels on teal readable while keeping ordinary copy dark on silver and white.

## 4. Layout

The stylesheet provides Adminer-specific shell offsets. Keep those measurements attached to their selectors when embedding the design in Adminer; other applications can reuse the same panel, title-bar, and control geometry inside their own shell.

| Area | Source rule | Reusable meaning |
| --- | --- | --- |
| Content, menu, footer, breadcrumb base rule | `padding: 16px` with raised panel bevel | Silver application surfaces; breadcrumb overrides vertical padding below |
| Content | `margin-top: 3em` | Clears the top chrome |
| Menu | `top: 3em; width: 18em` | Fixed-width navigation panel |
| Window title | `margin: -13px -13px 16px; padding: 3px 2px 3px 3px` | Title bar reaches the panel's inner edge |
| Fieldset | `padding: 10px; padding-block-start: 8px` | Compact group with a patterned edge |
| Preformatted content | `padding: 12px 8px` | Inset query or code surface |
| Breadcrumb | `padding: 2px 16px; top: calc(1.5em - 14px)` | Shallow silver strip above the content |
| Footer | `margin: 1em 0` | Separate panel below content |

The Adminer menu is `18em` wide. Its links are block-level. The menu heading has no fill, margin, or padding, and its link uses `--win98-text-color`. The language selector sits at `top: -2.8em`; its label is white over teal and returns to the inherited color below `800px`.

At `max-width: 800px`, the source moves the JavaScript-enabled logout control to `top: 0` and changes the language label color. It does not define a menu collapse or a content-column reflow. A consuming app must provide its own narrow-screen shell behavior and verify that panels and tables fit.

## 5. Edges and Depth

All depth comes from inset edge layers:

- **Raised controls:** `--win98-bevel-control-raised`, with dark lower/right edges and white upper/left highlights.
- **Raised panels:** `--win98-bevel-panel-raised`, the panel-specific ordering used by content, menu, breadcrumb, footer, and JavaScript columns.
- **Sunken fields:** `--win98-bevel-field-sunken`, with the light and dark edges reversed around a white surface.
- **Default action:** `--win98-bevel-button-default`, an extra inner bevel that distinguishes the default button.
- **Pressed action:** `--win98-bevel-button-pressed` or `--win98-bevel-button-default-pressed`, reversing the raised edges.

There are no ambient drop shadows in this system. The logout control is explicitly shadowless. Keep the bevels sharp and the panel edges visible against teal.

## 6. Shapes and Surfaces

Every control is square: `border-radius: 0`. Buttons and panels use the raised edge; editable fields and text areas use the sunken edge. Fieldsets and `pre` blocks use the source's 5-by-5 white-and-gray SVG border tile with a slice of `2`. The exact edge tile and both select-arrow variants are stored once in namespaced custom properties above.

The select arrow is a small 16-by-17 SVG placed at the top-right, with a second arrow variant for the pressed state. Keep both variants together in the shared select style. Text inputs and selects remove native appearance so the bevel and arrow remain consistent across browsers.

## 7. Components

The following class names are framework-neutral examples. Adminer selectors can map to the same recipes.

```css
.win98-panel {
  padding: var(--win98-panel-padding);
  background: var(--win98-surface-silver);
  box-shadow: var(--win98-bevel-panel-raised);
  color: var(--win98-text-color);
}

.win98-titlebar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: -13px -13px 16px;
  padding: 3px 2px 3px 3px;
  background: linear-gradient(90deg, var(--win98-title-navy), var(--win98-dialog-blue-title-aa));
  color: var(--win98-highlight-white);
  font-size: 120%;
  font-weight: 400;
  letter-spacing: 0;
}

.win98-button {
  box-sizing: border-box;
  min-width: var(--win98-button-min-width);
  min-height: var(--win98-button-min-height);
  padding: 0 12px;
  border: 0;
  border-radius: 0;
  background: var(--win98-surface-silver);
  box-shadow: var(--win98-bevel-control-raised);
  color: transparent;
  font: var(--win98-button-font-size) var(--win98-font-sans);
  text-shadow: 0 0 var(--win98-text-color);
}

.win98-button--default { box-shadow: var(--win98-bevel-button-default); }

.win98-button:not(:disabled):active {
  box-shadow: var(--win98-bevel-button-pressed);
  text-shadow: 1px 1px var(--win98-text-color);
}

.win98-button--default:not(:disabled):active {
  box-shadow: var(--win98-bevel-button-default-pressed);
}

.win98-button:focus-visible,
.win98-field:focus-visible {
  outline: 1px dotted var(--win98-focus-black);
  outline-offset: -4px;
}

a:focus-visible { outline: 1px dotted var(--win98-link-blue); }

.win98-field {
  box-sizing: border-box;
  min-height: var(--win98-field-min-height);
  padding: 3px 4px;
  border: 0;
  border-radius: 0;
  background: var(--win98-highlight-white);
  box-shadow: var(--win98-bevel-field-sunken);
  color: var(--win98-text-color);
}

.win98-field:disabled,
.win98-field:read-only { background: var(--win98-surface-silver); }

:disabled,
:disabled + label,
input[readonly],
input[readonly] + label { color: var(--win98-bevel-gray); }

:disabled + label,
button:disabled,
input[type="reset"]:disabled,
input[type="submit"]:disabled {
  text-shadow: 1px 1px 0 var(--win98-highlight-white);
}

select.win98-field:focus-visible {
  background: var(--win98-title-navy);
  color: var(--win98-highlight-white);
}

select.win98-field:focus-visible option {
  background: var(--win98-highlight-white);
  color: var(--win98-focus-black);
}

.win98-table { background: var(--win98-highlight-white); }
.win98-table thead th {
  background: var(--win98-surface-silver);
  box-shadow: var(--win98-bevel-control-raised);
}

.win98-select {
  appearance: none;
  background-image: var(--win98-select-arrow);
  background-position: top 2px right 2px;
  background-repeat: no-repeat;
  padding-right: 32px;
}

.win98-select:active { background-image: var(--win98-select-arrow-pressed); }

.win98-fieldset {
  border-image: var(--win98-edge-image) 2;
  margin: 0;
  padding: 10px;
  padding-block-start: 8px;
}

.win98-pre {
  border-image: var(--win98-edge-image) 2;
  border-style: solid;
  border-width: 1px;
  margin: 0;
  padding: 12px 8px;
}
```

- **Buttons:** minimum `75px` wide and `23px` tall, with `0 12px` padding. The default button gets the deeper double bevel. Pressing a button reverses the edge; disabled buttons do not enter the pressed state. The original uses transparent text plus a zero-offset text shadow to render labels in the classic style.
- **Focus:** buttons use a `1px dotted black` outline inset by `4px`; links use a dotted outline in `--win98-link-blue`. The source removes outlines from text fields, selects, and textareas. A reusable implementation must restore a visible dotted focus outline on those fields rather than ship them without focus feedback.
- **Disabled and read-only fields:** text turns gray; fields fill with silver. Disabled labels receive a white text shadow to retain the raised-label effect.
- **Inputs and textareas:** white, square, sunken, and padded `3px 4px`. Text-like inputs and selects have a minimum height of `21px`. Search fields hide browser clear, reveal, and decoration controls. Checkboxes, radios, and file inputs are not included in the source's custom field reset, so retain their native behavior unless the consuming app supplies a complete accessible replacement.
- **Select:** use a custom arrow asset at `top 2px right 2px`, reserve `32px` on the right, and switch the field to navy with white text while focused. Options remain white with black text. Keep a visible focus outline as well as the color change.
- **Fieldsets and code:** fieldsets use the patterned 5-by-5 SVG edge, no margin, and the compact padding above. `pre` uses the same edge with a solid 1px border. Inline `code` has no added background.
- **Tables:** the table body is white; header cells are silver and raised. Interactive rows use a pointer cursor, but also need a real link or keyboard-operable action. The source does not define row hover color.
- **Alerts:** this stylesheet does not define error or success colors. Reuse the host application's semantic alert styles instead of deriving new status colors from the desktop palette.
- **Adminer chrome:** `.logout` has no shadow and `margin-top: .8em`; `.links` uses `1em 0`; `.js .column` uses the raised panel bevel, `margin-top: -.4em`, and `padding: 4px`; `.icon` drops the button minimum dimensions. Login/table links, autocomplete surfaces, and footer child surfaces stay transparent. Autocomplete height is `auto`. Sort, search, and select fieldset groups space adjacent rows by `.5em`; partition fieldset paragraphs have no margin.

## 8. Interaction and Accessibility

- Use the raised bevel for rest, the reversed bevel for press, and the deeper bevel for the default action. Keep these states immediate; the reference has no animation.
- Keep the dotted focus indicator visible for every keyboard-operable element. The source does not provide focus styling for input fields, so add it as part of the reusable system.
- Do not use color as the only status signal. Pair success, error, and selection with a label or other readable cue.
- Use semantic links and buttons for interactive table rows. A pointer cursor alone does not make a row operable by keyboard.
- Keep ordinary copy on silver or white surfaces. Use white for text directly over the teal desktop. Use a darker title-bar gradient endpoint when white title text is normal-sized.
- Check text contrast and focus visibility after any palette change. The source's `#222222` on teal and white on `#1084d0` do not meet normal-text AA.

## 9. Do's and Don'ts

**Do** use teal for the desktop field and silver for application surfaces · keep panels and controls square · use the exact raised, sunken, default, and pressed bevel recipes · limit the gradient to the title bar · keep editable fields white and sunken · preserve the custom select arrow and its pressed variant · add dotted keyboard focus to fields · keep ordinary copy on silver or white · use the Adminer measurements only with the Adminer shell · provide the missing narrow-screen reflow in other applications.

**Don't** add rounded corners, soft outer shadows, glass effects, or unrelated gradients · flatten the bevels into plain borders · use `outline: none` without a visible replacement · put dark body text or blue links directly on teal · use the original light-blue title stop for normal white text without fixing its contrast · use pointer-only table rows · invent alert colors that the source does not specify · assume the `800px` media query provides a complete mobile layout.

**Summary:** use square silver surfaces and directional bevels to make the interface feel like a classic desktop utility, while keeping text readable and every state visible.
