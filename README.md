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
| Overall accuracy | 0.240 | 0.680 | +0.440 |
| Macro F1 | 0.330 | 0.270 | -0.060 |
| F1 — `analysis` | 0.240 | 0.000 | -0.240 |
| F1 — `news` | 0.000 | 0.810 | +0.810 |
| F1 — `opinion` | 0.750 | 0.000 | -0.750 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**Prediction:** I predict that my trained model will perform better than the baseline, especially on distinguishing news from analysis, because it was fine-tuned on posts labeled using my specific taxonomy.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

The trained model had much higher overall accuracy than the baseline, improving from 0.240 to 0.680. However, its macro F1 was slightly lower because it performed very well on news but failed to correctly identify opinion and analysis posts. This suggests that fine-tuning helped the model learn the majority news label, but it did not learn the smaller classes well.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.680 | 0.680 | 0.680 | Met |
| 2. Every label F1 | ≥ 0.60 each | News: 0.810; Opinion: 0.000; Analysis: 0.000 | News: 0.810; Opinion: 0.000; Analysis: 0.000 | News: 0.810; Opinion: 0.000; Analysis: 0.000 | Missed |
| 3. No label over 45% of dataset | ≤ 45% | N/A | N/A | N/A | Missed — News is 69.6% of dataset |
| 4. Labeling agreement | ≥ 85% | 100% | 100% | 100% | Met |
| 5. High-confidence accuracy | ≥ 80% | 1.000 | 0.875 | 1.000 | Met |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | news | opinion | analysis |
|---|---|---|---|
| **news** | 17 | 0 | 0 |
| **opinion** | 5 | 0 | 0 |
| **analysis** | 3 | 0 | 0 |

**My biggest off-diagonal number, and what it means:**
The biggest confusion was 5 real `opinion` posts being predicted as `news`, while 0 real `news` posts were predicted as `opinion`. This shows that the model strongly leans toward predicting `news` rather than distinguishing the two labels.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 | MET | Accuracy was 0.680 for all three seeds, which is above the 0.65 target. |
| 2 | Every label F1 | ≥ 0.60 each | MISSED | News had an F1 of 0.810, but opinion and analysis both had an F1 of 0.000 across all three seeds. |
| 3 | No label over 45% of dataset | ≤ 45% | MISSED | News makes up 69.6% of my dataset, which is above the 45% maximum. |
| 4 | Labeling agreement | ≥ 85% | MET | I matched the staff labels on all 30 posts, giving me 100% agreement. |
| 5 | High-confidence accuracy | ≥ 80% | MET | High-confidence accuracy was 1.000, 0.875, and 1.000 across the three seeds, all above 0.80. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

**Criterion 2 — Every label F1: MISSED**

The main cause was too few examples for the minority labels and an imbalanced dataset. News had an F1 of 0.810 across all three seeds, while opinion and analysis both had an F1 of 0.000. The confusion matrix shows that all 5 opinion posts and all 3 analysis posts in the test set were predicted as news. My agreement check was 100% (30/30), so it does not provide evidence that inconsistent application of written labeling rules was the main problem. Instead, the results point more strongly to class imbalance and too few examples of opinion and analysis.

**Criterion 3 — No label over 45% of dataset: MISSED**

The cause was class imbalance in my collected dataset. News accounts for 69.6% of my examples, which is far above my 45% target. This also helps explain the model's behavior in the confusion matrix, where it predicted every test example as news. My 100% staff-label agreement suggests that I can apply a clearly written taxonomy consistently, so the stronger evidence here is that I collected too many news examples and too few examples of the minority labels.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

****Agreement rate:** 30 / 30 = 100%

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

There were no disagreements. I matched the staff labels on all 30 posts.

I found the staff set relatively easy to apply because the definitions and decision rules clearly distinguished analysis, hot takes, and reactions. The rules were especially helpful for determining whether a fact was actually supporting an argument or was simply included alongside an unsupported claim.

---

## The Improvement

**What I changed:**

I added more examples from the minority classes to improve the class imbalance in my dataset, with a focus on adding more analysis examples.

**Which diagnosis pointed at it:**

The before results showed that the model was predicting everything as news. News had an F1 score of 0.810, while opinion and analysis both had an F1 score of 0.000. The dataset was also heavily imbalanced toward news, which made up 69.6% of the original dataset.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.654 | 0.654 | 0.654 | Met |
| 2. Every label F1 | ≥ 0.60 each | News 0.791; Opinion 0.000; Analysis 0.000 | News 0.791; Opinion 0.000; Analysis 0.000 | News 0.791; Opinion 0.000; Analysis 0.000 | Missed |
| 3. No label over 45% of dataset | ≤ 45% | N/A | N/A | N/A | Missed |
| 4. Labeling agreement | ≥ 85% | N/A | N/A | N/A | See agreement check |
| 5. High-confidence accuracy | ≥ 80% | 1.000 | 1.000 | 1.000 | Met |

**Did it help, and how do I know:**

No. The improvement did not improve the model's performance. Accuracy decreased from 0.680 before the change to 0.654 after the change, and macro F1 decreased from 0.270 to 0.264. Opinion and analysis F1 also remained at 0.000 across all three seeds. This shows that adding minority-class examples alone was not enough for the model to learn those labels.

---

## What's Still Broken

The model still fails to distinguish opinion and analysis from news. Both minority labels still have an F1 score of 0.000. I would next add substantially more high-quality opinion and analysis examples and work toward a more balanced dataset. I stopped after this improvement because the assignment required testing one targeted improvement and comparing its results against the original model.

**The gap between what I meant my labels to capture and what the model learned:**

I intended the labels to distinguish reporting, subjective viewpoints, and evidence-based analysis. However, the model learned to rely heavily on the majority news label, as shown by the confusion matrix and the 0.000 F1 scores for both opinion and analysis.



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
