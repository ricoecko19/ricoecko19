# 0003. Publish through the design tool's share integration

Date: 2026-09-12

## Status

Accepted

## Context

Publishing an eight-page document to the organization's company page had to be
possible without a person performing a file upload by hand every time.

Three routes were attempted and failed:

**Platform API.** Returns 403 on company-page posts. The available token carries
personal-profile scopes only; organization posting scopes sit behind an application
review process with no committed timeline.

**The platform's own document upload dialog.** Exposes no file input in the DOM. The
choose-file button spawns the operating system's native picker, which browser
automation cannot drive. On the admin route the dialog renders inside an iframe that
the accessibility tree does not traverse.

**Scheduling inside the design tool.** Does not exist on this account. The publish
panel offers visibility and comments, and a single publish-now button. The content
planner URL returns 404.

A fourth route was then noticed: the design tool's own share integration lists the
social platform, and that integration's authorization already covered the company
page, having been granted through a different consent flow than the API route needs.

## Decision

Publish through the design tool's share integration, with format set to Document,
all pages selected, and the author switched from the personal profile to the company
page.

Verified working 2026-09-12.

## Consequences

Publishing works, without an API approval process and without a human file upload.

The author selector defaults to the personal profile on every use and does not
remember the previous selection. This is now the single most dangerous step in the
pipeline, and is called out in the how-to guide, in the publishing prompt, and in the
failure instruction.

The route offers no scheduling, which forced [0006](0006-one-shot-task-per-post.md).

The route is a browser flow rather than an API, so it is brittle to interface
changes and cannot be driven headlessly.

The three failed routes are recorded here so they are not retried. If the platform
grants organization scopes later, a new record should supersede this one rather than
this one being edited.

An alternative was left on the table: rendering the PDF to images and using the
platform's image upload, which does expose a real file input and does have a native
scheduler. It was rejected because it posts as an image carousel rather than a
document post, losing the format chosen in [0001](0001-eight-page-document-carousel.md).
It remains the fallback if this route closes.
