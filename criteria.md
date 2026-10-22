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

## 1.

<!-- Your criterion. It must name a number. -->

The trained model must reach an overall accuracy of at least **0.70** on the
held-out test set for all three random seeds.

**Why this target:** With three labels, a roughly even random guess is near
33% accuracy. A 70% target requires the model to learn useful distinctions
between evidence-based analysis, unsupported claims, and immediate reactions,
rather than mostly guessing the majority label.

---

## 2.

<!-- Your criterion. -->

Every label must have an F1 score of at least **0.60** on the held-out test set
for all three random seeds.

**Why this target:** Accuracy alone could hide a label that the model almost
never recognizes. The three labels have different and potentially overlapping
language, so each label needs a useful balance of precision and recall. A
0.60 floor is demanding enough to matter while allowing for the small,
variable test counts produced from only 200 posts.

---

## 3.

<!-- Your criterion. -->

The final dataset must contain at least **20%** of its posts in each label and
no more than **60%** in any one label.

**Why this target:** The taxonomy has three labels, and the model should not
learn that one label is the answer simply because it dominates the dataset.
Allowing a range from 20% to 60% gives room for the natural distribution of
r/soccer posts while preventing an extremely rare label or a majority label
from determining the results.

---

## 4.

<!-- Your criterion. -->

My labels must agree with the staff labels on at least **75% of the 30-post
staff set**.

**Why this target:** The staff set contains deliberately ambiguous examples,
so perfect agreement would be unrealistic. A 75% target still requires me to
apply written rules consistently, especially the boundary between `hot_take`
and `reaction`, instead of treating the labels as subjective impressions.

---

## 5.

<!-- Your criterion. -->

The range between the highest and lowest overall accuracy across seeds 42, 7,
and 2024 must be no greater than **0.10**.

**Why this target:** The test split is relatively small because the dataset
contains only 200 posts, so some variation between seeds is expected. A
10-percentage-point maximum spread allows normal sampling noise but flags a
model whose result is too unstable to trust.

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
