# CLAUDE.md

Context for Claude Code working in this repo.

## What this is

A single static page for the band Mivim, served from GitHub Pages at
`mivim.net`. The entire site is `index.html`: markup, CSS, and inline SVG in
one file. There is no build step, no package.json, no framework, and none
should be added.

## Hard constraints

- **No build tooling.** No bundler, no npm, no Tailwind, no framework. If a
  change seems to need a build step, the change is wrong.
- **Single file.** CSS stays in the `<style>` block in `index.html`.
- **No JavaScript** unless a feature genuinely cannot work without it. The page
  ships zero JS today and should stay that way.
- **No external requests.** No Google Fonts, no CDNs, no analytics, no
  third-party icon libraries. Everything is served from this repo.
- **Do not modify `CNAME`.** It holds `mivim.net` and controls the custom
  domain. Deleting it silently unpublishes the domain.
- **Do not overwrite files in `images/`.** They are derived from source art
  that does not live in this repo.

## Layer model

The background plates carry no type. The wordmark is a separate transparent
image layered on top and positioned in percentages. This is deliberate: it
means the plate can crop at any aspect ratio without cutting letters. Do not
merge them, and do not add `object-fit: contain` to the plate.

- Landscape: `plate-wide` (3:1), wordmark at `top: 33%`, `min(46vw, 760px)`.
- Portrait: `plate-tall` (2:3), wordmark at `top: 30%`, `min(76vw, 520px)`.

The swap runs on `(orientation: portrait)` in both the `<picture>` element and
the CSS. Change one and you must change the other.

## Design intent

A moonlit marsh, quiet and dark. Restraint over decoration.

- Colors come from CSS custom properties in `:root`, sampled from the artwork.
  Use those variables. Do not introduce new hex values without a reason.
- Social icons are hand-drawn to a uniform 1.25 stroke weight on a 24x24
  viewBox so they read as one set and match the thin lettering. Do not swap in
  official brand marks; they have mismatched weights and filled shapes.
- Motion budget is spent in four places and nowhere else: a one-time arrival
  sequence (plate, then wordmark, then links), the slow wax-and-wane on live
  links, the leaf shimmer, and the date. Do not add more.

## The date

`.signal` holds the first show date, 2026-10-17, marked up as a real `<time>`
element with an `aria-label` carrying the full date. It is meant to be found,
not read at a glance: it surfaces on a 31s cycle, catches, slips, holds, and
goes. 31 is coprime with the shimmer's 13 and 23, so the three never repeat
together.

It is animated with `opacity`, never `display` or `visibility`, so it stays in
the accessibility tree and in the DOM at all times. Under
`prefers-reduced-motion` it holds steady at 0.4 rather than disappearing.

When the show is announced properly this becomes a link to tickets or a venue,
and the cycle should be retired. A date with no venue is a tease with a shelf
life.

## The leaf shimmer

`images/wordmark-leaf.png` is the leaf V isolated from the wordmark on the same
1200x360 canvas, with feathered edges. It stacks on top of the full wordmark at
`inset: 0` and is revealed through a narrow moving gradient mask, brightened and
screen-blended, so light appears to travel across the leaf.

Two animations run at different periods on purpose: `sweep` at 13s (the sweep
itself occupies only the first 24% of the cycle; the rest parks the mask
offscreen) and `glimmer` at 23s modulating opacity. Coprime periods mean the
sweeps land at varying brightness and the full pattern takes 299s to repeat. Do
not "simplify" these to matching durations, and do not replace them with
JavaScript randomness.

The glint is `display: none` by default and only enabled inside an `@supports`
check for `mask-image`. Without that guard, browsers lacking mask support would
render a permanently blown-out leaf. Selectors use `img.wordmark__glint`
rather than `.wordmark__glint` because `.wordmark img` would otherwise win on
specificity and force it visible.

## Quality floor for any change

- Works with JS disabled.
- Keyboard focus visible on every interactive element.
- `prefers-reduced-motion: reduce` kills all animation.
- Wordmark fully visible and uncropped at 375x667 and at 3440x1440.
- No horizontal scrollbar at any width.

## Preview

```bash
python3 -m http.server 8000
```

## Likely tasks

- Turning unlit icons into live links. Three edits: add `href`, remove
  `aria-disabled="true"`, shorten the `aria-label`. See README.
- Adding a mailing list capture or a contact link. Ask before adding anything
  that requires a third-party script.
