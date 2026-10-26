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

Overall accuracy on the held-out test set is at least 0.80 on all three seeds.

**Why this target:**

My labels are fact driven: I count the checkable facts in a post, and two or more makes it `analysis`. Posts full of stats or with no facts at all should be easy to tell apart, so most mistakes will be one-fact vs. two-fact posts near my boundary. That makes 0.80 reachable but not 0.90. It's also well above what the model gets by always guessing `hot_take` (under 70%, per criterion 5), and with only about 30 test posts it has to hold on all three seeds, so it's not a safe number.

---

## 2.

When I re-label my 20 cold-labelled posts a few days later without looking at my first labels, at least 80% of the posts either round labelled `analysis` get `analysis` both times.

**Why this target:**

Counting facts is easier to repeat than judging whether an argument is good, so I should mostly agree with myself. Where I could slip is the counting edge cases: whether "MVP, DPOY, Finals MVP" is one fact or three, whether "the efficiency numbers are a knock" counts, and years like "2020 Anthony Davis" that aren't evidence. I only expect about 6 `analysis` posts in the 20, so one disagreement drops me to about 83% and two drops me below 80%.

---

## 3.

Accuracy on the model's most-confident third of test posts is at least 0.85 on all three seeds.

**Why this target:**

Hot takes are opinions with no factual reasoning, so the model should be most sure on those, and on `analysis` posts packed with stats. Those clear cases should make up the most-confident third, so it should score higher than my 0.80 overall. The posts it's unsure about should be the one-fact posts near my boundary, like "Maybe Duncan… 5 rings." The third is only about 10 posts, so 0.85 means at most one mistake among the model's most confident calls on every seed.

---

## 4.

The `analysis` label has an F1 of at least 0.70 on the held-out test set on all three seeds.

**Why this target:**

Reddit is mostly fans posting opinions, so `analysis` will be my smaller label. The model gets fewer examples to learn it from, and there will only be about 9–10 `analysis` posts in the test set, so one miss moves F1 by 5–7 points. It's also the harder label: the model has to notice two facts, and one-fact posts look almost the same. That's why this target is lower than my 0.80 overall accuracy.

---

## 5.

`hot_take` makes up less than 70% of the rows in the final `labels.csv`.

**Why this target:**

Reddit leans toward opinions from passionate fans and rebuttals, so `hot_take` will naturally be the bigger label. If it goes past 70%, always guessing `hot_take` gets close to my 0.80 accuracy target and criterion 1 stops meaning anything. Keeping it under 70% means I have to look for `analysis` posts on purpose instead of taking whatever the thread gives me.

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
