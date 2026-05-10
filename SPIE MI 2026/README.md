# Primary Modality-Focused Hierarchical Fusion for Prediction of Prostate Cancer Aggressiveness in Bi-parametric MR Images

**Paper:** [https://doi.org/10.1117/12.3086018](https://doi.org/10.1117/12.3086018)

**Published at:** SPIE Medical Imaging 2026

---

## Authors

- **Seohyun Lee** — Department of Software Convergence, Seoul Women's University
- **Hansang Lee** — Department of Software Convergence, Seoul Women's University
- **Sung Il Hwang** — Department of Radiology, Seoul National University Bundang Hospital
- **Helen Hong** — Department of Software Convergence, Seoul Women's University

---

## Abstract

Accurate assessment of prostate cancer aggressiveness using bi-parametric MRI (bpMRI) is essential for effective treatment planning. Although conventional multimodal fusion methods integrate information from multiple modalities, they often lack explicit inter-modality alignment. Recent approaches such as Uniform Cross-Modality Attention (CMA) fusion address this limitation but fail to reflect the hierarchical diagnostic process in which clinicians prioritize specific modalities.

We propose a **Primary Modality-Focused Hierarchical CMA Fusion** framework for predicting prostate cancer aggressiveness from bpMRI. The framework consists of two key stages:

1. **Primary Modality Selection** — learnable modality weights identify the most informative modality across the dataset.
2. **Hierarchical CMA Fusion** — auxiliary modalities are aligned to the selected primary modality through a two-step hierarchical fusion process.

The proposed method achieved an **AUC of 0.70**, outperforming all baseline methods. Interpretability analyses (SHAP and Grad-CAM) showed consistent selection of ADC as the primary modality, in agreement with clinical intuition, and demonstrated robust tumor localization even under severe hemorrhagic artifact conditions.

**Keywords:** bpMRI, multimodal fusion, prostate cancer, aggressiveness prediction, SHAP

---

## Method Overview

<img width="1280" height="720" alt="슬라이드1" src="https://github.com/user-attachments/assets/731aef63-04d0-40e7-a956-65c20a0566be" />
<img width="1280" height="720" alt="슬라이드2" src="https://github.com/user-attachments/assets/2e295559-73fd-4ca8-a8a3-2d1092a38d92" />
<img width="1280" height="720" alt="슬라이드3" src="https://github.com/user-attachments/assets/c73492ca-9c37-4cd8-b3a0-248a87970542" />
<img width="1280" height="720" alt="슬라이드4" src="https://github.com/user-attachments/assets/bc444f98-5537-42c6-b8c0-435ed3efb2ad" />
<img width="1280" height="720" alt="슬라이드5" src="https://github.com/user-attachments/assets/4acc64c3-81b6-444f-b604-9c5be8461db4" />
<img width="1280" height="720" alt="슬라이드6" src="https://github.com/user-attachments/assets/569a508d-2e4c-4355-a4a1-8f49e8b4fce2" />
<img width="1280" height="720" alt="슬라이드7" src="https://github.com/user-attachments/assets/16c9d462-c02f-418f-a9dc-117c1da6b97e" />
<img width="1280" height="720" alt="슬라이드8" src="https://github.com/user-attachments/assets/a39df82a-13d8-4274-a758-2d61426aeaf2" />
<img width="1280" height="720" alt="슬라이드9" src="https://github.com/user-attachments/assets/dc0e9aea-71b4-430c-9bdc-dd7d4d71e71a" />
<img width="1280" height="720" alt="슬라이드10" src="https://github.com/user-attachments/assets/e594ec86-2a07-4609-af74-a620871f83ad" />
<img width="1280" height="720" alt="슬라이드11" src="https://github.com/user-attachments/assets/b82af512-0fa0-4b54-962f-17492aca10be" />
<img width="1280" height="720" alt="슬라이드12" src="https://github.com/user-attachments/assets/84634b46-6fd8-43f3-8a14-13efc6112625" />
<img width="1280" height="720" alt="슬라이드13" src="https://github.com/user-attachments/assets/bbb07ad7-3254-490d-9b4a-1402d32b6120" />
<img width="1280" height="720" alt="슬라이드14" src="https://github.com/user-attachments/assets/3aab013d-e27e-43e5-8144-9f0a6c5e8cd1" />
<img width="1280" height="720" alt="슬라이드15" src="https://github.com/user-attachments/assets/23807293-c24d-43e3-b4c6-8582b2fa0bd6" />
