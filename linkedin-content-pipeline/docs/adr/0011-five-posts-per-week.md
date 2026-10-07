# 0011. Publish five carousels per week, weekdays at noon

Date: 2026-10-06

## Status

Accepted

Supersedes [0007](0007-four-posts-per-week.md)

## Context

[0007](0007-four-posts-per-week.md) set four posts a week, and the binding constraint
it named was review capacity: one person reviewing a batch in one session, with
quality falling off past six pieces.

[0009](0009-automated-checks-replace-approval-gate.md) removed that constraint by
removing the reviewer.

Cadence then moved several times in quick succession — four a week on weekday
mornings, then briefly fourteen a week at two a day including weekends, then six a
week on alternate days at two times, then five. The two-a-day experiment is the
informative one: it doubled the demand on research, which is the slowest stage and
the one that cannot be compressed without thinning the briefs.

The topic rotation holds seven topics, and reusing an angle inside six weeks produces
visibly repetitive output.

## Decision

Five carousels per week, one each weekday at 12:00 local time.

Five topics are used each week, and the two left out rotate into the following week.

Generation runs on both weekend days for the coming Monday to Friday. The run is
idempotent: the second day fills any weekday the first missed or aborted, and does
nothing where a queue entry already exists.

## Consequences

One post per weekday is a legible rhythm for a reader, which the alternating-day and
twice-daily schedules were not.

Research is now the binding constraint, as it should be. Five briefs a week against
seven rotation topics is sustainable; the two-a-day rate was not.

No weekend posting. The audience is organizational and the posts are work-related.

The idempotent weekend generation removes a failure mode that the previous
single-run arrangement had: a Saturday run that aborted left a weekday empty with
nothing to catch it.

Volume will not fix the reach problem, and should not be increased in the belief that
it might — see [About the analytics](../explanation/analytics.md). Distribution and
follower growth are a separate problem that this pipeline does not address.

Four finished carousels from the September failures remain unpublished and are reused
before new ones are generated.
