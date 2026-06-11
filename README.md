# GNN_for_neutron_reconstruction
This repository contains the thesis notebooks for GNN-based neutron reconstruction in high-granularity detector data. The work studies how to reconstruct neutron candidates when UFC candidate clusters are imperfect, with emphasis on event multiplicity, purity, efficiency, fake-candidate control, and energy regression.
## Main Research Focus
The framework investigates:
> How GNN-based neutron reconstruction preserve full-event neutron multiplicity when candidate clusters are imperfect.

The work compares six GNN architectures under the same UFC candidate segmentation and uses a separate notebook for final combined plots and comparison tables.

## Notebooks

```text
baseline_DynamicEdgeConv.ipynb
baseline_GraphSAGE.ipynb
baseline_ufc_GAT.ipynb

Challenger_ufc_DynamicEdgeConv.ipynb
Challenger_ufc_GAT.ipynb
Challenger_ufc_GraphSAGE.ipynb

Extra_plots.ipynb
````

## Notebook Descriptions

| Notebook                               | Description                                                                                                                      |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `baseline_DynamicEdgeConv.ipynb`       | Baseline UFC Dynamic EdgeConv model without spectral gating.                                                                     |
| `baseline_GraphSAGE.ipynb`             | Baseline UFC GraphSAGE model without spectral gating.                                                                            |
| `baseline_ufc_GAT.ipynb`               | Baseline UFC GATv2 model without spectral gating.                                                                                |
| `Challenger_ufc_DynamicEdgeConv.ipynb` | Spectral-Gated UFC Dynamic EdgeConv model using the custom residual spectral gate.                                               |
| `Challenger_ufc_GAT.ipynb`             | Spectral-Gated UFC GATv2 model using the custom residual spectral gate.                                                          |
| `Challenger_ufc_GraphSAGE.ipynb`       | Spectral-Gated UFC GraphSAGE model using the custom residual spectral gate.                                                      |
| `Extra_plots.ipynb`                    | Combined result plots, final comparison tables, ROC/PR curves, metric bar charts, threshold sweeps, and energy-regression plots. |

## Main Novelty

A key novelty of this thesis is the custom residual spectral gate. Instead of concatenating Laplacian positional encodings directly to the node features, the model uses sign-invariant spectral information to learn a bounded correction:
This allows the model to use the internal structure of each UFC cluster without changing the cluster boundary.

## Models Compared
The six final architectures are:

1. Baseline UFC GraphSAGE
2. Baseline UFC GATv2
3. Baseline UFC Dynamic EdgeConv
4. Spectral-Gated UFC GraphSAGE
5. Spectral-Gated UFC GATv2
6. Spectral-Gated UFC Dynamic EdgeConv

All models use the same UFC candidate clusters, so the comparison focuses on architecture and representation rather than different segmentation procedures.

## Dataset Summary

After preprocessing with the deposited-energy cut (E_dep >0.003) GeV, the processed dataset contains:

* 1,748,494 detector hits
* 197,027 events
* 8.87 hits per event on average
* 521,570 neutron-associated hits
* 286,531 corrected neutron hits
* 6,944 unique corrected neutron object IDs

Each hit contains detector geometry, timing, deposited energy, detector side, truth particle identity, kinetic energy, and time-of-flight consistency information.

## Evaluation Metrics

The research evaluates both candidate-level and event-level performance.

Candidate-level metrics:

* ROC-AUC
* PR-AUC
* Graph F1

Event-level metrics:

* Event purity
* Event efficiency
* Fake fraction
* Duplicate fraction
* Selection score

Energy-regression metrics:

* Energy MAE
* Energy RMSE
* Energy bias
* Relative MAE

The selection score is defined as:

S = 0.45*efficiency + 0.45*purity - 0.1*fake fraction 

## Training Setup

The models are trained with:

* AdamW optimizer
* initial learning rate (10^{-3})
* up to 100 epochs
* 12 epochs early stopping 
* multi-seed evaluation using seeds 42, 7, and 21
* validation-selected best checkpoints
* strict train/validation/test event split

## Main Result

The strongest final model is:

> Spectral-Gated UFC Dynamic EdgeConv

It gives the best overall balance between candidate-level discrimination and full-event neutron reconstruction. The result suggests that adaptive neighborhood learning is especially useful for sparse and irregular neutron-induced detector-hit patterns, while the custom residual spectral gate provides additional internal-structure refinement.

## Reproducing the Workflow

Recommended order:

1. Run `baseline_GraphSAGE.ipynb`
2. Run `baseline_ufc_GAT.ipynb`
3. Run `baseline_DynamicEdgeConv.ipynb`
4. Run `Challenger_ufc_GraphSAGE.ipynb`
5. Run `Challenger_ufc_GAT.ipynb`
6. Run `Challenger_ufc_DynamicEdgeConv.ipynb`
7. Run `Extra_plots.ipynb` to generate combined result tables and thesis figures

## Output Files

Each model notebook exports standardized files for final comparison, including:

```
*_ALL_SEEDS_test_cluster_predictions.csv
*_metrics_by_seed_threshold.csv
*_metrics_MEAN_by_threshold.csv
*_metrics_STD_by_threshold.csv
```

The `Extra_plots.ipynb` notebook reads these files and produces combined plots for the thesis.

## Requirements

Typical dependencies:

```
python >= 3.10
torch
torch-geometric
numpy
pandas
scikit-learn
matplotlib
scipy
tqdm
```

Install with:

```bash
pip install -r requirements.txt
```

For Google Colab, PyTorch Geometric may require CUDA-specific installation.

## Notes

Large datasets, graph shards, checkpoints, and generated figures are not included by default. They should be stored separately, for example in Google Drive or external storage.

## Citation

If this repository is used or extended, please cite:


Billa Lewis, GNN-Based Neutron Reconstruction in High-Granularity Detector Data, 2026.
```

```
```
