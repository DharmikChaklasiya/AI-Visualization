# AI Reflection – Midpoint (Phase I)

**AI-aided pair**
| First name | Last name | Student ID |
|---|---|---|
| Dharmik | Chaklasiya | K51849432 |
| Saranajjan | Senthilkumar | k12442723 |

**Group:** AI and VIS - Finance Project · **Model:** FinBERT · **Data:** Twitter Financial News (train split)
Detailed record: [`ai-interactions/phase1_ai_pair_log.md`](ai-interactions/phase1_ai_pair_log.md)

## 1. How we used GenAI
2026-11-03 — Saranajjan Senthilkumar

I used ChatGPT to help develop a Colab inference pipeline for FinBERT, including dataset loading, mapping reference labels to sentiment names, extracting all three softmax scores, and comparing predictions with reference labels. AI also helped generate a confusion matrix and a per-tweet prediction-score chart. I ran the pipeline on an initial validation sample and inspected the outputs to check that prediction, label comparison, and visualization worked before extending the analysis.

## 2. How AI influenced our analysis
2026-11-03 — Saranajjan Senthilkumar

AI helped turn the initial test into questions for further investigation. The sample contained negative-to-neutral, positive-to-neutral, and neutral-to-positive disagreements. These suggested examining the direction of errors and their prediction scores rather than relying only on accuracy. For our pair’s neutral-tweet investigation, manual review will help distinguish model errors from ambiguous examples and questionable reference labels.

## 3. Failure patterns
2026-11-03 — Saranajjan Senthilkumar

Runnable AI-generated code does not automatically support reliable conclusions. Results from a small, balanced sample may not reflect performance across the dataset’s natural class distribution. Maximum softmax scores should not be treated as calibrated probabilities of correctness, and words associated with errors should not automatically be interpreted as their causes. These limitations require broader evaluation, calibration checks, and manual inspection before drawing conclusions.
## 4. Best practices we found
2026-11-03 — Saranajjan Senthilkumar

Map labels by meaning rather than assuming numeric IDs match between the model and dataset. Preserve all three sentiment scores, verify outputs after execution, and report sample sizes alongside error rates. Use row-normalized confusion matrices to compare classes with different frequencies. Inspect complete tweets before interpreting disagreements, and document which AI suggestions were accepted, modified, or left unverified.

## 5. Comparison with the human-only pair
2026-11-03 — Saranajjan Senthilkumar

Both pairs use FinBERT and the agreed train split, while developing their investigations separately. Our comparison should examine the questions explored, visualization choices, verification effort, and evidence supporting conclusions. My initial experience shows that AI can accelerate implementation, but we have not yet completed a structured comparison demonstrating whether it improved the depth or quality of the investigation.
