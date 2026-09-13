# L20 K2-128 No-Norm-Filter Recall Experiment Design

## Goal
Run a recall-focused experiment with hash table count fixed at `L=20`, hash code length swept over even values from `K=2` to `K=128`, and the previous norm/radial-layer filtering removed from candidate selection.

## Scope
Modify the existing large-K recall tuning experiment rather than creating a separate implementation framework. The experiment should write to a new output directory so previous results remain intact.

## Configuration
- `L_VALUES = [20]`
- `K_VALUES = list(range(2, 129, 2))`
- Keep the existing datasets: `isolet`, `cifar10`, `cifar10_gist512`, `amazon`
- Keep `query-adaptive R25`, `h(q)=0.14R25(q)`, and the existing `min_hamming_fraction` sweep
- Set a new output directory, e.g. `r25_l20_k2_128_no_norm_filter_recall_tuned`

## Candidate Selection
Keep the layer loop for computing the per-layer angular Hamming threshold, but remove the candidate intersection with `layer_masks[layer]`. For each relevant layer and hash table, all ids within the Hamming threshold are added directly to the selected candidate set.

This preserves the existing threshold schedule while removing the original norm/radial-layer filter from recall evaluation.

## Outputs
Continue writing the existing CSV and Markdown summaries:
- all experiment rows
- best recall by dataset and K
- recall-first best rows
- common summary by K
- best common recall by K

The summary should mention that norm/radial-layer filtering is disabled and that `L=20`, `K=2..128 step 2` are used.

## Testing
Run a lightweight syntax check after editing. A full experiment run may be long, so only run it if explicitly requested.