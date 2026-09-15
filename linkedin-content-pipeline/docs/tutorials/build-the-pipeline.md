# Tutorial: build the pipeline

In this tutorial we will build a working content pipeline and publish one carousel
with it. Along the way we will encounter a research brief, a reusable design
template, an approval folder structure, and a scheduled publishing task.

We will do this end to end, and at the finish you will have one real, source-verified
carousel live on a page you control.

This will take about three hours the first time. Set aside a single session; the
steps build on each other and the middle is a poor place to stop.

You do not need to know the reasoning behind any of these choices to complete the
tutorial. When you want that, it is in [About the architecture](../explanation/architecture.md).

## Before we start

You will need an account with a design tool that can share directly to your social
platform, admin rights on the page you intend to post to, and a machine that can be
left awake at the posting time.

Create a folder called **LinkedIn Content** in your design tool, and two folders
inside it called **Pending** and **Approved**. We will use them in step 5.

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

Notice that you now have something you can hand to another person. That is the
point of this step.

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

## Step 5: put a human in the middle

Move your finished design into the **Pending** folder.

Now stop, and go and look at it as a reviewer rather than as its author. Check every
number against the brief you wrote in step 1. Check that no claim in the design is
missing its source line.

If something does not hold up, go back to step 1 and fix the brief first, not the
design.

When it passes, move it to **Approved**.

You have just performed the only step in this pipeline that a machine never does.

## Step 6: publish it

Open the approved design and find your design tool's share options. Search them for
your social platform.

Set four things, in this order:

1. Format: **Document** — not Image Post, which is the default
2. Pages: **all 8** — switching to Document usually changes this for you
3. Posting as: **your company page** — this defaults to your personal profile
4. Title and caption

Check the author selector one more time before you type anything. Then publish.

Open the live post and swipe it. The accent line should run continuously across all
eight pages in the feed, exactly as it did in the design.

## Step 7: schedule the next one

Your platform's share route almost certainly has no scheduling option. Ours does not.

So we schedule from outside it: create one task that fires at your posting time and
carries out step 6 at that moment. Give the task everything a person with no memory
of today would need — the design identifier, the four settings above, the exact
title, and the exact caption.

Add one more instruction to the task, in these words or your own:

> If anything is not as expected, stop and notify. Do not publish.

Now set it for tomorrow morning, and let it run.

---

## What you built

You have a research standard, a design template you will reuse indefinitely, a
three-folder approval path, a published carousel, and a scheduled task for the
next one.

You also now have a `Deliberately not used` section with at least one entry in it,
which will matter more in six weeks than anything else on this list.

## Next

- To add a second content source, follow [Add a content source](../how-to/add-a-content-source.md).
- To understand why the human step sits where it does, read [About the approval model](../explanation/approval-model.md).
