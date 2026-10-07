# How to add a content source

This guide shows you how to add a new topic to the rotation so the generation run
picks it up, without repeating an angle you have already published.

## Check what has been used

Open `src/posts/` and read the front matter of the most recent ten records. Each
carries a `rotation_topic` field and a short note on the angle it took.

A topic is available if it has not run in the last six weeks. An *angle* is available
only if no recent post covers the same ground — two carousels can both sit under
"cybersecurity awareness" and still collide badly.

If you are unsure, read the recap page (08) of the two most recent posts on that
topic. The recap is the fastest summary of what a carousel actually claimed.

At five posts a week against seven topics, the cycle is close to the reuse floor.
Adding topics is how you create room, not a nice-to-have.

## Add the topic to the rotation

Open `src/prompts/rotation.md` and add a numbered line:

```markdown
8. Recovering from a compromised account
```

Keep the line short and describe the reader's situation, not the format. "Easy
step-by-step instructions" describes a shape rather than a subject, and will produce
a different carousel every time it comes up.

## Write the research brief before anything else

Create `src/research/<topic-slug>.md` and fill it against
[the research standard](../reference/configuration.md#research-standard): primary
sources only, each figure fetched directly, each with the fetch date recorded beside
it.

If a figure will be out of date within a month, prefer a duller one that will not.

If you cannot verify a figure you wanted, put it under **Deliberately not used** with
the reason. Do not leave it out silently — someone will find it again next quarter
and use it.

**The brief is the reference the verification step checks against.** A figure that is
not in it cannot appear on a page, and a carousel built from a thin brief will abort
rather than publish. This is no longer only a quality standard; it is the input that
makes the automated check possible.

## Point the generation run at it

The weekend run reads the rotation file and takes the next unused topics. No further
wiring is needed.

To force a specific topic on the next run, add it to `src/prompts/next-run.md` as a
single line. That file is read first and cleared after a successful run.

## Verify

Trigger the generation run manually rather than waiting for the weekend.

Check that a queue entry was written. If the run aborted instead, the report names
the condition and the page — almost always a figure on a page that is not in the
brief. Fix the brief, not the page.

Then check the built design: that page 02 carries a source line, that every number on
it appears in your brief, and that no step title has wrapped onto a second line.

If a title has wrapped, shorten the copy — see
[copy-length limits](../reference/design-system.md#copy-length-limits).

## Related

- [Change the publishing schedule](change-the-schedule.md)
- [About the research standard](../explanation/research-standard.md)
- [About the verification model](../explanation/verification-model.md)
