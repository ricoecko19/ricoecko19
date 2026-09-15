# Configuration values

Descriptive reference for every configurable value in the pipeline. For how to
change the schedule, see [Change the publishing schedule](../how-to/change-the-schedule.md).

## Cadence

| Value | Default | Description |
|---|---|---|
| `POSTS_PER_WEEK` | `4` | Carousels published per week. |
| `GENERATION_CRON` | `0 16 * * 6` | Generation run. Evaluated in UTC. |
| `POSTING_TIME_LOCAL` | `09:00` | Wall-clock posting time for every slot. |
| `POSTING_TIMEZONE` | `America/Los_Angeles` | Reference timezone for posting times. Not read by the scheduler; used when converting to UTC by hand. |

The scheduler takes no timezone. `GENERATION_CRON` must be recomputed at each
daylight-saving transition. For `09:00` Pacific: `0 16 * * 6` during daylight time,
`0 17 * * 6` during standard time.

## Carousel format

| Value | Default | Description |
|---|---|---|
| `CAROUSEL_PAGE_COUNT` | `8` | Pages per carousel. |
| Page dimensions | 1080 × 1350 px | 4:5 portrait. |
| Export format | PDF | Uploaded as a document post. |
| `DOC_TITLE_MAX_CHARS` | `68` | Platform limit. Titles are truncated without warning. |
| `CAPTION_MAX_CHARS` | `3000` | Platform limit. |

Page roles are fixed: 01 cover, 02 context with three statistics and a source line,
03–07 step pages of identical geometry, 08 recap and call to action.

## Folders

| Value | Description |
|---|---|
| `FOLDER_CONTENT` | Parent folder. |
| `FOLDER_PENDING` | Generated, awaiting review. |
| `FOLDER_APPROVED` | Reviewed and cleared. Only designs here may be published. |

Folder identifiers are account-specific and are not committed. See
[`.env.example`](../../.env.example).

## Publishing

| Value | Description |
|---|---|
| `TEMPLATE_DESIGN_ID` | The proven eight-page design that new carousels are copied from. |
| `PUBLISH_AS_PAGE_NAME` | Display name of the company page as it appears in the author selector. |
| `PUBLISH_DEVICE_NAME` | Machine the publishing tasks are bound to. |

The share dialog's author selector defaults to the signed-in personal profile on
every use. It does not remember the previous selection.

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
| Review batch | 4–6 carousels per session |
| Topic reuse interval | 6 weeks minimum |

See [Design system and copy limits](design-system.md) for element-level detail.
