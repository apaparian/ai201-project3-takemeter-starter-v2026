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

TakeMeter takes posts from r/rupaulsdragrace and sorts them into three labels: analysis, hot_take, and reaction. The goal is to see whether a model can learn the difference between someone making an argument, expressing a strong opinion, or simply reacting to something that happened.

I wanted to choose something representative of an LGBTQ+ community, and I did take the instructor's note about communities centered around a single show into account. Ultimately, I felt that discussion around Drag Race is closer to a sport or competition than a traditional television fandom. The community spends a lot of time debating performances, judging decisions, rankings, predictions, and contestant outcomes.

The discussions naturally fit the distinction between analysis, hot takes, and reactions that we covered in lecture. Because of that, it felt like a good place to study different styles of opinion rather than a community focused primarily on spoilers or plot discussion.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:**
Analysis: Makes an argument backed by a specific, checkable fact that carries the argument.

**Example 1:**
> **Shannel technically placed 4th on three different seasons!**
We all know that Jujubee was technically 3rd on four different seasons: S2 - 3rd, AS1 - 3rd/4th, AS5 - 2nd/3rd, UKvW1 - 3rd/4th. But Shannel was technically 4th on three different seasons: S1 - 4th, AS1 - 3rd/4th, AS9 - 4th/8th …

**Example 2:**
> **Currently rewatching S5 and damn Alyssa Edwards did so much worse than i remembered.**
4 weeks in a row in the bottom, which would have been 5 if not for her immunity. Also her being only low for the singing challenge is wild insofar that we never see her sing a single line …

### `hot_take`

**Definition:**
Hot Take: A confident general claim with no support offered, even if it mentions a number.

**Example 1:**
> **Pit Crew.**
WoW, I really think it’s time to get a variety of body representation in the Pit Crew. The Pit Crew has consistently been made of chiseled, strong, muscular men … But after 18 seasons of US DR, 11 All Stars, plus the many others such as UK, Canada and Down Under I truly feel like now the time we need some consistent Pit Crew members that have a variety of body types. We need a bear! …

**Example 2:**
> **The show should retire songs like they do hockey jerseys.**
There are particular lip syncs that are so iconic that I'm surprised the show will decide to reuse the songs. Most recently with Macarthur Park - cannot imagine another queen taking a stab at that song after seeing Manila's interpretation. Among other songs that would probably benefit from being retired: Boss Bitch, This will be, Shut up and drive, So emotional …

### `reaction`

**Definition:**
Reaction: An immediate personal response to something that just happened. Feeling rather than argument.

**Example 1:**
> **Never cried at a lip sync til now.**
Just watching AS11 for the first time and Kennedy + A’keria lip sync … This moment made me 🥲 because it’s as if you see a switch flip in A’keria’s mind where she realizes the weight of the situation and somehow digs deeper … and she’s so so beautiful …

**Example 2:**
> **Kandy Muse in the "I'm Not Perfect" lip-sync is so incredibly c*nt, I can't get over it.**
On top of this song being an underrated bop of Grace Jones, Kandy just has so much cuntensity throughout. She had me right from the beginning when she isolated her movements with "I'm" on the beat … I love her articulation and I love …

### The hardest boundary

**Which two labels:**
Analysis vs Hot Take

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If the post gives reasons, examples, statistics, challenge results, quotes, or other evidence to support its conclusion, label it analysis. If it mainly states a conclusion or opinion and expects the reader to agree without support, label it hot_take.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
r/rupaulsdragrace. I collected public Reddit posts discussing the Drag Race franchise, including episode reactions, judging decisions, contestants, and fandom discussions.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
     Say so plainly — the disclosure is required, not penalised. -->
I labeled the first 20 posts, as cold, in the note column. After that, I used AI to suggest labels in batches and then reviewed and corrected them by hand before adding them to the dataset.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| analysis | 59 | 29.5% |
| hot_take | 72 | 36.0% |
| reaction | 69 | 34.5% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Why do people hate Mistress so much?"
>
> *Could have been:* analysis or hot_take
>
> *I chose, because:* The post gives reasons and examples to support its position rather than only stating an opinion. I labeled it as analysis.

**2.**
> *The post:* "I see Plane as a troll rather than a mean girl."
>
> *Could have been:* analysis or hot_take
>
> *I chose, because:* The post explains its reasoning and provides supporting examples from the show. I labeled it as analysis rather than a hot take.

**3.**
> *The post:* "The real winner of DU vs TW."
>
> *Could have been:* hot_take or reaction
>
> *I chose, because:* The post was mostly an opinion about a contestant rather than an emotional reaction to an event. I labeled it as hot_take.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
distilbert-base-uncased

**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, seed 42.
Device: mps (Apple Silicon GPU), torch 2.14.1.

**Anything I changed from the defaults, and why:**
Nothing. I used the notebook's default settings unchanged.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
     split. If a label had fewer than about 8 in test, say so — it explains a lot
     of next unit's variance. -->
Train 139, validation 31, test 30 (70/15/15 stratified split of 200 posts).
Test label counts: analysis 9, hot_take 11, reaction 10.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Asked AI to stress-test the taxonomy and find posts that sat between analysis and hot_take.
- *What came back:* It generated borderline examples.
- *What I changed:* I used them to refine the decision rule.

**Moment 2**

- *What I asked for:* Asked AI to batch-suggest labels for collected posts.
- *What came back:* It returned labels based on my definitions.
- *What I changed:* I skimmed through the labels and corrected a few that were noticeable mis-categorized.

**Pre-labelling disclosure:**

AI suggested labels for much of the dataset. I skimmed through them and corrected some that were clearly mis-categorized.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.233 | 0.500 | +0.267 |
| Macro F1 | 0.126 | 0.399 | +0.273 |
| F1 — `analysis` | 0.378 | 0.000 | -0.378 |
| F1 — `hot_take` | 0.000 | 0.571 | +0.571 |
| F1 — `reaction` | 0.000 | 0.625 | +0.625 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I expect the trained model to outperform the baseline at distinguishing analysis from hot_take. Those labels depend on specific patterns from the taxonomy. The baseline only has the label definitions and no examples of how they were used.


**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
Analysis received an F1 score of zero in the trained model. That means that model learned how to classify hot_take and reaction, but it failed to understand the analysis boundary. Somehow the untrained model worked the other way around, and failed to understand hot_take or reaction.

This may be somewhat expected, as analyses are difficult to discern in a community that integrates subjective language with evidence and statistics. A general model may recognize evidence-based arguments more easily from the written definitions alone, while the fine-tuned model may have learned stronger patterns for hot_take and reaction from the training examples and begun classifying many borderline analysis posts into those categories instead.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | at least 0.65 on every seed | 0.500 | 0.500 | 0.433 | MISSED |
| 2. Per-label F1 | at least 0.55 for every label on every seed | 0.000 | 0.200 | 0.000 | MISSED |
| 3. Label balance | no label above 45% or below 25% of the dataset | 36.0 34.5 29.5 | 36.0 34.5 29.5 | 36.0 34.5 29.5 | MET 
| 4. Agreement with staff | at least 70% on the 30-post set | 30 | 30 | 30 | MET |
| 5. Confidence (revised) | most-confident third correct at least 75% on every seed | 0.800 | 0.600 | 0.600 | MISSED |

**Spread across three seeds** (seeds 42, 7, 2024; device `mps`):

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.500 | 0.433 | 0.067 |
| Macro F1 | 0.399 | 0.419 | 0.341 | 0.078 |
| F1 — `analysis` | 0.000 | 0.200 | 0.000 | 0.200 |
| F1 — `hot_take` | 0.571 | 0.595 | 0.462 | 0.133 |
| F1 — `reaction` | 0.625 | 0.462 | 0.560 | 0.163 |

Accuracy moved 0.067 across seeds. Criterion within that band cannot be called either way.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 0 | 9 | 0 |
| **hot_take** | 0 | 10 | 1 |
| **reaction** | 0 | 5 | 5 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
9 posts, that were analysis, were labeled as hot_take. This was expected. The boundary was anaylsis/ hot_take, the direction was towards hot_take. 0 posts were labeled in the opposite direcction. This is considered a labeling mistake by the notebook, but it could also be inherent to the style of discourse present within the community, where words that appear emotionally driven serve as slang representations of factual discussion.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | at least 0.65 | MISSED | 0.500, 0.500 and 0.433, so it missed the target on every seed. |
| 2 | Per-label F1 | at least 0.55 for every label | MISSED | `analysis` had an F1 of 0.000, 0.200 and 0.000, far below 0.55 on every seed. |
| 3 | Label balance | no label above 45% or below 25% | MET | The labels are 36.0%, 34.5% and 29.5% of the 200 posts. |
| 4 | Agreement with staff | at least 70% on the 30-post set | MET | I matched the staff label on all 30 posts (100%), including the 7 posts staff flagged as borderline. |
| 5 | Confidence (revised) | most-confident third correct at least 75% | MISSED | The most-confident third was right 0.800, 0.600 and 0.600 of the time, so it fell below 75% on two of the three seeds. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

Criterion 1 (accuracy at least 0.65): Accuracy missed on all three seeds at 0.500, 0.500 and 0.433. The confusion matrix helps explain why. Nine analysis posts were classified as hot_take, and none were misclassified in the opposite direction. Since analysis makes up roughly a third of the test set, those mistakes alone put a substantial cap on the overall accuracy. The direction of the confusion was not surprising. In this community, factual discussions are often wrapped in highly emotional or opinionated language, so posts that are making evidence-based arguments can still look like reactions or strong opinions at a surface level.


Criterion 2 (every label F1 at least 0.55): analysis had an F1 of 0.000, 0.200 and 0.000 across the three seeds. The model rarely predicted analysis and instead tended to label those posts as hot_take. My agreement check does not point to a problem in how I apply the analysis / hot_take boundary, since I matched staff on all 30 posts, including the 7 borderline cases. My best guess is either that there are too few analysis examples for the model to learn the distinction reliably, or that the distinction itself is difficult to learn from a dataset of this size. The agreement exercise used the staff taxonomy rather than my own, so it cannot completely rule out inconsistencies in my 200 labeled posts.


Criterion 5 (confidence): the most-confident third of predictions was correct 0.800, 0.600 and 0.600 of the time, so the target only held on one seed. Average confidence was about 0.37, which is close to the one-in-three confidence you would expect from guessing between three labels. Each confidence bucket contained only 10 posts, so the numbers are somewhat noisy. My interpretation is the same as above: the model never learned a strong enough signal to become consistently confident in its predictions.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

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

None. I matched the staff label on all 30 posts, so there are no disagreements to adjudicate.

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

There were no disagreements, so there is no pattern to read. I matched staff on all 30 posts, including all 7 that staff marked as borderline, which shows I can apply a `analysis` / `hot_take` / `reaction` taxonomy. It doesn't necessarily show my own 200 labels were consistent, because my rules were written slightly differently. The easy 10 / 10 / 10 split may have aided in the labeling.


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
