# 0006. Schedule each post as its own one-shot device-bound task

Date: 2026-09-14

## Status

Accepted

## Context

The publishing route adopted in [0003](0003-publish-via-share-integration.md) has no
scheduling option. It offers publish-now and nothing else.

Posts need to go out on weekday mornings, when the reviewer is not necessarily
available, so publishing has to happen unattended at a specified time.

The route is a browser flow, so whatever performs it needs a real browser on a
machine that is awake.

A single recurring task that walks a queue was considered. It was rejected because
one task holding four posts' worth of state means a single failure can affect all
four, and because the caption and title of each post differ, so the task would have
to read them from somewhere at fire time — introducing exactly the kind of
"instructions from a document" path that [0004](0004-single-human-approval-gate.md)
prohibits.

## Decision

Each approved carousel gets its own one-shot task, firing at its posting time.

Each task prompt carries everything a session with no memory of the approval needs:
the design identifier, the full route, the document title, the complete approved
caption, and the instruction to switch the author to the company page.

Each task ends with an explicit failure instruction: if anything is not as expected,
stop and notify; do not publish, and specifically do not publish from the personal
profile.

Tasks are bound to the machine that has the browser.

## Consequences

Failures are isolated. One failed post does not affect the other three.

The approved caption is embedded in the task at approval time, so what publishes is
what was approved. There is no later read of a file that could have changed.

If the bound machine is asleep or offline at fire time, the run fails and notifies
rather than posting late. This is the intended behaviour: a missed slot costs ten
minutes, a post published wrong cannot be quietly fixed.

Because all tasks in a week are usually bound to the same machine, one sleeping
machine fails the whole week the same way. This is a real exposure and is called out
in [Recover a failed scheduled run](../how-to/recover-a-failed-run.md).

A bound task's schedule, name, and enabled state can be changed freely, but a
**prompt** change must be re-approved on that same machine before the task will run.
Schedule changes must therefore be sent without touching the prompt, or they are
held back and the run misses its slot.

There is no central schedule to inspect. Answering "what is going out this week"
means listing tasks.
