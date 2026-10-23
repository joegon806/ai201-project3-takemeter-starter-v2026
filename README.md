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
TakeMeter is model trained to identify whether a text post is Seeking help from other users, Advising other users, or just sharing their own Experience. The model is trained on posts from the r/studytips subreddit on reddit.com. 200 posts from the subreddit have been collected, labeled, and fed to the model to split into sets for training, evaluation, and testing.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `seeking`

**Definition:** asking or prompting other people for something the user doesn't already have, such as advice they need or reassurance. Has the highest importance if there’s overlap with other categories.

**Example 1:**
>https://www.reddit.com/r/studytips/comments/1x6h64f/how_can_i_improve_my_study_space_in_a_very_tight/
```
How can I improve my study space in a very tight budget 🥹

Budgeting is very minimal
```

**Example 2:**
>https://www.reddit.com/r/studytips/comments/1x4ph7g/any_suggestions_on_how_to_utilise_3_devices/
```
Any suggestions on how to utilise 3 devices 😭

So I bought a new laptop yesterday and now I have 3 devices (2 laptops and a tablet). The older laptop is just capable of doing very light work (4gb of ram with a 2012 processor). Any suggestions on how to utilise all the 3 devices for my studying? I'm studying physics btw
```

### `advising`

**Definition:** talking about actions that people should do, or actions the user does and feels positively about.

**Example 1:**
>https://www.reddit.com/r/studytips/comments/1x5tz8w/i_count_5_seconds_to_force_myself_to_study/
```
I count 5 seconds to force myself to study.

Whenever I feel too lazy to study, I just use the 5 second rule to force myself to get up and go study. I just count down 5-4-3-2-1 and immediately start studying.

This honestly works because your brain won't have time to make excuses. You just need to be strict with yourself so you can stop being lazy and start studying instead.
```

**Example 2:**
>https://www.reddit.com/r/studytips/comments/1x5ns4e/homework_advice/
```
Homework Advice

Do some amount of homework every single day. Not saying you should be a grind but just get SOMETHING done each day. Because if you don’t, the work is just gonna pile up on you and you’ll be left scrambling and stressing trying to get the hw done.
```

### `experience`

**Definition:** only talking about their own experience with something, typically venting negatively. Has the lowest importance. Reserved for when the post is neither seeking nor giving advice.

**Example 1:**
>https://www.reddit.com/r/studytips/comments/1rbgk4t/common_student_in_south_korea/
```
Common student in south Korea

I feel so unmotivated today
```

**Example 2:**
>https://www.reddit.com/r/studytips/comments/1r4kea3/trying_to_get_it_together/
```
Trying to get it together
```

### The hardest boundary

**Which two labels:** seeking and advising

**The decision rule I used every time:**
If a post is asking a question for discussion instead of genuinely asking for something the user wants and does not have, then this question does not contribute to the category. Several posts have a question in the header or in the body as a prompt for discussion, while the post's body text is primarily giving advice. Such a post is an `advising` post. If a post happens to have both advice and a genuine question, then `seeking`'s higher importance takes precedence.

**Other specific boundary rules I used:**
- If a post is asking a question for discussion, this does not contribute to the category. Unless none of the other categories apply, in which case this is Seeking.
- If a post is talking about a user’s positive experience, but is definitively citing specific actions the user is doing as the cause of the positivity, using a tone that implies the user is imploring other users to do the same, then it’s Advising.




---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/StudyTips on reddit: https://www.reddit.com/r/studytips/?communityAnchorSource=daily_discussion

**How I labelled them:**
I labelled all of them cold first, then asked Claude to go through all of them and tell me if any of them feel wrong. The posts that Claude convinced me to relabel are noted "ai-revised".

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| seeking | 98 | 49.0% |
| advising | 57 | 28.5% |
| experience | 45 | 22.5% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* I only get to study when it’s night time??? I love going to the library at night because it’s empty, calm and I can finally focus. The only problem is that I don’t feel comfortable going home later than 10 pm, so I have to leave even when I’m still motivated. Weirdly during the day it’s the opposite: I usually wake up around 9 am ready to go, but once I get there I can’t concentrate for long and I end up leaving pretty quickly without getting much done. Crowded libraries just don’t work for me. When my sister visits, it’s easier bcs she walks with me, so I feel safe enough to stay 2–3 hours longer at night. My friends in other hand don’t study at night, so most of the time I’m on my own. Does anyone else have this problem? How do you manage to study productively during the day if your brain only seems to work at a certain time?… help

>
> *Could have been:* Experience
>
> *I chose, because:* Seeking, because despite the user giving experience, they do ask a specific question for help at the end. 

**2.**
> *The post:* I just wanna be stupidly good at math. But I'm just a noob😭😭
>
> *Could have been:* Seeking
>
> *I chose, because:* Experience; arguably, the post is implicitly asking for tips. But because the post doesn't explicitly ask for help, it comes across as the user sharing their experience with math.

**3.**
> *The post:* Give me your most toxic study motivation: Toxic finals motivation tip: stand outside in the cold when ur too tired to study so yk how you'll feel when ur homeless if you fail
>
> *Could have been:* Advising
>
> *I chose, because:* Seeking; the user gave joke advice instead of real advice, so this post is not Advising, it is Seeking joke advice from others.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** "distilbert-base-uncased"

**Settings:** seed 42, 3 epochs, lr 2e-05, batch 16

**Anything I changed from the defaults, and why:** N/A

**Split sizes:** train 139  ·  val 31  ·  test 30

```
            train  val  test
label                       
seeking        68   15    14
advising       40    9     9
experience     31    7     7
```
(experience has only 7 entries in both the val and test sets, and advising has 9 in both as well.)

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Eight posts that sit on the boundaries of my first-draft categories
- *What came back:* Eight posts with overlapping cases or with the vibes of multiple cases, along with explanations of the weak spots exposed by the posts.
- *What I changed:* I renamed one of my categories and added more explanation to tighten them.

**Moment 2**

- *What I asked for:* Whether or not my acceptance criteria could be checked by someone who couldn't asked me what I meant.
- *What came back:* Assurance on two of my criteria, and feedback on ambiguous parts of the other three criteria.
- *What I changed:* I heavily revised my criterion about confidence, I added a general line clarifying that all criteria are applied to the held-out test set, and replaced Accuracy for Experience with Recall for Experience.

**Pre-labelling disclosure:** I read and labelled every one of the 200 posts, and I had Claude review each one and explain to me which posts felt like they were labeled wrong. Posts whose labels are revised due to the AI's feedback are noted with "ai-revised".

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.467 | 0.467 | 0.467 | 0.000 |
| Macro F1 | 0.212 | 0.212 | 0.212 | 0.000 |
| F1 — `seeking` | 0.636 | 0.636 | 0.636 | 0.000 |
| F1 — `advising` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `experience` | 0.000 | 0.000 | 0.000 | 0.000 |

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.333 | 0.467 | +0.133 |
| Macro F1 | 0.323 | 0.212 | -0.111 |
| F1 — `advising` | 0.435 | 0.000 | -0.435 |
| F1 — `experience` | 0.286 | 0.000 | -0.286 |
| F1 — `seeking` | 0.250 | 0.636 | +0.386 |

**What I predicted before I looked:**
I predicted that the baseline model would be much more accurate than the trained model, because the trained model was only trained off the sentences themselves, which could vary in what words would indicate their categories, and because the baseline is trained off my definitions, which are intricate and designed to catch edge cases.

**What the gap actually means:**
The trained model guessed seeking on every post and got 47% overall accuracy, perhaps indicating that the majority of seeking posts overpowers the advising and experience posts, making it hard for the model to discern distinguishing characteristics between the categories.
By contrast, the baseline model had more varied guesses, but had low accuracy for every category and overall, perhaps indicating that my definitions need to be tighter.



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

| true \ predicted | seeking | advising | experience |
|---|---|---|---|
| **seeking** | 14 | 0 | 0 |
| **advising** | 9 | 0 | 0 |
| **experience** | 7 | 0 | 0 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
9 real `advising` posts called `seeking`, because every post was predicted as `seeking`. This indicates that the model was not able to pick up on the distinctive qualities of any of the categories, and that it fell back on giving the label of the most frequent category as its guess.


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

**Agreement rate:** 29 / 30 = 97%

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
> *The post:* Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem.
>
> *Staff said / I said:* `hot_take` / `analysis`
>
> *My call, and why:* I still say this is `analysis`. I don't believe that the word "obviously" etc definitively shows that the writer came to the conclusion on their own before learning the numbers. The way the sentence is structured (Evidence, so Conclusion) make it sound like the author learned the numbers and came to an extreme conclusion based on them. Alternatively, I could even make an argument that the post might be sarcastic, with "obviously" and "whole story" being purposefully extreme and absurd, and the claim that times decreased by such a small amount might be analytic evidence to prove that the shoes couldn't possibly be the one problem.
>
> *Which pile:* (c) the rule is ambiguous here and my reading is defensible

**What the pattern in my disagreements tells me:** It's difficult for different people to agree on taxonomy where decision rules are based on the author's intent, which could be interpreted in different ways.



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
