# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of practice.

An acceptance criterion names a number. *"The model is accurate"* is an opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a criterion.

Under each, write a sentence or two on **why that number**. A reason that says something about your data or your taxonomy earns credit — *"I picked 0.60 F1 for `reaction` because it's my smallest label and I only have about 50 examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so easy you can't miss it does.

------------------------------------------------------------------------

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets about 33%, so a target of 0.40 is barely a target. Your number should sit somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a form to fill in — a criterion that fits none of them is fine if it names a number.

| Area | A question it could answer |
|------------------------------------|------------------------------------|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect whether you hit them:

-   **Your smallest label will have the jumpiest score.** If a label has 50 examples, about 8 land in the test split. An F1 computed on 8 examples moves a lot between seeds. A target for that label should be looser than one for your biggest label, and saying so is a good reason.
-   **Unit 6 tests across three seeds, and the target has to hold across all three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**. Pick with that in mind — it is stricter than it first sounds.

------------------------------------------------------------------------

## 1. Overall accuracy is at least 0.70 on the held-out test split, on all three seeds (42, 7, 2024)

Measured by `accuracy` in `results_three_seeds_before.json`.

**Why this target:** `news` is 133 of my 263 posts (51%), so a model that always guesses `news` already scores about 0.51. 0.70 asks for roughly 20 points above that. I don't expect higher, because the `question`/`take` boundary is kind of hard to tell, even for me.

------------------------------------------------------------------------

## 2. `news` has an F1 of at least 0.80 on all three seeds

Measured by `per_label.news.f1`.

**Why this target:** `news` is my biggest label and its posts look alike on the surface: source tags like "[The Race]" or "[Autosport]", standings, "Final Starting Grid", stat titles full of numbers. So I set this high standard of 0.80, which is quite easy to achieve.

------------------------------------------------------------------------

## 3. `take` has an F1 of at least 0.45 on all three seeds, and `question` at least 0.60

Measured by `per_label.take.f1` and `per_label.question.f1`.

**Why this target:** `take` is my smallest label, so a single wrong guess moves its F1 by several points. It also sits on my hardest boundary: a take often ends in "What do you think?". So its target is looser than the others. `question` has 74 posts (about 11 in test), and most of them carry clear question wording, so I hold it higher at 0.60.

------------------------------------------------------------------------

## 4. The model's most-confident third of test predictions is at least 0.15 more accurate than its least-confident third, on all three seeds

Measured by `confidence.accuracy_most_confident_third` minus `confidence.accuracy_least_confident_third`.

**Why this target:** I want to be able to trust a confident `news` call without reading the post, and hand-check the unsure ones, which I expect to be the question/take borderline posts. With about 13 posts per third, a gap of 0.15 is about two more right answers in the confident third.

------------------------------------------------------------------------

## 5. `question` recall is at least 0.60 on all three seeds

Measured by `per_label.question.recall` in `results_three_seeds_before.json`

**Why this target:** the point of the classify is to find posts that want an answer so missing them is the worst error. `question` has about 11 posts in each test split so 0.60 means finding at least 7 of them.

------------------------------------------------------------------------

```{=html}
<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
```
