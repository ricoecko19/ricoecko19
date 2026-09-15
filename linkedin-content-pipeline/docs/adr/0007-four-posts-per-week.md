# 0007. Publish four carousels per week

Date: 2026-09-06

## Status

Accepted

## Context

An initial target of twelve posts per week was discussed. The pipeline can produce
that; production is not the constraint.

The constraint is [0004](0004-single-human-approval-gate.md). One person reviews the
whole batch in one session, and review quality falls off sharply somewhere past six
pieces — the reviewer stops reading and starts recognizing.

There is a second constraint. Each piece requires a research brief built on primary
sources, and research is the slowest stage and the one that cannot be automated.
Twelve briefs a week would either not get written or would get written thinly, and a
well-designed carousel on a thin brief is worse than nothing because it looks
authoritative.

There is also a topic constraint: the rotation holds seven topics, and reusing an
angle inside six weeks produces visibly repetitive output.

## Decision

Four carousels per week, one each on Monday, Tuesday, Wednesday and Thursday at
09:00 local time.

Scale up only after the pipeline has run at this volume without a quality failure,
and only if review capacity genuinely supports it.

## Consequences

The weekly review stays at roughly forty minutes, which is what makes it get done.

Four per week is below what the organization's audience could absorb. This is a
deliberate trade of reach for credibility, appropriate for an organization selling
judgement and probably wrong for one selling volume.

Friday is left empty, which has been useful as slack for a post that slipped.

Increasing volume requires adding rotation topics first; at four a week the existing
seven cycle roughly every six weeks, which is already close to the reuse floor.
