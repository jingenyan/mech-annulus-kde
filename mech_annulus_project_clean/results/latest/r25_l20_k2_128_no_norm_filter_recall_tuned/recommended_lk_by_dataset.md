# Recommended L/K By Dataset

Selection rule: first require `point_recall >= 0.90`; among satisfying settings, choose the highest `point_f1`, then higher `point_precision`, lower KDE error, and lower query time. In this experiment, `L` is fixed at 20, so only `K` is actually selected.

| dataset | status | L | K | precision | recall | F1 | KDE error | candidate size | candidate/true | query time ms |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| amazon | OK | 20 | 50 | 0.2510 | 1.0000 | 0.4012 | 0.0515 | 785.0000 | 3.9848 | 15.5115 |
| cifar10 | OK | 20 | 4 | 0.2500 | 1.0000 | 0.4000 | 0.0002 | 1800.0000 | 4.0000 | 2.3805 |
| cifar10_gist512 | OK | 20 | 50 | 0.3222 | 0.9139 | 0.4759 | 0.0070 | 1281.4250 | 2.8476 | 24.7451 |
| isolet | Not strictly guaranteed | 20 | 50 | 0.3231 | 0.8864 | 0.4707 | 0.0036 | 1263.6000 | 2.8078 | 23.6865 |

Notes:

- `amazon`: `K=50` gives full recall and low KDE error, but precision is low because the no-norm-filter variant retrieves a large candidate set.
- `cifar10`: all tested K values satisfy recall, but this no-norm variant effectively retrieves the full index; if L is allowed to vary, the separate L/K grid gives a better practical setting of `L=2, K=40`.
- `cifar10_gist512`: `K=50` is the best recall-guaranteed choice. `K=54` has higher F1, but recall drops to 0.7319, so it does not satisfy the recall guarantee.
- `isolet`: no tested `K` reaches `recall >= 0.90`; `K=50` is the closest recall-preserving choice. `K=72` gives higher F1, but recall is only 0.6448.
