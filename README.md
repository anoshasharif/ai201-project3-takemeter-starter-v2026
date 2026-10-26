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

TakeMeter is a text classifier trained on posts from r/pcgaming. It classifies posts into three categories: news, opinion, and analysis. I collected and labeled the dataset and fine-tuned a DistilBERT model to predict the category of a post.

---

## Label Taxonomy

### `analysis`

**Definition:** A post that supports its main claim with specific facts, numbers, quotes, examples, or a detailed explanation.

**Example 1:**
> Indies and original IP blew up in 2026. More than half of the games that sold more than 500,000 units in their first month were the start of a new series, and 43% were indies.

**Example 2:**
> Planetside 2 devs fix months old crash bug after a user shows them a solution.

### `news`

**Definition:** A post that primarily reports or announces a gaming-related event, release, update, or development without substantial additional reasoning.

**Example 1:**
> Torchlight II on Steam receives beta update with controller support and crash fixes.

**Example 2:**
> Metal Gear Online 2 (SaveMGO) now has a native PC port.

### `opinion`

**Definition:** A post whose main purpose is to express or relay a judgment, reaction, criticism, or subjective viewpoint rather than simply report information.

**Example 1:**
> Why a 21-year-old coder finally ported the real P.T. to PC: 'That something so influential could eventually become unplayable never sat right with me.'

**Example 2:**
> Discord's Second Attempt To Roll Out Age Verification Is Going Badly.

### The hardest boundary

**Which two labels:** `analysis` and `news`

**The decision rule I used every time:** If a post only reports what happened, it is `news`. If it goes beyond reporting the event and supports its main point with specific facts, numbers, examples, quotes, or detailed explanation, it is `analysis`.


---

## The Dataset

**Where the posts came from:**  
I collected public posts from the r/pcgaming subreddit. My final dataset contains 161 posts. This is below the 200 examples requested by the brief.

**How I labelled them:**  
I labelled the first 20 posts cold without AI assistance. For the remaining posts, I used AI to help pre-label them, then manually reviewed and checked each label before adding it to the final dataset.

**Counts per label:**

| Label | Count | Share |
|---|---:|---:|
| News | 112 | 69.6% |
| Opinion | 31 | 19.3% |
| Analysis | 18 | 11.2% |
| **Total** | **161** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* Why a 21-year-old coder finally ported the real P.T. to PC: "That something so influential could eventually become unplayable never sat right with me"
>
> *Could have been:* News or Opinion
>
> *I chose, because:* Opinion, because although the post reports on the PC port, the title focuses heavily on the developer's personal reasoning and perspective.

**2.**
> *The post:* Planetside 2 devs fix months old crash bug after a user shows them a solution
>
> *Could have been:* News or Analysis
>
> *I chose, because:* Analysis, because although it reports an update, the main focus is a technical problem and how that problem was solved.

**3.**
> *The post:* Cabernet (PC) Review - "This is One of The Best Vampire Games I've Played"
>
> *Could have been:* Opinion or Analysis
>
> *I chose, because:* Opinion, because a review can contain analysis, but this post focuses on the reviewer's personal judgment of the game.

---

## The Training Run

**Base model:** distilbert-base-uncased

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, seed 42.

**Anything I changed from the defaults, and why:** I kept the default training settings.

**Split sizes:** 112 train / 24 validation / 25 test. The test set contained 17 news, 5 opinion, and 3 analysis posts. Opinion and analysis have very small test sets, so their metrics may vary significantly between seeds.


---

## How I Used AI

**Moment 1:**
- *What I asked for:* I asked AI to help classify Reddit posts as news, opinion, or analysis.
- *What came back:* AI suggested labels for the posts based on my label definitions.
- *What I changed:* I reviewed the suggested labels myself and corrected them when necessary before adding them to my dataset.

**Moment 2:**
- *What I asked for:* I asked AI to help me understand errors and warnings while preparing and running the training notebook.
- *What came back:* AI explained issues such as duplicate posts, label imbalance, and the notebook's training output.
- *What I changed:* I removed duplicate data, reviewed my label distribution, and followed the notebook requirements before training.

**Pre-labelling disclosure:** I labeled my first 20 posts cold. AI was then used to pre-label additional posts, but I manually reviewed and checked the labels before using them.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

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

**Agreement rate:** ___ / 30 = ___%

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
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

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
