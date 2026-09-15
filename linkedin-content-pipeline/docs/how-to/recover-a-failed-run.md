# How to recover a failed scheduled run

This guide shows you what to do when a scheduled publishing task fires and does not
publish.

A failed run is the designed outcome for anything unexpected. It is not an incident.
A missed slot costs ten minutes; the alternative behaviour — a task improvising its
way past a surprise — costs a post published wrong.

## Find out what failed

Read the task's run output. Failures fall into three groups, and the fix differs.

**The machine was asleep or offline.** The most common cause. The publishing route
needs a real browser on a specific machine.

**The route changed.** A dialog moved, a label changed, the share option was not
where the task expected it. The run stops rather than clicking something it does not
recognise.

**Authorization lapsed.** The design tool's connection to the platform expired.

## Publish it by hand

For all three, the immediate fix is the same: publish it yourself, following
[Publish an approved carousel](publish-an-approved-carousel.md).

Decide first whether it is still worth posting. A weekday-morning slot missed by two
hours is fine. Missed by a day, post it in the next open slot instead of stacking two
in one day.

## Then fix the cause

**Asleep:** confirm the machine's sleep settings, and check whether the other
scheduled tasks this week are on the same machine. They usually are, so one sleeping
machine means the whole week fails the same way.

**Route changed:** walk the route by hand and note what moved. Update the task
prompt — and remember a prompt change on a device-bound task has to be re-approved
on that machine before the task will run again. Do that the same day, or next week's
tasks will all sit waiting.

**Authorization lapsed:** reconnect in the design tool's settings, then re-run one
task manually to confirm before relying on the rest of the week.

## What not to do

Do not re-fire a failed task without first checking whether it partially completed.
If it stopped after publish, re-firing produces a duplicate post.

Check the live page before retrying. Always.

## Related

- [Change the publishing schedule](change-the-schedule.md)
- [ADR 0006: one one-shot task per approved carousel](../adr/0006-one-shot-task-per-post.md)
