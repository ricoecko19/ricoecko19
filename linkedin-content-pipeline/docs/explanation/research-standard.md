# About the research standard

This page is about why the research rules are what they are. For the rules
themselves, see [Configuration values](../reference/configuration.md#research-standard).

## Why primary sources only

The rule is not about rigour for its own sake. It is about a specific failure mode.

Secondary coverage compresses. A report says a category of fraud rose from 17,367
complaints to 32,424; coverage of that report says it doubled; a post citing the
coverage says it doubled. Each step is small and defensible, and the end result is a
claim the underlying data does not support. Nobody lied. The number just drifted.

Requiring the original document and a direct fetch stops the drift at the first
step, because the writer sees the actual figure rather than someone's rounding of it.

This exact case occurred and was caught in review: a page read "government
impersonation doubled" against an underlying 1.87×. It was corrected to "nearly
doubled" before publication. That correction is the standard working.

## Why the fetch date is recorded

A URL is a claim about where something can be found, not evidence that it was found.
Recording the date the figure was actually retrieved converts the citation from an
assertion into a record.

It also makes staleness visible. A brief with fetch dates from eight months ago
announces itself.

## Why durable figures beat dramatic ones

Given a choice between a striking figure that will be superseded next quarter and a
duller one that will hold for two years, the standard takes the duller one.

The reason is asymmetric cost again. Content lives on the page indefinitely. A post
that was accurate when published and is wrong a year later is still wrong when
someone finds it, and there is no mechanism by which they learn that it used to be
right. Content that ages badly is a liability that accrues silently.

## Why exclusions are written down

This is the part that surprises people, and the part I would defend most strongly.

Each brief ends with a section listing figures that were considered and rejected,
with reasons. Examples from the real briefs:

- A widely circulated quality-improvement figure attributed to a well-known study
  was excluded, because it does not appear in the paper and could not be located in
  the source.
- A frequently quoted adoption statistic was excluded because it originates in an
  executive opinion survey reported by a research index, rather than a measurement.
  A government statistical survey was used instead, giving a much lower and much
  better-supported number.
- A familiar framing about the leading cause of breaches was dropped, because the
  most recent breach report reverses the ordering it depends on.

None of these would appear anywhere if the rejections were not recorded. And that is
precisely the problem: an attractive number that was rejected once will be found
again, by someone with no memory of the first decision, and used.

A research standard that only records what passed is a standard that quietly relaxes
over time. Recording the rejections is what makes it survive contact with a deadline.

## On attribution on the page

Every statistic appears with its source on the page where it appears, not in a
citations list at the end and not only in the caption.

Partly this is honest practice. Practically, it is also a constraint on the writing:
a claim you have to attribute in place is a claim you have to have a source for, and
the discipline shows up in the copy before it shows up in the review.

## The cost

This standard is expensive. Research is the slowest stage in the pipeline by a wide
margin, and it is the stage that cannot be automated away, because the judgement it
requires — is this source primary, is this figure fairly characterized, will this
hold — is the judgement being sold.

That is an acceptable trade for an organization whose product is technology
judgement. It might not be for one whose product is volume.

## Related

- [About the architecture](architecture.md)
- [ADR 0008: primary sources only, with a written exclusions list](../adr/0008-primary-sources-only.md)
