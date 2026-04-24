<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>TNBC-TCGA Dataset README</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=Sora:wght@300;400;500;600;700&display=swap');

  :root {
    --pink:       #D4537E;
    --pink-light: #FBEAF0;
    --pink-mid:   #F4C0D1;
    --pink-dark:  #4B1528;
    --teal:       #1D9E75;
    --teal-light: #E1F5EE;
    --teal-mid:   #5DCAA5;
    --teal-dark:  #04342C;
    --coral:      #D85A30;
    --coral-light:#FAECE7;
    --amber:      #BA7517;
    --amber-light:#FAEEDA;
    --gray-100:   #F1EFE8;
    --gray-200:   #D3D1C7;
    --gray-400:   #888780;
    --gray-700:   #444441;
    --gray-900:   #2C2C2A;
    --bg:         #FAFAF8;
    --white:      #FFFFFF;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Sora', system-ui, sans-serif;
    background: var(--bg);
    color: var(--gray-900);
    font-size: 14.5px;
    line-height: 1.75;
    padding: 0;
  }

  .page { max-width: 860px; margin: 0 auto; padding: 3rem 2rem 6rem; }

  /* ── HERO ── */
  .hero {
    border: 1px solid var(--pink-mid);
    border-radius: 16px;
    padding: 2.5rem 2.5rem 2rem;
    margin-bottom: 2.5rem;
    position: relative;
    overflow: hidden;
    background: var(--white);
  }
  .hero::before {
    content: '';
    position: absolute; top: -60px; right: -60px;
    width: 260px; height: 260px;
    border-radius: 50%;
    background: var(--pink-light);
    opacity: 0.7;
    pointer-events: none;
  }
  .hero::after {
    content: '';
    position: absolute; bottom: -40px; left: 30%;
    width: 180px; height: 180px;
    border-radius: 50%;
    background: var(--teal-light);
    opacity: 0.5;
    pointer-events: none;
  }
  .hero-inner { position: relative; z-index: 1; }
  .badge-row { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 1.25rem; }
  .badge {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    font-weight: 500;
    padding: 3px 10px;
    border-radius: 999px;
    letter-spacing: 0.03em;
  }
  .badge-pink   { background: var(--pink-light);  color: var(--pink);   border: 1px solid var(--pink-mid); }
  .badge-teal   { background: var(--teal-light);  color: #0F6E56;       border: 1px solid var(--teal-mid); }
  .badge-amber  { background: var(--amber-light); color: var(--amber);  border: 1px solid #FAC775; }
  .badge-coral  { background: var(--coral-light); color: var(--coral);  border: 1px solid #F5C4B3; }
  .badge-gray   { background: var(--gray-100);    color: var(--gray-700); border: 1px solid var(--gray-200); }

  h1 {
    font-size: 2rem;
    font-weight: 700;
    line-height: 1.2;
    color: var(--gray-900);
    margin-bottom: 0.6rem;
    letter-spacing: -0.02em;
  }
  h1 span { color: var(--pink); }
  .hero-sub {
    font-size: 14px;
    color: var(--gray-400);
    font-weight: 400;
    max-width: 540px;
    margin-bottom: 1.75rem;
  }

  /* ── STAT CARDS ── */
  .stat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
    gap: 10px;
    margin-bottom: 0;
  }
  .stat-card {
    background: var(--gray-100);
    border-radius: 12px;
    padding: 1rem 1.1rem;
    position: relative;
  }
  .stat-card .val {
    font-size: 1.75rem;
    font-weight: 700;
    line-height: 1;
    margin-bottom: 3px;
    letter-spacing: -0.03em;
  }
  .stat-card .lbl {
    font-size: 11.5px;
    color: var(--gray-400);
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.06em;
  }
  .stat-card .sub {
    font-size: 11px;
    color: var(--gray-400);
    margin-top: 2px;
    font-family: 'IBM Plex Mono', monospace;
  }
  .c-pink { color: var(--pink); }
  .c-teal { color: var(--teal); }
  .c-amber { color: var(--amber); }
  .c-coral { color: var(--coral); }

  /* ── SECTIONS ── */
  section { margin-bottom: 2.25rem; }
  h2 {
    font-size: 0.75rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--gray-400);
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  h2::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--gray-200);
  }

  h3 {
    font-size: 1rem;
    font-weight: 600;
    color: var(--gray-900);
    margin-bottom: 0.5rem;
  }

  /* ── PIPELINE ── */
  .pipeline {
    border: 1px solid var(--gray-200);
    border-radius: 12px;
    overflow: hidden;
    background: var(--white);
  }
  .pipeline-row {
    display: grid;
    grid-template-columns: 36px 1fr auto;
    align-items: start;
    gap: 0 12px;
    padding: 0.75rem 1.1rem;
    border-bottom: 1px solid var(--gray-100);
    position: relative;
  }
  .pipeline-row:last-child { border-bottom: none; }
  .pipeline-row:hover { background: var(--gray-100); }
  .step-num {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    font-weight: 500;
    color: var(--gray-400);
    padding-top: 2px;
  }
  .step-body { }
  .step-title {
    font-size: 13.5px;
    font-weight: 500;
    color: var(--gray-900);
    margin-bottom: 1px;
  }
  .step-desc {
    font-size: 12px;
    color: var(--gray-400);
    font-weight: 400;
  }
  .step-excl {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11.5px;
    font-weight: 500;
    color: var(--coral);
    white-space: nowrap;
    padding-top: 2px;
    text-align: right;
  }
  .step-excl.none { color: var(--teal); }

  /* ── FILES ── */
  .files-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  @media (max-width: 600px) { .files-grid { grid-template-columns: 1fr; } }
  .file-card {
    border: 1px solid var(--gray-200);
    border-radius: 12px;
    padding: 1.1rem 1.25rem;
    background: var(--white);
  }
  .file-name {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    font-weight: 500;
    color: var(--pink);
    margin-bottom: 0.4rem;
  }
  .file-desc {
    font-size: 13px;
    color: var(--gray-700);
    margin-bottom: 0.5rem;
    line-height: 1.55;
  }
  .file-meta {
    font-size: 11.5px;
    color: var(--gray-400);
    font-family: 'IBM Plex Mono', monospace;
  }

  /* ── DISTRIBUTION BARS ── */
  .dist-table { width: 100%; border-collapse: collapse; }
  .dist-table tr { border-bottom: 1px solid var(--gray-100); }
  .dist-table tr:last-child { border-bottom: none; }
  .dist-table td { padding: 6px 0; font-size: 13px; vertical-align: middle; }
  .dist-table .label { color: var(--gray-700); width: 220px; padding-right: 12px; }
  .dist-table .bar-cell { padding-right: 12px; }
  .bar-track { height: 6px; border-radius: 999px; background: var(--gray-100); overflow: hidden; }
  .bar-fill { height: 100%; border-radius: 999px; }
  .bar-pink  { background: var(--pink); }
  .bar-teal  { background: var(--teal); }
  .bar-amber { background: var(--amber); }
  .bar-coral { background: var(--coral); }
  .dist-table .count { color: var(--gray-400); font-family: 'IBM Plex Mono', monospace; font-size: 12px; text-align: right; white-space: nowrap; }

  /* ── CODE BLOCK ── */
  .code-block {
    background: var(--gray-900);
    border-radius: 10px;
    padding: 1.1rem 1.4rem;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12.5px;
    color: #D3D1C7;
    overflow-x: auto;
    line-height: 1.7;
  }
  .code-block .cm { color: var(--gray-400); }
  .code-block .ck { color: var(--teal-mid); }
  .code-block .cv { color: var(--pink-mid); }
  .code-block .cs { color: var(--amber-light); }

  /* ── CALLOUT ── */
  .callout {
    display: flex;
    gap: 12px;
    padding: 1rem 1.25rem;
    border-radius: 10px;
    border: 1px solid;
    margin-bottom: 1rem;
    font-size: 13.5px;
    line-height: 1.6;
  }
  .callout-teal  { background: var(--teal-light);  border-color: var(--teal-mid);  color: #0F6E56; }
  .callout-amber { background: var(--amber-light); border-color: #FAC775;          color: #854F0B; }
  .callout-pink  { background: var(--pink-light);  border-color: var(--pink-mid);  color: #72243E; }
  .callout-icon  { font-size: 16px; line-height: 1.6; flex-shrink: 0; }

  /* ── SCHEMA TABLE ── */
  .schema-table { width: 100%; border-collapse: collapse; font-size: 13px; }
  .schema-table th {
    text-align: left;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--gray-400);
    font-weight: 600;
    padding: 6px 10px 6px 0;
    border-bottom: 1px solid var(--gray-200);
  }
  .schema-table td {
    padding: 7px 10px 7px 0;
    border-bottom: 1px solid var(--gray-100);
    color: var(--gray-700);
    vertical-align: top;
  }
  .schema-table tr:last-child td { border-bottom: none; }
  .schema-table .field-name {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    color: var(--pink);
    font-weight: 500;
  }
  .schema-table .field-type {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    color: var(--teal);
  }

  /* ── FOOTER ── */
  .footer {
    margin-top: 3rem;
    padding-top: 1.5rem;
    border-top: 1px solid var(--gray-200);
    font-size: 12.5px;
    color: var(--gray-400);
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  .footer a { color: var(--pink); text-decoration: none; }

  p { color: var(--gray-700); margin-bottom: 0.75rem; }
  ul { list-style: none; padding-left: 0; }
  ul li { color: var(--gray-700); font-size: 13.5px; padding: 3px 0; padding-left: 1.2rem; position: relative; }
  ul li::before { content: '—'; position: absolute; left: 0; color: var(--gray-200); }
</style>
</head>
<body>
<div class="page">

  <!-- ══════════════════════════════ HERO ══════════════════════════════ -->
  <div class="hero">
    <div class="hero-inner">
      <div class="badge-row">
        <span class="badge badge-pink">TCGA-BRCA</span>
        <span class="badge badge-teal">Triple-Negative</span>
        <span class="badge badge-amber">153 Patients</span>
        <span class="badge badge-coral">150 WSIs · 148 GB</span>
        <span class="badge badge-gray">Open Access · GDC</span>
      </div>

      <h1>TNBC-TCGA<br/><span>Multi-Modal Dataset</span></h1>
      <p class="hero-sub">
        A reproducible, clinically curated cohort of Triple-Negative Breast Cancer patients derived from TCGA-BRCA, with linked whole-slide images from the NCI Genomic Data Commons.
      </p>

      <div class="stat-grid">
        <div class="stat-card">
          <div class="val c-pink">153</div>
          <div class="lbl">Confirmed TNBC</div>
          <div class="sub">after 19-step pipeline</div>
        </div>
        <div class="stat-card">
          <div class="val c-teal">150</div>
          <div class="lbl">WSI Slides</div>
          <div class="sub">DX diagnostic SVS</div>
        </div>
        <div class="stat-card">
          <div class="val c-amber">148 GB</div>
          <div class="lbl">Total Image Data</div>
          <div class="sub">open-access GDC</div>
        </div>
        <div class="stat-card">
          <div class="val c-coral">955</div>
          <div class="lbl">Excluded Rows</div>
          <div class="sub">fully logged</div>
        </div>
        <div class="stat-card">
          <div class="val" style="font-size:1.5rem">53 yrs</div>
          <div class="lbl">Median Age</div>
          <div class="sub">range: 29 – 90</div>
        </div>
        <div class="stat-card">
          <div class="val" style="font-size:1.5rem">62</div>
          <div class="lbl">Clinical Columns</div>
          <div class="sub">in master CSV</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════════════════════ OVERVIEW ══════════════════════════ -->
  <section>
    <h2>Overview</h2>
    <p>
      Triple-Negative Breast Cancer (TNBC) is defined by the absence of estrogen receptor (ER), progesterone receptor (PR), and HER2 expression — accounting for roughly 15% of all breast cancer diagnoses and associated with aggressive clinical behaviour and limited targeted therapy options.
    </p>
    <p>
      This dataset was constructed from the publicly available TCGA-BRCA cohort via cBioPortal, applying a rigorous 19-condition filtering pipeline to isolate confirmed TNBC cases with complete clinical metadata and linked whole-slide pathology images. All exclusion decisions are individually logged, making every cohort construction step fully auditable and reproducible.
    </p>
    <div class="callout callout-teal">
      <span class="callout-icon">✦</span>
      <div>All 150 whole-slide images are <strong>open-access</strong> on the NCI GDC — no data access agreement or dbGaP application is required. Downloads use standard HTTPS or the <code>gdc-client</code> CLI.</div>
    </div>
  </section>

  <!-- ══════════════════════════════ FILES ══════════════════════════════ -->
  <section>
    <h2>Dataset contents</h2>
    <div class="files-grid">
      <div class="file-card">
        <div class="file-name">TNBC_master.csv</div>
        <div class="file-desc">Full clinical and molecular dataset. The primary analysis file. One row per confirmed TNBC patient with all receptor status, staging, genomic, treatment, and outcome fields.</div>
        <div class="file-meta">153 rows · 62 columns</div>
      </div>
      <div class="file-card">
        <div class="file-name">TNBC_confirmed_patients.csv</div>
        <div class="file-desc">All columns from the original TCGA-BRCA TSV, filtered to confirmed TNBC patients only. Wider than master; includes all raw cBioPortal fields for custom downstream analyses.</div>
        <div class="file-meta">153 rows · all source columns</div>
      </div>
      <div class="file-card">
        <div class="file-name">TNBC_patient_slide_ids.csv</div>
        <div class="file-desc">Patient-to-slide mapping. Contains pathology report UUIDs, tissue source site codes, FFPE status, AJCC stage, and histologic type — sufficient to link patients to GDC image files.</div>
        <div class="file-meta">153 rows · 13 columns</div>
      </div>
      <div class="file-card">
        <div class="file-name">TNBC_GDC_File_IDs.csv</div>
        <div class="file-desc">Direct WSI download links from the GDC API. Includes GDC UUID, file name, MD5 checksum, file size, and four URL formats (direct, portal, legacy, gdc-client path).</div>
        <div class="file-meta">150 slides · 146 patients with SVS · 148 GB total</div>
      </div>
      <div class="file-card" style="grid-column: 1 / -1;">
        <div class="file-name">TNBC_exclusion_log.csv</div>
        <div class="file-desc">Full audit trail of every patient removed from the cohort. Each row records a Patient ID and the specific exclusion criterion that was triggered. Patients excluded by multiple conditions appear with their first-failing condition. Essential for reproducibility and for computing CONSORT-style flow diagrams.</div>
        <div class="file-meta">955 exclusion entries · 7 active exclusion conditions</div>
      </div>
    </div>
  </section>

  <!-- ══════════════════════════════ PIPELINE ══════════════════════════ -->
  <section>
    <h2>Cohort construction pipeline</h2>
    <p style="margin-bottom:1rem">Starting from 1,108 raw entries in the TCGA-BRCA TSV (downloaded from cBioPortal), the following 19 conditions were applied sequentially. Conditions that triggered no exclusions are shown in green.</p>

    <div class="pipeline">
      <div class="pipeline-row">
        <div class="step-num">C01</div>
        <div class="step-body">
          <div class="step-title">Breast cancer cases only</div>
          <div class="step-desc">Cancer Type ∈ {Breast Cancer, Breast Cancer NOS}</div>
        </div>
        <div class="step-excl">−8 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C02</div>
        <div class="step-body">
          <div class="step-title">Invasive carcinoma histology</div>
          <div class="step-desc">Cancer Type Detailed or Neoplasm Histologic Type must indicate invasive carcinoma</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C03</div>
        <div class="step-body">
          <div class="step-title">Primary tumor samples only</div>
          <div class="step-desc">Sample Type = "Primary" — metastatic and unknown samples removed</div>
        </div>
        <div class="step-excl">−7 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C04</div>
        <div class="step-body">
          <div class="step-title">ICD-10 breast site (C50.x)</div>
          <div class="step-desc">ICD-10 Classification must start with "C50" — excludes non-breast primaries</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C05</div>
        <div class="step-body">
          <div class="step-title">ER Negative (IHC)</div>
          <div class="step-desc">ER Status By IHC = "Negative" — largest single exclusion criterion</div>
        </div>
        <div class="step-excl">−857 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C06</div>
        <div class="step-body">
          <div class="step-title">PR Negative (IHC)</div>
          <div class="step-desc">PR Status By IHC = "Negative"</div>
        </div>
        <div class="step-excl">−18 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C07</div>
        <div class="step-body">
          <div class="step-title">HER2 Negative — ASCO/CAP criteria</div>
          <div class="step-desc">IHC-HER2 = Negative, or Equivocal/missing confirmed by FISH Negative. IHC Positive → excluded regardless of FISH.</div>
        </div>
        <div class="step-excl">−60 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C08</div>
        <div class="step-body">
          <div class="step-title">Pathology report UUID present</div>
          <div class="step-desc">Pathology report uuid must not be null — required to retrieve WSI from GDC</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C09</div>
        <div class="step-body">
          <div class="step-title">Valid Patient ID and Sample ID</div>
          <div class="step-desc">Both fields must be non-null</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C10</div>
        <div class="step-body">
          <div class="step-title">Deduplicated on Patient ID</div>
          <div class="step-desc">Where a patient appears multiple times, the first record is retained</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C11</div>
        <div class="step-body">
          <div class="step-title">Tumour tissue site = breast</div>
          <div class="step-desc">Tumor Tissue Site must contain "breast" where explicitly stated; null is permitted</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C12</div>
        <div class="step-body">
          <div class="step-title">No metastatic flag</div>
          <div class="step-desc">Metastatic Tumor Indicator must not be YES/TRUE/1</div>
        </div>
        <div class="step-excl">−5 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C13</div>
        <div class="step-body">
          <div class="step-title">Sex = Female</div>
          <div class="step-desc">Null is permitted (treated as Female for TCGA-BRCA)</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C14</div>
        <div class="step-body">
          <div class="step-title">TCGA-BRCA study</div>
          <div class="step-desc">Study ID must contain "brca_tcga"</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C15</div>
        <div class="step-body">
          <div class="step-title">TCGA Patient ID format</div>
          <div class="step-desc">Patient ID must match regex ^TCGA-[A-Z0-9]{2}-[A-Z0-9]{4}$</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C16</div>
        <div class="step-body">
          <div class="step-title">Valid diagnosis age (1–120)</div>
          <div class="step-desc">Diagnosis Age must be numeric and within physiological range</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C17</div>
        <div class="step-body">
          <div class="step-title">ER IHC% consistency check</div>
          <div class="step-desc">ER IHC Percent Positive must be &lt;10% — values ≥10% contradict ER Negative label</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row">
        <div class="step-num">C18</div>
        <div class="step-body">
          <div class="step-title">PR IHC% consistency check</div>
          <div class="step-desc">PR IHC Percent Positive must be &lt;10%</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
      <div class="pipeline-row" style="background:var(--teal-light)">
        <div class="step-num">C19</div>
        <div class="step-body">
          <div class="step-title">FFPE flag added (informational)</div>
          <div class="step-desc">All 153 samples confirmed Non-FFPE (fresh-frozen tissue). FFPE_Flag column added; no exclusions applied.</div>
        </div>
        <div class="step-excl none">0 excluded</div>
      </div>
    </div>
  </section>

  <!-- ══════════════════════════════ COHORT STATS ════════════════════════ -->
  <section>
    <h2>Cohort characteristics</h2>

    <div style="display:grid; grid-template-columns:1fr 1fr; gap:1.25rem;">

      <div>
        <h3>Histologic subtype</h3>
        <table class="dist-table">
          <tr>
            <td class="label">Infiltrating Ductal</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:86%"></div></div></td>
            <td class="count">131 (85.6%)</td>
          </tr>
          <tr>
            <td class="label">Other / specify</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-amber" style="width:4%"></div></div></td>
            <td class="count">6 (3.9%)</td>
          </tr>
          <tr>
            <td class="label">Infiltrating Lobular</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-teal" style="width:3.3%"></div></div></td>
            <td class="count">5 (3.3%)</td>
          </tr>
          <tr>
            <td class="label">Metaplastic</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-coral" style="width:3.3%"></div></div></td>
            <td class="count">5 (3.3%)</td>
          </tr>
          <tr>
            <td class="label">Medullary</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:2%"></div></div></td>
            <td class="count">3 (2.0%)</td>
          </tr>
          <tr>
            <td class="label">NOS / Mixed</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-amber" style="width:1.3%"></div></div></td>
            <td class="count">2 (1.3%)</td>
          </tr>
        </table>
      </div>

      <div>
        <h3>AJCC stage</h3>
        <table class="dist-table">
          <tr>
            <td class="label">Stage IIA</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:47%"></div></div></td>
            <td class="count">72 (47.1%)</td>
          </tr>
          <tr>
            <td class="label">Stage IIB</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:16%"></div></div></td>
            <td class="count">25 (16.3%)</td>
          </tr>
          <tr>
            <td class="label">Stage IA</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-teal" style="width:10.5%"></div></div></td>
            <td class="count">16 (10.5%)</td>
          </tr>
          <tr>
            <td class="label">Stage IIIA</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-amber" style="width:9%"></div></div></td>
            <td class="count">14 (9.2%)</td>
          </tr>
          <tr>
            <td class="label">Stage I</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-teal" style="width:8.5%"></div></div></td>
            <td class="count">13 (8.5%)</td>
          </tr>
          <tr>
            <td class="label">Stage III (B/C) + IV</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-coral" style="width:5.9%"></div></div></td>
            <td class="count">9 (5.9%)</td>
          </tr>
        </table>
      </div>

      <div>
        <h3>Race / ethnicity</h3>
        <table class="dist-table">
          <tr>
            <td class="label">White</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:57.5%"></div></div></td>
            <td class="count">88 (57.5%)</td>
          </tr>
          <tr>
            <td class="label">Black / African American</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-teal" style="width:32.7%"></div></div></td>
            <td class="count">50 (32.7%)</td>
          </tr>
          <tr>
            <td class="label">Asian</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-amber" style="width:5.2%"></div></div></td>
            <td class="count">8 (5.2%)</td>
          </tr>
          <tr>
            <td class="label">Not reported</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-coral" style="width:4.6%"></div></div></td>
            <td class="count">7 (4.6%)</td>
          </tr>
        </table>
      </div>

      <div>
        <h3>HER2 IHC result</h3>
        <table class="dist-table">
          <tr>
            <td class="label">Negative (IHC)</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-teal" style="width:73.9%"></div></div></td>
            <td class="count">113 (73.9%)</td>
          </tr>
          <tr>
            <td class="label">Equivocal + FISH Neg</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-amber" style="width:16.3%"></div></div></td>
            <td class="count">25 (16.3%)</td>
          </tr>
          <tr>
            <td class="label">IHC missing + FISH Neg</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-pink" style="width:9.2%"></div></div></td>
            <td class="count">14 (9.2%)</td>
          </tr>
          <tr>
            <td class="label">Indeterminate + FISH Neg</td>
            <td class="bar-cell"><div class="bar-track"><div class="bar-fill bar-coral" style="width:0.7%"></div></div></td>
            <td class="count">1 (0.7%)</td>
          </tr>
        </table>
      </div>

    </div>

    <div style="margin-top:1.25rem; display:grid; grid-template-columns:repeat(4,1fr); gap:10px;">
      <div class="stat-card">
        <div class="val c-pink" style="font-size:1.4rem">28.5</div>
        <div class="lbl">Median OS</div>
        <div class="sub">months</div>
      </div>
      <div class="stat-card">
        <div class="val c-teal" style="font-size:1.4rem">86%</div>
        <div class="lbl">Living at follow-up</div>
        <div class="sub">132 / 153 patients</div>
      </div>
      <div class="stat-card">
        <div class="val c-amber" style="font-size:1.4rem">47</div>
        <div class="lbl">Median mutations</div>
        <div class="sub">nonsynonymous TMB</div>
      </div>
      <div class="stat-card">
        <div class="val c-coral" style="font-size:1.4rem">Non-FFPE</div>
        <div class="lbl">Tissue preservation</div>
        <div class="sub">100% fresh-frozen</div>
      </div>
    </div>
  </section>

  <!-- ══════════════════════════════ SCHEMA ════════════════════════════ -->
  <section>
    <h2>Key columns — TNBC_master.csv</h2>
    <table class="schema-table">
      <thead>
        <tr>
          <th>Field</th>
          <th>Type</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr><td class="field-name">TNBC_Index</td><td class="field-type">int</td><td>Sequential patient index (1–153)</td></tr>
        <tr><td class="field-name">Patient ID</td><td class="field-type">str</td><td>TCGA patient barcode (format: TCGA-XX-XXXX)</td></tr>
        <tr><td class="field-name">Sample ID</td><td class="field-type">str</td><td>TCGA sample barcode</td></tr>
        <tr><td class="field-name">Pathology report uuid</td><td class="field-type">str</td><td>GDC pathology UUID — used to download WSI</td></tr>
        <tr><td class="field-name">Diagnosis Age</td><td class="field-type">float</td><td>Age at initial cancer diagnosis (years)</td></tr>
        <tr><td class="field-name">ER Status By IHC</td><td class="field-type">str</td><td>Negative for all confirmed TNBC patients</td></tr>
        <tr><td class="field-name">PR status by ihc</td><td class="field-type">str</td><td>Negative for all confirmed TNBC patients</td></tr>
        <tr><td class="field-name">IHC-HER2</td><td class="field-type">str</td><td>HER2 IHC result (Negative / Equivocal / null)</td></tr>
        <tr><td class="field-name">HER2 fish status</td><td class="field-type">str</td><td>FISH amplification status where IHC was equivocal</td></tr>
        <tr><td class="field-name">Neoplasm Histologic Type Name</td><td class="field-type">str</td><td>WHO histologic classification</td></tr>
        <tr><td class="field-name">Neoplasm Disease Stage … Code</td><td class="field-type">str</td><td>AJCC 6th/7th edition overall stage (Stage IA – IV)</td></tr>
        <tr><td class="field-name">Mutation Count</td><td class="field-type">float</td><td>Total somatic mutation count (median 47)</td></tr>
        <tr><td class="field-name">Fraction Genome Altered</td><td class="field-type">float</td><td>Proportion of genome with copy-number alterations</td></tr>
        <tr><td class="field-name">Overall Survival (Months)</td><td class="field-type">float</td><td>Follow-up duration in months</td></tr>
        <tr><td class="field-name">Overall Survival Status</td><td class="field-type">str</td><td>0:LIVING or 1:DECEASED</td></tr>
        <tr><td class="field-name">FFPE_Flag</td><td class="field-type">str</td><td>Non_FFPE for all samples (fresh-frozen tissue)</td></tr>
        <tr><td class="field-name">Race Category</td><td class="field-type">str</td><td>Self-reported race (WHITE / BLACK OR AFRICAN AMERICAN / ASIAN)</td></tr>
      </tbody>
    </table>
  </section>

  <!-- ══════════════════════════════ WSI ACCESS ════════════════════════ -->
  <section>
    <h2>Whole-slide image access</h2>

    <div class="callout callout-amber">
      <span class="callout-icon">◈</span>
      <div><strong>7 patients (4.6%) have no DX slide in the current GDC index.</strong> This is expected for a small subset of TCGA samples where only tissue-microarray (BS) slides were submitted. These patients retain full clinical metadata and are included in all CSV files.</div>
    </div>

    <h3 style="margin-bottom:0.5rem">Direct HTTPS download</h3>
    <div class="code-block">
<span class="cm"># Single slide download (no token required — open access)</span>
<span class="ck">curl</span> <span class="cs">-O</span> https://api.gdc.cancer.gov/data/<span class="cv">{GDC_File_ID}</span>
    </div>

    <h3 style="margin-top:1.25rem; margin-bottom:0.5rem">Bulk download via gdc-client</h3>
    <div class="code-block">
<span class="cm"># Install the GDC Data Transfer Tool</span>
<span class="ck">pip install</span> gdc-client

<span class="cm"># Download all 150 WSIs using the provided manifest</span>
gdc-client download <span class="cs">-m</span> TNBC_GDC_Manifest.txt

<span class="cm"># Manifest format: id  filename  md5  size  state (tab-separated)</span>
<span class="cm"># Files land in ./{GDC_File_ID}/{filename}.svs</span>
    </div>

    <h3 style="margin-top:1.25rem; margin-bottom:0.5rem">URL formats in TNBC_GDC_File_IDs.csv</h3>
    <table class="schema-table">
      <thead><tr><th>Column</th><th>Use case</th></tr></thead>
      <tbody>
        <tr><td class="field-name">Download_URL</td><td>Direct HTTPS download — use with curl, requests, wget</td></tr>
        <tr><td class="field-name">Portal_URL</td><td>Human-readable GDC Data Portal browser link</td></tr>
        <tr><td class="field-name">Legacy_URL</td><td>Legacy archive endpoint for older TCGA data ingestion pipelines</td></tr>
        <tr><td class="field-name">Storage_Path</td><td>/data/{uuid}/{filename} path format for gdc-client and HPC storage systems</td></tr>
      </tbody>
    </table>
  </section>

  <!-- ══════════════════════════════ REPRO ═════════════════════════════ -->
  <section>
    <h2>Reproducibility</h2>
    <ul>
      <li>All filtering logic is implemented in a single, dependency-minimal Python script (pandas + numpy only) — no custom packages required</li>
      <li>Every exclusion event is individually recorded in <code>TNBC_exclusion_log.csv</code> with Patient ID and the specific condition code and human-readable reason</li>
      <li>GDC file retrieval is fully scripted via the public <code>https://api.gdc.cancer.gov/files</code> API — no manual download steps</li>
      <li>All 19 filtering conditions are independently parameterised — any condition can be loosened or removed to study sensitivity</li>
      <li>The pipeline passes 10 programmatic validation checks (ER/PR/HER2 status, sample type, duplicates, age range, UUID completeness, Patient ID format)</li>
    </ul>
  </section>

  <!-- ══════════════════════════════ DATA SOURCE ═══════════════════════ -->
  <section>
    <h2>Data source &amp; ethics</h2>
    <div class="callout callout-pink">
      <span class="callout-icon">◉</span>
      <div>TCGA data was collected under IRB-approved protocols with patient consent. All patient-level data in this repository is already de-identified per TCGA policy. No additional data access agreement is required for the open-tier files used here.</div>
    </div>
    <ul>
      <li><strong>Primary source:</strong> The Cancer Genome Atlas — BRCA cohort (TCGA-BRCA), accessed via <a href="https://www.cbioportal.org" target="_blank">cBioPortal</a></li>
      <li><strong>Image source:</strong> NCI Genomic Data Commons (GDC) — <a href="https://portal.gdc.cancer.gov" target="_blank">portal.gdc.cancer.gov</a></li>
      <li><strong>Image format:</strong> SVS (Aperio ScanScope) — compatible with OpenSlide, QuPath, CLAM, and UNI</li>
      <li><strong>Receptor classification:</strong> ASCO/CAP 2018 HER2 testing guidelines; standard IHC for ER/PR</li>
      <li><strong>Derived dataset license:</strong> CC BY 4.0 — attribution required, commercial use permitted</li>
    </ul>
  </section>

  <!-- ══════════════════════════════ CITATION ══════════════════════════ -->
  <section>
    <h2>Citation</h2>
    <div class="code-block">
<span class="cm"># Placeholder — to be updated after MICCAI submission</span>

<span class="ck">@dataset</span>{tnbc_tcga_2025,
  <span class="cv">title</span>    = {TNBC-TCGA Multi-Modal Dataset},
  <span class="cv">author</span>   = {[Authors]},
  <span class="cv">year</span>     = {2025},
  <span class="cv">source</span>   = {TCGA-BRCA via cBioPortal + NCI GDC},
  <span class="cv">patients</span> = {153},
  <span class="cv">slides</span>   = {150},
  <span class="cv">license</span>  = {CC BY 4.0}
}
    </div>
    <p style="margin-top:0.75rem; font-size:13px; color:var(--gray-400)">
      If you use this dataset, please also cite the original TCGA-BRCA publication and acknowledge the NCI Genomic Data Commons.
    </p>
  </section>

  <div class="footer">
    <span>TNBC-TCGA Dataset · TCGA-BRCA · NCI GDC · CC BY 4.0</span>
    <span>153 patients · 150 WSIs · 148 GB · 955 exclusions logged</span>
  </div>

</div>
</body>
</html>
