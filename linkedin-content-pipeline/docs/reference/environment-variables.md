# Environment variables

Every variable read by the pipeline. Placeholder values are in
[`.env.example`](../../.env.example); `.env` is gitignored.

## Design and generation

| Variable | Type | Required | Description |
|---|---|---|---|
| `TEMPLATE_DESIGN_ID` | string | yes | Identifier of the proven eight-page design. Every carousel is a copy of it. |
| `FOLDER_CONTENT` | string | yes | Parent content folder. |
| `FOLDER_PENDING` | string | yes | Holds generated designs before checks run. |
| `FOLDER_VERIFIED` | string | yes | Holds designs that passed every abort condition. |

## Publishing

| Variable | Type | Required | Description |
|---|---|---|---|
| `LINKEDIN_CLIENT_ID` | secret | yes | Application identifier. |
| `LINKEDIN_CLIENT_SECRET` | secret | yes | Application secret. |
| `LINKEDIN_REFRESH_TOKEN` | secret | preferred | Exchanged for an access token on every run. Requires the provider to have enabled programmatic refresh tokens for the application. |
| `LINKEDIN_ACCESS_TOKEN` | secret | fallback | Long-lived token, 60-day expiry. Used only when no refresh token is set. |
| `LINKEDIN_ORG_URN` | string | yes | The authoring organization. Fixed at this value for every post; never read from the queue. |
| `LINKEDIN_API_VERSION` | `YYYYMM` | yes | API version string. Versions sunset on a published schedule. |

## Schedule

| Variable | Type | Required | Description |
|---|---|---|---|
| `GENERATION_CRON` | cron | yes | Generation schedule, in UTC. |
| `PUBLISH_CRON_EARLY` | cron | yes | First weekday attempt, in UTC. |
| `PUBLISH_CRON_LATE` | cron | yes | Second weekday attempt, in UTC. |
| `PUBLISH_LOCAL_CUTOFF` | `HH:MM` | yes | The publisher exits on any run earlier than this local time. |
| `POSTING_TIMEZONE` | IANA zone | yes | Timezone the cutoff is evaluated in. |
| `POSTS_PER_WEEK` | integer | no | Defaults to `5`. |

## Metrics

| Variable | Type | Required | Description |
|---|---|---|---|
| `PAGE_NUMERIC_ID` | string | yes | Numeric page identifier. Admin analytics routes do not resolve the vanity slug. |
| `METRICS_SHEET_ID` | string | no | Spreadsheet destination. Blank writes the log to the repository instead. |
| `METRICS_SHEET_TAB` | string | no | Defaults to `post_log`. |
| `NOTIFY_EMAIL` | string | yes | Address notified on a failed or aborted run. |

## Where the secrets live

The six publishing values are secrets. They are stored in the CI provider's
encrypted secret store and injected into the job environment at run time. They are
not in the repository, not in any committed file, and not in the job logs — the
publisher must not print a token on any code path, including error handlers.

Everything else is configuration rather than secret and may be committed.

This is a change from the pipeline's earlier posture, under which it held no
credentials at all because authorization lived in an interactive connector. Moving
to a direct API integration bought reliability and cost that property. See
[ADR 0010](../adr/0010-publish-via-github-actions.md).

### Token expiry is an operational hazard

Where the fallback access token is in use, publishing stops 60 days after the token
was issued, and the first symptom is a failed run. Set a calendar reminder for 50
days out, and treat migration to a refresh token as the real fix.
