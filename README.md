# NX-421 Mini-project: Language-task fMRI

Analysis of one subject's HCP language task run (`tfMRI_LANGUAGE_LR`: auditory story vs math blocks):
preprocessing, GLM and multivariate pattern analysis (temporal ICA).

**Group:** Maria Abi Khalil, Perrine Beurier, Arnaud Buehler, Lou Colombies, Erine Genilloud

## Repository structure

```
data/                     # dataset (not included, see below)
  T1w/T1w.nii.gz
  fMRI/tfMRI_LANGUAGE_LR/
    tfMRI_LANGUAGE_LR.nii.gz
    tfMRI_LANGUAGE_LR_task_onsets.csv
output/                   # preprocessing outputs, created by the notebooks, not included
notbooks/
  01_preprocessing.ipynb   # Part 1.1-1.2: structural and functional preprocessing
  02_GLM.ipynb              # Part 1.3: design matrix, GLM, contrasts, atlas and network analysis
  03_ICA.ipynb              # Part 2: temporal ICA
```

## Data

The data come from the Human Connectome Project and are not included in this repository. It is accessible through this link: https://drive.google.com/drive/u/2/folders/1P8_oebUgEBdF1urhXrYmxlmf6I_o4n8D
Place them in `data/` with the structure above.

## Requirements

- Python 3 with `numpy`, `pandas`, `matplotlib`, `nibabel`, `nilearn`, `scikit-learn`, `requests`
- FSL (BET, FAST, FLIRT, MCFLIRT) for preprocessing

## How to run

Run the notebooks in this order, as each one uses the outputs of the previous one:

1. **Preprocessing:** skull-stripping, segmentation, registration to MNI, removal of the first 10
   volumes, motion correction and 6 mm smoothing. Writes to `output/` the smoothed fMRI
   (`tfMRI_LANGUAGE_LR_trim_moco_smoothed-6mm.nii.gz`) and the motion parameters
   (`tfMRI_LANGUAGE_LR_trim_moco.par`).
2. **GLM:** builds the design matrix (story, math, motion, drifts), fits a first-level GLM, and computes
   the story vs math contrast with FDR correction. Regions are labelled with the Harvard-Oxford atlas,
   functional networks with the Yeo 7-network atlas (downloaded automatically, with the Schaefer 2018
   atlas as fallback if the download fails).
3. **ICA:** temporal ICA on the preprocessed data and correlation of the component time courses with
   the GLM regressors.

Atlases are downloaded on first use (to `nilearn_data/` and `data/atlases/`), so an internet connection
is needed the first time.
