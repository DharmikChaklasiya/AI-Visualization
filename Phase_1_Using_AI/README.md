# Phase I · AI-aided pair (Dharmik & Sarang)

**Model:** [ProsusAI/finbert](https://huggingface.co/ProsusAI/finbert) · **Data:** [zeroshot/twitter-financial-news-sentiment](https://huggingface.co/datasets/zeroshot/twitter-financial-news-sentiment), **train split** (9,543 tweets, not seen by FinBERT; same data as the human-only pair)
**Mode:** black box: only inputs, outputs and softmax scores. No other model is used (group decision).

## Our direction: the Neutral tweets
Main question: **why does FinBERT give a sentiment to tweets that humans labelled Neutral?**
(The human-only pair investigates the Bullish/Bearish direction.)

| Part | Question |
|---|---|
| A | Where do errors come from (counts and % per class)? Is FinBERT overconfident? |
| B | Where do Neutral tweets sit in FinBERT's probability triangle? Are the wrong calls confident? Which words and topics trigger them? Do tweets with several companies cause ambiguity? |
| C | Other systematic slices: signed price moves, mixed cues, questions |
| D | Manual review: clear model mistake vs. ambiguous label |

## Label mapping
FinBERT outputs are renamed to the dataset's labels **by name**: `negative → Bearish`, `positive → Bullish`, `neutral → Neutral`
(FinBERT: 0 positive, 1 negative, 2 neutral; dataset: 0 Bearish, 1 Bullish, 2 Neutral, so matching by index would be wrong).

## How to run
1. `00_baseline_finbert.ipynb` (Colab, T4 GPU): runs FinBERT and writes `results_train.csv`.
2. `01_neutral_error_analysis.ipynb` (CPU is fine): reads `results_train.csv`, saves figures to `figures/` and `manual_review_neutral.csv`.

## Files
| File | Content |
|---|---|
| `00_baseline_finbert.ipynb` | predictions + softmax scores, baseline views |
| `01_neutral_error_analysis.ipynb` | Phase I analysis (parts A–D) |
| `results_train.csv` | model outputs per tweet (created by 00) |
| `manual_review_neutral.csv` | 120 wrong Neutral tweets for hand labelling (created by 01) |
| `figures/` | all figures |
| `early_exploration_validation/` | first exploration on the validation split (superseded; kept for transparency and as a robustness check) |

AI interaction record: [`../ai-interactions/phase1_ai_pair_log.md`](../ai-interactions/phase1_ai_pair_log.md)
