# 0001. Publish as an eight-page document carousel

Date: 2026-08-29

## Status

Accepted

## Context

The organization needed a LinkedIn format that could carry a substantive, sourced
argument and still be produced repeatably by one person.

Single images carry one idea and leave no room for attribution, which conflicts with
the requirement that every statistic appear with its source. Long-form text posts
are read by few people and cannot be designed. Video has a production cost per piece
that does not amortize, since no two videos share a template.

Document posts render as a swipeable carousel in the feed, accept a PDF, and hold as
many pages as needed. Crucially for us, the format is repetitive by nature, which
means most of it can be templated.

The page count needed to be fixed rather than variable, so that a template could
exist at all.

## Decision

Every piece is an eight-page document carousel at 1080 × 1350, with fixed page roles:
cover, context with three attributed statistics, five step pages of identical
geometry, and a recap with a call to action.

Five step pages is the working span: enough for a genuinely useful list, few enough
that a reader finishes.

## Consequences

Writing a carousel becomes filling a known shape — roughly eleven short strings per
step page — rather than designing a piece of content. This is the precondition for
everything in [0005](0005-copy-template-and-replace-text.md).

Topics that do not decompose into five parallel items fit badly. In practice this has
constrained topic selection more than expected; two proposed topics were reshaped to
fit the format rather than the reverse.

The fixed count also imposes hard copy limits, since the layout does not reflow. See
[design-system.md](../reference/design-system.md#copy-length-limits).

Because a document post is a single artifact, a correction after publishing means
replacing the whole post rather than editing a line.
