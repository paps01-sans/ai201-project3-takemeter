# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

For this project, I collected public posts from r/soccer, a large football
community with match threads, discussion threads, news, and reactions. I read
30–40 recent posts before deciding what distinctions might be useful. The
classifier sorts posts into evidence-based analysis, unsupported general
claims, and immediate personal reactions.

### Milestone 1 observations

- **Specific match evidence vs. bare score reporting:** Substantial posts give
  scorers, minute marks, competition, kickoff time, and final status; thin
  posts state only a result. For example, the ADO Den Haag–PSV post-match
  thread included the 2–1 score, scorers and minutes, Eredivisie context,
  date, kickoff, and match status.
- **Stakes and competition context vs. low-context friendlies:** Posts about
  Champions League, major domestic fixtures, or competitive national-team
  matches invite more analysis because the result has clearer consequences.
  The AS Roma–Real Madrid 3–4 Champions League thread had more high-stakes
  framing than the Comoros–Senegal friendly thread, which mainly recorded a
  0–1 final score.
- **Structured discussion spaces vs. open-ended chatter:** Daily Discussion
  threads aggregate live scores, thread links, rules, and match-thread
  requests, although individual comments inside them can still be casual. One
  October 16 Daily Discussion listed multiple live and finished matches with
  links and explained how to request a match thread.
- **“Serious” framing vs. reactive discussion:** Posts explicitly labeled
  “Serious” signal that the community expects tactical, selection, or broader
  tournament analysis rather than jokes or one-line reactions. The “Serious
  Post-Match Thread” for the 2026 World Cup quarterfinals directed users
  toward in-depth discussion of several matches.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:**

`analysis` makes an argument about a team, player, match, or soccer issue using
at least one specific, checkable fact that does real work in the argument.

**Example 1:**

> [ADO Den Haag 2–1 PSV Eindhoven post-match thread](https://www.reddit.com/r/soccer/comments/1x8eiuw/post_match_thread_ado_den_haag_21_psv_eindhoven/)
> includes the final score, scorers and minutes, possession, shots, and match
> context. The excerpt I recorded included: “Final Score ADO Haag 2–1 PSV
> Eindhoven ... Possession: ADO Den Haag 53.1%, PSV Eindhoven 46.9% ... Shots:
> ADO Den Haag 16, PSV Eindhoven 14.”

**Example 2:**

> [AS Roma 3–4 Real Madrid post-match
> thread](https://www.reddit.com/r/soccer/comments/1x62ufv/post_match_thread_as_roma_34_real_madrid_uefa/)
> provides a detailed match record with competition and result context rather
> than only a bare reaction.

### `hot_take`

**Definition:**

`hot_take` is a confident general claim about a team, player, match, or soccer
issue with no specific supporting fact offered.

**Example 1:**

> [Jamie Carragher on Casemiro](https://www.reddit.com/r/soccer/comments/1clvikl/jamie_carragher_on_casemiro_i_know_hes_achieved/)
> makes a strong evaluation of a player without the post itself supplying a
> checkable fact that carries the claim.

**Example 2:**

> [José Mourinho and Daniel
> Levy](https://www.reddit.com/r/soccer/comments/lt7df6/jose_mourinho_and_the_decision_daniel_levy_doesnt/)
> presents a confident judgment about a manager and club decision without a
> load-bearing statistic, date, quote, or comparison.

### `reaction`

**Definition:**

`reaction` is an immediate response to something that just happened, focused
mainly on the writer's feeling or personal response rather than an argument.

**Example 1:**

> [Lucas Torreira goal in Arsenal 4–2
> Tottenham](https://www.reddit.com/r/soccer/comments/a2e6fk/l_torreira_goal_arsenal_42_tottenham_77/)
> captures an immediate response to a goal during a match.

**Example 2:**

> [Bayern Munich vs. Borussia post-match
> thread](https://www.reddit.com/r/soccer/comments/1f1ons/postmatch_thread_bayern_munich_vs_borussia/)
> captures immediate post-match responses rather than a supported general
> argument.

### The hardest boundary

**Which two labels:** `hot_take` and `reaction`

**The decision rule I used every time:** If the post makes a general claim about
a team, player, match, or soccer, label it `hot_take`; if it mainly reports an
immediate feeling or personal response to a recent event, label it `reaction`,
even when the language is emotional. Evidence outranks recency: a recent post
with a load-bearing checkable fact is `analysis`.

<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

### AI boundary stress test

I asked an AI tool for eight short posts designed to sit on the label
boundaries. The expected labels below follow the definitions above.

| Synthetic boundary post                                                                                        | Expected label | Why                                                            |
| -------------------------------------------------------------------------------------------------------------- | -------------- | -------------------------------------------------------------- |
| “The midfield collapsed after the 60th minute: they completed only 2 of 11 progressive passes after halftime.” | `analysis`     | The checkable statistic supports the claim.                    |
| “That manager is completely finished. No serious club should hire him.”                                        | `hot_take`     | It is a confident general judgment without evidence.           |
| “I cannot believe that goal; I am still shaking.”                                                              | `reaction`     | It reports an immediate personal feeling.                      |
| “The referee ruined the match because he gave three yellow cards in the first 20 minutes.”                     | `analysis`     | The timing fact is used to support the conclusion.             |
| “This is the greatest team in Europe, and nobody is close.”                                                    | `hot_take`     | It makes a broad unsupported claim.                            |
| “That equalizer just happened and I screamed so loudly my neighbors checked on me.”                            | `reaction`     | It is an immediate personal response.                          |
| “They were unlucky; they hit the post twice and had 19 shots.”                                                 | `analysis`     | The match facts carry the argument.                            |
| “What a disaster. I am speechless after watching that finish.”                                                 | `reaction`     | It expresses an immediate response without a general argument. |

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**

Public r/soccer posts collected with the project scraper and retained after
removing placeholders, deleted posts, duplicates, and posts that could not be
classified under the three-label taxonomy. The stop-rule outcome was 44 valid
rows rather than 200 because the collected feed contained many placeholders,
automated match content, and posts outside the taxonomy.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

All 44 rows were collected and pre-labelled by an AI tool. I did not complete
the required 20-post cold-labeling pass, so these results should not be treated
as evidence of independent labeling consistency.

**Counts per label:**

| Label      | Count  | Share    |
| ---------- | ------ | -------- |
| `analysis` | 25     | 56.8%    |
| `hot_take` | 14     | 31.8%    |
| `reaction` | 5      | 11.4%    |
| **Total**  | **44** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**

> _The post:_ A post comparing Lamine Yamal's goals, assists, appearances, and
> age with other major players.
>
> _Could have been:_ `analysis` or `hot_take`
>
> _I chose `analysis`, because:_ The comparisons and checkable numbers support
> the evaluation.

**2.**

> _The post:_ A post making a broad claim that a team or player is overrated
> without supplying match evidence.
>
> _Could have been:_ `hot_take` or `reaction`
>
> _I chose `hot_take`, because:_ It makes a general claim rather than reporting
> the writer's immediate personal response.

**3.**

> _The post:_ A post describing an immediate emotional response to a recent
> goal, result, or match event.
>
> _Could have been:_ `reaction` or `hot_take`
>
> _I chose `reaction`, because:_ The post focuses on the writer's immediate
> feeling and does not make a supported general argument.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

`distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->

3 epochs, learning rate `2e-5`, batch size `16`, maximum length `128`, seed
`42`.

**Anything I changed from the defaults, and why:**

Nothing. I used the notebook defaults so the run would remain comparable to
the project instructions.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

The 44 rows were split into 30 training, 7 validation, and 7 test rows. The
test set contained 4 `analysis`, 2 `hot_take`, and 1 `reaction` posts. The
`reaction` test count was far below 8, so its F1 score is especially unstable.
The small total dataset also makes the three-seed results unreliable.

**Training device:** CPU (`torch 2.14.1+cpu`).

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- _What I asked for:_ Help clarify the project instructions, label definitions,
  and scraper workflow.
- _What came back:_ AI explanations helped me understand the requirements,
  troubleshoot Reddit API access, and debug the collection script.
- _What I changed:_ I used the explanations to configure the workflow and
  correct the scraper errors.

**Moment 2**

- _What I asked for:_ Assist with collecting, filtering, and labeling public
  r/soccer posts using my three definitions.
- _What came back:_ AI produced a reduced labeled dataset after removing
  placeholders and unusable posts.
- _What I changed:_ I checked the CSV columns, labels, missing fields, and
  duplicate text before training.

**Pre-labelling disclosure:**

The entire 44-row dataset was AI-collected and AI-labelled. The AI output
reported 143 placeholder rows, one deleted row, and 44 retained rows from 288
candidate rows. The retained rows were not independently audited for live URL
accessibility.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure          | Baseline | Trained | Difference |
| ---------------- | -------- | ------- | ---------- |
| Overall accuracy | 0.429    | 0.571   | +0.143     |
| Macro F1         | 0.200    | 0.242   | +0.042     |
| F1 — `analysis`  | 0.600    | 0.727   | +0.127     |
| F1 — `hot_take`  | 0.000    | 0.000   | +0.000     |
| F1 — `reaction`  | 0.000    | 0.000   | +0.000     |

**What I predicted before I looked:**

     I did not record a prediction before viewing the trained numbers, so I am not
     claiming a retrospective prediction.

     **What the gap actually means:**

     Fine-tuning improved overall accuracy by 0.143 and macro F1 by 0.042 on this
     seven-post test set, with the largest improvement on `analysis`. It did not
     improve `hot_take` or `reaction`; the model still had zero F1 for both labels.
     Because the test set has only seven posts and the dataset has only 44 rows,
     this improvement is not stable evidence of generalization.

### Definitions versus bare-label baseline

I also ran the baseline against the same `test_split.csv` without
`label_definitions.txt`, so it used only the label names. This is a weaker
prompt and is reported separately from the fair definitions-based baseline.

| Measure          | With definitions | Bare label names | Difference |
| ---------------- | ---------------: | ---------------: | ---------: |
| Overall accuracy |            0.429 |            0.000 |     +0.429 |
| Macro F1         |            0.200 |            0.000 |     +0.200 |
| F1 — `analysis`  |            0.600 |            0.000 |     +0.600 |
| F1 — `hot_take`  |            0.000 |            0.000 |     +0.000 |
| F1 — `reaction`  |            0.000 |            0.000 |     +0.000 |

The written definitions substantially improved the baseline on this very small
seven-post test set, although the sample is too small to treat the gap as a
stable general result.

     ---

     ## Stretch: Confidence Check

     I will complete the confidence check because the notebook already records
     confidence results for the held-out predictions. I will compare accuracy on
     the most-confident third of predictions with accuracy on the least-confident
     third and report whether confidence was actually associated with correctness.


     ---

     ## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion           | Target  | Seed 42              | Seed 7               | Seed 2024            | Verdict |
| ------------------- | ------- | -------------------- | -------------------- | -------------------- | ------- |
| 1. Overall accuracy | 0.70    | 0.571                | 0.286                | 0.857                | MISSED  |
| 2. Every-label F1   | 0.60    | 0.000 for two labels | 0.000 for two labels | 0.000 for `reaction` | MISSED  |
| 3. Label balance    | 20%–60% | 56.8%, 31.8%, 11.4%  | same dataset         | same dataset         | MISSED  |
| 4. Staff agreement  | 75%     | not run              | not run              | not run              | NOT RUN |
| 5. Accuracy spread  | <= 0.10 | 0.571                | 0.571                | 0.571                | MISSED  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | analysis | hot_take | reaction |
| ---------------- | -------- | -------- | -------- |
| **analysis**     | 4        | 0        | 0        |
| **hot_take**     | 2        | 0        | 0        |
| **reaction**     | 1        | 0        | 0        |

**My biggest off-diagonal number, and what it means:**
The biggest off-diagonal number was 2: two real `hot_take` posts were called
`analysis`. The model predicted every test post as `analysis`, so it also
missed the one `reaction` post and showed a strong majority-label bias.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| #   | Criterion | Target | Verdict | How I decided |
| --- | --------- | ------ | ------- | ------------- |
| 1   |           |        |         |               |
| 2   |           |        |         |               |
| 3   |           |        |         |               |
| 4   |           |        |         |               |
| 5   |           |        |         |               |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** **_ / 30 = _**%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

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

**1.**

> _The post:_
>
> _Staff said / I said:_
>
> _My call, and why:_
>
> _Which pile:_

**2.**

> _The post:_
>
> _Staff said / I said:_
>
> _My call, and why:_
>
> _Which pile:_

**What the pattern in my disagreements tells me:**

---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
| --------- | ------ | ------- | ------ | --------- | ------- |
| 1.        |        |         |        |           |         |
| 2.        |        |         |        |           |         |
| 3.        |        |         |        |           |         |
| 4.        |        |         |        |           |         |
| 5.        |        |         |        |           |         |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**The gap between what I meant my labels to capture and what the model
learned:**

<!-- Two sentences. Your confusion matrix is the evidence. -->

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

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
