# How to change the publishing schedule

This guide shows you how to move the generation run, change the posting times, or
change how many carousels go out per week.

Read the daylight-saving warning below before you edit any cron expression.

## Move the generation run

The generation run is a recurring task with a cron expression evaluated **in UTC**.

To move it, change the schedule on the task. Do not change its prompt at the same
time — see [changing a device-bound task's prompt](#changing-a-device-bound-tasks-prompt)
below.

### Daylight saving will break this

The cron expression is UTC, so a run pinned to a local wall-clock time silently
shifts by an hour when local time changes.

| Local time wanted | During daylight time | During standard time |
|---|---|---|
| 09:00 Pacific | `0 16 * * 6` | `0 17 * * 6` |

Put a calendar reminder on both transition dates. There is no version of this that
is automatic, because the scheduler does not take a timezone.

If a run fires an hour early, this is almost always why.

## Change a posting time

Each approved carousel has its own one-shot task, so there is no central posting
schedule to edit. Change the schedule on the individual task.

Posting times are staggered one per weekday. If you need two on one day, give them
at least three hours apart — consecutive document posts from the same page compete
with each other.

## Change how many posts go out per week

1. Change `POSTS_PER_WEEK` in `.env`.
2. Add or remove weekday slots in `src/prompts/rotation.md`.
3. Confirm the rotation has enough unused topics to cover the new volume for six
   weeks. If it does not, add topics first.

Increase this slowly. The constraint is not the pipeline, which will produce as many
as you ask for. The constraint is that one person still has to review all of them in
one session, and review quality falls off a cliff somewhere past six.

## Changing a device-bound task's prompt

Publishing tasks are bound to one machine, because the publishing route needs a real
browser.

A bound task's **schedule, name, and enabled state** can be changed freely. Its
**prompt** cannot — a prompt change has to be re-approved on that same machine
before it will run.

So: send schedule changes on their own, in a call that does not touch the prompt.
A schedule change bundled with a prompt edit will be held back waiting for approval,
and the run will miss its slot.

## Verify

After any schedule change, list the pending tasks and read back the next fire time
as a local timestamp rather than a cron expression. Confirm it says the wall-clock
time you meant.

## Related

- [Recover a failed scheduled run](recover-a-failed-run.md)
- [ADR 0006: one one-shot task per approved carousel](../adr/0006-one-shot-task-per-post.md)
