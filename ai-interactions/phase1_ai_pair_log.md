# AI Interaction Record – AI-aided pair, Phase I
**Group:** AI and VIS – Finance Project · **Pair:** Dharmik(K51849432) & Sarang() · **Tool(s):** Claude
**Model / data:** ProsusAI/finbert on zeroshot/twitter-financial-news-sentiment

**Decision codes:** ✅ accepted · ✏️ modified · ❌ rejected · 🔍 verified (checked against a source)

---

## Log

| # | Date | Who | Prompt (short) | What the AI suggested | Decision | Why / effect on analysis |
|---|---|---|---|---|---|---|
| 1 | 2026-10-29 | Dharmik | Review the team's FinBERT + Twitter plan against the assignment | (a) FinBERT was fine-tuned on Financial PhraseBank → results there are optimistic, use it only as an "easy" baseline. (b) Dataset and FinBERT may order labels differently → check before analysing. (c) Edit-based tests (negation, company swap) count as a synthetic dataset, must be large enough to be systematic. | ✅ (a), 🔍 (b), ✏️ (c) | (a) Confirmed our choice of the tweet set as main data. (b) Led to name-based label mapping in the baseline (see #4). (c) Kept as optional, decided after baseline. |
| 2 | 2026-10-29 | Dharmik | Where should the AI-aided pair begin for Phase I? | Build a shared baseline table (text, true, pred, 3 softmax scores) → confusion matrix, confidence for correct vs. wrong, confidently wrong examples, slices (tickers, URLs, numbers, length), 2D map coloured by error. Propose an interactive linked dashboard as the "richer interaction" deliverable. | ✅ / proposed to group | Defines our Phase I starting point; dashboard to be decided after first results. |
| 3 | 2026-10-29 | Dharmik | (follow-up) Is a 2D projection of tweets still black-box? | Use a **separate** sentence encoder (MiniLM + UMAP) instead of FinBERT embeddings, keep FinBERT internals for Phase II; flagged as uncertain → ask instructors at Feedback 1. | ❌ (group decision, 31.10) | AI was itself unsure here. The group decided not to use any other model, so the MiniLM map was dropped; replaced in #7. |
| 4 | 2026-10-30 | Dharmik | Write the baseline notebook | `00_baseline_finbert.ipynb`: loads data + model, maps labels **by name** with an assert, saves `results_validation.csv`, confusion matrix, confidence histogram, top-25 confidently wrong tweets, slice error rates, optional UMAP map (Plotly, hover shows tweet). | ✅ (verified on Colab) | Now in `Phase_1_Using_AI/early_exploration_validation/`. AI verified dataset labels from the dataset card (0 Bearish, 1 Bullish, 2 Neutral) but **could not open FinBERT's config or run the model** (its sandbox had no Hugging Face access); it only tested the analysis cells on mock data. → We must check the printed `id2label` and the numbers ourselves. |
| 5 | 2026-10-30 | Dharmik | Verify FinBERT's label order and the real split sizes (AI given a Hugging Face Hub connector) | Read FinBERT `config.json`: **0 positive, 1 negative, 2 neutral**. Dataset: **0 Bearish, 1 Bullish, 2 Neutral**. Split sizes from the Hub: train ≈ 9.5k, validation ≈ 2.4k (card text 9,938 / 2,486 is outdated). | 🔍 ✅ | Confirms the orders **really differ**: matching by index would have scored every Bearish tweet predicted "positive" as correct, and vice versa. Name-based mapping in the notebook stays. Earlier recalled order (F1) turned out correct, but was only trustworthy after checking. |
| 6 | 2026-10-30 | Dharmik | Analyse our baseline run (`results_validation.csv`, 2,388 tweets, acc 0.725) for systematic error patterns | `01_error_patterns.ipynb` + figures F1–F4: (F1) 58% of errors = Neutral tweets given a sentiment; (F2) overconfidence, conf 0.6–0.8 → ~50–57% accuracy, 145 errors > 0.9; (F3) signed price moves 58% error (n=55, p=2e-6), mixed pos+neg cues 52% (n=46, p=3e-4), macro news 36% (p=4e-3); FinBERT follows cue words more than labels do; (F4) k-means clusters on UMAP: macro/China/debt/oil ~35% error vs. dividend/price-target templates 7–14%. Interpretation offered: dataset "Neutral" = no stance on a stock, FinBERT judges tone. | ✏️ group meeting 31.10: Neutral direction assigned to our pair; train split; no other model; no synthetic set; FiQA dropped | Now in `early_exploration_validation/`. AI looked at all 656 errors and tested each pattern with Fisher's test + CIs instead of picking examples. **Our own view before reading AI's analysis:** _(fill in!)_. Decision on which patterns become shared Phase I questions: _(after meeting)_. |
| 7 | 2026-10-31 | Dharmik | Rebuild our Phase I analysis for the group decisions (train split, no other model, our pair = Neutral direction) and add the teammates' questions | `01_neutral_error_analysis.ipynb` on `results_train.csv` (9,543 tweets, acc 0.712): (A1) errors in counts **and % per class** (Matthias' request); (B1) **probability triangle** of FinBERT's 3 softmax scores to replace the MiniLM map; (B2) confidence of wrong Neutral calls, (B3) trigger words, (B5) multi-company tweets (Sarang's questions); (B4) topics from word counts (TF-IDF → SVD → k-means); (D) 120-tweet manual-review sheet. Suggested keeping the validation work as `early_exploration_validation/` rather than deleting it. | ✅ | Results: 60% of errors are Neutral tweets; Neutral→Bearish triggered by non-stock bad news (*virus, court, protesters, scandal, strike*); Neutral earnings tweets (*beats/misses*) given a sentiment 68%, China/trade 47%, dividends 5%; signed price moves 59% error (n=192, p=6e-19), mixed cues 51% (n=173, p=8e-10), ≥2 tickers 38% vs 29% (p=2e-3). Patterns from #6 **replicate** on the 4× larger train split. |
| 8 |  |  |  |  |  |  |

---

## Failure patterns observed
| # | Log ref | What went wrong / risk | How we caught it |
|---|---|---|---|
| F1 | #1, #4 | AI stated FinBERT's label order "as far as I know" (from memory, not verified). | Notebook maps labels by name + assert. **Resolved in #5:** verified from config.json (recall was right, but unverified at the time). |
| F2 | #4 | Code delivered without being run on the real model/data (AI's sandbox can read the Hub but cannot download model weights). | Run on Colab; compare printed id2label, row counts and accuracy with expectations. |
| F3 | #4 | Dataset card says 9,938 / 2,486 rows, but the HF viewer shows ~9.54k / 2.39k. | **Resolved in #5:** the dataset's own stats show ~9.5k / 2.4k; the card text is outdated. Confirm with `df.shape`. **Confirmed by our run:** 2,388 validation tweets. |
| F4 | #6 | AI-chosen analysis settings are arbitrary: hand-made cue-word regexes, k = 14 clusters on 2D UMAP coordinates. Findings could shift with other choices. | Rerun with other k / on the train split; check the regex lists ourselves by reading matched tweets. **Partly resolved in #7:** patterns replicate on the train split; topics now from word counts, not UMAP. |
| F5 | #6 | AI's first figures had overlapping labels (F2, F3); it only noticed after rendering and looking at them. | AI re-rendered and checked; we still check every figure before the video. |
| F6 | #6 | The "why" (label-definition mismatch) is a plausible AI interpretation, not proven. Small slices (n = 46, 55) are still weak evidence. | Test causally with a synthetic edit set (flip sign / connector) and on more data. **#7:** more data done (n = 192 / 173); synthetic set dropped by group decision; manual review (D) tests the interpretation instead. |
| F7 | #7 | AI's first topic clustering (k-means directly on sparse TF-IDF) put most tweets into one huge group, so the topic chart was nearly meaningless. | AI checked the cluster sizes, switched to TF-IDF → SVD(100) → k-means, and labels the remaining catch-all group as "mixed / other". |
| F8 | #7 | AI-written code had bugs only visible when run: a pandas-3 incompatibility and a broken string escape that silently left an old figure in place. | Caught by executing the full notebook and comparing the saved figure, not by reading the code. |
|  |  |  |  |

## Emerging best practices
1. Never trust AI-stated label mappings / config values — encode them as assertions in code.
2. Ask the AI to separate *verified* facts from *recalled* ones, and log which is which.
3. AI is fast at producing standard plots; the human work is reading the confidently-wrong examples and forming the *why*.
4. Give the AI direct access to sources (here: the Hub connector) so it can check facts instead of recalling them; documentation text (dataset cards) can be outdated, so prefer the actual files/stats.
5. Ask the AI for significance tests and sample sizes with every claimed pattern; drop patterns that rest on a few examples.
6. Write down your own interpretation *before* reading the AI's, so the log can show what the AI changed.
7. Rendered output is the test: check that every figure actually changed after a fix (a failed run can leave the old image in place).
8. When the group changes a constraint (data split, no extra model), ask the AI to rebuild the pipeline and re-test earlier findings, rather than patching single plots.
9. 

## Effect on analysis decisions (summary for the midpoint video)
- 
