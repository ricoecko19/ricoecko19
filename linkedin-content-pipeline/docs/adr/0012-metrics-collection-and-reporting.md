# 0012. Collect post metrics on a fixed cadence and report weekly

Date: 2026-10-06

## Status

Accepted

## Context

With the human review step removed by
[0009](0009-automated-checks-replace-approval-gate.md), nothing in the pipeline
independently confirms that what the queue claims was published actually reached the
page. The queue is written by the same run that does the publishing.

The September incident recorded in [0010](0010-publish-via-github-actions.md) is the
concrete case: the log reported five published posts and two existed. The gap was
found by reading page analytics, not by anything inside the pipeline.

There is also a content-performance motivation, but the sample does not support it
yet — two published posts, five followers, 64 impressions across the first month.

The organization metrics API requires an approved read scope that is not yet
granted. Page admin analytics are readable through a browser today, with two gaps:
the post identifier is not exposed in the analytics table and has to be read from
each post's permalink, and unique impressions are unavailable.

## Decision

Maintain one row per published post, keyed on the post identifier, with the columns
in [metrics-schema.md](../reference/metrics-schema.md).

Refresh each row at **+24h, +72h and +7d**, overwriting the metric columns and
stamping the collection time. Refreshes upsert on the post identifier.

Carry a `source` column recording whether each row was read by a person or written by
an authenticated call.

Produce a weekly report covering per-post figures, the week's aggregates, material
changes, and run health — published, failed and aborted counts with reasons.

The report states reach and engagement separately, states the sample size in the same
sentence as any rate, and does not recommend content changes while the sample is this
small.

Collect no reader-level data. Follower demographics are kept coarse.

## Consequences

The primary value today is run health: the metrics log is the only independent
evidence that the pipeline did what it says it did. A week where the queue reads five
published and the log holds three rows is a pipeline failure that nothing else would
surface.

The three-point refresh cadence is what makes the numbers mean anything. A single read
at publish time measures the first few minutes of distribution.

Upserting rather than appending is load-bearing. Appending duplicates each row on
every refresh, which inflates every aggregate computed over the table afterwards, and
the inflation is invisible once it has happened.

The `source` column prevents hand-entered and API-sourced numbers becoming
indistinguishable after a few weeks.

The first month's data has already produced one durable conclusion, from refusing to
combine two figures: 64 impressions against a 15–28% engagement rate says that
distribution, not content, is the constraint. Publishing more often into an audience
of five does not address it.

Until the metrics API scope is approved, collection is manual and rows are marked
accordingly. This is tolerable at five posts a week and will not be at twenty.
