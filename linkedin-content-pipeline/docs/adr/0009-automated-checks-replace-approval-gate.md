# 0009. Replace the approval gate with automated abort conditions

Date: 2026-10-06

## Status

Accepted

Supersedes [0004](0004-single-human-approval-gate.md)

## Context

[0004](0004-single-human-approval-gate.md) put one human review between design and
publishing, on the reasoning that a rules check can confirm a source line exists but
cannot confirm that the claim above it is fairly characterized.

That reasoning has not been refuted. What changed is the cost of the gate in
practice. Review is one person's attention, available intermittently, and the gate
sits on the critical path of every post. Content that cleared generation sat waiting;
volume was capped by review capacity rather than by anything about the content; and a
week where the reviewer was unavailable was a week with no posts.

The organization's owner directed that the gate be removed, including founder
sign-off, so that publishing runs unattended.

The design question was therefore not whether to remove the gate but what has to be
true for removal to be survivable.

## Decision

Remove the human approval step from the publishing path entirely. No human or founder
sign-off of any kind. Replace it with two classes of automated check.

**Abort conditions.** One failure stops the carousel. Nothing is queued, nothing is
published, and a report goes out. No threshold, no score, no override:

- a statistic that does not trace to a source captured in the research brief
- an invented client, partnership, testimonial, outcome, funding award, or milestone
- a political, legal, or medical claim; a security claim not stated by an official source
- confidential client, employee, or student information
- a future plan presented as a confirmed result
- an author that is not the organization

**Fix-then-publish checks.** Corrected in place, run continues: tone, hashtag count,
call-to-action variation, the colour ratio, copy-length limits.

The verification step may not find a new source for an unmatched figure, substitute
an approximate figure, or drop an attribution to make a page publishable.

The folder structure is retained as an audit trail of verification state. It no
longer represents approval, because nothing is approved.

Every run reports its outcome — published, failed, or aborted — because without a
review step the report is the only signal that anything happened.

## Consequences

Publishing no longer waits on a person. Volume is now limited by research capacity
rather than review capacity, which is what made [0011](0011-five-posts-per-week.md)
possible.

The split between abort and fix-then-publish is doing the real work. A score would
let an invented statistic be outweighed by correct hashtags; the test for which list
a check belongs on is whether the failure could be fixed by editing after
publication.

Making the brief the reference is what allows the central check to be mechanical. The
check matches a number on a page against a number in a file assembled earlier under a
stricter standard, rather than asking a model whether a claim seems plausible — which
would be the removed judgement, performed worse.

**The gap this leaves is real and is accepted knowingly.** The checks verify that a
figure is sourced. They do not verify that the sentence around it characterizes the
source fairly. The one correction the human gate ever caught was precisely that kind:
a page reading "government impersonation doubled" against underlying figures of
17,367 to 32,424, a factor of 1.87. Every number was present and correctly
attributed; the word was wrong. These checks pass that page.

Partial mitigations: briefs record figures in the source's own terms, so copy that
stays close to the brief inherits its accuracy, and the weekly report makes published
work reviewable after the fact. Neither is equivalent to the removed step.

The cheapest way to close the remaining gap, if it is ever judged worth closing, is a
periodic read of published carousels against their briefs — after publication, by a
person, off the critical path. That would recover most of what [0004](0004-single-human-approval-gate.md)
provided without reintroducing the blocking cost that motivated this record.
