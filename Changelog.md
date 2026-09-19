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

