# Decision log

One record per architecturally significant decision, in the format popularized by
Michael Nygard and collected at [adr.github.io](https://adr.github.io): title,
status, context, decision, consequences.

**A record is not revised.** Once written, an ADR is left alone. If the decision
changes, a new record is written that supersedes it, and the two are linked in both
directions. The log is a history, not a description of the present — reading it in
order should show how the thinking moved.

Records 0003, 0004, 0006 and 0007 describe how the pipeline worked before October
2026. They are superseded, not deleted, and the reasoning in them is the reasoning
that was actually used at the time.

| # | Decision | Status |
|---|---|---|
| [0001](0001-eight-page-document-carousel.md) | Publish as an eight-page document carousel | Accepted |
| [0002](0002-generate-text-free-backgrounds.md) | Generate text-free backgrounds and add copy as text elements | Superseded by [0005](0005-copy-template-and-replace-text.md) |
| [0003](0003-publish-via-share-integration.md) | Publish through the design tool's share integration | Superseded by [0010](0010-publish-via-github-actions.md) |
| [0004](0004-single-human-approval-gate.md) | Require a single human approval gate | Superseded by [0009](0009-automated-checks-replace-approval-gate.md) |
| [0005](0005-copy-template-and-replace-text.md) | Produce carousels by copying a proven template | Accepted |
| [0006](0006-one-shot-task-per-post.md) | Schedule each post as its own one-shot device-bound task | Superseded by [0010](0010-publish-via-github-actions.md) |
| [0007](0007-four-posts-per-week.md) | Publish four carousels per week | Superseded by [0011](0011-five-posts-per-week.md) |
| [0008](0008-primary-sources-only.md) | Restrict research to primary sources and record exclusions | Accepted |
| [0009](0009-automated-checks-replace-approval-gate.md) | Replace the approval gate with automated abort conditions | Accepted |
| [0010](0010-publish-via-github-actions.md) | Publish from a scheduled CI job against the organization API | Accepted |
| [0011](0011-five-posts-per-week.md) | Publish five carousels per week, weekdays at noon | Accepted |
| [0012](0012-metrics-collection-and-reporting.md) | Collect post metrics on a fixed cadence and report weekly | Accepted |
