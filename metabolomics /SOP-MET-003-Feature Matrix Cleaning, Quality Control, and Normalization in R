# SOP-MET-003

## Feature Matrix Cleaning, Quality Control, and Normalization in R

**Code:** SOP-MET-003
**Area:** Metabolomics
**Application:** Untargeted LC–MS Metabolomics
**Software:** R / RStudio
**Author:** Tainá Schons
**Created:** 2026-06-04
**Last Updated:** 2026-06-06
**Level:** Advanced

---

## Overview

This SOP describes a reproducible workflow for cleaning, quality control, filtering, and normalization of untargeted LC–MS metabolomics data in R.

The workflow starts from feature tables exported from MZmine and includes:

* Import and organization of feature matrices
* Metadata alignment
* Technical replicate collapsing
* Initial quality control (QC)
* Exploratory PCA
* Blank filtering
* Prevalence filtering
* QC-based filtering
* Missing value handling
* Data normalization and transformation
* Preparation of the final matrix for statistical analysis

This protocol was developed for untargeted metabolomics datasets acquired by high-resolution LC–MS platforms and can be adapted to different experimental designs.

---

## Requirements

### Software

* R (≥ 4.3 recommended)
* RStudio

### Required Packages

```r
library(tidyverse)
library(readr)
library(readxl)
library(dplyr)
library(janitor)
library(tibble)
library(ggplot2)
library(scales)
```

---

## Input Files

### Feature Tables

Exported from MZmine:

```text
featurelist_neg.csv
featurelist_pos.csv
```

### Metadata

Excel workbook containing sample metadata:

```text
metadata_batch1.xlsx
```

Required columns:

```text
sample_id
filename
sample_type
group
batch
inj_order
```

---

## Workflow Structure

### Part 1 – Matrix Import and Metadata Alignment

* Import MZmine feature tables
* Extract feature annotations
* Build abundance matrices
* Harmonize sample names
* Align metadata and feature matrices
* Collapse technical replicates when necessary

### Part 2 – Initial Quality Control and Exploratory PCA

* Missingness assessment
* Total signal evaluation
* Number of detected features
* QC stability assessment
* Injection-order inspection
* Exploratory PCA
* Outlier screening

### Part 3 – Feature Filtering

* Blank filtering
* Prevalence filtering
* QC-based filtering

### Part 4 – Data Normalization

* Missing value imputation
* Signal normalization
* Log transformation
* Scaling

### Part 5 – Final Quality Assessment

* Post-filtering PCA
* QC clustering verification
* Outlier reassessment
* Export of processed matrices

---

## Notes

The filtering thresholds used in this SOP (e.g., blank ratio cutoff, prevalence threshold, QC variability threshold, and imputation strategy) reflect the analytical decisions adopted in our laboratory and should be adjusted according to dataset characteristics, analytical platform, QC performance, and study objectives.

This SOP is intended as a reproducible framework rather than a fixed set of universal parameters.
