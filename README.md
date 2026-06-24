<div align="center">

# A Clinically Curated and Provenance-Aware TCGA-BRCA Cohort of Triple-Negative Breast Cancer and Age-Matched Luminal A Controls with Linked Whole-Slide Images

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20765962.svg)](https://doi.org/10.5281/zenodo.20765962)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![MICCAI 2026](https://img.shields.io/badge/MICCAI-2026%20Open%20Data-blue)](https://miccai2026.org)

**Fahad Sajjad¹ · Jalal Ibrahim² · Yusuf Munir Aliyu³**

¹ Institute of Biotechnology and Genetic Engineering, University of Agriculture, Peshawar, Pakistan
² Institute of Computer Science and Information Technology, University of Agriculture, Peshawar, Pakistan
³ Department of Microbiology, Umaru Musa Yar'adua University, Katsina, Nigeria

📧 fahadsajjadbioinfo@aup.edu.pk

</div>

---

A reproducible, clinically annotated cohort of **153 confirmed TNBC patients** (ER−/PR−/HER2−) and **153 age-matched Luminal A controls** (ER+/PR+/HER2−) derived from TCGA-BRCA, with linked diagnostic whole-slide images (WSIs) from the NCI Genomic Data Commons (GDC). The cohort was built through a fully documented, 19-step clinical filtering pipeline (C01–C19), with a complete exclusion audit log for every patient removed. An illustrative failure-aware embedding-space analysis (ResNet50 → UMAP → HDBSCAN) is included for a 27-patient subset to support model-uncertainty research in computational pathology.

---

## Dataset at a Glance

| | TNBC (Label = 1) | Luminal A (Label = 0) |
|---|---|---|
| Confirmed patients | 153 | 153 |
| With linked diagnostic WSI | 146 (95.4%) | 146 (95.4%) |
| Total DX SVS files (GDC) | 146 | 157 |
| Patients with no DX slide in GDC | 7 | 7 |
| Receptor profile | ER− PR− HER2− | ER+ PR+ HER2− |
| Sex | Female (100%) | Female (100%) |
| Median age at diagnosis | 53.0 yrs (29–90) | 53.0 yrs (37–71) |
| Mean age ± SD | 54.9 ± 12.0 | 53.5 ± 8.8 |
| Overall survival: Living | 132 (86.3%) | 139 (90.8%) |
| Distinct tissue source sites | 22 | 18 |
| Approx. total WSI data size | ~150 GB | ~155 GB |
| GDC access level | Open | Open |

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
├── model_resNet/                                     # ResNet50 feature-extraction code for the embedding analysis
│
└── Results/
    └── section7_outputs.zip                          # ResNet50 + UMAP + HDBSCAN embedding analysis outputs
```

---

## How the Cohort Was Built

Clinical data came from the full TCGA-BRCA cBioPortal export (`brca_tcga_clinical_data from_cbioportal.tsv`, 1,107 patients, 145 clinical fields). A **19-step filtering pipeline (C01–C19)**, implemented in `Scripts/tnbc.py`, was applied to identify confirmed TNBC cases. HER2 status was assigned per ASCO/CAP guidelines (IHC, with FISH where IHC was equivocal). An age-matched Luminal A control cohort was then constructed by `Scripts/non_tnbc.py` using stratified age-bin sampling (fixed seed = 42), minimizing the absolute age difference to the TNBC median within each bin.

Every excluded patient is logged in `TNBC_exclusion_log.csv` with the corresponding pipeline step and reason code.

Key integrity checks (verified programmatically):
- No patient overlap between the TNBC and Luminal A cohorts
- No HER2-positive contamination in the Luminal A cohort
- All 153×2 patients confirmed Non-FFPE specimens

---

## Cohort Demographics (highlights)

| Characteristic | TNBC (n=153) | Luminal A (n=153) |
|---|---|---|
| Race: White | 88 (57.5%) | 119 (77.8%) |
| Race: Black / African American | 50 (32.7%) | 15 (9.8%) |
| AJCC Stage IIA | 72 (47.1%) | 43 (28.1%) |
| AJCC Stage I/IA | 29 (19.0%) | 21 (13.7%) |
| OS status: Deceased | 21 (13.7%) | 14 (9.2%) |
| Adjuvant chemotherapy: Yes | 29 (19.0%) | 15 (9.8%) |
| Surgical procedure: Lumpectomy | 51 (33.3%) | 29 (19.0%) |

Full demographic and clinical breakdown (ethnicity, all stage categories, disease-free status, surgical procedure, radiotherapy) is in the paper's Table 2 — consider keeping a `demographics_table.csv` alongside the master CSVs if you want this machine-readable.

> Consistent with known TNBC epidemiology, Black/African American women are over-represented relative to the Luminal A control group — worth accounting for when training or auditing models on this cohort.

---

## Downloading the WSIs

WSIs are not stored in this repository — download them from the NCI GDC (open access, no approval needed):

```bash
# Install: https://gdc.cancer.gov/access-data/gdc-data-transfer-tool
gdc-client download -m TNBC_GDC_Manifest.txt     -d ./TNBC_WSIs/
gdc-client download -m NonTNBC_GDC_Manifest.txt  -d ./NonTNBC_WSIs/
```

All slides are H&E-stained diagnostic (DX) sections in **Aperio SVS format**, compatible with OpenSlide, QuPath, and standard WSI libraries.

> 7 patients per cohort have clinical records but no linked DX SVS currently indexed in GDC. They're flagged in the metadata and can still be used for clinical/tabular tasks.

---

## Failure-Aware Embedding Analysis

A 27-patient TNBC subset was tiled into **8,100 non-overlapping 256×256 patches**, embedded with an ImageNet-pretrained **ResNet50**, reduced to 50 dimensions via PCA, projected to 2D with **UMAP**, then clustered with **HDBSCAN**:

| | Patches | % |
|---|---|---|
| Cluster 0 | 658 | 8.1% |
| Cluster 1 | 898 | 11.1% |
| Noise (unclustered) | 6,544 | 80.8% |
| **Failure-prone patches** (noise, manifold-boundary) | **405** | **5.0%** |

The raw UMAP projection is a single continuous, heterogeneous manifold rather than well-separated clusters — consistent with smooth tissue-pattern transitions. Failure-prone patches concentrate near manifold boundaries and image edges, i.e. where pretrained ResNet50 features are least reliable. See `Results/section7_outputs.zip` for the full outputs; cluster/failure labels here are exploratory and not expert-validated.

---

## Quick Start

```python
import pandas as pd

# Load cohorts
tnbc   = pd.read_csv("DATA SETS/TNBC_confirmed_patients.csv")
# Luminal A metadata CSV — regenerate with Scripts/non_tnbc.py if not yet released

# Load WSI linkage
slides = pd.read_csv("DATA SETS/TNBC_patient_slide_ids.csv")

# Check exclusion log
log = pd.read_csv("DATA SETS/TNBC_exclusion_log.csv")
print(log["reason_code"].value_counts())
```

---

## Limitations

1. **Cohort size** — 153 + 153 patients is small relative to pan-cancer imaging cohorts; sufficient for benchmarking, not for large-scale pretraining.
2. **WSI coverage gaps** — 7 patients per cohort (4.6%) have no indexed DX SVS in GDC; excluded from image-based tasks.
3. **Control cohort definition** — Luminal A here is strictly "pure" (ER+/PR+/HER2−). Models trained on this dataset should not be assumed to generalize to HER2-enriched, Luminal B, or basal-like subtypes without further data and retraining.
4. **Label granularity** — Labels are patient/sample-level only; no patch-, tile-, or region-level pathologist annotations exist. The embedding-analysis cluster/failure assignments are exploratory, not ground truth.
5. **Feature-extractor dependence** — The embedding analysis uses a generic pretrained ResNet50; cluster structure and failure regions may shift substantially with a different backbone.
6. **Multi-site imaging** — Slides originate from 22 (TNBC) and 18 (Luminal A) tissue source sites; scanner/staining protocol variation may introduce batch effects.

---

## Licensing, Access, and Ethics

- Clinical and imaging data originate from TCGA and are usable for research subject to TCGA's data access policy.
- All records are de-identified via TCGA barcodes; no names, dates of birth, or medical record numbers are present in any released file.
- No additional IRB review was required or performed; no new human-subject data were collected.
- **Curation code and released metadata (CSV/TSV):** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Whole-slide images:** governed by [TCGA data access policies](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/using-tcga/using-tcga-data) (open access)

---

## Citation

```bibtex
@dataset{sajjad2026tnbc,
  author    = {Sajjad, Fahad and Ibrahim, Jalal and Aliyu, Yusuf Munir},
  title     = {A Clinically Curated and Provenance-Aware TCGA-BRCA Cohort of
               Triple-Negative Breast Cancer and Age-Matched Luminal A Controls
               with Linked Whole-Slide Images},
  year      = {2026},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.20765962},
  url       = {https://doi.org/10.5281/zenodo.20765962}
}
```

## Data Availability

Metadata, exclusion logs, and curation pipeline: https://doi.org/10.5281/zenodo.20765962 (CC BY 4.0)
Whole-slide images: [NCI Genomic Data Commons](https://portal.gdc.cancer.gov/) via `TNBC_GDC_Manifest.txt` and `NonTNBC_GDC_Manifest.txt`.

## Contact

Open an issue, or email **fahadsajjadbioinfo@aup.edu.pk**
