# Predicting Gene Essentiality with Multi-Omics + VAE→MLP (DDLS 2025)

**Short link to web app:** <https://ef6d0df6cefa38a8b9.gradio.live/>

This repo contains a reproducible pipeline to predict CRISPR gene effect (CERES) from multi-omics features using a Variational Autoencoder (VAE) for cell embeddings and an MLP head for per-gene regression.

## 🧪 What’s inside
- `src/` – training & eval scripts (VAE, MLP, metrics)
- `app/` – Gradio app (predicts CERES, labels Essential/Non-essential)
- `figs/` – PCA, UMAP, observed vs predicted, residuals
- `outputs/` – sample predictions CSVs
- `data/` – **small artifacts only** (e.g., `latents.parquet`, `metrics_test.json`, `mlp.pt` if <100MB)
- `DDLS2025_Final_Project_Report.docx` – final report (5 pages)

## 📊 Results (test cells)
- R²: **0.020**
- MAE: **0.244**
- Spearman ρ: **0.136**
- ROC-AUC: **0.630**
- PR-AUC: **0.152**

## 🔁 Reproduce (Colab)
Open in Colab and run end-to-end:
1. Mount Drive; clone or open this repo.
2. Place DepMap 24Q2 CSVs in your Drive (do **not** commit them); run `00_prepare.ipynb` or `src/prepare_data.py` to build aligned Parquets.
3. `src/train_vae.py` → produces `data/latents.parquet`.
4. `src/train_mlp.py` → trains `data/mlp.pt`.
5. `src/eval.py` → writes `data/metrics_test.json`, figures to `figs/`.

> Note: We **do not** redistribute DepMap raw files. Get them from the [DepMap Portal](https://depmap.org/portal/data_page/) and accept their terms.

## 🌐 Web app
Run locally or in Colab:
```bash
python app/gradio_app.py
