# UNIONS and *Euclid*: Calibrating point source – galaxy separation for ground-based surveys

A neural-network (ANN) pipeline that separates point sources (stars) from extended sources (galaxies) in a ground-based, multi-band imaging survey (e.g. UNIONS/CFIS), using labels derived from the higher-resolution *Euclid* VIS imaging.

This code reproduces the method described in:

> Thomas, Simon, Gwyn et al. 2026, *"UNIONS and Euclid: Calibrating point source – galaxy separation for ground-based surveys"*

**Note on the Euclid data used here.** The original paper measures Euclid photometry directly with SExtractor on the VIS-pipeline stacked tiles, ahead of Euclid Data Release 1 (2026). This repository instead uses the public **DR1 MER Final Catalogue**, so results may differ slightly from the paper (different tiling scheme and aperture-photometry method — see the notebook for details).



## What this repository contains

`pts_gal_separation_public.ipynb` — an end-to-end notebook that:

1. Loads a ground-based photometric catalogue and the *Euclid* Q1 MER catalogue, renaming all columns to a common, self-explanatory scheme right after each file is read.
2. Applies photometric quality cuts.
3. Labels each source as a star or a galaxy from the *Euclid* size–magnitude diagram.
4. Splits the data into an independent train/test set.
5. Trains a fully-connected ANN (Keras/TensorFlow) on ground-based features only.
6. Saves the trained model, the feature scaler, and the labelled train/test tables.
7. Evaluates the model (ROC curve) and explains its predictions with SHAP.



## Data

- **Ground-based catalogue — not included (private).** You need your own photometric catalogue with, at minimum: sky coordinates, a total magnitude, a set of fixed-aperture magnitudes, a seeing/image-quality estimate, and a source shape (semi-major/minor axis). Point the `GROUND_COLUMN_MAP` dictionary (top of the notebook) at your own column names — the rest of the notebook only uses the generic names on the right-hand side of that mapping.

No proprietary data is distributed with this repository.



## Requirements

```
astropy
numpy
pandas
matplotlib
scikit-learn
tensorflow
shap
```

Install with:

```bash
pip install astropy numpy pandas matplotlib scikit-learn tensorflow shap
```

## Usage

1. Open `pts_gal_separation_public.ipynb`.
2. Edit `GROUND_COLUMN_MAP` to match your ground-survey column names.
3. Set the file paths for the ground-based catalogue and the Euclid Q1
   catalogue (cross-matched).
4. Run all cells.

## Outputs

| File | Description |
|---|---|
| `ground_euclid_matched_clean.fits` | Cross-matched catalogue after quality cuts |
| `star_galaxy_ann.keras` | Trained ANN |
| `scaler_star_galaxy_ann.pkl` | Fitted feature scaler (`StandardScaler`) |
| `train_set_with_predictions.fits` | Training set with predicted labels/probabilities |
| `test_set_with_predictions.fits` | Test set with predicted labels/probabilities |
| `training_loss_history.txt` | Training/validation loss per epoch |
| `StarGal_euclid.pdf` | Euclid size–magnitude diagram used to define labels |
| `feature_importance.pdf` | Global SHAP feature-importance plot |

## Citation

If you use this code, please cite Thomas, Simon, Gwyn et al. 2026 (see above).

## License

Add a license of your choice (e.g. MIT) before publishing.
