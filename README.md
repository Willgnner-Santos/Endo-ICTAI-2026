# Endoscopy Image Classification Pipeline

This repository contains the official codebase for our study on multi-label classification of endoscopic findings. To ensure full reproducibility of our experiments, we provide the exact data splits, the training/evaluation notebooks, and the generated figures.

## Repository Structure

- `Notebooks/`: Contains the Jupyter notebooks used for data auditing, exploratory data analysis (EDA), baseline model training, loss function optimization (M2 Loss), and model explainability (Grad-CAM).
- `splits/`: Contains the CSV files with the exact train/validation/test splits used in our experiments, ensuring the exact same data partitions can be reproduced without data leakage.
- `figs/`: Contains the generated figures, comparison charts, and Grad-CAM visualizations.

## Dataset

The dataset used in this study is hosted publicly on Hugging Face. You can access the original images and their respective annotations via the link below:

🔗 **[Dataset on Hugging Face (Placeholder Link)](https://huggingface.co/datasets/placeholder-endo-dataset)**

*Note: The images should be downloaded and placed in your local environment. You will need to adjust the `IMGS_DIR` path inside the notebooks to point to your local image folder.*

## How to Reproduce the Experiments

1. Clone this repository to your local machine.
2. Download the dataset from the Hugging Face link above.
3. Open the notebooks in the `Notebooks/` directory.
4. Update the path configurations in the initial cells of each notebook to point to the downloaded dataset and the `splits/` directory provided in this repository.
5. Execute the notebooks in sequential order to replicate the baseline models, the optimized multi-label approaches, and the explainability visualizations.

## Requirements

The experiments were conducted using the following main dependencies:
- Python 3.10+
- PyTorch & Torchvision
- pandas, numpy, scikit-learn, matplotlib
- timm (for standard backbone architectures)
