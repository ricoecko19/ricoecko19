# How to recover a failed run

This guide shows you what to do when a run reports a failure, an abort, or nothing at
all.

A failed run is the designed outcome for anything unexpected. It is not an incident.
A missed slot costs ten minutes; the alternative behaviour — a run improvising past a
surprise with nobody watching — costs a post published wrong.

## First, work out which of three things happened

| Signal | Meaning |
|---|---|
| Report says **aborted** | Generation produced something that failed an abort condition. Nothing was queued. |
| Report says **failed** | The publish attempt ran and did not produce a verified post. |
| **No report at all** | The job did not run. Different problem, different fix. |

The third is the dangerous one, because silence looks like success. A weekday that
passes with no notification of any kind needs the workflow run history checked
directly.

## An aborted carousel

Read the abort reason in the report. It names the condition and the page.

The common one is a statistic that does not trace to the research brief. Fix it at the
brief, not at the page: either find and record the primary source properly, or remove
the claim. Then let the next generation run rebuild it.

**Do not publish an aborted carousel by hand to make the slot.** The abort is the only
check the pipeline has, and routing around it is worse than a missed day.

If the abort looks wrong — the figure *is* in the brief and the check disagrees — that
is a verification bug and worth fixing properly, because a check that cries wolf will
be ignored within a month.

## A failed publish

Read the failure reason. Three groups cover almost everything.

**Authorization.** A 401, or a 403 on token refresh. Where a long-lived token is in
use, it expires 60 days after issue and this is the first symptom. Issue a new token,
update the secret, and re-run. Where refresh is in use, a 403 saying refresh is not
allowed usually means the API product approval lapsed or was never granted.

**Upload or post call.** The document did not become available, or the post call was
rejected. Re-run once; these are often transient. If it fails twice, check the API
version — versions sunset on a published schedule and an unbumped version fails on a
date nobody is watching for.

**Read-back.** The post was created but could not be read back, or came back with the
wrong author. **Check the live page before doing anything else.** This is the one case
where the post may actually exist despite the failure status, and re-running would
duplicate it.

## No run at all

Open the workflow run history. Either the job did not trigger, or it triggered and
exited.

An exit before the local cutoff is correct behaviour — that is the daylight-saving
mechanism, and one of the two daily runs is always expected to exit. If *both* runs
exited, the cutoff and the cron expressions have drifted out of alignment. See
[Change the publishing schedule](change-the-schedule.md).

If nothing triggered: CI providers disable scheduled workflows on repositories with no
recent activity, and cron can fire late or be skipped under load. Trigger it manually,
then check whether the schedule is still enabled.

## Publish it, or let it go

Decide whether the post is still worth making. A noon slot missed by two hours is
fine. Missed by a day, take the next open slot rather than stacking two in one day.

To publish it now, follow [Publish by hand](publish-by-hand.md), then update the queue
entry and add the metrics row yourself — a hand-published post is invisible to the
pipeline otherwise.

## What not to do

Do not re-run a failed publish without checking the live page first. If it failed at
read-back, the post may exist and re-running produces a duplicate.

Do not publish an aborted carousel to fill a slot.

Do not fix a repeated failure by widening a check. If the same abort fires every week,
the brief standard is being applied inconsistently somewhere upstream; find that
instead.

## Related

- [Change the publishing schedule](change-the-schedule.md)
- [Publish by hand](publish-by-hand.md)
- [About the verification model](../explanation/verification-model.md)
