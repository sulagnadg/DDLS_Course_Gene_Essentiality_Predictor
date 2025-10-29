# 🧬 Predicting Gene Essentiality Using Multi-Omics Features and Deep Learning (DDLS 2025)

**Author:** *Sulagna dasgupta*  
**Course:** Data-Driven Life Science (DDLS 2025) 

---

## 🚀 Overview
This project predicts gene essentiality (CRISPR CERES scores) for human cancer cell lines using public multi-omics data from the **Broad Institute DepMap (24Q2)**.  
A **Variational Autoencoder (VAE)** learns compact cell-level embeddings from gene expression data, and a **Multilayer Perceptron (MLP)** predicts CERES values from these embeddings combined with copy-number variation (CNV) and mutation features.

The full workflow is implemented in Python and can be run in **Google Colab** or locally.  
A **Gradio web app** provides an accessible interface for gene-level predictions and essential/non-essential classification.

---

## 📂 Repository Contents
| File / Folder | Description |
|----------------|-------------|
| `DDLS Final Project.ipynb` | Complete Jupyter notebook — preprocessing → training → evaluation → figures. |
| `DDLS2025_Final_Project_Report.docx` | 5-page final report following DDLS format (Abstract, Methods, Results, FAIR data). |
| `latent_pca2.png`, `latent_umap.png` | PCA and UMAP visualizations of VAE latent embeddings. |
| `obs_vs_pred_test.png`, `residual_hist_test.png` | Evaluation figures for MLP predictions. |
| `latents.parquet` | 32-dimensional latent embeddings (VAE output). |
| `metrics_test.json` | Quantitative performance metrics (R², MAE, Spearman ρ, ROC-AUC, PR-AUC). |
| `mlp.pt` | Trained MLP model checkpoint. |
| `requirements.txt` | Python dependencies for reproducibility. |
| `LICENSE` | MIT open-source license. |
| `README.md` | Project overview and instructions. |

---

## 📊 Key Results
| Metric | Value |
|:--------|:------:|
| R² | **0.020** |
| MAE | **0.244** |
| Spearman ρ | **0.136** |
| ROC-AUC | **0.630** |
| PR-AUC | **0.152** |

**Interpretation:**  
Despite modest absolute accuracy, the model captured biologically meaningful patterns — core essential genes (e.g. *PCNA*, *RPA1*, *POLR2A*) consistently had CERES < −1, while oncogenes (*KRAS*, *MYC*, *BRAF*) showed cell-specific dependencies.

---

## 🧠 Method Summary
### 1. Data
DepMap Public 24Q2:
- Expression (`OmicsExpressionProteinCodingGenesTPMLogp1.csv`)
- Copy Number (`OmicsCNGene.csv`)
- Mutation (`OmicsSomaticMutationsProfile.csv`)
- Gene Effect (`CRISPRGeneEffect.csv`)
- Profile mapping (`OmicsProfiles.csv`)

### 2. Model Workflow
1. **VAE** (32-D latent) trained on log-TPM expression.  
   - Encoder: 1024 → 256 → μ/log σ  
   - Loss: MSE + KLD  
2. **MLP** regression head:  
   - Inputs: `[Z_cell, expr_g, cnv_g, mut_g]`  
   - Architecture: 256 → 64 → 1 (GELU, dropout 0.2)  
   - Loss: Smooth L1  
3. **Evaluation:** 80/20 cell-line split; R², MAE, ρ, ROC-AUC, PR-AUC.

---

## 💻 Reproducibility
Run end-to-end in Colab:

```bash
# Mount Drive and navigate
from google.colab import drive
drive.mount('/content/drive')
%cd "/content/drive/MyDrive/DDLS final project/ddls-essentiality-repo"

# Install dependencies
!pip install -r requirements.txt

# Open and execute notebook
