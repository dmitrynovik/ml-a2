# Task 6
1. Curse of dimeonsionality: adding the CV as better indicator. Adding the CV plot.
2. PCA: Changing the thresold - 1000 components do not explain the chosen 80% of cumulative variance.
3. Fixing the cells output order
4. Adding perplexity steps (choosing the optimal perplexity needs justification).
5. Changing the optimal perplexity to: *20*. 

# Tasl 7
1. Added handling anomalies and outliers.
2. Correction: k=3 means 3 clusters
3. Note: `IsBadBuy` as a clustering feature causes circularity.
4. Missing cluster distribution plots (task's hint)

# Task 8
1. Fixed train/test metric mismatch: training RMSE was computed on standardized (z-score) values while test RMSE was inverse-transformed to original TSDM units, making the two incomparable. Training metrics are now inverse-transformed per-paddock too.
2. Model 3 (multivariate) was using a fixed lookback=10 window, contradicting the "no fixed lookback" requirement. Replaced with a growing-window approach (`create_sequences_flexible`/`prepare_tsdm_data_flexible`) that uses each paddock's full available history, padded only for batching.
3. Added hyperparameter tuning (grid search over hidden_size/num_layers/lr) for Model 1, with the winning configuration reported and used for the final training run.
4. Added markdown explaining the importance of each preprocessing step (chronological sort, per-paddock exclusion criterion, per-paddock train-only scaling, reserving last 5 timesteps, padding + tracked lengths).
5. Removed leftover debug/scratch cells; folded their useful diagnostics (distribution-shift check, per-paddock RMSE spread) into a proper "Additional Diagnostics" section using the corrected evaluation code.

# Task 9
1. Fixed missing/incorrect ROC-AUC values: `bert-base-uncased`'s AUC was never computed anywhere in the notebook, and the final report table's AUC numbers for all three models didn't match what the code actually produced. Added explicit `auc_m1` computation and an "AUC Summary" print so the report table is copied from real output.
2. Fixed stale notebook output: cell 19's code prints a `bert-base-uncased` classification report, but the saved output was from before that line was added, so it never appeared. Re-executed the notebook end-to-end and updated the final report's accuracy/precision/recall/F1/AUC numbers to match the verified run.
3. Fixed an unverified assumption that `tfidf_features_small.csv` rows line up 1:1 with `hydrogen_small.csv` rows (no shared ID column existed). Added a sanity check that confirms in-vocabulary words from sampled tweets have nonzero TF-IDF weight in the matching row (validated: 100% match on real data vs ~9% on a deliberately shifted control).
4. Fixed the attention-weight visualization being dominated by the attention-sink every transformer directs at its special tokens (`<s>`/`</s>`), which swamped the actual content-word pattern and didn't support the written interpretation. Added `filter_special_tokens` (drops special tokens, renormalizes) and a "top attended words" bar chart so the visualization highlights the words actually driving the classification.

# Task 10
1. Fixed `preprocess()` tokenizing the question twice: `question_with_context` already embeds the question ("question: {q} context: {c}"), but it was passed alongside `question` again as a `text_pair`, and T5's tokenizer just concatenates pair sequences (no BERT-style `[SEP]`), so the question was duplicated in every training input. Now tokenizes `question_with_context` alone.
2. Fixed `generate_response()` truncating the question+context input to `MAX_OUTPUT_LENGTH` (128) instead of `MAX_INPUT_LENGTH` (512) at inference time, inconsistent with training preprocessing and cutting off longer contexts for both the fine-tuned model and the pretrained-model comparison. Also added explicit `max_new_tokens=MAX_OUTPUT_LENGTH` on `.generate()` instead of relying on the deprecated default.
3. Added the missing pretrained-model (`mrm8488/t5-base-finetuned-squadv2`) without-context ROUGE evaluation and first-5-sample generative analysis, so its evaluation now replicates Tasks 3/4 the same way the fine-tuned model's does (previously only with-context ROUGE was computed for it).
4. Translated remaining Korean markdown headers and code comments to English.
5. Note: fixes #1–#2 change training/inference behavior, so the notebook needs a full re-run (re-fine-tuning, not just re-evaluation) and `Assessment2-Report.docx`'s Task 10 numbers/tables need updating to match.

