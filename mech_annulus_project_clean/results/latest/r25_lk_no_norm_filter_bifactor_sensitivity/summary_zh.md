# No-Norm L/K 双因子敏感性分析

任务：同时调节哈希表数量 L 和哈希码长度 K，为每个数据集选择一组最优参数。

## 实验设置

- L 网格：2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20。
- K 网格：10, 20, 30, 40, 50, 60, 70, 80, 90, 100。
- 召回达标线：point_recall >= 0.90。
- 候选策略：禁用原模长/径向层候选过滤；径向层只用于计算角度阈值。
- 最优选择：若存在召回达标组合，则在达标组合里优先最大化 F1，再看 precision、recall、候选膨胀数和查询时间；若无达标组合，则退化为最高 recall。

## 每个数据集的最优 L/K

| dataset | selection | L | K | point_precision | point_recall | point_f1 | candidate_to_true_ratio | candidate_size | query_time_ms | optimal_score | recall_feasible |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| amazon | recall_ge_0.90_max_f1_precision | 8 | 50 | 0.2510 | 1.0000 | 0.4012 | 3.9848 | 785.0000 | 1.3127 | 0.4263 | True |
| cifar10 | recall_ge_0.90_max_f1_precision | 2 | 20 | 0.2500 | 1.0000 | 0.4000 | 4.0000 | 1800.0000 | 0.9954 | 0.4454 | True |
| cifar10_gist512 | recall_ge_0.90_max_f1_precision | 13 | 50 | 0.3486 | 0.9041 | 0.5021 | 2.6146 | 1176.5750 | 2.3782 | 0.4871 | True |
| isolet | fallback_max_recall | 20 | 50 | 0.3367 | 0.8812 | 0.4848 | 2.6686 | 1200.9500 | 2.1458 | 0.4537 | False |

## 固定 K=50 的哈希表数量敏感性

| dataset | selection | K | best_L | point_precision | point_recall | point_f1 | candidate_expansion_ratio | candidate_size | query_time_ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| isolet | fallback_max_recall | 50 | 20 | 0.3367 | 0.8812 | 0.4848 | 2.6686 | 1200.9500 | 2.1458 |
| cifar10 | recall_ge_0.90_max_f1_precision | 50 | 2 | 0.2500 | 1.0000 | 0.4000 | 4.0000 | 1800.0000 | 1.0336 |
| cifar10_gist512 | recall_ge_0.90_max_f1_precision | 50 | 13 | 0.3486 | 0.9041 | 0.5021 | 2.6146 | 1176.5750 | 2.3782 |
| amazon | recall_ge_0.90_max_f1_precision | 50 | 8 | 0.2510 | 1.0000 | 0.4012 | 3.9848 | 785.0000 | 1.3127 |

## 固定 L=20 的哈希码长度敏感性

| dataset | selection | L | best_K | point_precision | point_recall | point_f1 | candidate_expansion_ratio | candidate_size | query_time_ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| isolet | fallback_max_recall | 20 | 50 | 0.3367 | 0.8812 | 0.4848 | 2.6686 | 1200.9500 | 2.1458 |
| cifar10 | recall_ge_0.90_max_f1_precision | 20 | 10 | 0.2500 | 1.0000 | 0.4000 | 4.0000 | 1800.0000 | 2.3108 |
| cifar10_gist512 | recall_ge_0.90_max_f1_precision | 20 | 50 | 0.3336 | 0.9310 | 0.4903 | 2.8116 | 1265.2000 | 2.6452 |
| amazon | recall_ge_0.90_max_f1_precision | 20 | 50 | 0.2510 | 1.0000 | 0.4012 | 3.9848 | 785.0000 | 1.6007 |

## 公共参数 Top 10

| L | K | mean_precision | mean_recall | min_recall | mean_f1 | mean_candidate_expansion_ratio | mean_query_time_ms | mean_optimal_score | recall_feasible_all |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 16 | 40 | 0.3636 | 0.7777 | 0.4382 | 0.4452 | 2.6112 | 1.8820 | 0.4727 | False |
| 15 | 40 | 0.3644 | 0.7723 | 0.4294 | 0.4433 | 2.5973 | 1.8356 | 0.4724 | False |
| 17 | 40 | 0.3632 | 0.7786 | 0.4354 | 0.4453 | 2.6139 | 1.9320 | 0.4723 | False |
| 18 | 40 | 0.3626 | 0.7815 | 0.4388 | 0.4462 | 2.6221 | 1.9534 | 0.4722 | False |
| 20 | 40 | 0.3618 | 0.7856 | 0.4496 | 0.4480 | 2.6333 | 2.0395 | 0.4721 | False |
| 19 | 40 | 0.3624 | 0.7834 | 0.4440 | 0.4473 | 2.6277 | 2.0061 | 0.4721 | False |
| 14 | 40 | 0.3646 | 0.7687 | 0.4215 | 0.4421 | 2.5869 | 1.8124 | 0.4720 | False |
| 9 | 40 | 0.3701 | 0.7498 | 0.3893 | 0.4351 | 2.5351 | 1.6218 | 0.4718 | False |
| 7 | 40 | 0.3707 | 0.7436 | 0.3904 | 0.4345 | 2.5220 | 1.5742 | 0.4717 | False |
| 8 | 40 | 0.3701 | 0.7483 | 0.3901 | 0.4347 | 2.5348 | 1.5873 | 0.4717 | False |

## 输出文件

- `r25_lk_no_norm_bifactor_all.csv`
- `optimal_lk_by_dataset.csv`
- `common_lk_no_norm_bifactor_summary.csv`
- `grid_point_recall.csv` / `grid_point_precision.csv` / `grid_point_f1.csv`
- `figures/<dataset>/recall_heatmap.png`
- `figures/<dataset>/precision_heatmap.png`
- `figures/<dataset>/candidate_expansion_heatmap.png`
- `figures/L_sensitivity_fixed_K50/<dataset>_precision_recall_by_L_fixed_K50.png`
- `figures/K_sensitivity_fixed_L20/<dataset>_precision_recall_by_K_fixed_L20.png`