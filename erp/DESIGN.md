# DESIGN.md — The ERP, i just need to get the shit done systematically (Tailwind)

Reusable design system for **operational software**: tools people run all day to move records, approve work, and not make mistakes. Swap the accent hue per project; keep every role, rule, and guardrail.

**Tailwind posture.** Tokens live once in `@theme`; utilities do layout and spacing; shared atoms (button, badge, field, table) live in `@layer components` so modules cannot drift into one-off Tailwind soup. Never hardcode a hex or px in a class — every value below is a token. The `@theme` block ports 1:1 into a v3 `theme.extend`.

**Boring on purpose.** Not a product to love — a tool that gets out of the way. Calm, dense, unremarkable; excellence is keystrokes saved and errors prevented, never delight.

- **Audience:** the repeat operator doing the same task for the 4,000th time this quarter.
- **Emotion:** quiet competence — _oriented_, not impressed.
- **Leading words:** _dense_ (default posture), _signal_ (color = status/link/primary only), _record_ (one entity), _module_ (a sidebar domain), _skeleton_ (the fixed page template), _queue_ (most-actionable first).
- **Principles, in order:** density over whitespace · tables are the primary UI · keyboard-first data entry · predictable beats clever · one way to do a thing · every screen states its actions in order.

## 1. Brand Philosophy

**Look:** a well-run ledger — white surfaces, hairline borders, one accent, more rows than expected. If a choice adds a click, a scroll, or a moment of "where is that button" — it loses. Predictability is the product; delight is not a goal.

## 2. Colors

```css
@theme {
  --color-brand-30: #0a4a82;
  --color-brand-60: #115ea3;
  --color-brand-70: #0f6cbd;
  --color-brand-90: #2886de;
  --color-brand-140: #b4d6fa;
  --color-brand-150: #cfe4fa;
  --color-brand-160: #eff6fc;
  --color-fg-1: #1b1b1b;
  --color-fg-2: #424242;
  --color-fg-3: #616161;
  --color-fg-4: #8a8a8a;
  --color-fg-disabled: #bdbdbd;
  --color-surface-1: #ffffff;
  --color-surface-2: #fafafa;
  --color-surface-3: #f3f3f3;
  --color-surface-4: #f0f0f0;
  --color-surface-5: #ebebeb;
  --color-surface-hover: #f5f5f5;
  --color-surface-selected: #eff6fc;
  --color-stroke-1: #d1d1d1;
  --color-stroke-2: #e0e0e0;
  --color-stroke-3: #ededed;
  --color-neutral-fg: #525252;
  --color-neutral-bg: #f0f0f0;
  --color-neutral-bd: #e0e0e0;
  --color-warning-fg: #835c00;
  --color-warning-bg: #fff4ce;
  --color-warning-bd: #f7e39b;
  --color-success-fg: #0e6b0e;
  --color-success-bg: #f1faf1;
  --color-success-bd: #dff6dd;
  --color-danger-fg: #b10e1c;
  --color-danger-bg: #fdf3f4;
  --color-danger-bd: #f1bbbc;
  --color-danger-hover: #8f0b16;
  --color-info-fg: #0f6cbd;
  --color-info-bg: #eff6fc;
  --color-info-bd: #cfe4fa;
}
```

**Accent** (`bg-brand-70`, `text-brand-70`, `border-brand-140`, `bg-brand-160` for selected/active): primary actions, links, active nav, focus, selection. Hover `bg-brand-60`; pressed `text-brand-30`.

**Secondary** = a neutral control: `bg-surface-1 border border-stroke-1 text-fg-1`. There is no second chromatic accent.

**Foreground:** `text-fg-1` primary · `text-fg-2` body · `text-fg-3` muted/caption/icon · `text-fg-4` placeholder · `text-fg-disabled`. **Surfaces:** `bg-surface-1` cards · `bg-surface-2` table header · `bg-surface-3` canvas · `bg-surface-4` inset · `bg-surface-hover` hover · `bg-surface-selected` selected row. **Strokes:** `border-stroke-1` inputs · `border-stroke-2` default · `border-stroke-3` row dividers.

**Semantic — five tones, reused for every entity; map new states onto a tone, never invent a sixth.**
| Tone | Text | Background | Border |
| --- | --- | --- | --- |
| Neutral | `text-neutral-fg` | `bg-neutral-bg` | `border-neutral-bd` |
| Warning | `text-warning-fg` | `bg-warning-bg` | `border-warning-bd` |
| Success | `text-success-fg` | `bg-success-bg` | `border-success-bd` |
| Danger | `text-danger-fg` | `bg-danger-bg` | `border-danger-bd` |
| Info | `text-info-fg` | `bg-info-bg` | `border-info-bd` |

Status is always a **badge carrying text**; color reinforces, never replaces, the label. No arbitrary color values (`bg-[#...]`).

## 3. Typography

```css
@theme {
  --font-sans:
    Inter, "Segoe UI", -apple-system, BlinkMacSystemFont, Roboto,
    "Helvetica Neue", Arial, sans-serif;
  --font-mono:
    SFMono-Regular, "Cascadia Mono", Consolas, "Liberation Mono", Menlo,
    monospace;
  --text-micro: 10px;
  --text-micro--line-height: 14px;
  --text-caption: 12px;
  --text-caption--line-height: 16px;
  --text-body-sm: 13px;
  --text-body-sm--line-height: 18px;
  --text-body: 14px;
  --text-body--line-height: 20px;
  --text-label: 12px;
  --text-label--line-height: 16px;
  --text-button: 13px;
  --text-button--line-height: 1;
  --text-section: 14px;
  --text-section--line-height: 20px;
  --text-subtitle: 16px;
  --text-subtitle--line-height: 22px;
  --text-title: 20px;
  --text-title--line-height: 26px;
}
```

| Utility                                                          | Use                                    |
| ---------------------------------------------------------------- | -------------------------------------- |
| `text-micro font-semibold uppercase tracking-[.05em]`            | Summary-field labels, nav group titles |
| `text-caption text-fg-3`                                         | Hints, timestamps, errors, help        |
| `text-body-sm text-fg-3`                                         | Subtitles, breadcrumbs                 |
| `text-body text-fg-2`                                            | Cells, inputs, prose                   |
| `text-label font-semibold`                                       | Form labels                            |
| `text-button font-semibold`                                      | Buttons/controls                       |
| `text-section font-semibold`                                     | Section headings                       |
| `text-subtitle font-semibold`                                    | Record sub-headings                    |
| `text-title font-semibold`                                       | Page titles, record name               |
| `text-[11px] font-semibold uppercase tracking-[.04em] text-fg-3` | Table headers                          |

**Weights:** `font-normal` and `font-semibold` only (`font-medium` for small tags). No `font-bold`, no italics for emphasis. Numerics and IDs use `font-mono tabular-nums`; dates are `YYYY-MM` / `YYYY-MM-DD`. Prose maxes ~`max-w-[80ch]`; truncate cells with `truncate` rather than wrapping.

## 4. Layout

**Spacing is Tailwind's default 4px scale — no extension.** Map the design names to utilities: `--s-2`→`1`, `--s-3`→`1.5`, `--s-4`→`2`, `--s-6`→`3`, `--s-8`→`4`, `--s-10`→`5`, `--s-12`→`6`, `--s-16`→`8` (e.g. `gap-2`, `p-3`, `px-5`, `py-8`).

**App shell:** `h-12` app bar (fixed) · `w-[232px]` sidebar (`w-12` collapsed rail) · `max-w-[1440px]` page · page padding `pt-4 px-5 pb-8`. `main` offsets by sidebar + app bar (`ml-[232px] pt-12`). Sidebar is persistent; below `lg` it becomes an off-canvas drawer (`fixed -translate-x-full`, toggled with `translate-x-0`).

**Density targets:** table header `h-[34px]`, body row `h-9`, line-item header `h-[30px]`, control `h-8` (`h-7` small), table scroll box `max-h-[calc(100vh-280px)]`.

**Grid:** content is a vertical stack of full-width panels. Summary fields: `grid grid-cols-[repeat(auto-fit,minmax(200px,1fr))] gap-px bg-stroke-2` with white cells (a dense record grid, not a card wall). Two columns only for asymmetric media+info. **Forms are `grid-cols-1`, always.**

**Breakpoints:** `lg` (1024) drawer + search collapses · `md` (768) tighter padding, breadcrumbs hide, header stacks · `sm` (640) / `[480px]` wordmark hides, summary grid one column, actions fill width.

## 5. Elevation & Depth

**Flat by default.** Panels, tables, cards, inputs, sidebar, and app bar use `border border-stroke-2 bg-surface-1` — **no shadow**. Hierarchy comes from surface, border, and type.

```css
@theme {
  --shadow-overlay: 0 0 2px rgb(0 0 0/0.12), 0 1px 2px rgb(0 0 0/0.14);
  --shadow-elevated: 0 0 2px rgb(0 0 0/0.12), 0 4px 8px rgb(0 0 0/0.14);
  --shadow-modal: 0 0 2px rgb(0 0 0/0.12), 0 8px 16px rgb(0 0 0/0.16);
}
```

Shadows only for genuinely floating overlays: `shadow-elevated` toasts/popovers, `shadow-modal` dialogs/palette/drawer. Never `shadow-sm`/`shadow-lg` on a panel — if a surface wants a shadow, ask whether a border would do.

**Layering (`z-*`):** app bar `z-40` · sidebar `z-30` · drawer backdrop `z-20` · toast `z-50` · dialog `z-[60]` · palette `z-[70]`. Keep overlays above chrome in that order.

## 6. Shapes

```css
@theme {
  --radius-sm: 2px;
  --radius-md: 4px;
  --radius-lg: 8px;
}
```

`rounded-sm` skeleton bars, sort arrows, kbd · `rounded-md` **default** — buttons, inputs, selects, panels, tables, tags, toasts · `rounded-lg` dialogs, palette, large media · `rounded-full` status badges, chips, avatar only.

Buttons and inputs share `rounded-md` — never `rounded-full` buttons. Badges are pills with a `size-1.5 rounded-full bg-current` dot when live/active. Tags stay `rounded-md` rectangles; the pill-vs-rectangle split is status vs. metadata.

## 7. Components

**State rules (compose into every atom).** Hover: `hover:bg-surface-hover`, or one accent step `hover:bg-brand-60`; never translate/scale/shadow. Focus: `focus-visible:outline-2 focus-visible:outline-offset-1 focus-visible:outline-brand-70` (inside rows `-outline-offset-2`); fields use `focus:border-brand-70 focus:ring-1 focus:ring-brand-70`. Active: `active:bg-brand-30` or `active:bg-surface-4`. Disabled: `disabled:opacity-50 disabled:cursor-not-allowed`. Loading: a **skeleton matching real column widths** — never a `animate-spin` circle. Error: inline beside the field, `text-danger-fg`, saying what failed and what to do.

Shared atoms belong in `@layer components`; call sites add layout utilities only.

```css
@layer components {
  .btn {
    @apply inline-flex h-8 items-center justify-center gap-2 rounded-md px-3 text-button font-semibold transition-colors focus-visible:outline-2 focus-visible:outline-offset-1 focus-visible:outline-brand-70 disabled:opacity-50 disabled:cursor-not-allowed;
  }
  .btn-primary {
    @apply bg-brand-70 text-white hover:bg-brand-60;
  }
  .btn-secondary {
    @apply border border-stroke-1 bg-surface-1 text-fg-1 hover:bg-surface-hover hover:border-fg-4;
  }
  .btn-ghost {
    @apply text-fg-2 hover:bg-surface-hover;
  }
  .btn-danger {
    @apply bg-danger-fg text-white hover:bg-danger-hover;
  }
  .field {
    @apply h-8 w-full rounded-md border border-stroke-1 bg-surface-1 px-2 text-body text-fg-1 focus:border-brand-70 focus:ring-1 focus:ring-brand-70;
  }
  .badge {
    @apply inline-flex h-5 items-center gap-1 rounded-full border px-2 text-[11px] font-semibold leading-none;
  }
  .tag {
    @apply inline-flex h-5 items-center rounded-md border border-stroke-2 bg-surface-2 px-2 text-[11px] font-medium text-fg-2;
  }
  .panel {
    @apply rounded-md border border-stroke-2 bg-surface-1;
  }
}
```

**Buttons.** Primary `btn-primary` (**one per screen**) · secondary `btn-secondary` (the default) · ghost for cancel/tertiary · `btn-danger` never adjacent to primary (use a danger-outline for low emphasis). Labels are verbs; icons (`size-3`) accompany a label, never replace it on a primary action.

**Fields.** `field` for input/select; textarea `min-h-32 py-2`. Validate on blur (touched fields); validate all on submit and focus the first invalid. Required `*` is announced; errors carry `role="alert" aria-live="polite"`.

**Data table (the actual product).** `panel overflow-auto max-h-[calc(100vh-280px)]`; header `sticky top-0 z-10 bg-surface-2 h-[34px] text-[11px] font-semibold uppercase tracking-[.04em] text-fg-3`, sort by **clicking the header** (no dropdown; active column sets `aria-sort` + a brand arrow). Rows `h-9 border-b border-stroke-3 hover:bg-surface-hover`; selected `bg-surface-selected`. **Row click opens the detail** (no view icon). Numerics right-aligned `font-mono tabular-nums`; long text `truncate`. Toolbar directly above holds inline `field` search + filter selects — **filters are never a modal/drawer**. Bulk checkbox column is leftmost; the **bulk bar renders only when rows are selected**. Empty state = one line + one action. Footer = count + page size (10/25/50) + pagination.

**Page skeletons — every module reuses these three; never invent a new one.** _List:_ breadcrumb → title + count + top-right primary action → toolbar → table → footer. _Detail:_ breadcrumb → title + status badge + actions → dense **summary grid** → sections (line items / history / attachments) → **activity log collapsed** at the bottom. _Form:_ breadcrumb → title → single-column grouped sections → **sticky footer** (`sticky bottom-0`) with Cancel | Save Draft | Submit.

| Support         | Tailwind anchor                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| Summary grid    | `grid grid-cols-[repeat(auto-fit,minmax(200px,1fr))] gap-px bg-stroke-2` + micro-label/value cells    |
| Line-item table | `border border-stroke-2 rounded-md text-body`; numbered first col; read-only once approved            |
| Activity log    | `<details>` collapsed; `grid grid-cols-[148px_1fr]` with mono timestamps                              |
| Panel           | `.panel`; header `border-b border-stroke-2 px-3 py-2` title + count; body `p-3`                       |
| Toolbar         | `flex flex-wrap items-center gap-2 border-b border-stroke-2 px-3 py-2`; `ml-auto` for actions         |
| Breadcrumb      | `flex items-center gap-1.5 text-body-sm text-fg-3`; last item `text-fg-1 font-semibold aria-current`  |
| Toast           | `fixed bottom-4 right-4 z-50 shadow-elevated border-l-4 rounded-md`, auto-dismiss ~4.2s               |
| Dialog          | destructive/irreversible only; `max-w-[440px] rounded-lg shadow-modal bg-black/40` backdrop           |
| Search palette  | `Ctrl/Cmd+K` or `/`; single input, grouped results, `ArrowUp/Down`, `Enter`, `Esc`                    |
| Empty state     | `text-body text-fg-2` sentence + one `.btn-secondary` action                                          |
| Skeleton        | `animate-pulse rounded-sm bg-surface-4` blocks matching real column widths                            |
| Sidebar         | grouped by module, noun labels, two levels max; active `bg-brand-160 text-brand-30` + accent bar      |
| App bar         | `h-12 border-b border-stroke-2` launcher, wordmark, breadcrumb, centered search, icon buttons, avatar |

**Interaction contract.** Tab order follows visual order (test it — it breaks constantly). Enter/Space activate a focused row, header, or control. Enter on the last editable line adds a line and moves focus. No auto-refresh that reorders rows under the cursor. Success is a toast, never a modal. Confirmation dialogs only for destructive/irreversible actions; routine saves never ask "Are you sure?".

## 8. Do's and Don'ts

**Do** put status in the leftmost column as a text badge · default-sort lists to the **most actionable first** · reuse the five status tones · make every page answer _where am I, what is the state, what can I do next_ · keep one verb-labelled primary action per screen · show hover on the surface and focus as a visible ring · use `sticky top-0` headers, `tabular-nums`, and `YYYY-MM` dates · show a skeleton table while loading and a one-line empty state with one action · keep forms `grid-cols-1` with on-blur validation and a sticky footer · keep everything reachable by keyboard · reuse the three page skeletons across all modules · compose shared atoms in `@layer components`.

**Don't** use cards/tiles/charts where the data is tabular · add gradients, heroes, illustrations, decorative shadows, or extra corners · introduce a second accent or color anything that isn't status/link/primary · use arbitrary colors (`bg-[#...]`) or one-off px (`p-[13px]`) where a token exists · put filters in a modal/drawer or sort behind a dropdown · use icon-only buttons for primary actions or hide actions in overflow menus · build a wizard for a one-page form or nest a modal in a modal · animate beyond ~150ms opacity/transform (no bounce, confetti, or page choreography) · add onboarding tours, first-load tooltips, welcome modals, streaks, user badges, or completeness meters · auto-refresh a table mid-scan · use `animate-spin` as a loading state · ask "Are you sure?" for routine saves · mutate an approved record in place (changes go through a revision) · reintroduce a display font, `shadow-sm` on a panel, or a `rounded-full` button.

**Write the positive, not the ban:** "increase density" (not "reduce whitespace") · "place the primary action top-right, always visible" (not "don't hide actions") · "reserve color for status, links, primary actions" (not "don't use decorative color") · "use a skeleton table" (not "don't use a spinner") · "optimize the fastest, least error-prone path from intent to done" (not "don't add prettiness").

**One-line summary:** if a screen looks impressive in a portfolio screenshot but slows down the person doing the same task for the 4,000th time this quarter, redesign it.
