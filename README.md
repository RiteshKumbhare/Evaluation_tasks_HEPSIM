# HEPSIM Submission - Evaluator Guide
## This repo is my submission for "HEPSIM : Jet Observable Library for Monte Carlo Validation"

This guide explains how to run and evaluate the notebook:
- `hepsim_submission.ipynb`

## 1) What This Notebook Delivers

The notebook covers:
- Part (a): data loading + exploratory analysis
- Part (b): jet-observable engineering (mass, width, pT^D, multiplicity, etc.)
- Part (c): Lorentz boost to jet rest frame and numerical verification
- Part (d): quark vs gluon classification (LR, RF, BDT, MLP)
- 8 ablation studies for feature/frame/model/data-efficiency analysis

## 2) Environment Requirements

Required Python version:
- Python 3.13 (the setup cell enforces this)

Required packages (auto-installed by setup cell):
- numpy>=2.1
- matplotlib>=3.9
- scikit-learn>=1.5
- torch>=2.6

## 3) Dataset Placement

Place the QG dataset files in a folder named `jets_data` next to the notebook:

- `./jets_data/QG_jets.npz` (optional base file)
- `./jets_data/QG_jets_*.npz`

The notebook loads up to 5 standard files (excluding `withbc` variants).

## 4) How To Run

1. Open `hepsim_submission.ipynb` in Jupyter/VS Code.
2. Select a Python 3.13 kernel.
3. Run all cells top-to-bottom once.
4. Wait for training/ablation sections to complete (Part d is the longest).

Recommended execution mode:
- Run All (single pass, in order)

## 5) Expected Key Results (Reference Run)

If data and environment match, you should see results close to:

- Dataset size: 500,000 jets (250,000 quark / 250,000 gluon)
- Mean multiplicity:
  - quark: 33.41
  - gluon: 53.14

Main classifier test AUC:
- Logistic Regression: 0.8570
- Random Forest: 0.8679
- Gradient BDT: 0.8754
- MLP (all features): 0.8791

Frame ablation (BDT):
- Lab-only: 0.8649
- CM-only: 0.8514
- Lab+CM: 0.8741
- Gain (Lab+CM vs Lab): +0.0092 AUC

Most discriminating single feature:
- multiplicity (single-feature AUC: 0.8403)

Cross-validation stability (BDT):
- mean AUC approx 0.8717 +/- 0.0026

## 6) What Evaluators Should Check Quickly

- Setup cell finishes without errors.
- Data loading reports balanced classes and expected dimensions.
- ROC plot in Part (d.iii) shows model ordering (MLP/BDT near top).
- Ablation 1 confirms combined lab+CM > lab-only > CM-only.
- Final summary cell prints consistent metrics.

## 7) Approximate Runtime Notes

Runtime depends on CPU/GPU and environment.
- CPU-only: Part (d) and ablations can take significantly longer.
- GPU available: MLP training is faster.

If time is limited, evaluate correctness by running through Part (d.iii), then optionally run full ablations.

## 8) Troubleshooting

If Python version error occurs:
- Switch kernel to Python 3.13 and rerun.

If data folder error occurs:
- Ensure `jets_data` is in the same directory as the notebook.

If package install fails in notebook:
- Rerun setup cell.
- Confirm internet/package index access.

If memory/time is constrained:
- Reduce loaded files in `load_qg_files(..., max_files=5)`.
- Run fewer ablation experiments for a quick functional check.

## 9) Reproducibility

The notebook sets a fixed seed (`SEED = 42`) for NumPy/PyTorch and model splits. Minor differences can still occur across hardware and library builds.
