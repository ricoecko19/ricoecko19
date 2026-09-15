# 0002. Generate text-free backgrounds and add copy as text elements

Date: 2026-08-30

## Status

Superseded by [0005](0005-copy-template-and-replace-text.md)

## Context

The first carousels were to be produced by asking the design tool's generator to
create each page from a description that included the copy.

The output was unusable. The generator renders text into the background as raster.
One early run produced a headline reading `AI-WRITEN`, and body copy that was not
language. Because the text is part of the image, there is no way to correct it — the
page has to be regenerated, with no guarantee the next attempt is better.

This is not an occasional defect. It is how the tool works.

## Decision

Never allow the generator to place copy. Generate a background containing no text at
all, then add every word as a real text element positioned over it.

The text API accepts no formatting parameters, so each page requires two passes: add
the elements, then format them against the identifiers returned by the first pass.
Text lands as small black type until the second pass runs.

## Consequences

Output became reliable and correctable. Every word is live text that can be edited
without touching the design.

Production is slow. Two passes per page across eight pages, plus positioning, put a
carousel at most of a working day.

Two operational notes were recorded while working this way and remain true:

- Pages added within a transaction are not readable until it commits.
- Thumbnails lag commits. A design can render blank in a cached thumbnail moments
  after a successful save. Verify against design content inside a transaction rather
  than concluding that content was lost.

The per-page cost made this untenable at four pieces a week, which led directly to
[0005](0005-copy-template-and-replace-text.md).
