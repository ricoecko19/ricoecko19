# Configuration values

Descriptive reference for every configurable value in the pipeline. For how to
change the schedule, see [Change the publishing schedule](../how-to/change-the-schedule.md).

## Cadence

| Value | Default | Description |
|---|---|---|
| `POSTS_PER_WEEK` | `5` | One per weekday, Monday to Friday. |
| `GENERATION_CRON` | `52 15 * * 6,0` | Generation run, in UTC. Fires both weekend days. |
| `PUBLISH_CRON_EARLY` | `50 18 * * 1-5` | First weekday publish attempt, in UTC. |
| `PUBLISH_CRON_LATE` | `50 19 * * 1-5` | Second weekday publish attempt, in UTC. |
| `PUBLISH_LOCAL_CUTOFF` | `11:40` | The publisher exits on any run earlier than this local time. |
| `POSTING_TIMEZONE` | `America/Los_Angeles` | Timezone the cutoff is evaluated in. |

Two publish schedules plus a local cutoff produce exactly one publish per weekday at
approximately noon local time, in both daylight and standard time, with no manual
change at the transitions. The early expression lands at noon during daylight time
and is skipped during standard time; the late expression does the reverse.

Generation is idempotent: the second weekend run fills any weekday the first run
missed or aborted, and does nothing where a queue entry already exists.

## Carousel format

| Value | Default | Description |
|---|---|---|
| `CAROUSEL_PAGE_COUNT` | `8` | Pages per carousel. |
| Page dimensions | 1080 × 1350 px | 4:5 portrait. |
| Export format | PDF | Uploaded through the documents API. |
| `DOC_TITLE_MAX_CHARS` | `68` | Platform limit. Titles are truncated without warning. |
| `CAPTION_MAX_CHARS` | `3000` | Platform limit. |

Page roles are fixed: 01 cover, 02 context with three statistics and a source line,
03–07 step pages of identical geometry, 08 recap and call to action.

## Queue

The queue holds one entry per weekday. Each entry carries the publish date, the
weekday, the rotation topic, the design identifier, the document title, the caption,
the source list, and a status.

| Status | Meaning |
|---|---|
| `queued` | Passed every automated check. Eligible to publish. |
| `published` | Published and read back successfully. Carries the post URL. |
| `failed` | The publish attempt ran and did not produce a verified post. Carries a reason. |
| `aborted` | Failed an abort condition at generation. Never reached the queue as publishable. |

An entry with an empty `sources` list is refused by the publisher regardless of
status.

## Abort conditions

A carousel failing any of these is not published. The run records the failure and
notifies.

| Condition |
|---|
| A statistic on a page does not trace to a source captured during research |
| An invented client, partnership, testimonial, outcome, funding award, or milestone |
| A political, legal, or medical claim; a security claim not stated by an official source |
| Confidential client, employee, or student information |
| A future plan presented as a confirmed result |
| An author that is not the organization |

These are checked and fixed rather than aborted on: tone, hashtag count, call-to-action
variation, the colour ratio, and the copy-length limits below.

## Research standard

Accepted source types, in the order they are preferred:

1. Government agencies and standards bodies
2. Peer-reviewed publications
3. Primary industry reports published by the organization that collected the data
4. Official vendor documentation, for claims about that vendor's own product

Not accepted: news coverage of a primary source in place of the source, executive
surveys presented as measurement, aggregators, and any figure that cannot be
retrieved at the URL recorded for it.

Every figure carries the URL it was fetched from and the date of the fetch. Every
statistic appearing in a carousel is attributed on the page where it appears.

Each research brief ends with a section headed **Deliberately not used**, listing
figures that were considered and rejected, each with its reason.

## Limits observed in practice

| Constraint | Value |
|---|---|
| Step-page title | ≤ 30 characters before wrapping |
| Recap headline | ≤ 22 characters |
| Recap call to action | ≤ 35 characters |
| Topic reuse interval | 6 weeks minimum |

Seven rotation topics against five posts a week means two topics carry over to the
following week. See [Design system and copy limits](design-system.md) for
element-level detail.
