---
editor_options: 
  markdown: 
    wrap: 72
---

# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ``` bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside
> this project. Run section 1, which reports the hardware you'll be
> training on. Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The
> notebook runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

------------------------------------------------------------------------

```{=html}
<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->
```

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

```{=html}
<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
```

TakeMeter classifies posts from r/formula1 by what the poster is trying
to do. Every post gets one of three labels: `question`, `news`, or
`take`. The goal is to separate posts that ask for or pass along
information from posts that mainly express the poster's own view, joke
or feeling.

------------------------------------------------------------------------

## Label Taxonomy

```{=html}
<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->
```

### `question`

**Definition: the poster sincerely wants an answer, whether a fact, a
first hand experience or a recommendation, and a serious informative
reply would satisfy them.**

**Example 1:** \> Is this difference in 2017 cars to 2025 cars accurate
or is it a Bburago inaccuracy?

**Example 2:** \> Life of a formula 1 engineer: Any F1 engineer (or
former) out there? I'd like to know about work-life balance in the
circus for factory guys and trackside engineers.

### `news`

**Definition: the post passes along information from an outside source,
such as an announcement, an article or a data point with little or no
stance from the poster.**

**Example 1:** \> Pirelli have chosen to go one step softer on the hard
tyre for every race of the next triple header, compared to last year

**Example 2:** \> Jos Verstappen was hyped like Max - until it all went
wrong

### `take`

**Definition: the poster is expressing their own view, joke or feeling,
and the post would still make sense if nobody replied with
information.**

**Example 1:** \> F1 should try out their luck at Le Mans

**Example 2:** \> Why don't FIA give Safety Car duty to Ferrari for
once?

### The hardest boundary

**Which two labels: question vs take**

**The decision rule I used every time:**

If the post's main point is a question to the community — asking for a
fact, a first-hand experience, or an opinion or prediction ("Who wins
P2?", "Are we too harsh on Ferrari's race strategy?") — it's `question`,
even if the poster gives their own view along the way. If the main point
is the poster's own claim and a "What do you think?" is only tacked on
at the end ("Hot take: RBR are deciding between Lindblad and Tsolov…
What do y'all think?"), it's `take`. Rhetorical questions, jokes and
complaints are `take` too ("Why don't FIA give Safety Car duty to
Ferrari for once?").

------------------------------------------------------------------------

## The Dataset

```{=html}
<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->
```

**Where the posts came from:**

r/formula1, a public subreddit. On 18 October 2026 I saved six of its
public listings as JSON (`data/raw_data/`): hot, new, top of the week,
and the Discussion, Technical and Statistics flairs.
`scripts/json_to_csv.py` drops stickied and removed posts and titles
under five words, and de-duplicates across listings. That leaves 456
posts in `data/raw_posts.csv`. I left out mod threads (daily discussion,
live race threads, "rate the race", bingo) and one survey. From the rest
I kept every post that might be a `question` or a `take`, plus a random
sample of the `news` posts, for 263 in `labels.csv`. Each post is the
title plus the body text, unedited.

**How I labelled them:**
<!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

I labelled 20 posts cold first by myself. The other 243 were
pre-labelled by Claude from my definitions, then re-checked by Claude
against my decision rule after I decided that opinion questions count as
`question`. And then I read through the results.

**Counts per label:**

| Label      | Count | Share |
|------------|-------|-------|
| `news`     | 133   | 51%   |
| `question` | 74    | 28%   |
| `take`     | 56    | 21%   |
| **Total**  | 263   | 100%  |

**Three hard cases**

```{=html}
<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->
```

**1.** \> *The post:* \>

```         
"Are we too harsh on Ferrari's race strategy?

I was thinking about Bahrain GP, and I felt that people often criticized Ferrari in the race strategy a little bit too aggressively. 

I completely agree Ferrari should not have gone with slicks, and that was bad race strategy, but keeping Hamilton out and not pitting him on the softs was actually very valuable and had allowed him to work up the ranks. I think there are a lot of other examples of this, where, in the moment, we're highly critical of Ferrari compared to other teams, but they may not fully deserve it.

Don't get me wrong. I think Ferrari has serious communication and strategy issues, but are we being too critical of them?"
```

\> *Could have been:* \> take

\> *I chose question, because:* the title is the question and the whole
post builds up to "are we being too critical of them?". The poster gives
a view, but the main point is asking the sub to weigh in. Under my rule
that's `question`.

**2.** \> *The post:* \>

```         
"Langkawai Boys trip Triple Header Progress Update

OP: Malaysia P6, Singapore Sprint DNF  
G dawg: Malaysia DNF, Singapore Sprint DNF  
Albino: Malaysia DNF, Singapore Sprint DNF  
Ollieb: Malaysia P14, Singapore Sprint P6  
Gabriii: Malaysia P18, Singapore Sprint DNF  
55: Malaysia P17, Singapore Sprint P12 (also cursed with the FIA)  
Lando: Malaysia P9, Singapore Sprint P8

What’s your prediction for tomorrow?

Edit: Sorry typo Langkawi! Can’t edit title anymore"
```

\> *Could have been:* \> news

\> *I chose question, because:* the "standings" are an in-joke between
the poster's friends, not information from an outside source, so it
isn't `news`. The one thing the post actually asks for is "What's your
prediction for tomorrow?".

**3.** \> *The post:* \>

```         
"Seamless Camera transition from handheld pit cam to onboard cam (using AI)

I noticed this smooth transition between a handheld camera and Max's onboard shown during a replay of a pit stop and thought it was quite interesting.

What do you think about using AI (which I presume this is) in this way?

I think, if this can be pulled off during the live broadcast, this would be a very cool and immersive action shot."
```

\> *Could have been:* \> take

\> *I chose question, because:* the post shares a clip, but what it
actually asks is "What do you think about using AI … in this way?". The
poster's own opinion only comes after the question.

------------------------------------------------------------------------

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed --> 3 epochs,
learning rate 2e-5, batch size 16, max length 128 tokens, seed 42.
Trained on **`mps` (Apple Silicon GPU)**, torch 2.14.1, as recorded in
`results.json`.

**Anything I changed from the defaults, and why:** Nothing. All
defaults.

**Split sizes:**
<!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. --> train 183 · val 40 · test 40

| Label      | train | val | test |
|------------|-------|-----|------|
| `question` | 52    | 11  | 11   |
| `news`     | 93    | 20  | 20   |
| `take`     | 38    | 9   | 9    |

Every label has at least 8 in test, but `take` only just clears it with
9, so its F1 will jump around between seeds next unit.

------------------------------------------------------------------------

## How I Used AI

```{=html}
<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->
```

**Moment 1**

-   *What I asked for:* I gave Claude my three definitions and the
    scraped posts, and asked it to pre-label them.
-   *What came back:* a first pass that put opinion polls like "Who wins
    P2?" under `take`, on the grounds that an opinion question has no
    single "informative" answer.
-   *What I changed:* I angrily disagreed. A poster asking "Who wins
    P2?" sincerely wants answers, so that's a `question`. I wrote the
    decision rule above, and about 30 rows moved from `take` to
    `question`. My own cold labels then sharpened the rule: "Hot take: …
    What do y'all think?" stays `take`, because the claim is the point
    and the question is tacked on.

**Moment 2**

-   *What I asked for:* I asked Claude to scrape reddit r/Formula 1 for
    me.
-   *What came back:* Claude refused and we went back and forth a couple
    of times, then I have to give up. And switch tactics.
-   *What I changed:* I hand copy pasted json files and asked Claude to
    clean it into raw posts. That worked.

**Moment 3**

-   *What I asked for:* I asked Claude to to format this and criteria
    file for me: adding the quotes as blocks and clean out any grammar
    mistake.
-   *What came back:* Claude helped part of the formatting, but somehow
    messed up most of it.
-   *What I changed:* I cleaned the format again by myself.

**Pre-labelling disclosure:**

243 of the 263 rows were pre-labelled by Claude. I labelled the 20
`cold` rows myself, before looking at any pre-labels, and hand-checked
14 of the pre-labelled rows (note: `checked by me`). Claude re-checked
the other 229 against my decision rule, but I did read all of them.

```{=html}
<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->
```

------------------------------------------------------------------------

## Baseline vs. Trained

```{=html}
<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->
```

| Measure          | Baseline | Trained | Difference |
|------------------|----------|---------|------------|
| Overall accuracy | 0.600    | 0.525   | -0.075     |
| Macro F1         | 0.419    | 0.281   | -0.138     |
| F1 — `news`      | 0.731    | 0.690   | -0.041     |
| F1 — `question`  | 0.526    | 0.154   | -0.372     |
| F1 — `take`      | 0.000    | 0.000   | +0.000     |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

I predict my trained model will beat the baseline only on `news`,
because news posts share surface cues (source tags like “[The Race]”,
numbers, standings) that 183 training examples can pick up. I expect the
baseline to win on `question` and `take`, because that boundary depends
on what the poster intends, which my written definitions spell out and
the trained model only saw in about 90 examples.

**How the prediction held up:** wrong on `news`, where the baseline beat
the trained model too (0.731 vs 0.690). Right that the baseline would
win on `question` (0.526 vs 0.154). Wrong on `take`: neither model got a
single `take` right.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

The zero-shot baseline beat my fine-tuned model on every measure. My
fine-tuning added nothing; it made things worse.

The biggest gap is `question` (0.526 vs 0.154). The baseline could read
my definition of "sincerely wants an answer", while the trained model
never learned to separate questions from news.

Both models scored 0.000 on `take`, but the bare-names run below shows
that's not because `take` is impossible to find. My definitions raised
accuracy by 0.075 only by pushing posts toward `news` , and they cost
0.307 on `question` and all 0.414 on `take`. Measured by accuracy, my
definitions were worth +0.075; measured by macro F1, they were worth
−0.120. So accuracy alone would have told me my definitions helped, when
for my two minority labels they hurt.

My best guess at why: the `take` definition is phrased mostly by what
the post is *not* "would still make sense if nobody replied with
information", and the `news` definition is broad enough to describe
almost any post that mentions a fact.

**Definitions vs. bare label names** (same 40 held-out posts, same
zero-shot model, `facebook/bart-large-mnli`):

| Measure | Baseline, bare names | Baseline, my definitions | Trained | Definitions worth |
|--------------|--------------|----------------|--------------|--------------|
| Overall accuracy | 0.525 | 0.600 | 0.525 | +0.075 |
| Macro F1 | 0.539 | 0.419 | 0.281 | −0.120 |
| F1 — `news` | 0.370 | 0.731 | 0.690 | +0.361 |
| F1 — `question` | 0.833 | 0.526 | 0.154 | −0.307 |
| F1 — `take` | 0.414 | 0.000 | 0.000 | −0.414 |

The baseline in the main table above is the run with my definitions
(`baseline_results.json`); the bare-names run is in
`baseline_bare.json`.

------------------------------------------------------------------------

## Run Log — Before

```{=html}
<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->
```

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|-----------|--------|---------|--------|-----------|---------|
| 1\.       |        |         |        |           |         |
| 2\.       |        |         |        |           |         |
| 3\.       |        |         |        |           |         |
| 4\.       |        |         |        |           |         |
| 5\.       |        |         |        |           |         |

### Confusion matrix

```{=html}
<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->
```

| true  predicted |     |     |     |
|-----------------|-----|-----|-----|
| \*\*\*\*        |     |     |     |
| \*\*\*\*        |     |     |     |
| \*\*\*\*        |     |     |     |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

------------------------------------------------------------------------

## Verdicts and Diagnoses

```{=html}
<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->
```

| \#  | Criterion | Target | Verdict | How I decided |
|-----|-----------|--------|---------|---------------|
| 1   |           |        |         |               |
| 2   |           |        |         |               |
| 3   |           |        |         |               |
| 4   |           |        |         |               |
| 5   |           |        |         |               |

**Diagnoses**

```{=html}
<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->
```

------------------------------------------------------------------------

## Agreement Report

```{=html}
<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->
```

**Agreement rate:** \_\_\_ / 30 = \_\_\_%

```{=html}
<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->
```

**Disagreements**

```{=html}
<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->
```

**1.** \> *The post:* \> \> *Staff said / I said:* \> \> *My call, and
why:* \> \> *Which pile:*

**2.** \> *The post:* \> \> *Staff said / I said:* \> \> *My call, and
why:* \> \> *Which pile:*

**What the pattern in my disagreements tells me:**

------------------------------------------------------------------------

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|-----------|--------|---------|--------|-----------|---------|
| 1\.       |        |         |        |           |         |
| 2\.       |        |         |        |           |         |
| 3\.       |        |         |        |           |         |
| 4\.       |        |         |        |           |         |
| 5\.       |        |         |        |           |         |

**Did it help, and how do I know:**

```{=html}
<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->
```

------------------------------------------------------------------------

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

```{=html}
<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->
```

------------------------------------------------------------------------

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
