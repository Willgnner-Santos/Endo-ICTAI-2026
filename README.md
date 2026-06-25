# Multi-Label Classification in Brazilian Gastroscopy: Clinical Co-occurrence, Shortcuts, and Explainability

Official repository for the paper submitted to **ICTAI 2026**.

> **Authors:** [ANONYMIZED for review]

---

## Overview

This repository provides the full reproducibility package for our study on clinically structured multi-label classification of gastroscopic findings. It includes:

- All 10 Jupyter notebooks covering data auditing through ML-Decoder comparison
- Exact cross-validation splits (hash-frozen, no data leakage)
- All generated figures and Grad-CAM panels
- Annotation CSV files (data license: CC BY 4.0)

The paper evaluates five dimensions of clinical reliability: predictive performance, co-occurrence structure (CCR), artifact-induced shortcuts, cross-center generalization (LOCO), and multi-rater explainability validation.

---

## Repository Structure

```
Endo-ICTAI-2026/
├── Notebooks/
│   ├── 00_splits_audit.ipynb           # Data integrity and split validation
│   ├── 01_image_eda.ipynb              # Exploratory data analysis
│   ├── 02_baseline_backbones.ipynb     # Architecture comparison (ResNet-50, EfficientNet-B3, ConvNeXt-Tiny, Swin-Tiny)
│   ├── 03_optimized_m2.ipynb           # CCR (lambda=0.6) + imbalance strategy ablations
│   ├── 04_weighted_loss_and_focal.ipynb # Focal Loss / ASL / sampling ablations
│   ├── 05_gradcam_explainability.ipynb # Grad-CAM generation and medical validation analysis
│   ├── 06_ccr_control.ipynb            # CCR structural controls (real C vs shuffled vs uniform vs base)
│   ├── 07_shortcut_audit.ipynb         # Ten-pair artifact-pathology shortcut audit
│   ├── 08_leave_one_center_out.ipynb   # LOCO cross-center generalization experiment
│   └── 09_mldecoder_baseline.ipynb     # ML-Decoder vs linear head comparison
├── splits/
│   ├── fold_{0-4}_{train,val,test}.csv   # 5-fold CV partitions (stratified, hash-frozen)
│   └── image_group_mapping.csv           # Image-to-group mapping for deduplication
├── figs/
│   ├── eda/                            # EDA figures
│   ├── gradcam/                        # Grad-CAM overlay panels
│   ├── training/                       # Training curves and performance charts
│   └── others/                         # Calibration, error matrix, co-prediction figures
├── LICENSE                             # MIT License (code)
├── LICENSE-DATA                        # CC BY 4.0 License (data/annotations)
└── README.md
```

---

## Dataset

The dataset comprises **1,990 gastroscopy images** from two Brazilian institutions (Center A: 720 images; Center B: 1,270 images), annotated across **11 binary labels** (7 clinical findings + 2 artifacts + 2 general indicators). Imbalance ratios range from 1.0 to 496:1.

### Accessing the Images

> **Note on image count:** The original collection contains 2,007 images. After cosine-similarity deduplication and quality filtering, **1,990 images** were retained and used in all experiments. The exact set is defined by the `image_name` column in the `splits/` CSV files.

The images are available in `data/images.zip` (tracked via Git LFS — see the `data/` folder). Upon acceptance, the full dataset will be formally published on Hugging Face with a persistent identifier and DOI.

After extracting, set `IMGS_DIR` in the first cell of each notebook to point to the extracted folder.

### Data License

Annotation files (splits CSVs, labels) are released under **CC BY 4.0** — see `LICENSE-DATA`.  
Images are anonymized and carry no patient-identifying information, in accordance with Ethics Committee approval (Plataforma Brasil, CNS 466/2012).

---

## How to Reproduce

```bash
# 1. Clone this repository
git clone https://github.com/[ANONYMIZED]/[ANONYMIZED].git
cd [ANONYMIZED]

# 2. Install dependencies
pip install torch torchvision timm pandas numpy scikit-learn matplotlib seaborn statsmodels

# 3. Extract images
#    Unzip data/images.zip to a local folder, e.g.: /data/gastroscopy_images/

# 4. Open notebooks and set IMGS_DIR and SPLITS_DIR in the first cell
jupyter notebook
```

**Recommended execution order:**

| Step | Notebook | Purpose |
|------|----------|---------|
| 1 | 00, 01 | Data validation and EDA |
| 2 | 02, 04 | Architecture and loss ablations |
| 3 | 03 | CCR training — produces checkpoints used downstream |
| 4 | 05 | Grad-CAM generation and medical validation |
| 5 | 06–09 | CCR controls, shortcut audit, LOCO, ML-Decoder |

---

## Key Results

| Experiment | Key Finding |
|---|---|
| Architecture comparison | Swin-Tiny: macro-F1 0.605 ± 0.031; PR-AUC 0.705 ± 0.029 |
| CCR (lambda=0.6) | Polyp F1: 0.529 → 0.657 (+12.8 pp; Cohen's d=1.08) |
| CCR structural controls | real C > shuffled > uniform > base confirms clinical structure contribution |
| Shortcut audit | 2 flagged pairs: SALIVA+EROSION (+10.5 pp FN), LIGHT+ERYTHEMA (+11.3 pp FN) |
| LOCO | A→B macro-F1=0.250; B→A macro-F1=0.186 (vs 0.663/0.506 in-distribution) |
| ML-Decoder vs linear | Delta=−0.045; CI95[−0.079, −0.008] — linear head wins at this data scale |
| Multi-rater explainability | Fleiss' kappa=0.49; 81.3% of Grad-CAM maps clinically acceptable (3 endoscopists, 64 ratings) |

---

## Requirements

- Python 3.10+
- PyTorch 2.1+ and Torchvision
- timm 1.0
- pandas, numpy, scikit-learn, matplotlib, seaborn
- statsmodels (bootstrap intervals)

---

## Licenses

| Component | License |
|---|---|
| Code (notebooks, scripts) | [MIT](LICENSE) |
| Data (annotations, splits CSV files) | [CC BY 4.0](LICENSE-DATA) |

---

## Citation

> Paper under review — citation to be added upon acceptance.
