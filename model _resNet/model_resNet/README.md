# 🧬 TNBC Whole Slide Image (WSI) Deep Learning & Morphological Analysis Pipeline

## 📌 Overview

This repository provides a complete pipeline for **Triple-Negative Breast Cancer (TNBC)** analysis using TCGA Whole Slide Images (WSIs).

It integrates:

* 📥 Dataset preparation (TCGA-derived TNBC cohort)
* 🔗 WSI download automation (wget manifests)
* 🧩 Patch extraction from WSIs
* 🧠 Deep feature extraction (ResNet-50)
* 📊 Unsupervised clustering & outlier detection
* 📉 Embedding visualization & morphological discovery

The goal is to transform raw histopathology slides into **structured machine-learning representations** for cancer research.

---

## 📂 Repository Structure

```text id="repo_structure"
model_resNet/
│
├── Data set/
│   ├── patches.zip                 # Extracted WSI patches (256×256)
│   ├── TNBC_Supplementary.zip      # Additional dataset files
│   ├── Derived_TNBC_27.csv         # Selected TNBC patient metadata
│   └── TNBC_27_wget.txt            # TCGA WSI download links
│
├── Results/
│   ├── embeddings.npy             # PCA/ResNet feature embeddings
│   ├── clusters.npy               # HDBSCAN cluster assignments
│   ├── outliers.npy               # Detected abnormal patches
│   ├── umap.npy                   # 2D visualization space
│   └── results.csv                # Final summary table
│
├── Scripts/
│   └── discovering-morphological-outliers.ipynb
│
└── README.md
```

---

## 📊 Dataset Description

### 🧾 Derived_TNBC_27.csv

Contains:

* Selected **27 TNBC patients**
* Clinical metadata from TCGA-BRCA
* Filtering-based cohort selection
* Used for downstream analysis and patch mapping

---

### 🔗 TNBC_27_wget.txt

Contains:

* Direct download links for WSIs
* Compatible with `wget` or GDC tools
* Maps each patient to corresponding slide files

### ▶️ Download WSIs:

```bash
wget -i model_resNet/Data set/TNBC_27_wget.txt
```

---

## 🧩 Patch Extraction Pipeline

### 🔧 Key Settings:

* Patch size: **256 × 256**
* Sampling: Random tissue-based extraction
* Patches per slide: **300**

### 🧠 Tissue Filtering:

```python id="tissue_filter"
def is_tissue(img):
    gray = np.array(img.convert("L"))
    return gray.mean() < 220
```

Removes:

* Background regions
* Non-informative white areas

---

## 🧠 Deep Feature Extraction (ResNet-50)

A ResNet-50 model is used as a **feature encoder**:

```python id="resnet"
model = models.resnet50(weights=None)
model = torch.nn.Sequential(*list(model.children())[:-1])
```

### Output:

* 2048-dimensional embeddings per patch
* No classification head (pure representation learning)

---

## 📉 Dimensionality Reduction

### PCA:

```python
PCA(n_components=50)
```

Used for:

* Noise reduction
* Compact feature representation

---

## 🔍 Unsupervised Learning

### 📌 HDBSCAN Clustering:

```python
HDBSCAN(min_cluster_size=30)
```

* Detects natural tissue groups
* No predefined cluster count required
* Handles noisy patches automatically

---

### 🚨 Outlier Detection:

```python
IsolationForest(contamination=0.05)
```

Detects:

* Blurry patches
* Background artifacts
* Failed extraction regions

---

## 🧭 Visualization (UMAP)

```python
UMAP(n_neighbors=15, min_dist=0.1)
```

Produces:

* 2D embedding space
* Visual clustering of tissue morphology

Each point represents a **histopathology patch**

---

## 📦 Results Directory

All outputs are stored in:

```text id="results_dir"
Results/
```

### Files:

| File           | Description          |
| -------------- | -------------------- |
| embeddings.npy | PCA/ResNet features  |
| clusters.npy   | Cluster labels       |
| outliers.npy   | Detected anomalies   |
| umap.npy       | 2D visualization     |
| results.csv    | Final analysis table |

---

## 📊 Scientific Insights

This pipeline enables:

* 🧬 Discovery of morphological tissue patterns
* 🧠 Unsupervised cancer phenotype grouping
* 🔬 Detection of abnormal histology patches
* 📉 Dimensional structure analysis of TNBC

---

## 🧪 Applications

* TNBC classification (weakly supervised learning)
* Multiple Instance Learning (MIL)
* Computational pathology research
* Slide-level cancer prediction
* Dataset cleaning & artifact removal

---

## ⚙️ Requirements

```bash
pip install openslide-python torch torchvision numpy scikit-learn umap-learn hdbscan matplotlib pillow pandas
```

System dependency:

```bash
sudo apt-get install openslide-tools
```

---

## 🚀 How to Run

### 1. Download WSIs

```bash
wget -i model_resNet/Data set/TNBC_27_wget.txt
```

### 2. Extract patches

```bash
python Scripts/extract_patches.py
```

### 3. Generate embeddings

```bash
python Scripts/discovering-morphological-outliers.ipynb
```

---

## 👨‍🔬 Author
**Jalal Ibrahim**  
**TNBC Medical AI Research Pipeline**
Focus:

* Histopathology AI
* Deep Learning (ResNet, MIL)
* Unsupervised representation learning
* Cancer morphology analysis

---

