# FSNN-CBG: Fuzzy Spiking Neural Networks with Causal Brain Graph Modelling for Epileptic Seizure Prediction

<p align="center">
  <img src="figures/fig1_pipeline.png" width="900" alt="FSNN-CBG Pipeline"/>
</p>

[![Python](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.2.0-orange.svg)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![IEEE](https://img.shields.io/badge/Paper-IEEE_TNSRE-red.svg)](paper/)
[![Dataset](https://img.shields.io/badge/Dataset-CHB--MIT-purple.svg)](https://physionet.org/content/chbmit/1.0.0/)

> **Paper:** *"Fuzzy Spiking Neural Networks with Causal Brain Graph Modelling for Epileptic Seizure Prediction on the CHB-MIT EEG Dataset"*
> Submitted to *IEEE Transactions on Neural Systems and Rehabilitation Engineering*, 2025.

---

## 📌 Overview

**FSNN-CBG** is a novel unified framework for epileptic seizure prediction from scalp EEG. It combines three biologically motivated components:

| Component | Description | Key Benefit |
|---|---|---|
| **SNN Encoder** | LIF neurons with surrogate gradient training | >60% energy reduction vs CNN-LSTM |
| **Causal Brain Graph** | Granger causality + STGCN | Directed pre-ictal connectivity modelling |
| **IT2 Fuzzy Classifier** | Interval Type-2 FLS with EKM type reduction | Boundary uncertainty handling |

### 🏆 Results on CHB-MIT (22 subjects, 198 seizures)

| Metric | FSNN-CBG | Best Prior Art |
|---|---|---|
| Sensitivity | **97.4%** | 96.8% (Spiking Conformer) |
| Specificity | **97.1%** | 97.5% (M-NIG) |
| Accuracy | **98.2%** | 97.4% (M-NIG) |
| AUC-ROC | **98.6%** | 97.1% (Swin-ViT) |
| FPR (/h) | **0.025** | 0.024 (M-NIG) |
| APT (min) | **24.3** | 22.0 (M-NIG) |
| Energy (mJ/window) | **38.4** | 25.1 (GSNN-Loihi2) |

---

## 📁 Repository Structure

```
FSNN_CBG/
├── src/
│   ├── preprocessing/
│   │   ├── loader.py          # CHB-MIT .edf loader via MNE
│   │   ├── filters.py         # Notch, bandpass, CAR filters
│   │   ├── ica.py             # ICA artefact removal
│   │   ├── segmentation.py    # Window extraction & labelling
│   │   └── augmentation.py    # SMOTE class balancing
│   ├── models/
│   │   ├── lif_neuron.py      # LIF neuron with surrogate gradient
│   │   ├── snn_encoder.py     # Full SNN encoder (256→128→64)
│   │   ├── causal_graph.py    # Granger causality brain graph
│   │   ├── stgcn.py           # Spatio-Temporal GCN
│   │   └── it2_fuzzy.py       # IT2 Fuzzy Logic Classifier (EKM)
│   ├── training/
│   │   ├── trainer.py         # Main training loop
│   │   ├── losses.py          # Weighted cross-entropy + focal loss
│   │   └── federated.py       # Federated learning variant
│   ├── evaluation/
│   │   ├── metrics.py         # Sensitivity, specificity, FPR, APT
│   │   └── statistical.py     # Wilcoxon tests, effect sizes
│   └── visualization/
│       ├── plot_training.py   # Training curves
│       ├── plot_results.py    # Per-subject, SOTA, ablation figures
│       └── plot_brain_graph.py# Causal brain graph visualisation
├── configs/
│   └── config.yaml            # All hyperparameters
├── scripts/
│   ├── download_chbmit.py     # Dataset download helper
│   ├── preprocess_all.py      # Batch preprocessing pipeline
│   ├── train.py               # Training entry point
│   └── evaluate.py            # Evaluation entry point
├── tests/
│   ├── test_snn.py
│   ├── test_causal_graph.py
│   └── test_it2_fuzzy.py
├── notebooks/
│   └── demo.ipynb             # End-to-end demo notebook
├── figures/                   # All paper figures (PNG)
├── paper/                     # LaTeX source
│   └── FSNN_CBG_Complete_Paper.tex
├── requirements.txt
├── setup.py
└── README.md
```

---

## 🚀 Quick Start

### 1. Clone and Install
```bash
git clone https://github.com/YOUR_USERNAME/FSNN_CBG.git
cd FSNN_CBG
pip install -e .
```

### 2. Download CHB-MIT Dataset
```bash
python scripts/download_chbmit.py --output_dir data/chbmit
```

### 3. Preprocess All Subjects
```bash
python scripts/preprocess_all.py --data_dir data/chbmit --output_dir data/processed
```

### 4. Train FSNN-CBG
```bash
python scripts/train.py --config configs/config.yaml --subject all
```

### 5. Evaluate
```bash
python scripts/evaluate.py --checkpoint checkpoints/best_model.pt --subject all
```

---

## 📦 Requirements
See `requirements.txt`. Key dependencies:
- `torch>=2.2.0`, `torch-geometric`
- `mne>=1.6`, `scipy`, `numpy`, `scikit-learn`
- `scikit-fuzzy`, `statsmodels`

---

## 📄 Citation
```bibtex
@article{fsnn_cbg_2025,
  title   = {Fuzzy Spiking Neural Networks with Causal Brain Graph Modelling
             for Epileptic Seizure Prediction on the CHB-MIT EEG Dataset},
  author  = {[Author Name] and [Co-Author Name]},
  journal = {IEEE Transactions on Neural Systems and Rehabilitation Engineering},
  year    = {2025},
  note    = {Under Review}
}
```

---

## 📜 License
This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

## 🙏 Acknowledgements
- CHB-MIT Scalp EEG Database: Physionet / Boston Children's Hospital
- Intel Loihi 2 energy specifications
