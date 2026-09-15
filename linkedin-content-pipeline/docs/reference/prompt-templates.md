# Prompt templates

The prompts the pipeline runs, in execution order. Full text is in `src/prompts/`.
Bracketed values are substituted at run time.

Templates are described here, not explained. For the reasoning behind their
constraints, see [About the research standard](../explanation/research-standard.md).

## `01-generate-ideas.md`

**Reads:** `business.md`, `strategy.md`, `rotation.md`, `src/posts/`
**Produces:** ten candidate ideas
**Substitutions:** `[WEEK]`, `[USED_TOPICS]`

Each idea returns a working title, a content pillar, a one-sentence angle, a hook
preview, and a call-to-action direction.

Constraints stated in the template: mix pillars across the week; at least two ideas
from the existing idea bank; propose no idea that depends on an unverified client,
statistic, partnership, or outcome; flag any fact requiring verification.

## `02-research-brief.md`

**Reads:** the selected idea
**Produces:** `src/research/<slug>.md`
**Substitutions:** `[TOPIC]`, `[DATE]`

Returns five to eight verified figures, each with claim, source URL, and fetch date;
a set of concrete examples; and a **Deliberately not used** section.

Constraints stated in the template: primary sources only; direct fetch required;
reject figures that will go stale within a month; never invent a number, client,
testimonial, partnership, or outcome.

## `03-write-carousel.md`

**Reads:** the research brief, `brand-guidelines.md`
**Produces:** eight pages of copy plus the caption
**Substitutions:** `[BRIEF]`, `[PILLAR]`, `[AUDIENCE]`

Returns per-page copy in the fixed page roles, a caption under the platform limit,
a document title under the title limit, and three to five hashtags.

Constraints stated in the template: one idea per page; no full paragraph on a page;
explain any jargon at first use; observe the copy-length limits in
[design-system.md](design-system.md#copy-length-limits).

## `04-build-design.md`

**Reads:** the carousel copy, the locator map
**Produces:** a design in the Pending folder
**Substitutions:** `[COPY]`, `[TEMPLATE_DESIGN_ID]`

Copies the template, then replaces text against the locator map. Resizes the
narrow elements on later pages before replacing their text. Does not rebuild pages
and does not re-style elements.

## `05-publish.md`

**Reads:** the approved design identifier, title, caption
**Produces:** a published document post
**Substitutions:** `[DESIGN_ID]`, `[TITLE]`, `[CAPTION]`, `[PAGE_NAME]`

Carries the full route and the four settings. Ends with an explicit failure
instruction: if anything is not as expected, stop and notify; do not publish, and do
not publish from the personal profile.

Each one-shot publishing task embeds a filled copy of this template, because the run
that executes it has no access to this session's context.

## `06-analyze-performance.md`

**Reads:** `src/posts/`, supplied analytics data
**Produces:** a written analysis
**Substitutions:** `[DATA]`

Constraint stated in the template: do not over-read small sample sizes.
