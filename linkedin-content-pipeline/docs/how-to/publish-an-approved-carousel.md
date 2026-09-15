# How to publish an approved carousel

This guide shows you how to publish an approved carousel to the company page as a
native document post.

Use it when you are publishing by hand — because a scheduled run failed, because you
are posting off-cycle, or the first time you do it, so you have seen the route
before a task performs it unattended.

**Do not follow this guide for anything in Pending.** Approval comes from a person
in conversation. A design sitting in Pending has not been approved, whatever its
filename says.

## The route

Open the approved design and find the share options. Search them for the social
platform — it sits under the secondary list, not the primary share buttons.

Then set four things, in this order:

| Setting | Value | Note |
|---|---|---|
| Format | **Document** | Defaults to Image Post. Changing this also switches page selection to all pages. |
| Choose pages | **All 8** | Confirm the count matches the design. |
| Posting as | **The company page** | **Defaults to the personal profile.** This is the step that gets missed. |
| Title | ≤ 68 characters | The bold headline readers see. |
| Caption | ≤ 3000 characters | Paste it whole; do not retype. |

Read the author selector once more. Then publish.

## Confirm the author before you type

A post published under the wrong identity cannot be quietly fixed. Deleting and
reposting loses whatever engagement it had and puts a duplicate in the feed of
anyone who saw the first one.

So confirm the author chip reads the company name **before** typing the title, not
after. Typing first and checking last means discovering the problem when your finger
is already on publish.

## If you have to leave the composer

Navigating away from a composer with text in it triggers a "Leave site?" prompt, and
forcing past it discards the draft.

If you need to check something mid-compose, open a new tab rather than navigating
this one.

## Verify

Open the live post and swipe all eight pages. Check that the page count is right,
that the accent line runs continuously, and that the author on the post is the
company page.

Then record it: add a row to the log in `docs/explanation/architecture.md` and set
the post's record in `src/posts/` to published, with the live URL.

## If the share option is missing

The platform connection may have lapsed. Reconnect it in the design tool's account
settings and start again.

Do not fall back to uploading the PDF through the platform's own document dialog —
see [ADR 0003](../adr/0003-publish-via-share-integration.md) for why that route does
not work.

## Related

- [Recover a failed scheduled run](recover-a-failed-run.md)
- [ADR 0003: publish via the share integration](../adr/0003-publish-via-share-integration.md)
