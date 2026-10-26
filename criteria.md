# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy of at least 65%

The model should achieve at least 65% overall accuracy on the held-out set.

**Why this target:** I chose 65% because I have three labels (`analysis`, `news`, and `opinion`), so about 33% would be roughly random guessing. Since some r/pcgaming posts can sit between my categories, especially `analysis` and `news`, I think correctly classifying about two-thirds of unseen posts would be useful.

---

## 2. Every label has an F1 score of at least 0.60

The `analysis`, `news`, and `opinion` labels should each achieve an F1 score of at least 0.60 on the held-out set.

**Why this target:** I chose 0.60 because I do not want strong performance on one common label to hide poor performance on another. My `analysis` and `news` labels also have a difficult boundary, so I expect some mistakes between them.

---

## 3. No label makes up more than 45% of the dataset

No single label should account for more than 45% of my labeled examples.

**Why this target:** I chose 45% because the three types of posts do not appear equally often in r/pcgaming, but I still want enough examples of `analysis`, `news`, and `opinion` to prevent one label from dominating the training data.

---

## 4. At least 85% labeling agreement

Another person applying my taxonomy to the same posts should agree with my labels at least 85% of the time.

**Why this target:** I chose 85% because my three labels have written definitions and I created a specific decision rule for the difficult `analysis` versus `news` boundary. I expect those rules to make most posts consistently classifiable even when some are borderline.

---

## 5. High-confidence predictions are at least 80% accurate

Predictions that the model identifies as high-confidence should be correct at least 80% of the time.

**Why this target:** I chose 80% because posts that clearly fit one of my three definitions should be easier for the model than borderline posts. I expect predictions the model is most confident about to therefore be more accurate than its overall predictions.


---

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
