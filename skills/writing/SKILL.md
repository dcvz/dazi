---
name: writing
description: Write or revise prose that doesn't read as machine-written — docs, READMEs, PR descriptions, issue and PR comments, release notes, emails, chat replies, commit bodies. Use before drafting any prose, and when auditing text you or another agent already wrote.
---

# Writing

Everything below is a habit, not a rule you broke. The fix is to notice it while
drafting, not to sand it off afterwards.

These are signals, not proof. Every one of them shows up in human writing too.
What gives generated text away is density: several in a paragraph, or the same
one in every paragraph. Over-correcting has its own smell: prose that visibly
avoids ordinary words reads worse than prose that used one. Judgment, not a
linter.

## Vocabulary

Watch for: delve, crucial, pivotal, underscore, showcase, highlight (as a verb),
emphasize, foster, garner, bolster, leverage (as a verb), enhance, align with,
robust, seamless, comprehensive, meticulous, intricate, tapestry, landscape (as
an abstract noun), testament, enduring, vibrant, boasts (meaning "has"),
valuable, key (as an adjective), deep dive.

Also the connective throat-clearing: *Additionally*, *Moreover*, *Furthermore*,
*It's worth noting that*, *It's important to*.

One is a coincidence. Several in a paragraph is worth a second look. The point
is not that these words are banned, it's that reaching for them is a reflex and
the plain word you skipped was usually better. Swapping in a synonym does not
fix it — the tell is the abstraction, not the specific word, so *pivotal* to
*vital* changes nothing. Write the plain version instead.

Intensifiers and adverbs work the same way: *actually*, *really*, *genuinely*,
*truly*, *properly*, and *quietly*, *deeply*, *fundamentally*, *remarkably*. "The
actual problem" is the problem. Delete the word when it adds weight without
changing the meaning.

## Sentence shapes

**Negative parallelism.** "Not just X, but Y." "It's not X, it's Y." "No X, no
Y, just Z." Contrast can carry a real distinction, and in a correction it's
often the clearest thing to write. The tell is reaching for it by default, when
no one held the belief being corrected. If the foil is invented, drop it and say
the thing. ("X rather than Y" is the weakest member of this family — frequently
it's just the shortest accurate comparison.)

**Rule of three.** Three adjectives or three parallel clauses where two would do.
Triads make a thin point sound furnished. If a sentence has three of anything,
check whether the third earns its place.

**Copula avoidance.** *serves as*, *stands as*, *functions as*, *represents*,
*marks*, *reflects*, *offers*, *features*, *maintains*, *boasts*. These replace
plain *is* and *has*. "The script serves as the entry point" is "the script is
the entry point" wearing a costume.

**Vague connection.** *in connection with*, *associated with*, *related to*,
*in the context of*. If two things are connected, say how: caused by, used for,
written by, part of.

**Set-ups.** "The result? Devastating." "Here's the catch." "Here's what most
people miss." A question nobody asked, or a drumroll before the point. Make the
point.

**Counting.** "For two reasons." "Three things stand out." The count tells the
reader nothing the list doesn't, and it goes stale when someone adds an item.

**Comma-clipped tails.** A short fragment hung off a comma to close a sentence:
"asked forty times, mentoring." Work the fragment into the sentence, or cut it.

**Uniformity.** Paragraphs that all open the same way, sentences that run to the
same length, sections that all follow claim then explanation then imperative.
Human writing is lumpy. A three-word sentence sits next to a long one; a section
runs two lines because there wasn't more to say. Prose containing none of the
tells above can still read as generated when every unit is the same size and
shape.

## Names

**Synonym cycling.** Pick one noun for one thing and repeat it. A dashboard that
becomes an interface, then a portal, then the analytics hub reads as four
different things.

**Invented labels.** "The supervision paradox", "config creep", "the onboarding
trap": a coined compound used as if it were an established term. Describe the
thing instead.

**Self-echo.** Reusing your own earlier phrase as if paying it off. It reads as a
callback, and is usually narrow vocabulary resurfacing.

## Formatting

**Bold lead-in lists.** A bullet, a bolded phrase, a colon, then a sentence,
repeated down the page. It's one of the most common tells in agent output, and
it's usually prose that got shredded into fragments. Write the paragraph.

Lists are fine when the items form a real set, such as options or ordered steps.
The tell is a list of *arguments*, where the connective tissue between points was
the actual content and it has been thrown away.

**Boldface as emphasis.** Bolding every term you consider important turns a page
into a slide deck and tells the reader nothing, since everything is emphasized.
Bold works for scan targets in reference material — labels, defined terms,
repeated fields. It stops working as a way of sounding emphatic.

**Em dashes where a comma would do.** Not a ban. An em dash is right for a sharp
aside or a reversal. It is wrong as a general-purpose connector, three times a
paragraph.

**Also:** headings in Title Case, generic Wh-headings that could sit on any
section ("Why it matters"), a heading that restates the document title,
headings that contain only more headings, emoji as bullets or section markers,
horizontal rules between every section, curly quotes pasted in from nowhere, and
tables for things that aren't tabular.

## What the prose is doing

**Significance inflation.** "Marked a turning point." "Cemented its place." "Has
become an essential part of." Grand claims about importance or legacy, attached
to something ordinary.

**The future-prospects ending.** "Despite these challenges, the outlook remains
promising." A closing paragraph about challenges ahead and potential yet to be
realized, which carries no information and exists because the shape of an essay
seemed to demand one.

**Closing restatement.** A final paragraph summarizing what the reader just read.
If they read it, they know. Stop at the last real sentence.

**Scene-setting openings.** "In today's fast-paced development environment..."
Throat-clearing before the first real sentence. Start at the point.

**Vague attribution.** "Experts say." "It is widely regarded as." "Industry
reports suggest." Name who, or cut the claim.

**Invented specificity.** Version numbers, dates, benchmarks, and quotations that
sound precise and were never checked. Fabricated detail is worse than admitted
vagueness, because it survives review by looking rigorous.

**Hedge stacking.** "This may potentially cause somewhat unexpected behavior in
certain cases." Each qualifier drains the sentence. Either you know it or you
don't; say which.

**Quotable one-liners.** A standalone line built to be quoted: "Every metric
that rewards volume punishes leverage." If it carries no information, cut it.

**Docs as a diary.** A doc says how things are now. "We changed X to Y",
"previously" and "now uses" belong in the commit or the PR.

**Analysis that isn't.** A paragraph that describes what something does and calls
it significance. If removing the sentence loses no information, it was filler.
So does a trailing *-ing* clause that claims significance without explaining
it: "..., highlighting its importance", "..., reflecting broader trends".

## Replies

Lead with the answer, the change or the decision. Don't announce what's coming
("Two constraints shape the design.") or restate the question, and make the
point before the evidence for it.

Leave out the working: what you checked, weighed or plan to do next. Explain the
reasoning when it's asked for, or when the reader needs it to act.

Match the detail to the ask. A yes-or-no question usually needs only the answer
and any qualification that changes it; offer more rather than giving it unasked.

## Assistant register in things that aren't chat

Chat habits leaking into documents and review comments: "Of course!",
"Certainly!", "You're absolutely right", "I hope this helps", "Let me know if
you'd like...", "Would you like me to...". Also meta-commentary about the writing
("This section covers...", "In this document we will..."), unrequested next
steps appended at the end, and the teaching voice: "Let's break this down",
"Let's dive in", "Think of it as..." before an analogy nobody asked for.

A README does not address the reader as a client. A PR comment is not a support
ticket reply.

## Register

Short, varied, shaped by what it's responding to. Sentences of
different lengths. A thought that runs on a little. Backticked paths. Apologizing
for a slow reply when it was slow. Leaving the obvious unsaid.

Two messages should not share a skeleton. If a reply follows thanks, then
context, then policy, then a promise, and the last one did too, it reads as a
template no matter how the words differ.

## Auditing a draft

Reading for these is more reliable than writing around them, because a draft
holds still.

1. Read once for shape. Does it open by restating the title, close by restating
   itself, or end on future prospects? Cut those.
2. Scan for the vocabulary list and for *serves as* / *represents* / *not just X
   but Y*. Replace with the plain version, one at a time.
3. Count bold lead-in bullets. If the list is arguments rather than a set, turn
   it back into paragraphs.
4. Count em dashes and triads. Keep the ones doing work.
5. Check the shape of the paragraphs against each other. If they all open the
   same way or run the same length, break the pattern.
6. Read the first and last sentence together. If the last adds nothing to the
   first, delete it.

Report what you cut and why, briefly. Do not list every change.
