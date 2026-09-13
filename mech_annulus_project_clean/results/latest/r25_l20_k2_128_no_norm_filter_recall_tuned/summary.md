# R25 L=20, K=2..128 No-Norm-Filter Recall Tuning

Fixed hash tables L=20 and varied hash code length K=2,4,...,128.
The original norm-table and radial-layer candidate filters are disabled; layers are only used to compute per-layer angular Hamming thresholds.
The recall boost increases until K=50 and then decreases toward K=128 so the recall-focused setting is centered around K=50.
Candidate/true ratio is `candidate_expansion_ratio` = |candidate set| / |exact R25 true set|.
Norm/radial filter: `disabled: no norm-table or radial-layer candidate filter`.

## Best Settings by Precision/Recall/Ratio Score

| dataset | selection | L | K | point_precision | point_recall | point_f1 | candidate_to_true_ratio | candidate_size | exact_r25_candidate_size | query_time_ms | score_precision_recall_ratio |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| amazon | precision_recall_ratio_score | 20 | 2 | 0.2505 | 0.9865 | 0.3995 | 3.9388 | 775.9500 | 197.0000 | 1.0222 | 0.4986 |
| cifar10 | precision_recall_ratio_score | 20 | 4 | 0.2500 | 1.0000 | 0.4000 | 4.0000 | 1800.0000 | 450.0000 | 2.3805 | 0.4838 |
| cifar10_gist512 | precision_recall_ratio_score | 20 | 54 | 0.4006 | 0.7319 | 0.5150 | 1.8294 | 823.2250 | 450.0000 | 24.9793 | 0.5315 |
| isolet | precision_recall_ratio_score | 20 | 74 | 0.4857 | 0.5931 | 0.5176 | 1.3055 | 587.5000 | 450.0250 | 28.1567 | 0.5231 |

## Outputs

- `r25_l20_k2_128_no_norm_filter_recall_tuned_all.csv`
- `L20_K2_128_no_norm_filter_recall_tuned.csv`
- `mean_L20_K2_128_precision_recall_ratio.csv`
- `best_no_norm_filter_recall_tuned_by_dataset.csv`
- `figures/by_dataset/<dataset>_L20_K2_128_precision_recall_ratio.png`
- `figures/aggregate/mean_L20_K2_128_precision_recall_ratio.png`