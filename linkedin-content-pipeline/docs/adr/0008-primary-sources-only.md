# 0008. Restrict research to primary sources and record exclusions

Date: 2026-09-01

## Status

Accepted

## Context

The organization's content makes factual claims about security, fraud, and
technology adoption. Those claims are the product.

Secondary sources compress in a direction that is hard to see. A report says a
complaint category rose from 17,367 to 32,424; coverage says it doubled; a post
citing the coverage says it doubled. Each step is small, nobody lies, and the final
claim is unsupported.

A second problem is recurrence. An attractive figure that is rejected once will be
found again by someone with no memory of the earlier decision, and used.

## Decision

Research happens before any design work, and only primary sources qualify: government
agencies and standards bodies, peer-reviewed publications, primary industry reports
published by whoever collected the data, and official vendor documentation for
claims about that vendor's own product.

Every figure is verified by direct fetch. The source URL and the fetch date are
recorded beside the claim. Every statistic is attributed on the page where it
appears, not in an endnote.

Where two figures would serve, prefer the one that will not go stale within a month.

Every research brief ends with a section headed **Deliberately not used**, listing
figures that were considered and rejected, each with its reason.

Never invent a number, client, testimonial, partnership, or outcome.

## Consequences

Research is the slowest stage in the pipeline and cannot be automated away, because
the judgement it requires is the judgement being sold.

Some good stories cannot be told, because the figure that makes them work cannot be
verified. This has happened and the stories were dropped.

The exclusions sections have accumulated real value. Recorded to date: a widely
circulated quality figure absent from the paper it is attributed to; a much-quoted
adoption statistic that originates in an executive opinion survey rather than a
measurement; and a familiar framing about the leading cause of breaches that the
most recent data reverses.

Attributing on the page constrains the copy, since a claim you must attribute in
place is a claim you must have a source for.

The standard caught a mischaracterization before publication — "doubled" against an
underlying 1.87× — which was corrected to "nearly doubled".
