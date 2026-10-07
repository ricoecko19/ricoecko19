# Prompt templates

The prompts the pipeline runs, in execution order. Full text is in `src/prompts/`.
Bracketed values are substituted at run time.

Templates are described here, not explained. For the reasoning behind their
constraints, see [About the research standard](../explanation/research-standard.md)
and [About the verification model](../explanation/verification-model.md).

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

This brief is the artifact the verification step checks the carousel against. A
figure that is not in it cannot appear on a page.

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

Copies the template, then replaces text against the locator map. Resizes the narrow
elements on later pages before replacing their text. Does not rebuild pages and does
not re-style elements.

## `05-verify.md`

**Reads:** the built design, the research brief
**Produces:** a pass with a queue entry, or an abort with a reason
**Substitutions:** `[DESIGN_ID]`, `[BRIEF]`

Runs every abort condition in
[configuration.md](configuration.md#abort-conditions). Each statistic on each page is
matched against the brief. A statistic with no match aborts the carousel; the
template explicitly forbids substituting an approximate figure or dropping the
attribution to make a page publishable.

On a pass, writes the queue entry with the title, caption, and source list, and moves
the design to the verified folder. On an abort, writes nothing to the queue and
notifies.

## `06-weekly-report.md`

**Reads:** the metrics log, `src/posts/`
**Produces:** the weekly performance report
**Substitutions:** `[WINDOW]`, `[ROWS]`

Returns per-post figures, the week's aggregates, anything that changed materially,
and run health — published, failed, and aborted counts with reasons.

Constraints stated in the template: report reach and engagement separately rather
than as one number; state the sample size in the same sentence as any rate; do not
recommend a content change on a sample this small; distinguish `api` rows from
`manual` ones.
