# Data

## images.zip (Git LFS — 512 MB)

Contains the 2,007 original gastroscopy images together with the annotation
file (`annotations.csv`), which lists all 11 binary labels per image.

The **1,990 images used in experiments** are those referenced by `image_name`
in the `../splits/` CSV files (17 images were excluded after cosine-similarity
deduplication and quality filtering).

After extracting, set `IMGS_DIR` in the first cell of each notebook to point
to the extracted folder.

> Upon acceptance, the full dataset will be formally published on Hugging Face
> with a persistent identifier and DOI.

**License:** CC BY 4.0 — see `../LICENSE-DATA`
