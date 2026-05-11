# Detection of Inflammatory Bowel Diseases in MRE Images Using Multi-Slice Multi-Label Spatial Context Learning

> **Korea Computer Graphics Society** | Vol. 31, No. 1, pp. 15–23 | DOI: [10.15701/kcgs.2025.31.1.15](https://doi.org/10.15701/kcgs.2025.31.1.15)

**Hansang Lee¹\***, **Seohyun Lee¹\***, Nieun Seo², Joonseok Lim², Helen Hong¹†

¹ Department of Software Convergence, Seoul Women's University  
² Department of Radiology, Yonsei University College of Medicine  
\* Co-first authors &nbsp; † Corresponding author: hlhong@swu.ac.kr

---

## Overview

Crohn's disease is a chronic inflammatory bowel disease (IBD) requiring precise localization of inflamed regions in MR Enterography (MRE) images for diagnosis and severity assessment. Conventional deep-learning approaches analyze each MRE slice independently, missing the spatial continuity that exists across consecutive slices.

This paper proposes **Multi-Slice Multi-Label (MSML)**, a method that integrates cross-slice spatial context into YOLOv4 by:

1. **Multi-channel input** — combining three consecutive slices (m−s, m, m+s) as a 3-channel image.
2. **Frequency-based labeling** — assigning labels (1, 2, or 3) to each inflammatory bounding box based on how many of the three slices share the same lesion, directly encoding spatial persistence.

This design improves detection consistency across slices and reduces false positive detections compared to single-slice baselines.

---

## Key Results

| Method | All GT mAP@10 (%) | Label 3 GT mAP@10 (%) |
|---|---|---|
| SSSL – Baseline | 68.83 | 68.15 |
| MSSL (s=1) | 63.60 | 62.06 |
| MSSL (s=2) | 65.30 | 62.67 |
| MSSL (s=3) | 66.95 | 63.22 |
| **MSML (s=1)** | **69.27** | 71.01 |
| MSML (s=2) | 67.45 | 65.45 |
| **MSML (s=3)** | 67.65 | **74.18** |

- MSML (s=1) achieves the best overall performance on the full ground-truth set.
- MSML (s=3) achieves the best performance on Label 3 (large/severe lesions), suggesting wider spatial context benefits severe inflammation detection.
- MSML consistently reduces false positive detections vs. SSSL baseline.

---

## Method

### Data

- **208 patients** collected at Sinchon Severance Hospital (March 2016 – December 2018).
- Severity stratified by sMARIA score: normal (0), mild (0–8), severe (≥8) in a 28:105:75 ratio.
- **16,542 slices** with **7,800 bounding boxes** manually annotated by three radiologists.
- Train / Validation / Test split: **104 / 52 / 52 patients** (stratified).

### Preprocessing

Gaussian intensity normalization is applied to each MRE slice:

$$\mu = \frac{1}{N}\sum_{i=1}^{N} I(x_i, y_i), \quad \sigma = \sqrt{\frac{1}{N}\sum_{i=1}^{N}(I(x_i,y_i)-\mu)^2}$$

$$I_{\text{normalized}}(x,y) = \frac{I(x,y)-\mu}{\sigma}, \quad I_{\text{scaled}}(x,y) = \frac{I_{\text{normalized}}(x,y)-\min}{\max-\min}\times 255$$

### MSML Data Generation

Three input/label strategies are compared:

| Strategy | Input | Label |
|---|---|---|
| **SSSL** | Single slice | Single label |
| **MSSL** | 3 consecutive slices (3-ch) | Label from center slice only |
| **MSML** (proposed) | 3 consecutive slices (3-ch) | Label = occurrence count across 3 slices (1, 2, or 3) |

Label distribution in the dataset (s=1): Label 1 : Label 2 : Label 3 ≈ **1 : 7 : 9**

### Model

**YOLOv4** with:
- **Backbone**: CSPDarknet53 (CSPNet-based)
- **Neck**: SPP (Spatial Pyramid Pooling) + PAN (Path Aggregation Network)
- **Head**: YOLOv3-style multi-scale detection

| Setting | SSSL / MSSL | MSML |
|---|---|---|
| Classes | 1 | 3 (labels 1, 2, 3) |
| Filters | 18 | 24 |
| NMS | DIoU-NMS | DIoU-NMS |
| Max batches | 6000 | 6000 |
| Learning rate | 0.0015 | 0.0015 |
| Pretrained weights | yolov4-tiny.conv.29 (MS COCO) | same |

---

## Evaluation

Two evaluation protocols are used:

- **All GT**: All ground-truth bounding boxes, MSML labels collapsed to a single class.
- **Label 3 GT**: Only bounding boxes present in all three consecutive slices (large, persistent lesions) — clinically important for severity scoring.

A **modified mAP** is used: all predicted boxes exceeding the IoU threshold are counted as true positives (not just the highest-confidence one), better reflecting cross-slice detection consistency.

---

## Qualitative Analysis

- **Slice consistency**: SSSL misses lesions in some slices while correctly detecting the same lesion in adjacent slices. MSML detects the same lesion consistently across all consecutive slices.
- **False positive reduction**: SSSL generates false positives in subtly thickened bowel-wall regions due to appearance similarity with true inflammation. MSML suppresses these by leveraging cross-slice context.

---

## Environment

| Component | Version |
|---|---|
| Python | 3.10.12 (training) / 3.11.7 (preprocessing) |
| Framework | [Darknet](https://github.com/AlexeyAB/darknet) |
| CUDA | 12.2 |
| cuDNN | 8.9.6 |
| Platform | Google Colab (training) / Jupyter Notebook 7.0.8 (preprocessing) |

---

## Citation

If you find this work useful, please cite:

```bibtex
@article{lee2025msml,
  title     = {Detection of Inflammatory Bowel Diseases in MRE Images Using
               Multi-Slice Multi-Label Spatial Context Learning},
  author    = {Lee, Hansang and Lee, Seohyun and Seo, Nieun and Lim, Joonseok and Hong, Helen},
  journal   = {Korea Computer Graphics Society},
  volume    = {31},
  number    = {1},
  pages     = {15--23},
  year      = {2025},
  doi       = {10.15701/kcgs.2025.31.1.15}
}
```

---

## Acknowledgements

This research was supported by the National Research Foundation of Korea (NRF) funded by the Ministry of Science and ICT (No. RS-2024-00336063 and No. RS-2023-00207947), and by the Seoul Women's University Research Grant (2025).

---

## Related Work

- [Lee et al., KMS 2024] Detection of chronic inflammatory disease incorporating spatial information of continuous slices based on YOLOv4.
- [Lee et al., RSNA 2024] Detection of inflammatory bowel diseases in MRE images with spatially context-aware YOLOv4.
