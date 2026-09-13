# Experiment entry points

## `run_common_optimal_multidataset_sensitivity_L18_local.py`

在 ISOLET、CIFAR-10、GIST-512 和 Amazon 数据集上执行公共参数及单因素敏感性分析。脚本依赖 `src/mech_annulus_experiments.py`，运行前应从项目目录设置：

```bash
export PYTHONPATH="$PWD/src:$PYTHONPATH"
```

## `UNSW_NB15.py`

用于论文中的 UNSW-NB15 KDE 异常检测验证。脚本目前保留原始数据路径，需要先修改为本机数据文件位置。

## `run_r25_l1_25_k10_100_midk_high_recall_angle_grid_sensitivity.py`

用于 query-adaptive R25、哈希表数量和哈希码长度的网格敏感性实验。该脚本沿用原始归档中的辅助脚本命名，若要重新运行，需要将相应辅助脚本一并从原始归档复制到 `experiments/`。
