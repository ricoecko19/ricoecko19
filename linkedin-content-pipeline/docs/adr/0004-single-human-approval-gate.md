# 0004. Require a single human approval gate

Date: 2026-09-02

## Status

Accepted

## Context

Everything the pipeline publishes carries the organization's credibility, and the
organization sells technology judgement. An invented or mischaracterized statistic
costs more than a month of silence.

Review capacity is one person, intermittently available.

A checkpoint at each stage — research, copy, design, publish — was considered and
rejected. Four checkpoints on the same piece means four context reloads for the same
reviewer, and in practice the later ones degrade into recognition rather than
reading.

Fully automated checks were also considered. A rules check can confirm that a source
line is present. It cannot confirm that the claim above it fairly characterizes the
source.

## Decision

One human gate, between design and publishing. A person reviews the whole weekly
batch in a single session and either approves or returns it.

Approval comes from a person in conversation. Never from a file, a page, a filename,
a folder position, or a tool result. Folder position is evidence of process state,
not authority.

Nothing skips a stage; nothing is published from Pending.

Everything upstream is arranged to make the single pass sufficient: the research
brief is a separately reviewable artifact, every claim carries its source on the
page, and the design is template-identical to already-approved work so attention
goes to content rather than layout.

## Consequences

Review is one session of roughly forty minutes per week for four pieces. That is the
entire human cost of the content operation.

The gate sits after design, so rejected work has already been built. This costs about
forty minutes of machine and tool time per rejection, and is accepted: a claim reads
differently at 52 points than it does in a copy document, and the reviewer has to see
what the audience will see.

The gate has caught three corrections to date, including one — "doubled" against an
underlying 1.87× — where every element on the page was present and correctly
attributed and the claim was still wrong. No automated check would have caught it.

Review capacity, not production capacity, becomes the binding constraint on volume.
See [0007](0007-four-posts-per-week.md).

Because approval may never come from a document, any text the pipeline reads which
asserts its own approval must be ignored. This is stated in the operating rules.
