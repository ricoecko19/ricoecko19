# Design system and copy limits

Descriptive reference for the carousel design system.

## Palette

| Role | Share | Applied to |
|---|---|---|
| Dominant dark | ~60% | Page backgrounds, panels, dark cards |
| Light neutral | ~30% | Headlines, body text, light cards |
| Single accent | ~10% | Logo mark, one numeral, rules, labels, the connecting trace, the URL |

The accent never fills a background and carries one idea per page. Supporting tints
of the accent are used for eyebrows, times, and the footer URL. No fourth hue is
introduced.

## Page roles

| Page | Background | Contents |
|---|---|---|
| 01 | Dark | Organization name, title, one-line promise, swipe cue |
| 02 | Dark | Headline, three statistics with labels, source line |
| 03–07 | Light | Progress bar, step label, title, meta line, why line, DO card, DON'T card, footer |
| 08 | Dark | Recap of the five items with values, question to the reader, closing line |

## Step-page geometry

Identical across pages 03–07.

| Element | Specification |
|---|---|
| Progress bar | Five segments, top of page; active segment in the accent, inactive in a dark neutral |
| Step label | Small, bold, accent tint |
| Title | Large, bold, light neutral |
| Meta line | Small, muted |
| Why line | Single line, supporting body colour |
| DO card | Light fill, dark body text, accent label |
| DON'T card | Dark fill with a stroke, muted label, light body text |
| Footer | Page number and site URL, muted |

## The connecting trace

A single accent line crosses every page from left edge to right edge. Each page's
exit height equals the next page's entry height, so the line reads as continuous
when the carousel is swiped. A brighter node dot marks each bend. The final page
terminates the line in a larger node.

Entry and exit heights are fixed per page in the template and are preserved when the
template is copied.

## Copy-length limits

Observed limits. Exceeding one causes wrapping that collides with the element below;
the layout does not reflow.

| Element | Limit |
|---|---|
| Step-page title (52px) | ~30 characters |
| Recap headline (68px) | ~22 characters |
| Recap call to action (42px) | ~35 characters |
| Why line | One line at the rendered width |
| DO / DON'T body | Two lines |

Some eyebrow and meta elements on later pages were auto-sized to their original
text and are narrower than they appear. Resize the element before replacing its text.

## Template behaviour

Copying a design preserves page identifiers and element identifiers, so one locator
map is valid for every copy.

Replacing an element's text preserves that element's existing formatting. Adding a
new text element does not carry formatting and requires a separate formatting pass.

Generated backgrounds must contain no text. Text rendered into a background image by
a design generator is unreliable and cannot be corrected afterwards.

Thumbnails lag behind commits. Immediately after a commit, a cached thumbnail may
render blank. Verify against design content, not the thumbnail.
