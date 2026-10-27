# Transient Splicing-Associated Isoform Remodeling Defines Early MAPKi Resistance Adaptation in Melanoma

This repository contains computational analysis scripts, selected processed data, and supporting transcript quantification files associated with the study:

## Overview

A long-standing question in cancer therapeutics is how tumor cells rapidly adapt to targeted therapies and survive drug exposure before acquiring stable resistance. In BRAF-mutant melanoma, adaptation to MAPK inhibitors (MAPKi) involves extensive transcriptional reprogramming and phenotypic plasticity, but the contribution of transcript isoform remodeling remains incompletely understood.

This study investigates the dynamics of differential transcript usage (DTU) during MAPKi treatment by integrating bulk and single-cell RNA-seq datasets from melanoma cell lines, murine models, patient-derived xenografts, and patient samples.

## Key Findings

- **Transient isoform remodeling:** Differential transcript usage is most prominent during the early on-treatment adaptive state and is substantially attenuated following the emergence of acquired resistance.
- **Distinct from gene-expression changes:** Most genes exhibiting differential transcript usage do not show corresponding changes in total gene expression.
- **Resistance-associated signaling:** Transiently altered transcripts are associated with pathways involved in MAPKi adaptation, including PI3K/AKT/mTOR and receptor tyrosine kinase signaling.
- **Identification of adaptive states:** Transcript-usage-based classifiers outperform conventional gene-expression-based models in distinguishing no-treatment from on-treatment melanoma samples in the evaluated validation datasets.
- **Developmental plasticity:** MAPKi-associated transcript remodeling recapitulates melanoblast-associated developmental splicing programs.
- **Candidate splicing regulators:** Computational modeling identifies seven candidate factors associated with the transient isoform program, particularly MBNL2, DDX5, and AHNAK.

## Publicly Available Datasets

| GEO accession | Description | Role |
|---|---|---|
| [GSE75299](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE75299) | MAPKi-treated melanoma cell lines and patient biopsies | Primary discovery and patient evaluation |
| [GSE103725](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE103725) | Murine melanoma transcriptomic data | Cross-model validation |
| [GSE116237](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE116237) | Melanoma PDX single-cell RNA-seq | Treatment-phase evaluation |

The developmental splicing analysis additionally uses melanoblast and melanocyte RNA-seq data described by Baggiolini et al. (*Science*, 2021).


## Analysis Scripts

### 1. Differential Transcript Usage Analysis

**Script:** `scripts/differential_transcript_usage.R`

Performs differential transcript usage analysis using DTUrtle and DRIMSeq.

The repository includes Salmon transcript quantification files (`quant.sf`) for 15 GSE75299 melanoma cell-line samples representing untreated and on-treatment conditions.

The script reads Salmon estimated transcript counts and uses GENCODE v43 transcript annotations.

**Additional requirement:** Download the GENCODE release 43 GTF annotation and place it in `data/reference/` using the filename specified in the script.

**Note:** The provided DTU script is adapted to read the included Salmon files directly. Its numerical equivalence to the original manuscript-generating workflow has not yet been established. The statistical threshold should be checked against the final manuscript.

### 2. Splicing-Factor Regression Analysis

**Script:** `scripts/splicing_factor_plsr.R`

Implements partial least squares regression (PLSR) to examine associations between splicing-factor expression and mean transcript inclusion levels.

The workflow evaluates a curated splicing-factor set and a reduced model containing seven selected candidate factors:

MBNL2, AHNAK, NONO, DDX5, SF3B1, SLC38A2, and SON.

The script retains the original modeling approach, including jackknife-based coefficient evaluation and leave-one-out cross-validation.

**Additional requirements:** The original transcript-proportion matrix, gene-expression matrix, candidate transcript workbook, and splicing-factor annotation file are required. Their expected locations are documented in `data/plsr/README.md`.

### 3. Classification Performance Evaluation

**Notebook:** `notebooks/roc_pr_curves.ipynb`

Generates receiver operating characteristic (ROC) and precision–recall curves to compare DTU-based and DEG-based prediction results.

The `data/predictions/` directory contains 12 prediction CSV files provided by the authors.

The notebook evaluates previously generated predictions and does not reproduce the original classifier-training or feature-selection procedures.

## Software and Dependencies

The analyses use R and Python, including:

**R:** DTUrtle, DRIMSeq, DESeq2, BiocParallel, pls, dplyr, ggplot2, preprocessCore, and related packages.

**Python:** Jupyter, pandas, NumPy, matplotlib, scikit-learn, and other dependencies specified in `requirements.txt`.

See `r_dependencies.R` for the R package requirements.

## Usage

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/MAPKi-splicing.git
cd MAPKi-splicing
```

Install Python dependencies:

```bash
python -m pip install -r requirements.txt
```

Open the classification analysis notebook:

```bash
cd notebooks
jupyter notebook roc_pr_curves.ipynb
```

Run the DTU analysis from the repository root after supplying the required reference annotation:

```bash
Rscript scripts/differential_transcript_usage.R
```

Run the splicing-factor model after adding the necessary PLSR input files:

```bash
Rscript scripts/splicing_factor_plsr.R
```

## Reproducibility and Availability

This repository provides selected computational scripts, transcript quantification files, and processed prediction data supporting the manuscript.

The repository does not currently include every upstream processing and model-training step, including the complete pseudo-bulk generation workflow, classifier training, and developmental exon-usage analysis. These components should not be considered fully reproducible from the repository alone.

Raw sequencing data are available through the corresponding public accession records.

## Citation

If you use this repository or its associated analyses, please cite:

Mukherjee S., Singh A., et al. *Transient Splicing-Associated Isoform Remodeling Defines Early MAPKi Resistance Adaptation in Melanoma.* 

The complete publication citation and DOI will be updated after publication.

## Contact

**Sumit Mukherjee**  
Cancer Data Science Laboratory  
National Cancer Institute, National Institutes of Health, USA  
Email: mukherjee.sumit89@gmail.com
