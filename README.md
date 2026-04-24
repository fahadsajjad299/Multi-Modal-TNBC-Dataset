# TNBC-TCGA: A Curated Multi-Modal Dataset of Triple-Negative Breast Cancer

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Data: Open Access](https://img.shields.io/badge/Data-Open%20Access-green.svg)](https://portal.gdc.cancer.gov)
[![Source: TCGA-BRCA](https://img.shields.io/badge/Source-TCGA--BRCA-blue.svg)](https://www.cbioportal.org/study/summary?id=brca_tcga)
[![Patients: 153](https://img.shields.io/badge/Patients-153-orange.svg)]()
[![WSI: 150 slides · 148 GB](https://img.shields.io/badge/WSI-150%20slides%20·%20148%20GB-purple.svg)]()

---

## Overview

153 confirmed Triple-Negative Breast Cancer (TNBC) patients derived from the TCGA-BRCA cohort (1,107 total), with linked open-access whole-slide pathology images from the NCI Genomic Data Commons. TNBC is defined by the absence of ER, PR, and HER2 expression, representing ~15% of all breast cancers and associated with aggressive clinical behaviour and limited targeted therapy options.

The cohort was constructed by applying a **19-condition sequential filtering pipeline** to the cBioPortal TCGA-BRCA dataset. Every exclusion decision is individually logged, making cohort construction fully auditable and reproducible from the included source file.

No data access agreement or dbGaP application is required — all 150 whole-slide images are open-access on the NCI GDC.

---

## Dataset at a Glance

| | |
|---|---|
| **Source cohort** | TCGA-BRCA via cBioPortal (1,107 patients, 141 fields) |
| **Confirmed TNBC patients** | 153 |
| **Exclusion rate** | 86.2% (955 patients filtered, all logged) |
| **Clinical fields (master CSV)** | 62 curated from 141 source fields |
| **Whole-slide images** | 150 DX diagnostic SVS files (146 patients) |
| **Total image size** | 148 GB |
| **Patients without GDC slide** | 7 (4.6%) — retained in all CSVs with full clinical data |
| **Median age at diagnosis** | 53 years (range: 29–90) |
| **Median overall survival** | 28.5 months |
| **Tissue preservation** | 100% fresh-frozen (Non-FFPE) |
| **Image format** | Aperio SVS (OpenSlide / QuPath / CLAM compatible) |
| **License** | CC BY 4.0 |

---

## Repository Contents

```
.
├── TNBC_master.csv                          # Primary analysis file — 153 × 62 clinical fields
├── TNBC_confirmed_patients.csv              # 153 patients × all 141 source columns (full-width)
├── TNBC_patient_slide_ids.csv               # Patient-to-slide mapping with GDC UUIDs
├── TNBC_GDC_File_IDs.csv                    # WSI download URLs (150 slides, 4 URL formats)
├── TNBC_exclusion_log.csv                   # Audit log — 955 excluded entries with reasons
└── brca_tcga_clinical_data_from_cbioportal__.tsv   # Original unfiltered TCGA-BRCA source
```

### File Descriptions

**`TNBC_master.csv`** — 153 rows × 62 columns  
The primary analysis file. One row per confirmed TNBC patient, containing receptor status, AJCC staging, histologic subtype, genomic metrics, treatment history, and survival outcomes. Curated from the 141-column source TSV. Start here.

**`TNBC_confirmed_patients.csv`** — 153 rows × 141 columns  
All original cBioPortal fields, filtered to confirmed TNBC patients only. Use this for custom downstream analyses requiring fields not included in the master file.

**`TNBC_patient_slide_ids.csv`** — 153 rows × 13 columns  
Patient-to-slide mapping. Contains pathology report UUIDs, tissue source site codes, FFPE status, AJCC stage, and histologic type — the linking table between clinical data and GDC image files.

**`TNBC_GDC_File_IDs.csv`** — 150 slides across 146 patients  
GDC file metadata and download links for all whole-slide images. Includes GDC UUID, filename, MD5 checksum, file size, and four URL formats: direct HTTPS download, GDC portal, legacy archive, and `gdc-client` storage path.

**`TNBC_exclusion_log.csv`** — 955 entries  
Complete audit trail. Every excluded patient is recorded with their Patient ID and the specific filtering condition triggered (C01–C19). Required for CONSORT-style flow diagrams and pipeline reproducibility.

**`brca_tcga_clinical_data_from_cbioportal__.tsv`** — 1,107 rows × 141 columns  
The original unfiltered source file as downloaded from cBioPortal. Included for full transparency and re-derivation of the cohort with alternative criteria.

---

## Cohort Characteristics

**Histologic subtype**

| Subtype | n | % |
|---|---|---|
| Infiltrating Ductal Carcinoma | 131 | 85.6% |
| Infiltrating Lobular Carcinoma | 5 | 3.3% |
| Metaplastic Carcinoma | 5 | 3.3% |
| Medullary Carcinoma | 3 | 2.0% |
| Other / Mixed / NOS | 9 | 5.9% |

**AJCC Stage**

| Stage | n | % |
|---|---|---|
| Stage IA | 16 | 10.5% |
| Stage I | 13 | 8.5% |
| Stage IIA | 72 | 47.1% |
| Stage IIB | 25 | 16.3% |
| Stage IIIA | 14 | 9.2% |
| Stage IIIB / IIIC / IV | 9 | 5.9% |
| Not reported | 4 | 2.6% |

**Race**

| | n | % |
|---|---|---|
| White | 88 | 57.5% |
| Black or African American | 50 | 32.7% |
| Asian | 8 | 5.2% |
| Not reported | 7 | 4.6% |

**HER2 confirmation path**

| Path | n | % |
|---|---|---|
| Negative by IHC alone | 113 | 73.9% |
| Equivocal IHC → FISH Negative | 25 | 16.3% |
| IHC missing → FISH Negative | 14 | 9.2% |
| Indeterminate IHC → FISH Negative | 1 | 0.7% |

**Outcomes:** 132 living (86.3%), 21 deceased (13.7%) at follow-up.

---

## Quick Start

### Load the cohort

```python
import pandas as pd

df = pd.read_csv('TNBC_master.csv')
print(f"{len(df)} confirmed TNBC patients")

# Stage distribution
print(df['Neoplasm Disease Stage American Joint Committee on Cancer Code'].value_counts())

# Young patients (age < 40)
young = df[df['Diagnosis Age'] < 40]
```

### Merge clinical data with WSI download links

```python
df_clinical = pd.read_csv('TNBC_master.csv')
df_images   = pd.read_csv('TNBC_GDC_File_IDs.csv')

cohort = df_clinical.merge(df_images, left_on='Patient ID', right_on='Patient_ID')
print(cohort[['Patient ID', 'Diagnosis Age', 'Download_URL']].head())
```

### Download a single slide

```bash
curl -O https://api.gdc.cancer.gov/data/{GDC_File_ID}
```

### Bulk download via gdc-client

```bash
pip install gdc-client
gdc-client download -m TNBC_GDC_Manifest.txt -d ./WSI_Data
```

### Audit the filtering pipeline

```python
df_excl = pd.read_csv('TNBC_exclusion_log.csv')

# Exclusions by condition
print(df_excl['Exclusion Reason'].value_counts())

# Check a specific patient
pid = 'TCGA-3C-AAAU'
row = df_excl[df_excl['Patient ID'] == pid]
print(row['Exclusion Reason'].values)
```

---

## Cohort Construction Pipeline

Starting from the TCGA-BRCA TSV (1,107 records), 19 sequential conditions were applied:

| Step | Condition | Excluded |
|---|---|---|
| C01 | Cancer Type = Breast Cancer | −8 |
| C02 | Invasive carcinoma histology | 0 |
| C03 | Primary tumor samples only | −7 |
| C04 | ICD-10 code = C50.x (breast site) | 0 |
| C05 | ER Status By IHC = Negative | −857 |
| C06 | PR Status By IHC = Negative | −18 |
| C07 | HER2 Negative — ASCO/CAP 2018 (IHC + FISH) | −60 |
| C08 | Pathology report UUID present | 0 |
| C09 | Patient ID and Sample ID non-null | 0 |
| C10 | Deduplicate on Patient ID | 0 |
| C11 | Tumour tissue site = breast | 0 |
| C12 | Metastatic Tumor Indicator ≠ YES | −5 |
| C13 | Sex = Female (null permitted) | 0 |
| C14 | Study ID contains brca_tcga | 0 |
| C15 | Patient ID matches TCGA-XX-XXXX | 0 |
| C16 | Diagnosis Age numeric, range 1–120 | 0 |
| C17 | ER IHC% < 10% (consistency check) | 0 |
| C18 | PR IHC% < 10% (consistency check) | 0 |
| C19 | FFPE flag added — informational | 0 |

**HER2 classification logic (C07)** implements ASCO/CAP 2018 guidelines:

| IHC-HER2 | FISH Result | Classification |
|---|---|---|
| Negative | Any | HER2 Negative ✓ |
| Equivocal | Negative | HER2 Negative ✓ |
| Equivocal | Missing / Positive | Excluded ✗ |
| Positive | Any | Excluded ✗ |
| Missing | Negative | HER2 Negative ✓ |
| Missing | Missing / Positive | Excluded ✗ |

> **Note:** In the TCGA cBioPortal dataset, `IHC Score` is an internal numeric scale and does **not** correspond to standard HER2 scoring (0/1+/2+/3+). Only the categorical `IHC-HER2` text field is used for classification.

After filtering, all 153 patients pass 10 independent programmatic validation checks covering ER/PR/HER2 status, sample type, Patient ID format, age range, UUID completeness, and deduplication.

---

## Whole-Slide Image Access

All images are **Aperio SVS format**, open-access, no token required.

```
Format         : Aperio SVS (TIFF-based)
Slide count    : 150 DX diagnostic slides
Patients       : 146 (7 have no DX slide in GDC index)
Total size     : 148 GB
Resolution     : Typically 20× or 40× objective
Access         : Open (no dbGaP agreement required)
Tool support   : OpenSlide, QuPath, CLAM, UNI, Aperio ImageScope
```

**`TNBC_GDC_File_IDs.csv` includes four URL formats:**

| Field | Use |
|---|---|
| `Download_URL` | Direct curl/wget/requests download |
| `Portal_URL` | Browse file in GDC web interface |
| `Legacy_URL` | Legacy archive endpoint |
| `Storage_Path` | Used by `gdc-client` and HPC systems |

**Tissue microarray (BS) slides are excluded.** Only DX (diagnostic) whole-slide images are collected — BS slides have limited spatial resolution and are not suitable for deep learning patch extraction.

---

## Reproducibility

This cohort is fully reproducible from the included source file:

1. **Source data included** — `brca_tcga_clinical_data_from_cbioportal__.tsv` is the exact input used
2. **All filtering logic documented** — 19 conditions with rationales above and in the pipeline script
3. **Complete audit trail** — `TNBC_exclusion_log.csv` records every exclusion with Patient ID and condition code
4. **No black boxes** — single Python script, `pandas` and `numpy` only
5. **Validation automated** — 10 post-filtering checks fail loudly on any discrepancy
6. **GDC integration scripted** — SVS file IDs retrieved via the public GDC API

To re-derive this exact cohort:
```bash
pip install pandas numpy requests
python Cohort_Construction_Pipeline.py
# Expected output: 153 confirmed TNBC patients, 955 excluded
```

---

## Data Schema — `TNBC_master.csv` (key fields)

| Field | Type | Description |
|---|---|---|
| `TNBC_Index` | int | Sequential patient index (1–153) |
| `Patient ID` | str | TCGA barcode — format: TCGA-XX-XXXX |
| `Pathology report uuid` | str | GDC UUID — links to WSI download |
| `Diagnosis Age` | float | Age at initial diagnosis (years) |
| `ER Status By IHC` | str | Negative for all patients |
| `PR status by ihc` | str | Negative for all patients |
| `IHC-HER2` | str | Negative / Equivocal / null |
| `HER2 fish status` | str | FISH result where IHC was equivocal |
| `Neoplasm Histologic Type Name` | str | WHO histologic classification |
| `Neoplasm Disease Stage Code` | str | AJCC overall stage (IA – IV) |
| `Mutation Count` | float | Total nonsynonymous somatic mutations |
| `Fraction Genome Altered` | float | Proportion of genome with CNA |
| `Overall Survival (Months)` | float | Follow-up duration |
| `Overall Survival Status` | str | 0:LIVING or 1:DECEASED |
| `FFPE_Flag` | str | Non_FFPE for all samples |
| `Race Category` | str | Self-reported race |

Full schema: 62 fields spanning demographics, receptor status, staging, genomics, treatment, and outcomes. See `TNBC_confirmed_patients.csv` for all 141 original source fields.

---

## Source Data & Ethics

TCGA data was collected under IRB-approved protocols with informed patient consent. All data is de-identified per TCGA policy. No additional data access agreement is required for the open-tier files used here.

| | |
|---|---|
| **Clinical source** | TCGA-BRCA via cBioPortal — https://www.cbioportal.org/study/summary?id=brca_tcga |
| **Image source** | NCI Genomic Data Commons — https://portal.gdc.cancer.gov |
| **ER/PR classification** | ASCO/CAP 2007 IHC standards (≤10% positive cells = Negative) |
| **HER2 classification** | ASCO/CAP 2018 HER2 testing guidelines |
| **Staging** | AJCC Cancer Staging Manual, 6th and 7th editions |
| **Histology** | WHO Classification of Tumours of the Breast (ICD-O-3) |
| **Diagnosis dates** | 2003–2014 (TCGA enrollment period) |

---

## Citation

> *(To be updated after submission.)*

If you use this dataset, please also cite the original TCGA-BRCA study and the NCI GDC:

```bibtex
@article{tcga2013,
  author  = {Weinstein, John N. and others},
  title   = {The Cancer Genome Atlas Pan-Cancer analysis project},
  journal = {Nature Genetics},
  year    = {2013},
  volume  = {45},
  pages   = {1113--1120},
  doi     = {10.1038/ng.2764}
}

@article{cerami2012,
  author  = {Cerami, Ethan and others},
  title   = {The cBio Cancer Genomics Portal: an open platform for exploring multidimensional cancer genomics data},
  journal = {Cancer Discovery},
  year    = {2012},
  volume  = {2},
  pages   = {401--404},
  doi     = {10.1158/2159-8290.CD-12-0095}
}

@misc{gdc,
  author = {{NCI Genomic Data Commons}},
  title  = {GDC Data Portal},
  url    = {https://portal.gdc.cancer.gov}
}
```
