# 0010. Publish from a scheduled CI job against the organization API

Date: 2026-10-06

## Status

Accepted

Supersedes [0003](0003-publish-via-share-integration.md) and
[0006](0006-one-shot-task-per-post.md)

## Context

[0003](0003-publish-via-share-integration.md) published through the design tool's
share integration, and [0006](0006-one-shot-task-per-post.md) scheduled each post as
its own one-shot task bound to a specific machine, because that route needed a real
browser.

That arrangement failed in production. In one week, four of five scheduled publishes
never fired. The tasks were placed on hold, the hold expired, and they auto-disabled
without running — no run, no error, no notification. The failure was not a sleeping
machine; it was the scheduling mechanism itself.

It went unnoticed for two weeks, because the log had been written at scheduling time
rather than after a confirmed post. The log said five posts went out. Two had.

That is a 4-of-5 failure rate on the publishing mechanism, and it is independent of
the approval question settled in [0009](0009-automated-checks-replace-approval-gate.md).
Removing the human gate would have made it worse, not better: faster generation into
a publishing path that silently drops most of what it is given.

Separately, the organization API route that was closed in
[0003](0003-publish-via-share-integration.md) became worth revisiting, since the
scopes it needs are obtainable through an application review rather than being
unavailable in principle.

## Decision

Publish from a scheduled job on a CI runner, talking directly to the platform's
documents and posts APIs as the organization.

The publisher: refuses any queue entry with an empty source list; uploads the PDF
through the documents API and waits for it to become available; creates the post with
the author taken from configuration; reads the post back and confirms the author;
then commits the status and URL back to the queue. A failed run notifies.

Two cron expressions per weekday, roughly an hour apart in UTC, with the publisher
exiting immediately on any run before the local cutoff. Exactly one lands near noon
local time in both daylight and standard time, with no manual change at the
transitions.

The browser route from [0003](0003-publish-via-share-integration.md) is retained and
documented as the manual fallback.

## Consequences

The machine dependency, the browser, and the hold-expiry failure mode are all gone.
Scheduling is a property of the repository rather than of a workstation.

The author becomes a configuration constant rather than a dropdown that defaults to a
personal profile. This is the clearest win, and it came from changing the route
rather than from any check.

Daylight saving stops being an operational task. The previous arrangement required
recomputing a UTC cron expression twice a year, and a missed recomputation published
an hour early.

**The pipeline now holds credentials.** Under the browser route there was no key to
leak and no token to rotate, because authorization was interactive and belonged to a
signed-in person. That was a better security posture and it was traded for
reliability. Secrets live in the CI provider's encrypted store; the publisher must
not print a token on any code path, including error handlers.

Token expiry becomes a scheduled hazard. Where a long-lived token is used instead of
a refresh token, publishing stops 60 days after issue and the first symptom is a
failed run.

The API version is a dated string with a published sunset schedule, so it must be
bumped yearly. An unbumped version fails on a date nobody is watching for.

**This is not yet live.** At the time of writing, token refresh returns a 403
indicating the application is not enabled for programmatic refresh tokens, which
usually means the organization API product is not yet approved. Until it is,
publishing runs through the documented manual fallback. The read-only check workflow
is the way to test the connection without posting.
