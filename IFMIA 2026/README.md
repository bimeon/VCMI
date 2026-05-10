# Beyond CLIP: Clinical Knowledge-Guided Vision-Language Fusion for Prostate Cancer Aggressiveness Prediction

> **IFMIA 2026** (International Forum on Medical Imaging in Asia) — January 12–14, 2026

**Hansang Lee<sup>1</sup>, Seohyun Lee<sup>1</sup>, Sung Il Hwang<sup>2</sup>, Helen Hong<sup>1</sup>**

<sup>1</sup>Department of Software Convergence, Seoul Women's University, Seoul, South Korea  
<sup>2</sup>Department of Radiology, Seoul National University Bundang Hospital, Seongnam, South Korea

📧 Corresponding Author: [hlhong@swu.ac.kr](mailto:hlhong@swu.ac.kr)

---

## 📋 Overview

<img width="1280" height="720" alt="슬라이드1" src="https://github.com/user-attachments/assets/1887f1e1-13c9-43d5-90de-cdd57d27bfbb" />

We propose a novel vision-language fusion framework that leverages a medical Vision-Language Model (VLM) to generate **Clinical Pseudo-Reports (CliPR)** — structured, clinically-grounded text descriptions synthesized directly from bi-parametric MRI — and fuses them with image features to improve prostate cancer aggressiveness prediction (Gleason Score grading).

---

## 🔍 Background

<img width="1280" height="720" alt="슬라이드2" src="https://github.com/user-attachments/assets/4e25671a-7370-4b34-bfd5-de24e12b770b" />

<img width="1280" height="720" alt="슬라이드3" src="https://github.com/user-attachments/assets/48da434a-9133-455e-8787-e6fe9f5f288a" />

Bi-parametric MRI (bpMRI), combining **anatomical (T2W)** and **functional (DWI, ADC)** sequences, is the standard modality for assessing Prostate Cancer (PCa) aggressiveness via Gleason Score.

Conventional deep learning approaches focus solely on image-to-image fusion, **neglecting high-level diagnostic reasoning** as captured in clinical reports (PI-RADS guidelines). While fusing images with expert-written radiology reports is a known solution, curating large-scale paired image-report datasets is **prohibitively expensive and difficult to scale**.

---

## 💡 Purpose

<img width="1280" height="720" alt="슬라이드4" src="https://github.com/user-attachments/assets/7cf8028f-deb0-45be-accb-0c64a6e3ff2c" />

Instead of relying on scarce human-written reports, we:
- **Synthesize structured clinical interpretations** directly from bpMRI using a VLM
- Reframe the VLM as a **"Clinical Interpreter"**, generating structured **Clinical Pseudo-Reports (CliPR)** on the fly
- Fuse CliPR embeddings with image features for classification

---

## 🏗️ Method Overview

<img width="1280" height="720" alt="슬라이드5" src="https://github.com/user-attachments/assets/cdb2e084-acfe-4fea-977b-e745c86d8d33" />

The proposed framework consists of **three stages**:

### Stage 1: Image Encoding
Each MRI modality (T2W, DWI, ADC) is encoded by an independent **ResNet-34** encoder (frozen), producing per-modality image feature embeddings.

### Stage 2: CliPR Generation

<img width="1280" height="720" alt="슬라이드7" src="https://github.com/user-attachments/assets/ef9d86cb-45aa-4b0c-b901-f401de88b113" />

MedGemma-2 is prompted through a **structured Q&A process** based on clinical PI-RADS v2.1 guidelines:

- **Step 1 — Image Description:** The VLM answers clinically-grounded questions (lesion location, signal intensity, margin, EPE existence, etc.)
- **Step 2 — Rule-based Scoring:** A second prompt reads the generated description and applies PI-RADS scoring rules to output a predicted PI-RADS score

The resulting text is encoded by **BioGPT** (1024-dim, mean pooling) into a CliPR embedding.

### Stage 3: Image-CliPR Fusion

<img width="1280" height="720" alt="슬라이드10" src="https://github.com/user-attachments/assets/c055cc31-6008-489b-9fc5-ada677e9fafe" />

The CliPR embedding interacts with the fused image representations via **Cross-Modality Attention (CMA)** and **Hierarchical CMA (H-CMA) Fusion**:
- **Primary Modality Selection**: Dynamically selects the dominant imaging modality
- **Hierarchical CMA Fusion**: Text-informed attention reweights and fuses all modalities

---

## 🧪 Experiments

### Dataset

<img width="1280" height="720" alt="슬라이드11" src="https://github.com/user-attachments/assets/fcd4ed7b-8add-48fa-9236-679bcba68cc7" />

- **Seoul National University Bundang Hospital Dataset** (246 patients), IRB-approved
- 2D prostate cancer MR images: T2wMR, DWI, ADC map
- 180×180 tumor-centered cropped patches
- Labels: **lPCa** (low-grade; Gleason Score ≤ 3+4) and **hPCa** (high-grade; Gleason Score ≥ 4+3)

### Implementation

<img width="1280" height="720" alt="슬라이드12" src="https://github.com/user-attachments/assets/6073812b-bfc8-4f78-967e-769f8d887faf" />

---

## 📊 Results

### GS Prediction Performance

<img width="1280" height="720" alt="슬라이드13" src="https://github.com/user-attachments/assets/5049a20a-43a4-4e6f-8505-df1be70b65f4" />

Adding VLM-generated CliPR to the image fusion model **significantly improves performance**, particularly boosting **specificity** and **suppressing false positives** for high-grade cancer.

### CliPR at Work (Qualitative Example)

<img width="1280" height="720" alt="슬라이드14" src="https://github.com/user-attachments/assets/d16c8e72-73e1-4142-beaa-016dc1eb80a6" />

A case where the image-only model misclassified a **PI-RADS 5 / Low-grade** lesion as high-grade. The CliPR correctly identified low-risk features (transition zone location, no EPE suspected), guiding the model to the correct prediction.

### CliPR as a Structural Prior (SHAP Analysis)

<img width="1280" height="720" alt="슬라이드15" src="https://github.com/user-attachments/assets/94bb9cc2-8f0d-44b6-8351-75f64ce342fb" />

SHAP analysis reveals that:
- **Without CliPR**: The model's decision is dominated by the ADC map (functional information)
- **With CliPR**: The model rebalances attention, increasing the importance of T2W and DWI (anatomical and structural information) — more aligned with expert clinical reasoning

---

## ✅ Conclusion

<img width="1280" height="720" alt="슬라이드16" src="https://github.com/user-attachments/assets/5f95dea4-cbe8-4c8a-94bb-df86f1b41737" />

- We proposed using a VLM as a **"Clinical Interpreter"** to generate Clinical Pseudo-Reports (CliPR), overcoming the data scarcity bottleneck
- PI-RADS-guided prompting elicits **structured, clinically-relevant reasoning**, not just generic captions
- Future work involves detecting and rectifying **hallucinations** within VLM-generated CliPR to further enhance diagnostic reliability and prediction accuracy

---

## 🙏 Acknowledgments

This work was supported by the National Research Foundation of Korea (NRF) grant funded by the Korea government (MSIT). (No. RS-2023-00207947, RS-2025-00520184)

---

## 👥 Authors

<img width="1280" height="720" alt="슬라이드17" src="https://github.com/user-attachments/assets/61c0c674-d518-4cfe-a22e-c70986a3a267" />

| | Name | Affiliation | Contact |
|---|---|---|---|
| | Hansang Lee | Seoul Women's University | hansanglee@swu.ac.kr |
| | **Seohyun Lee** | Seoul Women's University | seohyunlee@swu.ac.kr |
| | Sung Il Hwang | Seoul National University | hwangsi49@gmail.com |
| ✉️ | Helen Hong *(Corresponding)* | Seoul Women's University | hlhong@swu.ac.kr |

---

## 📄 Citation

If you find this work useful, please consider citing:

```bibtex
@inproceedings{lee2026beyondclip,
  title     = {Beyond CLIP: Clinical Knowledge-Guided Vision-Language Fusion for Prostate Cancer Aggressiveness Prediction},
  author    = {Hansang Lee and Seohyun Lee and Sung Il Hwang and Helen Hong},
  booktitle = {Proceedings of the International Forum on Medical Imaging in Asia (IFMIA)},
  year      = {2026}
}
```

