# MECH Approximate Annulus KDE Experiments

## Experimental Setup

- Dataset: isolet
- Training/index/query split: 2800/1800/40
- Base annulus: distance percentiles 15.0--25.0
- Base kernel bandwidth: 6.722598
- MECH: 12 encoders, 12 bits, 24 epochs
- Base hash query: 12 tables, 8 bits, Hamming probe 2
- Query mode: radius_first; minimum hash collisions: 2
- Fixed query radius: 7.0; annulus count: 1
- Full MECH per-ring KDE sampling: at most 16 candidates are sampled from each annulus; each ring contribution is estimated as ring size times the sampled mean kernel value.
- Other variants and non-MECH methods compute KDE over all retrieved candidates without ring sampling.
- Time metric: Online ms is the average online query time per query. It is decomposed into Filter ms for candidate retrieval/filtering and KDE ms for KDE estimation.
- Training time, hash index construction time, sphere table construction time, and distance-table construction time are excluded from all Online ms values.
- In the structure ablation table, only online query time is reported. Training time and build/index construction time are not recorded for structure ablation.
- MECH query hashes are cached in this timing protocol; SimHash/Angular LSH/Hyperplane LSH query hashes are computed online in the method comparison.

## Metric Definitions

- Exact fixed-radius neighbor set: points within the specified query radius R=7.0.
- Retrieved set: candidates returned by the approximate annulus query and used for KDE.
- P: retrieved points that are exact fixed-radius neighbors divided by retrieved points.
- R: retrieved points that are exact fixed-radius neighbors divided by exact fixed-radius neighbors.
- FP/True: false positives divided by exact fixed-radius neighbors.
- FN/True: false negatives divided by exact fixed-radius neighbors.
- CER: retrieved candidate count divided by exact fixed-radius neighbor count. CER close to 1 only means the candidate count is close to the true neighbor count; it does not mean retrieval is perfectly accurate.
- KDE Eval.: average number of points whose kernel values are actually computed. For Full MECH this is the per-ring sample count; for other variants it is the full candidate count.
- KDE Err.: absolute relative error between the reported KDE estimate and the exact KDE over the fixed-radius neighbor set.
- Filter ms: candidate retrieval and filtering time, in milliseconds per query.
- KDE ms: KDE estimation time after retrieval, in milliseconds per query.
- Online ms: online query time only, in milliseconds per query.

## Hash Method Comparison

| Method | P | R | F1 | FP/True | FN/True | $R^k$ | $E_+^k$ | KDE Err. | CER | Cand. | KDE Eval. | Raw Hash | R-Pool | Rings | Mean Ring | Max Ring | Filter ms | KDE ms | Online ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SimHash | 0.3360 | 1.0000 | 0.4850 | 3.4461 | 0.0000 | 1.0000 | 2.4973 | 2.4973 | 4.4461 | 1800.00 | 1800.00 | 1800.0 | 1800.0 | 1 | 1800.0 | 1800.0 | 1.490 | 1.170 | 2.777 |
| Angular LSH | 0.3360 | 1.0000 | 0.4850 | 3.4461 | 0.0000 | 1.0000 | 2.4973 | 2.4973 | 4.4461 | 1800.00 | 1800.00 | 1800.0 | 1800.0 | 1 | 1800.0 | 1800.0 | 1.177 | 0.989 | 2.205 |
| Hyperplane LSH | 0.3360 | 1.0000 | 0.4850 | 3.4461 | 0.0000 | 1.0000 | 2.4973 | 2.4973 | 4.4461 | 1800.00 | 1800.00 | 1800.0 | 1800.0 | 1 | 1800.0 | 1800.0 | 1.071 | 0.853 | 1.930 |
| MECH | 0.5810 | 0.8000 | 0.6406 | 1.2508 | 0.2000 | 0.8085 | 0.9654 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.807 | 0.106 | 2.917 |

\begin{table}[htbp]
\centering
\caption{Hash method comparison with kernel-weighted annulus KDE metrics on ISOLET.}
\label{tab:mech_hash_comparison}
\resizebox{\linewidth}{!}{%
\begin{tabular}{lccccccccccccccccccc}
\toprule
Method & P & R & F1 & FP/True & FN/True & $R^k$ & $E_+^k$ & KDE Err. & CER & Cand. & KDE Eval. & Raw Hash & R-Pool & Rings & Mean Ring & Max Ring & Filter ms & KDE ms & Online ms \\
\midrule
SimHash & 33.60\% & 100.00\% & 48.50\% & 344.61\% & 0.00\% & 100.00\% & 249.73\% & 249.73\% & 4.4461 & 1800.00 & 1800.00 & 1800.0 & 1800.0 & 1 & 1800.0 & 1800.0 & 1.490 & 1.170 & 2.777 \\
Angular LSH & 33.60\% & 100.00\% & 48.50\% & 344.61\% & 0.00\% & 100.00\% & 249.73\% & 249.73\% & 4.4461 & 1800.00 & 1800.00 & 1800.0 & 1800.0 & 1 & 1800.0 & 1800.0 & 1.177 & 0.989 & 2.205 \\
Hyperplane LSH & 33.60\% & 100.00\% & 48.50\% & 344.61\% & 0.00\% & 100.00\% & 249.73\% & 249.73\% & 4.4461 & 1800.00 & 1800.00 & 1800.0 & 1800.0 & 1 & 1800.0 & 1800.0 & 1.071 & 0.853 & 1.930 \\
MECH & 58.10\% & 80.00\% & 64.06\% & 125.08\% & 20.00\% & 80.85\% & 96.54\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.807 & 0.106 & 2.917 \\
\bottomrule
\end{tabular}%
}
\end{table}

## Structure Ablation

The structure ablation evaluates the same trained MECH model and the same fixed-radius task. This table only compares online query behavior; it intentionally excludes training and build/index construction costs.

| Variant | P | R | FP/True | FN/True | $R^k$ | KDE Err. | CER | Cand. | KDE Eval. | Filter ms | KDE ms | Online ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hash Only | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8511 | 2.0509 | 803.38 | 803.38 | 1.844 | 0.612 | 2.720 |
| Hash + Sphere | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8511 | 2.0509 | 803.38 | 803.38 | 4.010 | 0.628 | 4.697 |
| Hash + Distance Table | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8511 | 2.0509 | 803.38 | 803.38 | 3.226 | 0.579 | 3.858 |
| Full MECH | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 2.966 | 0.118 | 3.107 |

\begin{table}[htbp]
\centering
\caption{Structure ablation of MECH approximate annulus query on ISOLET.}
\label{tab:mech_structure_ablation}
\resizebox{\linewidth}{!}{%
\begin{tabular}{lcccccccccccc}
\toprule
Variant & P & R & FP/True & FN/True & $R^k$ & KDE Err. & CER & Cand. & KDE Eval. & Filter ms & KDE ms & Online ms \\
\midrule
Hash Only & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 85.11\% & 2.0509 & 803.38 & 803.38 & 1.844 & 0.612 & 2.720 \\
Hash + Sphere & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 85.11\% & 2.0509 & 803.38 & 803.38 & 4.010 & 0.628 & 4.697 \\
Hash + Distance Table & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 85.11\% & 2.0509 & 803.38 & 803.38 & 3.226 & 0.579 & 3.858 \\
Full MECH & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 2.966 & 0.118 & 3.107 \\
\bottomrule
\end{tabular}%
}
\end{table}

Oracle upper bound, not included in the deployable structure comparison:

| Variant | P | R | FP/True | FN/True | $R^k$ | KDE Err. | CER | Cand. | KDE Eval. | Filter ms | KDE ms | Online ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hash + Exact Distance (Oracle) | 1.0000 | 0.8000 | 0.0000 | 0.2000 | 0.8085 | 0.1915 | 0.8000 | 465.27 | 465.27 | 2.583 | 0.251 | 2.843 |

## MECH Sensitivity

| Setting | P | R | FP/True | FN/True | $R^k$ | KDE Err. | CER | Cand. | KDE Eval. | Raw Hash | R-Pool | Rings | Mean Ring | Max Ring | Filter ms | KDE ms | Online ms |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| L=2.0 | 0.7629 | 0.1745 | 0.0676 | 0.8255 | 0.1827 | 0.7647 | 0.2421 | 116.75 | 16.00 | 116.8 | 1800.0 | 1 | 116.8 | 116.8 | 0.558 | 0.073 | 0.632 |
| L=4.0 | 0.7017 | 0.4355 | 0.3290 | 0.5645 | 0.4484 | 0.4978 | 0.7645 | 333.00 | 16.00 | 333.0 | 1800.0 | 1 | 333.0 | 333.0 | 1.047 | 0.084 | 1.131 |
| L=8.0 | 0.6221 | 0.6764 | 0.8349 | 0.3236 | 0.6877 | 0.6086 | 1.5114 | 615.62 | 16.00 | 615.6 | 1800.0 | 1 | 615.6 | 615.6 | 1.891 | 0.092 | 1.985 |
| L=12.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.736 | 0.101 | 2.840 |
| K=4.0 | 0.3468 | 0.9990 | 3.3160 | 0.0010 | 0.9990 | 2.5274 | 4.3150 | 1738.58 | 16.00 | 1738.6 | 1800.0 | 1 | 1738.6 | 1738.6 | 1.155 | 0.102 | 1.260 |
| K=8.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.689 | 0.097 | 2.786 |
| K=12.0 | 0.8250 | 0.2994 | 0.0931 | 0.7006 | 0.3140 | 0.6266 | 0.3925 | 182.28 | 16.00 | 182.3 | 1800.0 | 1 | 182.3 | 182.3 | 6.770 | 0.082 | 6.852 |
| delta=0.25 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.665 | 0.093 | 2.759 |
| delta=0.5 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.650 | 0.091 | 2.742 |
| delta=1.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.793 | 0.105 | 2.935 |
| delta=2.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.734 | 0.102 | 2.836 |
| rho=0.75 | 0.0785 | 0.9714 | 40.9259 | 0.0286 | 0.9717 | 30.1554 | 41.8973 | 799.05 | 16.00 | 799.1 | 1800.0 | 1 | 799.1 | 799.1 | 2.679 | 0.096 | 2.775 |
| rho=1.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.663 | 0.096 | 2.759 |
| rho=1.25 | 0.9581 | 0.5467 | 0.0284 | 0.4533 | 0.5824 | 0.3985 | 0.5751 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.650 | 0.093 | 2.744 |
| rho=1.5 | 0.9997 | 0.4519 | 0.0002 | 0.5481 | 0.5080 | 0.4935 | 0.4520 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.651 | 0.093 | 2.746 |
| sigma=0.5 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8349 | 0.4058 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.795 | 0.107 | 2.904 |
| sigma=1.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8085 | 0.8376 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.754 | 0.103 | 2.858 |
| sigma=2.0 | 0.5810 | 0.8000 | 1.2508 | 0.2000 | 0.8021 | 1.0368 | 2.0509 | 803.38 | 16.00 | 803.4 | 1800.0 | 1 | 803.4 | 803.4 | 2.861 | 0.114 | 2.978 |

\begin{table}[htbp]
\centering
\caption{MECH parameter sensitivity with kernel-weighted KDE metrics on ISOLET.}
\label{tab:mech_sensitivity}
\resizebox{\linewidth}{!}{%
\begin{tabular}{lccccccccccccccccc}
\toprule
Setting & P & R & FP/True & FN/True & $R^k$ & KDE Err. & CER & Cand. & KDE Eval. & Raw Hash & R-Pool & Rings & Mean Ring & Max Ring & Filter ms & KDE ms & Online ms \\
\midrule
L=2.0 & 76.29\% & 17.45\% & 6.76\% & 82.55\% & 18.27\% & 76.47\% & 0.2421 & 116.75 & 16.00 & 116.8 & 1800.0 & 1 & 116.8 & 116.8 & 0.558 & 0.073 & 0.632 \\
L=4.0 & 70.17\% & 43.55\% & 32.90\% & 56.45\% & 44.84\% & 49.78\% & 0.7645 & 333.00 & 16.00 & 333.0 & 1800.0 & 1 & 333.0 & 333.0 & 1.047 & 0.084 & 1.131 \\
L=8.0 & 62.21\% & 67.64\% & 83.49\% & 32.36\% & 68.77\% & 60.86\% & 1.5114 & 615.62 & 16.00 & 615.6 & 1800.0 & 1 & 615.6 & 615.6 & 1.891 & 0.092 & 1.985 \\
L=12.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.736 & 0.101 & 2.840 \\
K=4.0 & 34.68\% & 99.90\% & 331.60\% & 0.10\% & 99.90\% & 252.74\% & 4.3150 & 1738.58 & 16.00 & 1738.6 & 1800.0 & 1 & 1738.6 & 1738.6 & 1.155 & 0.102 & 1.260 \\
K=8.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.689 & 0.097 & 2.786 \\
K=12.0 & 82.50\% & 29.94\% & 9.31\% & 70.06\% & 31.40\% & 62.66\% & 0.3925 & 182.28 & 16.00 & 182.3 & 1800.0 & 1 & 182.3 & 182.3 & 6.770 & 0.082 & 6.852 \\
delta=0.25 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.665 & 0.093 & 2.759 \\
delta=0.5 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.650 & 0.091 & 2.742 \\
delta=1.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.793 & 0.105 & 2.935 \\
delta=2.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.734 & 0.102 & 2.836 \\
rho=0.75 & 7.85\% & 97.14\% & 4092.59\% & 2.86\% & 97.17\% & 3015.54\% & 41.8973 & 799.05 & 16.00 & 799.1 & 1800.0 & 1 & 799.1 & 799.1 & 2.679 & 0.096 & 2.775 \\
rho=1.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.663 & 0.096 & 2.759 \\
rho=1.25 & 95.81\% & 54.67\% & 2.84\% & 45.33\% & 58.24\% & 39.85\% & 0.5751 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.650 & 0.093 & 2.744 \\
rho=1.5 & 99.97\% & 45.19\% & 0.02\% & 54.81\% & 50.80\% & 49.35\% & 0.4520 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.651 & 0.093 & 2.746 \\
sigma=0.5 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 83.49\% & 40.58\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.795 & 0.107 & 2.904 \\
sigma=1.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.85\% & 83.76\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.754 & 0.103 & 2.858 \\
sigma=2.0 & 58.10\% & 80.00\% & 125.08\% & 20.00\% & 80.21\% & 103.68\% & 2.0509 & 803.38 & 16.00 & 803.4 & 1800.0 & 1 & 803.4 & 803.4 & 2.861 & 0.114 & 2.978 \\
\bottomrule
\end{tabular}%
}
\end{table}

## Main Observations

- The lowest annulus KDE relative error in the method comparison is obtained by MECH (83.76\%).
- Full MECH obtains point precision 58.10\%, point recall 80.00\%, FP/True 125.08\%, FN/True 20.00\%, CER 2.0509, KDE evaluations 16.00, kernel-weighted recall 80.85\%, and KDE relative error 83.76\%.
- In the structure ablation, all variants use cached MECH hash candidates; Filter ms includes materializing the cached candidate list and applying variant-specific filtering, while KDE ms measures KDE estimation. Only Full MECH uses per-ring KDE sampling. Training and build/index construction time are excluded.
- In the structure ablation, Full MECH is the best deployable structure; the exact-distance variant is an oracle upper bound because it uses exact Euclidean distance information.
- Kernel-weighted metrics expose whether retrieved points preserve KDE contribution, not just point counts.
- Candidate expansion ratio reports candidate count relative to the exact fixed-radius neighbor count; FP/True and FN/True expose retrieval errors even when CER is close to 1.
- Raw Hash is the hash-only candidate count before radius filtering; R-Pool is the candidate pool after the fixed-radius prefilter, so R-Pool rather than Raw Hash indicates whether fixed-radius retrieval avoided scanning the whole index.