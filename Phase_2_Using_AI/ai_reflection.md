# AI Reflection – FinBERT Explainability Dashboard

**AI-aided pair**

| First name | Last name | Student ID |
|---|---|---|
| first-name | last | k........ |
| first-name | last | k........ |

**Group:** AI and VIS - Finance Project · **Model:** FinBERT · **Data:** Twitter Financial News

Detailed record: `ai-interactions/phase2_ai_pair_log.md`

## 1. How we used GenAI

We used ChatGPT to explore how to investigate FinBERT's incorrect sentiment classifications and to develop an interactive explainability dashboard in our Jupyter Notebook.

ChatGPT introduced Explainable AI (XAI) techniques, particularly Integrated Gradients, SHAP and attention visualization. We chose Integrated Gradients as the primary approach because it provides token-level attribution scores that help us investigate which parts of a tweet contribute to a particular prediction.

ChatGPT assisted with accessing the underlying FinBERT model and tokenizer through the Hugging Face `pipeline()` API, implementing Integrated Gradients with Captum, and developing an interactive dashboard using ipywidgets and HTML.

The dashboard uses our existing misclassification datasets:
- `df_bear_but_pred_bull`: Tweets with an actual negative label that FinBERT classified as positive.
- `df_bull_but_pred_bear`: Tweets with an actual positive label that FinBERT classified as negative.

## 2. How AI influenced our analysis

ChatGPT helped us move from simply identifying incorrect predictions towards investigating their potential explanations.

The dashboard provides the following functionality:

- **Misclassification selection:** Select between the two categories of sentiment disagreements.
- **Individual tweet inspection:** Select individual misclassified tweets from a dropdown and navigate between examples.
- **Prediction comparison:** Display the actual sentiment alongside FinBERT's predicted sentiment.
- **Confidence analysis:** Display FinBERT's probability scores for negative, neutral and positive sentiment.
- **Token-level attribution:** Apply Integrated Gradients to identify which tokens contribute towards or against the predicted class.
- **Attribution ranking:** Display the most influential tokens according to their absolute attribution scores.

The aim is to investigate whether FinBERT systematically struggles with particular financial expressions, terminology or contextual patterns.

The dashboard also allows us to compare the two directions of misclassification separately, potentially revealing asymmetric weaknesses in FinBERT's sentiment classification.

## 3. Failure patterns and limitations

The explainability dashboard provides useful information, but its results require careful interpretation.

- **Attributions are not causal explanations:** Integrated Gradients estimates feature contributions relative to a selected baseline. It does not reveal FinBERT's exact internal reasoning.
- **Baseline dependency:** Attribution scores depend on the baseline used for the calculation. Replacing tokens with padding tokens is a modeling choice that can influence the explanations.
- **Tokenization:** FinBERT uses WordPiece tokenization, meaning individual words can be split into multiple tokens. This can complicate interpretation of financial expressions, company names and numerical values.
- **Confidence interpretation:** Softmax scores represent model confidence scores, not necessarily calibrated probabilities of correctness.
- **Potentially misleading interpretations:** A token with a high attribution score does not automatically mean that it is responsible for the misclassification.
- **Limited error categories:** Our current dashboard focuses on positive-versus-negative disagreements. Neutral-related errors are not yet included.

## 4. Best practices we found

- Use the existing prediction datasets instead of unnecessarily running predictions again.
- Preserve all three sentiment scores when examining individual tweets.
- Verify that actual and predicted labels are mapped correctly.
- Investigate individual tweets in their complete textual context.
- Distinguish between contributions towards the predicted class and sentiment polarity.
- Use Integrated Gradients as an exploratory explanation technique rather than definitive proof of model reasoning.
- Compare multiple misclassified examples before identifying systematic patterns.
- Record the baseline, attribution method and other relevant settings to ensure reproducibility.
- Validate interesting findings through manual inspection and, where possible, additional explainability methods.

## 5. Next steps

The current implementation provides an interactive environment for investigating individual positive/negative misclassifications.

Potential extensions include:

- Adding neutral-related misclassification categories.
- Aggregating token attribution scores across groups of misclassified tweets.
- Comparing correctly and incorrectly classified tweets.
- Investigating recurring financial expressions and terminology.
- Comparing Integrated Gradients with SHAP or other attribution methods.
- Exporting explanations and selected examples for further statistical analysis.

The overall objective is to move beyond measuring how often FinBERT makes mistakes and investigate which textual patterns may be associated with those errors.
