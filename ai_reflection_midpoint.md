# AI Reflection – Midpoint (Phase I)

**AI-aided pair**
| First name | Last name | Student ID |
|---|---|---|
| Dharmik | Chaklasiya | K51849432 |
| Saranajjan | Senthilkumar | k12442723 |

**Group:** AI and VIS - Finance Project · **Model:** FinBERT · **Data:** Twitter Financial News (train split)
Detailed record: [`ai-interactions/phase1_ai_pair_log.md`](ai-interactions/phase1_ai_pair_log.md)

## 1. How we used GenAI
**Dharmik Chaklasiya** _(DRAFT; edit)_

Claude (with a Hugging Face Hub connector) for planning, code, statistics, figures and fact checks; details in section 2 and log #1–#7, #11.

2026-11-07 — Saranajjan Senthilkumar:

ChatGPT suggested verdicts and notes for 120 neutral-labelled disagreements. I reviewed and accepted all suggestions without changes, then uploaded the CSV. This was AI-assisted annotation with human verification, not independent annotation.

## 2. How AI influenced our analysis
2026-11-03 — Saranajjan Senthilkumar

I reviewed B2 and B3 with ChatGPT’s assistance. The review produced suggestions to mark the 0.9 threshold in B2, describe word associations without implying causation in B3, and investigate whether “misses” co-occurs with “beats.” These remain suggestions; no plot changes have been implemented.

**Dharmik Chaklasiya** _(DRAFT, written with AI help from the log; rewrite in your own words)_

I used Claude for most of the pipeline: reviewing our plan, writing the baseline and analysis notebooks, testing patterns statistically and drawing the figures. It also had a Hugging Face connector, so it could check FinBERT's config and the dataset sizes against the source files (log #5).
AI shaped our analysis in three ways. It proposed the main hypothesis, that the dataset's *Neutral* means "no stance on a stock" while FinBERT judges tone (#6). It suggested testing every pattern on the whole split, with Fisher tests and confidence intervals, instead of reading examples. And when the group ruled out a second model, it proposed the probability triangle as a FinBERT-only replacement for the MiniLM map (#7).
Rejected or changed: the MiniLM/UMAP map (group decision, #3) and the synthetic edit set (group decision).


## 3. Failure patterns
2026-11-03 — Saranajjan Senthilkumar

Runnable AI-generated code does not automatically support reliable conclusions. Results from a small, balanced sample may not reflect performance across the dataset’s natural class distribution. Maximum softmax scores should not be treated as calibrated probabilities of correctness, and words associated with errors should not automatically be interpreted as their causes. These limitations require broader evaluation, calibration checks, and manual inspection before drawing conclusions.

**Dharmik Chaklasiya** _(DRAFT; edit)_

- **Recalled facts presented as knowledge:** FinBERT's label order came from memory before it was verified (F1).
- **Code that only fails when run:** pandas incompatibilities, a broken string escape that left an old figure in place, and an export cell that would have overwritten Sarang's reviewed CSV (F8, F10).
- **Arbitrary settings:** cue-word lists, number of clusters, and a first topic clustering that put most tweets into one group (F4, F7).
- **Plausible but unproven interpretations:** the "why" is a hypothesis until the manual review and the blind check support it (F6, F9).


## 4. Best practices we found
2026-11-03 — Saranajjan Senthilkumar

Map labels by meaning rather than assuming numeric IDs match between the model and dataset. Preserve all three sentiment scores, verify outputs after execution, and report sample sizes alongside error rates. Use row-normalized confusion matrices to compare classes with different frequencies. Inspect complete tweets before interpreting disagreements, and document which AI suggestions were accepted, modified, or left unverified.

**Dharmik Chaklasiya** _(DRAFT; edit)_

Turn every AI-stated fact into an assertion or a check against the source. Ask for sample sizes and significance with every claimed pattern. Judge figures by rendering them, not by reading the code. Write down your own interpretation before reading the AI's. Keep AI-assisted annotations separate from independent human ones.


## 5. Comparison with the human-only pair
2026-11-03 — Saranajjan Senthilkumar

Both pairs use FinBERT and the agreed train split, while developing their investigations separately. Our comparison should examine the questions explored, visualization choices, verification effort, and evidence supporting conclusions. My initial experience shows that AI can accelerate implementation, but we have not yet completed a structured comparison demonstrating whether it improved the depth or quality of the investigation.

**Dharmik Chaklasiya** _(to fill in after comparing with Matthias & Henry)_

<!-- what each pair found / missed, visualization choices, breadth vs. depth, time saved vs. time spent verifying, confidence and its evidence -->

