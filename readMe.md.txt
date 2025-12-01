# PhaseAwareLSTM – Projection-Based Transfer Learning for EEG (MATLAB)

PhaseAwareLSTM provides a minimal, self-contained MATLAB implementation of projection-based transfer learning for low-channel EEG decoding.  
The code accompanies the corresponding research work and focuses on clarity, reproducibility, and simplicity rather than full-scale dataset distribution.

This repository includes:
- Source-domain training for DeepRNN / EEGNet / Transformer models  
- Target-domain projection models (FFNN, LSTM, GRU, Phase-Aware LSTM)  
- Modular cross-validation framework  
- Clean configuration system (`configureExperiment`)  
- A **small demo fold** of the CWL dataset for verifying end-to-end execution  

---

## Requirements

- MATLAB R2022b or later  
- Deep Learning Toolbox  
- A GPU is recommended (but not required for the demo)

---

## Folder Structure

PhaseAwareLSTM/
│
├── projectionTL_mainScript.m # Main entry point
├── configureExperiment.m # Project-wide configuration system
│
├── models/ # Architectures: DeepRNN, EEGNet, Transformer, Projections
├── utils/ # Data loading, metrics, cross-validation, helpers
├── experimentscripts/ # Optional: example/extended experiments
│
└── datasets/
└── folds/
└── CWL/
└── NEC21_C2/
└── fullchannels/
└── source_fold1.mat # (Demo subset)


## Quick Start (Demo)

The repository includes a small **demo fold** (~100 trials) so users can run the pipeline without the full dataset.

1. Clone the repo  
2. Open MATLAB and set the project root:  
```matlab
   cd <repo_path>
3. Add subfolders to the MATLAB path (if not using a .prj file):
addpath(genpath(pwd));
4. Run the main experiment:
```matlab
   projectionTL_mainScript

Running Full Experiments (If you have the dataset)

If you have the complete dataset structure, place it under:

datasets/folds/<DATASET>/<SUBJECT>/<...>.mat


Then modify the configuration inside run_experiment.m:

configs.datasetType  = 'CWL';
configs.domain       = 'source-domain';   % or 'target-domain'
configs.typeNet      = 'DeepRNN';         % EEGNet | transformer
configs.layerName    = 'phase-lstm';      % for projection-based TL
configs.ncval        = 10;                % # of CV folds %% Only if you have the full-dataset, else use config.ncval = 1;
configs.ncats        = 2;                 % #classes


## Custom Phase-Aware LSTM layer

The main custom recurrent unit used in the paper is implemented in:

`utils/modelstuff/layerdefinitions/phaseAwareLSTM.m`

You can construct it directly as:

```matlab
layer = phaseAwareLSTM(128, "Name","phase_lstm", "OutputMode","sequence");


Please cite:

V. K N, S. Sundaram, and C. N. Gupta, “Phase-aware LSTM projections for learning from heterogeneous electroencephalogram montages,” Phys. Scr., 2025, doi: 10.1088/1402-4896/ae229d.
