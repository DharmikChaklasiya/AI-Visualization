# Early exploration · validation split (superseded)

First Phase I exploration by the AI-aided pair, on the **validation split** (2,388 tweets), before the group agreed on the train split.

- `00_baseline_finbert_validation.ipynb`: FinBERT predictions (accuracy 0.725), written to `results_validation.csv`
- `01_error_patterns_validation.ipynb`: first error analysis; figures in `figures/` (F1–F4)

**Note:** the 2D map (F4) uses a separate encoder (MiniLM + UMAP). The group later decided not to use any other model, so the main analysis replaces it with FinBERT's probability triangle and word-count topics.
Kept for transparency (it is referenced in the AI interaction log, entries #4–#6) and as a robustness check: the main patterns (Neutral tweets given a sentiment, signed price moves, mixed cues, overconfidence) appear on both splits.

Main analysis: `../01_neutral_error_analysis.ipynb` (train split).
