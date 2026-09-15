# Environment variables

Every variable read by the pipeline. Placeholder values are in
[`.env.example`](../../.env.example); `.env` is gitignored.

| Variable | Type | Required | Description |
|---|---|---|---|
| `TEMPLATE_DESIGN_ID` | string | yes | Identifier of the proven eight-page design. Every carousel is a copy of it. |
| `FOLDER_CONTENT` | string | yes | Parent content folder. |
| `FOLDER_PENDING` | string | yes | Holds generated designs awaiting review. |
| `FOLDER_APPROVED` | string | yes | Holds reviewed designs cleared to publish. |
| `PUBLISH_AS_PAGE_NAME` | string | yes | Company page display name, matched against the author selector. |
| `PUBLISH_DEVICE_NAME` | string | yes | Machine that publishing tasks are bound to. |
| `GENERATION_CRON` | cron | yes | Generation schedule, in UTC. |
| `POSTING_TIME_LOCAL` | `HH:MM` | yes | Wall-clock posting time. |
| `POSTING_TIMEZONE` | IANA zone | no | Documentation only; the scheduler does not read it. |
| `POSTS_PER_WEEK` | integer | no | Defaults to `4`. |
| `CAROUSEL_PAGE_COUNT` | integer | no | Defaults to `8`. |
| `DOC_TITLE_MAX_CHARS` | integer | no | Defaults to `68`. Platform-imposed. |
| `CAPTION_MAX_CHARS` | integer | no | Defaults to `3000`. Platform-imposed. |

## Credentials

There are none in this file.

Authorization to the design tool and to the social platform is held by the connector
layer and granted interactively by a signed-in human. The pipeline holds no API key,
token, OAuth secret, or password, and none has been committed at any point in this
repository's history.

Design identifiers, folder identifiers, and scheduled-task identifiers are treated
as account details rather than secrets, and are also kept out of the repository.

See [About the architecture](../explanation/architecture.md) for why the pipeline
holds no credentials of its own.
