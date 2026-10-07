# Metrics schema

Descriptive reference for the post performance log. For how to populate it, see
[Collect performance metrics](../how-to/collect-performance-metrics.md).

## Table

One row per published post, keyed on the post identifier.

| Column | Type | Written by | Description |
|---|---|---|---|
| `post_date` | date | publish run | ISO 8601, posting timezone. |
| `post_time` | time | publish run | Scheduled fire time. |
| `design_id` | text | publish run | Source design. |
| `title` | text | publish run | Document title shown to readers. |
| `topic_number` | int | rotation | 1–7. |
| `topic_label` | text | rotation | Human-readable topic name. |
| `post_urn` | text | publish run | Platform post identifier. The join key for every metric. |
| `post_url` | text | publish run | Public permalink. |
| `format` | text | publish run | `document` or `image_carousel`. |
| `impressions` | int | collection run | |
| `unique_impressions` | int | collection run | Not available through the browser route. |
| `clicks` | int | collection run | |
| `reactions` | int | collection run | |
| `comments` | int | collection run | |
| `shares` | int | collection run | |
| `engagement_rate` | formula | derived | `(reactions + comments + shares + clicks) / impressions`. |
| `follower_count` | int | collection run | Denominator at time of collection. |
| `collected_at` | datetime | collection run | When the row was last refreshed. |
| `source` | text | collection run | `api` or `manual`. |

## The `source` column

This column exists so that hand-entered numbers are never mistaken for API-sourced
ones. A row read off a dashboard by a person is `manual`; only a row written by an
authenticated metrics call is `api`.

Any later analysis that treats the two as equivalent should say so explicitly.

## Collection cadence

Engagement moves for roughly 72 hours after posting, so a single read at publish time
measures almost nothing. Each row is refreshed at **+24h, +72h and +7d**, overwriting
the metric columns and stamping `collected_at`.

Refreshes upsert on `post_urn`. Appending instead of upserting duplicates the row on
every refresh.

## Availability

| Route | Gives | Does not give |
|---|---|---|
| Page admin analytics, read through a browser | impressions, clicks, CTR, reactions, comments, reposts, engagement rate, follower counts and demographics | `post_urn`, unique impressions |
| Organization metrics API | the full row, keyed cleanly | nothing — but requires an approved organization read scope |

The browser route does not expose the post identifier. It has to be read from each
post's permalink before a row can be keyed for upsert.

## Derived figures worth naming

| Figure | Definition |
|---|---|
| Engagement rate | Interactions divided by impressions. High with low reach means distribution, not content, is the constraint. |
| Follower-relative reach | Impressions divided by follower count. Above 1 means the post travelled beyond the follower graph. |
| Reply rate | Comments divided by impressions. The honest test of a closing question. |

See [About the analytics](../explanation/analytics.md) for what these can and cannot
support at low volume.
