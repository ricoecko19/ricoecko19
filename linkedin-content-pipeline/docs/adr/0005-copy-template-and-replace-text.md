# 0005. Produce carousels by copying a proven template

Date: 2026-09-13

## Status

Accepted

Supersedes [0002](0002-generate-text-free-backgrounds.md)

## Context

Under [0002](0002-generate-text-free-backgrounds.md), each carousel took most of a
working day: a text-free background per page, then two passes per page to add and
format every text element. At four pieces a week this does not fit.

While building the second carousel it became apparent that duplicating a design
preserves page identifiers and element identifiers, and that replacing an element's
text preserves that element's existing formatting.

That changes the unit of work from "build a page" to "address an element".

## Decision

Build the eight-page design correctly once. Produce every subsequent carousel by
copying that design and replacing text against the preserved element identifiers.

Maintain one locator map — page identifiers and element identifiers for every
editable element — which is valid for every copy.

Do not rebuild pages, do not re-style elements, and do not run a formatting pass.

## Consequences

Build time per carousel fell from most of a day to under an hour.

The design system, the connecting trace geometry, the progress bar, and the footers
come along in every copy untouched, so visual consistency stops being something
anyone maintains.

The template becomes a single point of failure. A defect in it propagates to every
future carousel, and fixing it does not fix carousels already copied.

Copy must fit the existing element geometry, because the layout does not reflow.
Observed limits are recorded in
[design-system.md](../reference/design-system.md#copy-length-limits). Some eyebrow
and meta elements on later pages were auto-sized to their original text and must be
resized before their text is replaced.

Changing the format substantially means building a new template by hand, at the cost
described in [0002](0002-generate-text-free-backgrounds.md). That cost is now the
real barrier to format changes, which is a reasonable place for it to sit.

The generator is still used, for text-free backgrounds only. The prohibition in
[0002](0002-generate-text-free-backgrounds.md) on letting it place copy stands.
