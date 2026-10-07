# About the analytics

This page is about what the performance reporting is for, and the ways it can
mislead. For the table itself see [Metrics schema](../reference/metrics-schema.md);
for how to populate it see
[Collect performance metrics](../how-to/collect-performance-metrics.md).

## What it is actually for

Not optimization. Not yet, and possibly not for a long time.

The reporting exists first as **run health**. With no human in the publishing path,
the metrics log is the only independent evidence that the pipeline is doing what the
queue says it did. A week where the queue reads five published and the log holds
three rows is a pipeline failure, and nothing else in the system would surface it.

That is a less exciting purpose than content optimization, and it is the one that
earns the effort today.

## Why a single read is worthless

Engagement moves for roughly 72 hours after a post goes out. A number captured at
publish time measures the first few minutes of distribution and nothing else.

So every row is refreshed at +24 hours, +72 hours, and +7 days, overwriting the
metric columns and stamping the collection time. Refreshes upsert on the post
identifier; appending instead duplicates the row on every pass, which quietly inflates
every aggregate computed over the table afterwards.

## Reach and engagement are different problems

The most useful thing the first month of data produced was a diagnosis, and it came
from refusing to combine two numbers.

Across two published posts there were 64 impressions and a 15–28% engagement rate.
Treated as one "performance" figure that reads as a mixed result. Read separately it
is unambiguous: almost nobody saw the posts, and a high proportion of those who did
clicked. Content quality is not the constraint. Distribution is.

That conclusion has a direct consequence — publishing more often into an audience of
five does not fix reach, and five posts a week will not fix it either. Follower growth
and distribution are the prior problem, and they are not problems this pipeline
solves.

This is why the report states reach and engagement separately and never multiplies
them into a single score.

## The sample is too small to optimize against

Two posts, five followers, 64 impressions over a month. At that scale every apparent
finding is noise.

One of the two posts had roughly double the click-through rate of the other. It is
tempting to conclude something about length or concreteness from that. It supports no
conclusion at all — it is one pair of posts, and the difference is within what two
coin flips would produce.

The reporting prompt carries this as an explicit constraint: state the sample size in
the same sentence as any rate, and do not recommend a content change on a sample this
small. The constraint lives in the tooling rather than in a reader's good intentions,
because the pressure to find a finding is strongest exactly when there is nothing
there.

Revisit content optimization when there are enough published posts to read a signal.
There is no shortcut, and more frequent reporting is not one.

## Why the `source` column exists

Some rows are read off a dashboard by a person; some are written by an authenticated
metrics call. The column records which.

Without it, hand-entered numbers and API numbers become indistinguishable after a few
weeks, and any later analysis silently averages two different measurement processes
with different error characteristics. The column costs nothing and prevents a class
of mistake that is invisible once made.

The same instinct as the **Deliberately not used** section in the research briefs:
record the provenance of a number alongside the number, because the number outlives
the memory of where it came from.

## What is not collected

Follower demographics beyond location, individual reactor identities, and anything
that would amount to building a profile of a reader. The page-level demographic
breakdown is kept coarse deliberately.

There is no audience-targeting use case here that would justify finer collection, and
an organization that publishes about privacy should be able to describe its own data
practices in one honest paragraph.

## Related

- [Metrics schema](../reference/metrics-schema.md)
- [ADR 0012: metrics collection and weekly reporting](../adr/0012-metrics-collection-and-reporting.md)
