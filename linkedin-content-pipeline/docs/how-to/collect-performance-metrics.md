# How to collect performance metrics

This guide shows you how to populate and refresh the metrics log. For the columns
themselves, see [Metrics schema](../reference/metrics-schema.md).

Use the browser route below while the organization metrics API scope is pending.

## Capture the post identifier at publish time

Do this first, on the day of the post. It is the step that gets skipped and the one
that cannot be recovered cheaply later.

The analytics table does not expose the post identifier. Read it from the post's
permalink and write it into the row immediately. Without it, the row has no key, so
every later refresh appends a duplicate instead of updating.

A scheduled publish records this automatically. A hand-published post does not — see
[Publish by hand](publish-by-hand.md).

## Open the analytics

Admin analytics routes resolve the **numeric** page identifier only. The vanity slug
redirects to an unavailable page, which looks like a permissions problem and is not.

Use `PAGE_NUMERIC_ID` from your configuration. The content analytics view gives
per-post impressions, clicks, click-through rate, reactions, comments and reposts;
the followers view gives follower counts and coarse location demographics.

## Refresh on the cadence, not on impulse

Refresh each row at **+24h, +72h and +7d** after its post date. Engagement moves for
roughly 72 hours, so a reading taken at publish time measures almost nothing.

On each refresh: overwrite the metric columns, stamp `collected_at`, and set `source`
to `manual` for a reading taken by eye.

**Upsert on the post identifier. Do not append.** Appending duplicates the row on
every pass, and the duplication is invisible once it has happened — every aggregate
computed over the table afterwards is inflated and nothing flags it.

## Reconcile against the queue

This is the part that is actually load-bearing, and it takes a minute.

Count the posts the queue says were published this week. Count the rows in the
analytics table. If they disagree, the pipeline reported something that did not
happen.

This check exists because it has already caught exactly that: a week logged as five
published posts where only two existed on the page. Nothing inside the pipeline would
have surfaced it, because the queue is written by the same run that does the
publishing.

Any post on the page that is not in the queue is worth investigating too — it means
something published outside the pipeline.

## Generate the weekly report

Run `06-weekly-report.md` against the log.

Read run health first: published, failed and aborted counts, with the abort reasons.
That is the part that is actionable today.

Read the performance figures second, and read reach and engagement separately. A high
engagement rate against low impressions is not a mixed result — it says distribution
is the constraint and content is not.

## Do not act on a small sample

At current volume the content figures support no conclusions. Two posts with different
click-through rates is a coin flip, not a finding.

Resist changing the content strategy on it, and resist increasing volume to fix reach
— more posts into a small follower graph does not reach more people. See
[About the analytics](../explanation/analytics.md).

## Related

- [Metrics schema](../reference/metrics-schema.md)
- [About the analytics](../explanation/analytics.md)
- [ADR 0012: metrics collection and weekly reporting](../adr/0012-metrics-collection-and-reporting.md)
