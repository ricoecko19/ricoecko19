# About the verification model

This page is about what replaced the human approval gate, why the replacement is
shaped the way it is, and what it does not catch.

It supersedes an earlier page about the approval model. The decision itself is
recorded in [ADR 0009](../adr/0009-automated-checks-replace-approval-gate.md), and
the reasoning that produced the original gate is preserved unedited in
[ADR 0004](../adr/0004-single-human-approval-gate.md). Reading them in order is the
honest version of this story.

## What changed

The pipeline used to have exactly one human step: a person reviewed the week's batch
in a single session and either approved it or sent it back. Nothing published without
that.

That step was removed. Content now generates, verifies itself against a set of abort
conditions, and publishes with no human in the path. The folder structure that used
to represent approval now represents verification state and nothing else.

## Why abort conditions rather than a checklist

The natural way to automate a review is to score it: run a list of checks, count the
failures, and publish if the score is good enough. That design is wrong here, and the
reason is worth being precise about.

A score lets a serious failure be outweighed by trivial passes. An invented statistic
is not 12% of a problem, and it is not compensated for by having the right number of
hashtags. So the checks are split into two kinds, and they behave differently.

**Abort conditions** stop the carousel. Nothing is published, nothing is queued, and
a report goes out. There is no threshold, no override, and no count — one failure is
the whole answer. These cover the things that damage credibility irreversibly: an
unsourced figure, an invented client or partnership or outcome, a claim in a category
the organization is not authorized to make, confidential information, a plan
described as a result, and a post authored as anything other than the organization.

**Fix-then-publish checks** cover the things that are merely wrong. Tone, hashtag
count, call-to-action repetition, the colour ratio, copy-length limits. These are
corrected in place and the run continues.

The test for which list a check belongs on is simple: if the failure were published,
could it be fixed by editing afterwards? Hashtag count, yes. A fabricated statistic
that somebody screenshotted, no.

## Why the brief is the reference

The central abort condition is that every statistic on every page must trace to a
figure captured in the research brief for that topic.

This is what makes mechanical verification possible at all. The check is not asking a
model whether a claim seems plausible — that would be the same judgement that was
removed, performed less reliably. It is matching a number on a page against a number
in a file that was assembled under a stricter standard, at an earlier time, with its
source URL and fetch date attached.

Two rules keep that from degrading. The verification step may not go and find a new
source for an unmatched figure, and it may not substitute an approximate figure or
drop an attribution to make a page publishable. Either would turn the check into a
negotiation with itself.

## Why reading the post back is a separate thing

A publish call that returns successfully is a claim. Reading the post back and
finding it, authored by the organization, is evidence.

This exists because of a specific incident. Four of five scheduled posts in one week
never reached the page, and the log recorded all five as published, because the log
entry had been written at scheduling time rather than after a confirmed post. The
discrepancy went unnoticed for two weeks.

So the rule is: a run that cannot read its post back writes **Failed**, not
Published. A status that was never verified is worse than a missing one, because it
stops anyone from looking.

## Why the author is configuration, not data

The organization identifier is read from configuration and fixed for every post. The
publisher does not accept an author from the queue entry, and there is no code path
that falls back to a personal profile.

Under the old browser route this was the single most dangerous step, because the
author was a dropdown that defaulted to the wrong value and did not remember the
previous selection. Making it a constant does not reduce the consequence of getting
it wrong — a post under the wrong identity still cannot be quietly fixed — it removes
the opportunity.

This is the clearest win from the change. It is worth noticing that it came from
changing the route, not from automating the review.

## What this does not catch

Stated plainly, because the rest of the page would be misleading without it.

The checks verify that a figure is sourced. They do not verify that the sentence
around it is a fair characterization of the source.

The one real correction the human gate ever caught was exactly that: a page claiming
a category of fraud had "doubled" when the underlying figures were 17,367 to 32,424,
a factor of 1.87. Every number on the page was present, correct, and correctly
attributed. The word was wrong. A check matching numbers against a brief passes that
page without hesitating.

There are two partial mitigations. Briefs record figures in the source's own terms,
so copy that stays close to the brief inherits its accuracy. And the weekly report
surfaces what was published, which makes the failure discoverable after the fact
rather than never.

Neither is a substitute. The residual risk is a fully sourced page that still says
something untrue, and it is accepted knowingly rather than overlooked. Anyone
inheriting this pipeline should know that is the gap, and that the cheapest way to
close it is a periodic read of published work against its briefs — after publication,
but by a person.

## Related

- [About the architecture](architecture.md)
- [About the research standard](research-standard.md)
- [ADR 0009: automated checks replace the approval gate](../adr/0009-automated-checks-replace-approval-gate.md)
- [ADR 0004: the original approval gate](../adr/0004-single-human-approval-gate.md), superseded
