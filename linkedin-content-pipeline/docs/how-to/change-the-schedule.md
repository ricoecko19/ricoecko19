# How to change the publishing schedule

This guide shows you how to move the generation run, change the posting time, or
change how many carousels go out per week.

## How the schedule avoids daylight saving

Read this before editing any cron expression, because the arrangement looks wrong
until you know why it is there.

The CI scheduler evaluates cron in UTC and takes no timezone. A single expression
pinned to a local wall-clock time therefore drifts by an hour twice a year.

So there are **two** publish expressions per weekday, an hour apart, and the
publisher exits immediately on any run earlier than the local cutoff:

| Variable | Default |
|---|---|
| `PUBLISH_CRON_EARLY` | `50 18 * * 1-5` |
| `PUBLISH_CRON_LATE` | `50 19 * * 1-5` |
| `PUBLISH_LOCAL_CUTOFF` | `11:40` |

During daylight time the early run lands just before noon local and publishes; the
late run finds the day already published and exits. During standard time the early
run is before the cutoff and exits; the late run publishes. Exactly one post per
weekday, all year, with nothing to change at the transitions.

If you move the posting time, move **both** expressions and the cutoff together. The
cutoff must sit between the two expressions in local terms, or you will get two posts
a day or none.

## Move the posting time

1. Decide the new local time.
2. Set both cron expressions to that time expressed in UTC, one for daylight offset
   and one for standard offset.
3. Set `PUBLISH_LOCAL_CUTOFF` to roughly twenty minutes before the target.
4. Commit, then verify as below.

CI cron is best-effort and can fire late under load. Schedule a few minutes early so
a late start still lands near the intended time.

## Move the generation run

Generation runs on both weekend days and is idempotent, so its timing is not
sensitive. Change `GENERATION_CRON` and commit.

Keep it early enough on Saturday that a Sunday run has a full day to fill anything
that aborted.

## Change how many posts go out per week

1. Change `POSTS_PER_WEEK`.
2. Add or remove weekday slots in `src/prompts/rotation.md`.
3. Confirm the rotation has enough unused topics to cover the new volume for six
   weeks. At five a week against seven topics, the cycle is already close to the
   reuse floor — add topics before adding volume.

The constraint on volume is research, not the pipeline. More posts means more briefs
built on primary sources, and a thin brief produces a carousel that looks
authoritative and is not.

More volume will not improve reach. See [About the analytics](../explanation/analytics.md).

## Verify

After any schedule change, trigger the publish workflow manually in **check** mode.
It is read-only: it resolves the queue entry for today, tests the API connection, and
reports what it would do without posting.

Then confirm the next scheduled run appears with the local time you intended, not the
UTC expression you typed.

Watch the first week after a change. A weekday that passes with no notification at
all — neither published nor failed — means the job did not run, which is a different
problem from a job that ran and failed.

## Related

- [Recover a failed run](recover-a-failed-run.md)
- [ADR 0010: publish from a scheduled CI job](../adr/0010-publish-via-github-actions.md)
