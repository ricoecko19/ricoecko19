# Tutorial: build the pipeline

In this tutorial we will build a working content pipeline and publish one carousel
with it. Along the way we will encounter a research brief, a reusable design
template, a set of automated checks, a scheduled publishing job, and a metrics row.

We will do this end to end, and at the finish you will have one real, source-verified
carousel live on a page you control, and a second one queued to publish on its own.

This will take about four hours the first time. Set aside a single session; the steps
build on each other and the middle is a poor place to stop.

You do not need to know the reasoning behind any of these choices to complete the
tutorial. When you want that, it is in [About the architecture](../explanation/architecture.md).

## Before we start

You will need an account with a design tool, admin rights on the page you intend to
post to, and a code repository with a CI runner — we will use scheduled jobs in step 6.

Create a folder called **LinkedIn Content** in your design tool, and two folders
inside it called **Pending** and **Verified**. We will use them in step 5.

---

## Step 1: write one research brief

We begin with research, before any design work, because the design step is fast and
the research step is not.

Pick one topic your audience actually deals with. We will use online scams as our
example, but yours can be anything.

Open a plain markdown file and find five facts from primary sources only —
government agencies, standards bodies, official vendor documentation, or
peer-reviewed papers. Not a blog summarizing them. The original.

For each fact, write three things: the claim, the URL you fetched it from, and
today's date.

Your file should look something like this:

```markdown
# Online scams — research brief
Researched 14 Sep 2026. All figures verified by direct fetch.

- $20.9B lost to cyber-enabled crime in 2025
  https://www.ic3.gov/AnnualReport/Reports/2025_IC3Report.pdf
- 62% of breaches involve the human element
  https://www.verizon.com/business/resources/reports/dbir/
```

Now add one more section at the bottom, headed **Deliberately not used**. Every time
you find a number you wanted but could not verify, write it there with the reason.
Leave the section empty for now if you have nothing. You will use it before the
tutorial is over.

Notice that you now have a file a machine can check a carousel against. That is what
this step is really for, and it becomes load-bearing in step 5.

## Step 2: turn the brief into eight pages

Open a second file and write your carousel as plain text, eight pages, in this
shape:

```
01 Cover        — the title, one line of promise, a swipe cue
02 Why it matters — three numbers from your brief, with the source line
03–07 Five steps  — each with: a title, one "why" line, a DO line, a DON'T line
08 Recap + CTA    — the five, then one question to the reader
```

Write all of it before you open a design tool. Pages 3 through 7 are the same shape
five times, so you are writing about eleven short strings per page.

Every number you write must already be in the brief from step 1. Do not round one,
and do not add a figure you remember but did not record. Step 5 will catch it, and
it is faster to not do it.

Keep each step title under 30 characters. This matters more than it sounds like it
does, and you will see why in step 4.

## Step 3: build the template once, by hand

Now open your design tool and create an eight-page design at 1080 × 1350.

Generate a **text-free background** for each page. Then add every word as a real
text element on top of it.

This ordering is not optional. If you ask a design generator to produce a page with
your copy in it, it will render the copy into the background image, and the result
will be unreliable — our first attempt produced a headline reading `AI-WRITEN` and
body text that was not language. Generate the background empty, add text as text.

Build page 3 carefully — progress bar, step label, title, why line, a light "DO"
card and a dark "DON'T" card — then duplicate it four times for pages 4 through 7.

Take your time here. You are building this page once, ever.

When page 3 looks right, add the connecting device: a single accent-colored line
that enters at the left edge, bends once, and exits at the right edge. Set page 4's
entry height to page 3's exit height. Continue across all eight pages.

Swipe through the design now. The line should appear to run unbroken across the
whole carousel. That effect is the reason to bother.

## Step 4: copy the template and replace the text

Here is where the work gets fast.

Duplicate the whole design. In the copy, replace the text of each element with the
copy you wrote in step 2 — do not rebuild anything, and do not re-style anything.
Page identifiers and element identifiers survive a duplication, and replacing an
element's text preserves its existing formatting.

Write down the identifier of each element you touched, page by page, in a file
called `docs/reference/locator-map.md`. That map is now valid for every copy you
ever make of this template.

You will probably find one or two titles wrapping onto a second line and colliding
with what is below them. That is the 30-character limit from step 2 asserting
itself. Shorten the copy rather than resizing the text.

Your second carousel will take under an hour. This is the whole reason the template
exists.

## Step 5: check it against the brief

Move your finished design into **Pending**, and now check it — mechanically, the way
the pipeline will.

Take every number that appears on any page. For each one, find the line in your step
1 brief that it came from. Not a line that is close. The line.

If a number on a page has no match in the brief, you have found the exact failure
this step exists for. Do not reword the page to make the number defensible, and do
not find a new source for it after the fact. Remove it, or go back to step 1 and
research it properly.

Then check the rest of the abort conditions: nothing invented, no political or legal
or medical claim, no confidential information, nothing planned described as done.

When it passes, move it to **Verified** and write a queue entry — date, design
identifier, title, caption, and the list of sources. An entry with an empty source
list will be refused later, which is the point.

## Step 6: publish it from a scheduled job

Write a small publisher that takes today's queue entry and does five things:

1. Refuses the entry outright if its source list is empty.
2. Uploads the exported PDF through the platform's documents API and waits for it to
   become available.
3. Creates the post **as the organization**, with the author taken from configuration
   rather than from the queue entry.
4. Reads the post back and confirms the author is the organization.
5. Writes the status and the post URL back to the queue.

Step 4 is not optional and is not the same as step 3 succeeding. A publish call that
returns successfully is a claim; reading the post back is evidence. If the read fails,
write `failed`, not `published`.

Run it once by hand against today's entry. Open the live post and swipe it — the
accent line should run continuously across all eight pages in the feed.

Now put it on a schedule. Two cron expressions per weekday, roughly an hour apart in
UTC, with the publisher exiting immediately on any run before your local cutoff time.
One will land near noon in summer, the other in winter, and you will never touch them
at a daylight-saving transition.

## Step 7: record what it did

Add a row for your published post: date, title, topic, the post identifier, and the
permalink. Leave the metric columns empty for now.

Come back in 24 hours and fill them in. Then again at 72 hours. You will see the
numbers move, which is the single most useful thing to learn before you start
reading reports — a figure captured at publish time measures nothing.

Add one more column called `source`, and write `manual` in it, because you read these
numbers off a screen. When you later automate collection, that column is what stops
the two kinds of row being quietly averaged together.

---

## What you built

You have a research standard, a design template you will reuse indefinitely, a
mechanical check that runs against the brief rather than against your judgement, a
scheduled publisher that verifies its own work, and the first row of a metrics log.

You also now have a `Deliberately not used` section with at least one entry in it,
which will matter more in six weeks than anything else on this list.

## Next

- To add a second content source, follow [Add a content source](../how-to/add-a-content-source.md).
- To understand what the automated checks can and cannot catch, read
  [About the verification model](../explanation/verification-model.md).
