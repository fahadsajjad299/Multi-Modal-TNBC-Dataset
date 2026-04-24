# TNBC-TCGA Multi-Modal Dataset
**A reproducible, clinically curated cohort of Triple-Negative Breast Cancer patients derived from TCGA-BRCA, with linked whole-slide images from the NCI Genomic Data Commons.**

---

## Overview

Triple-Negative Breast Cancer (TNBC) is defined by the absence of estrogen receptor (ER), progesterone receptor (PR), and HER2 expression — accounting for roughly 15% of all breast cancer diagnoses and associated with aggressive clinical behaviour and limited targeted therapy options.

This dataset was constructed from the publicly available TCGA-BRCA cohort via cBioPortal, applying a rigorous 19-condition filtering pipeline to isolate confirmed TNBC cases with complete clinical metadata and linked whole-slide pathology images. All exclusion decisions are individually logged, making every cohort construction step fully auditable and reproducible.

All 150 whole-slide images are **open-access** on the NCI GDC — no data access agreement or dbGaP application is required. Downloads use standard HTTPS or the `gdc-client` CLI.

---

## Key Statistics

| Metric | Value |
|---|---|
| Confirmed TNBC patients | 153 |
| Whole-slide images (WSI) | 150 DX diagnostic SVS files |
| Total image data | 148 GB |
| Patients with no GDC slide | 7 (4.6%) |
| Excluded records (logged) | 955 |
| Clinical columns in master CSV | 62 |
| Median age at diagnosis | 53 years (range: 29–90) |
| Median overall survival | 28.5 months |
| Median mutation count | 47 (nonsynonymous) |
| Tissue preservation | 100% fresh-frozen (Non-FFPE) |

---

## Dataset Contents

**`TNBC_master.csv`** — 153 rows × 62 columns
Full clinical and molecular dataset. The primary analysis file. One row per confirmed TNBC patient with all receptor status, staging, genomic, treatment, and outcome fields.

**`TNBC_confirmed_patients.csv`** — 153 rows × all source columns
All columns from the original TCGA-BRCA TSV, filtered to confirmed TNBC patients only. Wider than master; includes all raw cBioPortal fields for custom downstream analyses.

**`TNBC_patient_slide_ids.csv`** — 153 rows × 13 columns
Patient-to-slide mapping. Contains pathology report UUIDs, tissue source site codes, FFPE status, AJCC stage, and histologic type — sufficient to link patients to GDC image files.

**`TNBC_GDC_File_IDs.csv`** — 150 slides across 146 patients
Direct WSI download links from the GDC API. Includes GDC UUID, file name, MD5 checksum, file size, and four URL formats: direct download, GDC portal, legacy archive, and gdc-client storage path.

**`TNBC_exclusion_log.csv`** — 955 exclusion entries
Full audit trail of every patient removed from the cohort. Each row records a Patient ID and the specific exclusion criterion triggered. Essential for reproducibility and CONSORT-style flow diagrams.

---

## Cohort Construction Pipeline

Starting from the TCGA-BRCA TSV downloaded from cBioPortal, 19 sequential filtering conditions were applied:

| Step | Condition | Excluded |
|---|---|---|
| C01 | Cancer Type = Breast Cancer only | −8 |
| C02 | Invasive carcinoma histology required | 0 |
| C03 | Primary tumor samples only (no metastatic) | −7 |
| C04 | ICD-10 code must be C50.x (breast site) | 0 |
| C05 | ER Status By IHC = Negative | −857 |
| C06 | PR Status By IHC = Negative | −18 |
| C07 | HER2 Negative by ASCO/CAP criteria (IHC + FISH) | −60 |
| C08 | Pathology report UUID must be present | 0 |
| C09 | Patient ID and Sample ID must be non-null | 0 |
| C10 | Deduplicate on Patient ID (keep first) | 0 |
| C11 | Tumour tissue site = breast (where stated) | 0 |
| C12 | Metastatic Tumor Indicator must not be YES | −5 |
| C13 | Sex = Female (null permitted) | 0 |
| C14 | Study ID must contain brca_tcga | 0 |
| C15 | Patient ID must match TCGA-XX-XXXX format | 0 |
| C16 | Diagnosis Age must be numeric, range 1–120 | 0 |
| C17 | ER IHC% must be <10% (consistency check) | 0 |
| C18 | PR IHC% must be <10% (consistency check) | 0 |
| C19 | FFPE flag added — informational, no exclusions | 0 |

**HER2 classification logic (C07):**
- IHC-HER2 = Negative → HER2 Negative ✓
- IHC-HER2 = Equivocal + FISH Negative → HER2 Negative ✓
- IHC-HER2 = Equivocal + FISH missing or Positive → Excluded
- IHC-HER2 = Positive → Excluded
- IHC-HER2 missing + FISH Negative → HER2 Negative ✓
- IHC-HER2 missing + FISH missing or Positive → Excluded

---

## Cohort Characteristics

**Histologic subtype**
- Infiltrating Ductal Carcinoma: 131 (85.6%)
- Other / specify: 6 (3.9%)
- Infiltrating Lobular Carcinoma: 5 (3.3%)
- Metaplastic Carcinoma: 5 (3.3%)
- Medullary Carcinoma: 3 (2.0%)
- NOS / Mixed Histology: 2 (1.3%)

**AJCC stage**
- Stage IIA: 72 (47.1%)
- Stage IIB: 25 (16.3%)
- Stage IA: 16 (10.5%)
- Stage IIIA: 14 (9.2%)
- Stage I: 13 (8.5%)
- Stage IIIB / IIIC / IV: 9 (5.9%)

**Race / ethnicity**
- White: 88 (57.5%)
- Black or African American: 50 (32.7%)
- Asian: 8 (5.2%)
- Not reported: 7 (4.6%)

**HER2 confirmation path**
- Negative by IHC alone: 113 (73.9%)
- Equivocal IHC, confirmed Negative by FISH: 25 (16.3%)
- IHC missing, confirmed Negative by FISH: 14 (9.2%)
- Indeterminate IHC, confirmed Negative by FISH: 1 (0.7%)

**Outcomes**
- Living at follow-up: 132 (86.3%)
- Deceased: 21 (13.7%)

---

## Whole-Slide Image Access

WSIs are in SVS format (Aperio ScanScope), compatible with OpenSlide, QuPath, CLAM, and UNI. All files are open-access — no token required.

**Single slide download:**
```bash
curl -O https://api.gdc.cancer.gov/data/{GDC_File_ID}
```

**Bulk download via gdc-client:**
```bash
pip install gdc-client
gdc-client download -m TNBC_GDC_Manifest.txt
```

**URL formats in `TNBC_GDC_File_IDs.csv`:**
- `Download_URL` — direct HTTPS download (curl, requests, wget)
- `Portal_URL` — GDC Data Portal browser link
- `Legacy_URL` — legacy archive endpoint for older TCGA pipelines
- `Storage_Path` — `/data/{uuid}/{filename}` format for gdc-client and HPC systems

> **Note:** 7 patients (4.6%) have no DX diagnostic slide in the current GDC index. For these patients, only tissue microarray (BS) slides were submitted to TCGA. These patients retain full clinical metadata and are included in all CSV files.

---

## Reproducibility

- All filtering logic is implemented in a single Python script using only `pandas` and `numpy` — no custom packages required.
- Every exclusion event is individually recorded in `TNBC_exclusion_log.csv` with the Patient ID and the specific condition code and human-readable reason.
- GDC file retrieval is fully scripted via the public `https://api.gdc.cancer.gov/files` API — no manual download steps.
- All 19 filtering conditions are independently parameterised — any condition can be loosened or removed to study pipeline sensitivity.
- The pipeline passes 10 programmatic validation checks covering ER/PR/HER2 status, sample type, duplicates, age range, UUID completeness, and Patient ID format.

---

## Key Schema — TNBC_master.csv

| Field | Type | Description |
|---|---|---|
| TNBC_Index | int | Sequential patient index (1–153) |
| Patient ID | str | TCGA barcode (format: TCGA-XX-XXXX) |
| Sample ID | str | TCGA sample barcode |
| Pathology report uuid | str | GDC UUID — used to download WSI |
| Diagnosis Age | float | Age at initial diagnosis (years) |
| ER Status By IHC | str | Negative for all patients |
| PR status by ihc | str | Negative for all patients |
| IHC-HER2 | str | Negative / Equivocal / null |
| HER2 fish status | str | FISH result where IHC was equivocal |
| Neoplasm Histologic Type Name | str | WHO histologic classification |
| Neoplasm Disease Stage Code | str | AJCC overall stage (Stage IA – IV) |
| Mutation Count | float | Total somatic mutation count |
| Fraction Genome Altered | float | Proportion of genome with CNA |
| Overall Survival (Months) | float | Follow-up duration |
| Overall Survival Status | str | 0:LIVING or 1:DECEASED |
| FFPE_Flag | str | Non_FFPE for all samples |
| Race Category | str | Self-reported race |

---

## Data Source & Ethics

TCGA data was collected under IRB-approved protocols with patient consent. All patient-level data is de-identified per TCGA policy. No additional data access agreement is required for the open-tier files used here.

- **Primary source:** The Cancer Genome Atlas — BRCA cohort (TCGA-BRCA), accessed via cBioPortal (https://www.cbioportal.org)
- **Image source:** NCI Genomic Data Commons (GDC) — https://portal.gdc.cancer.gov
- **Image format:** SVS (Aperio ScanScope)
- **Receptor classification:** ASCO/CAP 2018 HER2 guidelines; standard IHC for ER/PR
- **Derived dataset license:** CC BY 4.0

---

## Citation

*(To be updated after MICCAI submission)*

If you use this dataset, please also cite the original TCGA-BRCA publication and acknowledge the NCI Genomic Data Commons.
