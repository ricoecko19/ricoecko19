# How to publish by hand

This guide shows you how to publish a carousel manually through the design tool's
share integration.

Use it as the fallback when the scheduled job cannot run — while the organization API
is pending approval, after a failed run you have decided to recover, or when posting
off-cycle.

**Only publish something that passed verification.** A design that aborted did so for
a reason recorded in the run report. Publishing it by hand routes around the only
checks the pipeline has. Read the abort reason first, fix the underlying brief, and
let it regenerate.

## The route

Open the verified design and find the share options. Search them for the social
platform — it sits under the secondary list, not the primary share buttons.

Then set four things, in this order:

| Setting | Value | Note |
|---|---|---|
| Format | **Document** | Defaults to Image Post. Changing this also switches page selection to all pages. |
| Choose pages | **All 8** | Confirm the count matches the design. |
| Posting as | **The organization** | **Defaults to the personal profile.** This is the step that gets missed. |
| Title | ≤ 68 characters | The bold headline readers see. |
| Caption | ≤ 3000 characters | Paste the queued caption whole; do not retype it. |

Read the author selector once more. Then publish.

## Confirm the author before you type

The scheduled publisher takes the author from configuration, so it cannot get this
wrong. Publishing by hand reintroduces exactly the risk that route removed.

A post published under the wrong identity cannot be quietly fixed. Deleting and
reposting loses whatever engagement it had and puts a duplicate in the feed of anyone
who saw the first one.

So confirm the author reads the organization **before** typing the title, not after.
Typing first and checking last means discovering the problem when your finger is
already on publish.

## If you have to leave the composer

Navigating away from a composer with text in it triggers a "Leave site?" prompt, and
forcing past it discards the draft.

If you need to check something mid-compose, open a new tab rather than navigating
this one.

## Verify and record

Open the live post and swipe all eight pages. Check the page count, that the accent
line runs continuously, and that the author on the post is the organization.

Then record it, because a hand-published post is invisible to the pipeline otherwise:

1. Set the queue entry to `published` with the post URL.
2. Add a metrics row, with `source` set to `manual`.
3. Read the post identifier from the permalink — the analytics table does not expose
   it, and the metrics row cannot be keyed for refresh without it.

Skipping step 3 is the common mistake. The row cannot be upserted later, so every
refresh appends a duplicate.

## Related

- [Recover a failed run](recover-a-failed-run.md)
- [Collect performance metrics](collect-performance-metrics.md)
- [ADR 0003](../adr/0003-publish-via-share-integration.md), superseded by [ADR 0010](../adr/0010-publish-via-github-actions.md)
