# Multi-Label Classification in Brazilian Gastroscopy: Clinical Co-occurrence, Shortcuts, and Explainability

Official code repository for the paper published at **IEEE ICTAI 2026**.

**Authors:** Willgnner F. Santos · Paula A. Merhi · Daniela M. M. Cardoso · Paulo V. dos Santos · Amanda T. Silva · Reinaldo Falluh Filho · Marcos M. Macedo Neto · Sandro B. de Andrade Júnior · Marcella S. R. Martins · Ronaldo M. da Costa

[![DOI Dataset](https://zenodo.org/badge/DOI/10.5281/zenodo.22864869.svg)](https://doi.org/10.5281/zenodo.22864869)
[![HuggingFace](https://img.shields.io/badge/🤗_Dataset-BraGED-yellow)](https://huggingface.co/datasets/Willgnner-Santos/braged)
[![License: MIT](https://img.shields.io/badge/Code-MIT-green.svg)](LICENSE)
[![License: CC BY 4.0](https://img.shields.io/badge/Data-CC_BY_4.0-blue.svg)](LICENSE-DATA)

---

## Overview

This repository provides the full reproducibility package for the study on clinically structured multi-label classification of gastroscopic findings using the **BraGED** dataset. It includes:

- 10 Jupyter notebooks covering data auditing through ML-Decoder comparison
- Exact cross-validation splits (hash-frozen, no data leakage)
- Annotation CSV files (CC BY 4.0)

The paper evaluates five dimensions of clinical reliability: predictive performance, co-occurrence structure (CCR), artifact-induced shortcuts, cross-center generalization (LOCO), and multi-rater explainability validation.

---

## Repository Structure

```
Endo-ICTAI-2026/
├── Notebooks/
│   ├── 00_splits_audit.ipynb            # Data integrity and split validation
│   ├── 01_image_eda.ipynb               # Exploratory data analysis
│   ├── 02_baseline_backbones.ipynb      # Architecture comparison (ResNet-50, EfficientNet-B3, ConvNeXt-Tiny, Swin-Tiny)
│   ├── 03_optimized_m2.ipynb            # CCR (lambda=0.6) + imbalance strategy ablations
│   ├── 04_weighted_loss_and_focal.ipynb # Focal Loss / ASL / sampling ablations
│   ├── 05_gradcam_explainability.ipynb  # Grad-CAM generation and medical validation
│   ├── 06_ccr_control.ipynb             # CCR structural controls (real C vs shuffled vs uniform vs base)
│   ├── 07_shortcut_audit.ipynb          # Ten-pair artifact-pathology shortcut audit
│   ├── 08_leave_one_center_out.ipynb    # LOCO cross-center generalization experiment
│   └── 09_mldecoder_baseline.ipynb      # ML-Decoder vs linear head comparison
├── splits/
│   ├── fold_{0-4}_{train,val,test}.csv  # 5-fold CV partitions (stratified, hash-frozen)
│   └── image_group_mapping.csv          # Image-to-group mapping for deduplication
├── LICENSE                              # MIT License (code)
├── LICENSE-DATA                         # CC BY 4.0 License (data/annotations)
└── README.md
```

---

## Dataset — BraGED

The **BraGED (Brazilian Gastro-Endoscopy Dataset)** comprises **1,990 gastroscopy images** from two Brazilian institutions (Center 1 / HGG: 720 images; Center 2 / IAD: 1,270 images), annotated across **11 binary labels** (5 clinical findings + 2 artifacts + 2 general indicators + 2 rare findings). Imbalance ratios range from 1.0 to 496:1.

### Download

| Platform | Link |
|---|---|
| **Zenodo** (primary, DOI-citable) | [https://doi.org/10.5281/zenodo.22864869](https://doi.org/10.5281/zenodo.22864869) |
| **HuggingFace** | [Willgnner-Santos/braged](https://huggingface.co/datasets/Willgnner-Santos/braged) |

> **Note:** The dataset is currently restricted pending paper publication. It will be made fully public upon publication of the associated paper. To request early access, contact: eng.willgnner@gmail.com

After downloading, extract the images and set `IMGS_DIR` in the first cell of each notebook to point to the extracted folder.

### Data License

Images and annotation files are released under **CC BY 4.0** — see `LICENSE-DATA`.
Images are anonymized and carry no patient-identifying information (Ethics approval: Plataforma Brasil, CAAE 59898122.7.0000.0035; CNS 466/2012).

---

## How to Reproduce

```bash
# 1. Clone this repository
git clone https://github.com/Willgnner-Santos/Endo-ICTAI-2026.git
cd Endo-ICTAI-2026

# 2. Install dependencies
pip install torch torchvision timm pandas numpy scikit-learn matplotlib seaborn statsmodels

# 3. Download BraGED images from Zenodo or HuggingFace
#    Extract to a local folder and set IMGS_DIR in the first notebook cell

# 4. Run notebooks in order
jupyter notebook
```

**Recommended execution order:**

| Step | Notebook | Purpose |
|---|---|---|
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
| CCR (λ=0.6) | Polyp F1: 0.529 → 0.657 (+12.8 pp; Cohen's d=1.08) |
| CCR structural controls | real C > shuffled > uniform > base — confirms clinical structure contribution |
| Shortcut audit | 2 flagged pairs: SALIVA+EROSION (+10.5 pp FN excess), LIGHT+ERYTHEMA (+11.3 pp FN excess) |
| LOCO | Center 1→2: macro-F1=0.250; Center 2→1: macro-F1=0.186 (vs 0.605 in-distribution) |
| ML-Decoder vs linear | Δ=−0.036 — linear head outperforms ML-Decoder at this data scale |
| Multi-rater explainability | Fleiss' κ=0.49; 81.3% of Grad-CAM maps clinically acceptable (3 endoscopists, 64 ratings) |

---

## Authors & Affiliations

| Author | Affiliation |
|---|---|
| Willgnner Ferreira Santos | Institute of Informatics (INF), Federal University of Goiás (UFG); SENAI Fatesg – NIAA, Goiânia, Brazil |
| Paula Andraous Merhi | Alberto Rassi State Hospital (HGG), Goiânia, Brazil |
| Daniela Medeiros Milhomem Cardoso | Alberto Rassi State Hospital (HGG), Goiânia, Brazil |
| Paulo Victor dos Santos | IIG, Federal University of Goiás (UFG); SENAI Fatesg – NIAA, Goiânia, Brazil |
| Amanda Teles Silva | Alberto Rassi State Hospital (HGG), Goiânia, Brazil |
| Reinaldo Falluh Filho | Alberto Rassi State Hospital (HGG), Goiânia, Brazil |
| Marcos Martins Macedo Neto | Alberto Rassi State Hospital (HGG), Goiânia, Brazil |
| Sandro Batista de Andrade Júnior | Goiás Emergency Hospital (HUGO), Goiânia, Brazil |
| Marcella Scoczynski Ribeiro Martins | Federal University of Technology – Paraná (UTFPR), Curitiba, Brazil |
| Ronaldo Martins da Costa | Institute of Informatics (INF), Federal University of Goiás (UFG), Goiânia, Brazil |

Corresponding author: eng.willgnner@gmail.com

---

## Licenses

| Component | License |
|---|---|
| Code (notebooks, scripts) | [MIT](LICENSE) |
| Data (images, annotations, splits CSVs) | [CC BY 4.0](LICENSE-DATA) |

---

## Citation

If you use this code or the BraGED dataset, please cite:

```bibtex
@inproceedings{santos2026multilabel,
  author    = {Santos, Willgnner Ferreira and Merhi, Paula Andraous and
               Cardoso, Daniela Medeiros Milhomem and Santos, Paulo Victor dos and
               Silva, Amanda Teles and Falluh Filho, Reinaldo and
               Macedo Neto, Marcos Martins and Andrade J{\'u}nior, Sandro Batista de and
               Martins, Marcella Scoczynski Ribeiro and Costa, Ronaldo Martins da},
  title     = {Multi-Label Classification in {Brazilian} Gastroscopy: Clinical Co-occurrence, Shortcuts, and Explainability},
  booktitle = {Proceedings of the IEEE International Conference on Tools with Artificial Intelligence (ICTAI)},
  year      = {2026}
}

@dataset{santos2026braged,
  author    = {Santos, Willgnner Ferreira and others},
  title     = {{BraGED}: {Brazilian} {Gastro-Endoscopy} {Dataset}},
  year      = {2026},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.22864869},
  url       = {https://doi.org/10.5281/zenodo.22864869}
}
```
