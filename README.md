# TCGA-BRCA TNBC Dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20690243.svg)](https://doi.org/10.5281/zenodo.20690243)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![MICCAI 2026](https://img.shields.io/badge/MICCAI-2026%20Open%20Data-blue)](https://miccai2026.org)

A curated binary-classification dataset of **153 confirmed TNBC patients** and **153 age-matched Luminal A controls** derived from TCGA-BRCA, with linked diagnostic whole-slide images (WSIs) from the NCI Genomic Data Commons.

---

## Dataset at a Glance

| | TNBC (Label = 1) | Luminal A (Label = 0) |
|---|:---:|:---:|
| Patients | 153 | 153 |
| With linked WSI | 146 (95.4%) | 146 (95.4%) |
| DX SVS files | 146 | 157 |
| Receptor profile | ER− PR− HER2− | ER+ PR+ HER2− |
| Median age | 53.0 yrs | 53.0 yrs |
| WSI size | ~150 GB | ~155 GB |
| GDC access | Open | Open |

---

## Repository Structure

```
Multi-Modal-TNBC-Dataset/
│
├── DATA SETS/
│   ├── brca_tcga_clinical_data from_cbioportal.tsv   # Raw TCGA-BRCA export (1,107 patients, 145 variables) — pipeline input
│   ├── TNBC_master.csv                               # Full TNBC cohort with all clinical variables (n=153)
│   ├── TNBC_confirmed_patients.csv                   # Final confirmed TNBC patients after all 19 filtering steps
│   ├── TNBC_patient_slide_ids.csv                    # Patient → GDC slide UUID mapping for WSI download
│   └── TNBC_exclusion_log.csv                        # Every excluded patient with pipeline step and reason code
│
├── Scripts/
│   ├── tnbc.py                                       # TNBC cohort construction pipeline (C01–C19)
│   └── non_tnbc.py                                   # Age-matched Luminal A control cohort construction
│
└── Results/
    └── section7_outputs.zip                          # ResNet50 + UMAP + HDBSCAN embedding analysis outputs
```

---

## Downloading the WSIs

WSIs are not stored here — download them from the NCI GDC (open access, no approval needed).

```bash
# Install: https://gdc.cancer.gov/access-data/gdc-data-transfer-tool
gdc-client download -m TNBC_GDC_Manifest.txt     -d ./TNBC_WSIs/
gdc-client download -m NonTNBC_GDC_Manifest.txt  -d ./NonTNBC_WSIs/
```

All slides are H&E-stained diagnostic sections in **Aperio SVS format**, compatible with OpenSlide, QuPath, and standard WSI libraries.

> 7 patients per cohort have no linked DX SVS in GDC — their IDs are flagged in the metadata files. They can still be used for clinical/tabular tasks.

---

## How the Cohort Was Built

Clinical data came from the full TCGA-BRCA cBioPortal export (`brca_tcga_clinical_data from_cbioportal.tsv`, 1,107 patients). A **19-step filtering pipeline (C01–C19)** implemented in `Scripts/tnbc.py` was applied to identify confirmed TNBC cases. An age-matched Luminal A control cohort was then constructed by `Scripts/non_tnbc.py` using stratified sampling across five age bins (fixed seed = 42). Every excluded patient is recorded in `TNBC_exclusion_log.csv` with the corresponding step and reason.

Key criteria:
- Invasive primary breast carcinoma only
- HER2 classification per ASCO/CAP guidelines (IHC ± FISH)
- No cohort overlap — verified programmatically
- No HER2-positive contamination in Luminal A — verified programmatically

---

## Quick Start

```python
import pandas as pd

# Load cohorts
tnbc    = pd.read_csv("DATA SETS/TNBC_confirmed_patients.csv")
# Luminal A metadata coming in next release — see Scripts/non_tnbc.py to regenerate

# Load WSI linkage
slides  = pd.read_csv("DATA SETS/TNBC_patient_slide_ids.csv")

# Check exclusion log
log     = pd.read_csv("DATA SETS/TNBC_exclusion_log.csv")
print(log["reason_code"].value_counts())
```

---

## Important Notes

- **Labels are patient-level only.** No patch, tile, or region annotations are provided.
- **Luminal A is "pure"** (ER+/PR+/HER2−). Do not assume models trained here generalise to Luminal B, HER2-enriched, or basal-like subtypes.
- **Multi-site data.** WSIs come from 22 source sites (TNBC) and 18 (Luminal A) — consider site-level batch effects in your pipeline.
- **Embedding outputs** in `Results/section7_outputs.zip` are exploratory (ResNet50 + UMAP + HDBSCAN on 27-patient subset). Cluster/failure labels are not expert-validated.

---

## Citation

```bibtex
@dataset{sajjad2026tnbc,
  author    = {Sajjad, Fahad and Ibrahim, Jalal and Aliyu, Yusuf Munir},
  title     = {A Curated TNBC and Age-Matched Luminal A Cohort from TCGA-BRCA
               with Linked Diagnostic Whole-Slide Images},
  year      = {2026},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.20690243},
  url       = {https://doi.org/10.5281/zenodo.20690243}
}
```

---

## License

Metadata and curation code: **CC BY 4.0**.  
Whole-slide images: governed by [TCGA data access policies](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/using-tcga/using-tcga-data) (open access).

---

## Contact

Open an issue or email **fahadsajjad299@gmail.com**
