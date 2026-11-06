# AI Reflection – Midpoint (Phase I)

**AI-aided pair**
| First name | Last name | Student ID |
|---|---|---|
| Dharmik | Chaklasiya | K51849432 |
| Saranajjan | Senthilkumar | k12442723 |

**Group:** AI and VIS - Finance Project · **Model:** FinBERT · **Data:** Twitter Financial News (train split)
Detailed record: [`ai-interactions/phase1_ai_pair_log.md`](ai-interactions/phase1_ai_pair_log.md)

## 1. How we used GenAI
2026-11-07 — Saranajjan Senthilkumar:

ChatGPT suggested verdicts and notes for 120 neutral-labelled disagreements. I reviewed and accepted all suggestions without changes, then uploaded the CSV. This was AI-assisted annotation with human verification, not independent annotation.

## 2. How AI influenced our analysis
2026-11-03 — Saranajjan Senthilkumar

I reviewed B2 and B3 with ChatGPT’s assistance. The review produced suggestions to mark the 0.9 threshold in B2, describe word associations without implying causation in B3, and investigate whether “misses” co-occurs with “beats.” These remain suggestions; no plot changes have been implemented.

## 3. Failure patterns
2026-11-03 — Saranajjan Senthilkumar

Runnable AI-generated code does not automatically support reliable conclusions. Results from a small, balanced sample may not reflect performance across the dataset’s natural class distribution. Maximum softmax scores should not be treated as calibrated probabilities of correctness, and words associated with errors should not automatically be interpreted as their causes. These limitations require broader evaluation, calibration checks, and manual inspection before drawing conclusions.
## 4. Best practices we found
2026-11-03 — Saranajjan Senthilkumar

Map labels by meaning rather than assuming numeric IDs match between the model and dataset. Preserve all three sentiment scores, verify outputs after execution, and report sample sizes alongside error rates. Use row-normalized confusion matrices to compare classes with different frequencies. Inspect complete tweets before interpreting disagreements, and document which AI suggestions were accepted, modified, or left unverified.

## 5. Comparison with the human-only pair
2026-11-03 — Saranajjan Senthilkumar

Both pairs use FinBERT and the agreed train split, while developing their investigations separately. Our comparison should examine the questions explored, visualization choices, verification effort, and evidence supporting conclusions. My initial experience shows that AI can accelerate implementation, but we have not yet completed a structured comparison demonstrating whether it improved the depth or quality of the investigation.
