# FinBERT Explainability Dashboard

## Overview

This notebook implements an interactive Explainable AI (XAI) dashboard to investigate incorrect sentiment classifications made by FinBERT on Twitter Financial News data.

Instead of only examining classification accuracy, the dashboard uses **Integrated Gradients** to investigate which input tokens contribute towards or against FinBERT's predicted sentiment.

The implementation uses Hugging Face Transformers, PyTorch, Captum and ipywidgets.

## 1. Objectives

The dashboard aims to:

- Investigate individual FinBERT misclassifications.
- Compare actual and predicted sentiment labels.
- Visualize token-level attribution scores.
- Identify potentially misleading financial terminology and contextual patterns.
- Explore differences between positive-to-negative and negative-to-positive classification errors.

## 2. Misclassification datasets

The dashboard uses two previously generated CSV datasets.

| Dataset | Actual label | Predicted label |
|---|---|---|
| `df_bear_but_pred_bull.csv` | Negative | Positive |
| `df_bull_but_pred_bear.csv` | Positive | Negative |

Both datasets contain the original tweet text, actual sentiment, predicted sentiment and the three FinBERT sentiment scores.

Expected columns:

- `text`
- `label`
- `predicted_scores`
- `predicted_label`
- `negative`
- `neutral`
- `positive`
- `pred_top_score`

## 3. Technical implementation

### Libraries

- `transformers`: Loads and executes the pretrained FinBERT model through the Hugging Face pipeline.
- `torch`: Provides the underlying model execution and tensor operations.
- `captum`: Implements Integrated Gradients.
- `ipywidgets`: Creates interactive notebook controls.
- `pandas`: Loads and manages the misclassification datasets.
- `IPython.display`: Renders the HTML-based dashboard inside Jupyter Notebook.

### Integrated Gradients

The dashboard uses Captum's `LayerIntegratedGradients` to calculate token-level attributions.

The attribution method operates on FinBERT's word embedding layer.

The calculation follows these steps:

1. Tokenize the selected tweet.
2. Obtain FinBERT's prediction and class probabilities.
3. Generate a baseline by replacing ordinary input tokens with padding tokens while preserving special tokens.
4. Calculate Integrated Gradients for the predicted class.
5. Aggregate embedding-dimension attributions into one score per token.
6. Visualize the resulting scores.

Positive attribution scores support the predicted class, while negative scores oppose it.

### Dashboard components

**Dataset selection**
- Select between the two misclassification categories.
- Browse individual tweets through a dropdown.
- Navigate using Previous and Next buttons.

**Classification results**
- Display the actual sentiment.
- Display the predicted sentiment.
- Show the prediction confidence.
- Visualize negative, neutral and positive class scores.

**Token-level explanations**
- Highlight tokens according to their attribution scores.
- Use green for positive contributions and red for negative contributions towards the predicted class.
- Display exact attribution scores when hovering over tokens.

**Influential tokens**
- Rank the ten tokens with the highest absolute attribution scores.

## 4. How to run the dashboard

### Prerequisites

Install the required libraries:

```bash
pip install transformers torch captum ipywidgets pandas
```

### Data loading

Load the two misclassification datasets:

```python
df_bear_but_pred_bull = pd.read_csv(
    "../data/predictions/df_bear_but_pred_bull.csv"
)

df_bull_but_pred_bear = pd.read_csv(
    "../data/predictions/df_bull_but_pred_bear.csv"
)
```

### Model requirements

Before executing the dashboard, ensure that:

- FinBERT is loaded through a Hugging Face pipeline.
- The underlying `model` and `tokenizer` are available.
- The Integrated Gradients object (`lig`) is initialized.
- The `explain_tweet()` function is defined.
- Both misclassification DataFrames are loaded.

### Execution

Execute the notebook cells in order:

1. Load libraries, FinBERT and the datasets.
2. Initialize Integrated Gradients.
3. Define `explain_tweet()`.
4. Define the dashboard functions and widgets.
5. Display the interactive dashboard.

## 5. Interpretation and limitations

The attribution scores should be interpreted as estimates of feature contributions relative to the selected baseline.

They do not represent the model's exact internal reasoning or establish causal relationships.

Additional considerations:

- Softmax scores should not automatically be interpreted as calibrated probabilities.
- WordPiece tokenization can split individual words into multiple tokens.
- Padding-based baselines can influence attribution results.
- Individual examples are insufficient to establish systematic model weaknesses.
- The current dashboard only covers positive/negative misclassifications.

Findings should therefore be validated through manual inspection and broader analysis across multiple examples.

## 6. Potential extensions

- Include neutral-related misclassifications.
- Compare correctly and incorrectly classified tweets.
- Aggregate token attributions across misclassification categories.
- Compare Integrated Gradients with SHAP.
- Export individual explanations for further analysis.
- Investigate recurring financial expressions associated with classification errors.

## 7. Reproducibility

For reproducible results, document the FinBERT model checkpoint, tokenizer, model configuration, Captum version, attribution baseline, number of integration steps and dataset version.

The dashboard is intended as an exploratory analysis tool rather than a standalone evaluation of model performance.
