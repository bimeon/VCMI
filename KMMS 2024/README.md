# Detection of Chronic Inflammatory Bowel Disease Using YOLOv4 with Spatial Information from Continuous Slices

> **Published at:** Korea Multimedia Society Spring Conference 2024 (Vol. 27, No. 1)  
> **Authors:** SeoHyun Lee†, Hansang Lee†, Helen Hong* 
> *† Equal contribution · * Corresponding author*

---
## Paper

[KMMS 2024-Detection of Chronic Inflammatory Bowel Disease Using YOLOv4 with Spatial Information from Continuous Slices.pdf](https://github.com/user-attachments/files/27598506/KMMS.2024-Detection.of.Chronic.Inflammatory.Bowel.Disease.Using.YOLOv4.with.Spatial.Information.from.Continuous.Slices.pdf)

---

## Overview

This repository contains the implementation of a deep learning-based detection method for **chronic inflammatory bowel disease (Crohn's disease)** in MR Enterography (MRE) images. To enhance contextual understanding across slices and reduce false-positive detections, we propose a **multi-channel input strategy** that encodes three consecutive slices into the R, G, and B channels of a single image before training YOLOv4.


---

## Background

**Crohn's disease** is a chronic inflammatory condition affecting the gastrointestinal tract, causing inflammation and ulceration. Accurate assessment of disease activity is critical since disease progression leads to irreversible damage to the intestinal wall.

MR Enterography (MRE) is a non-invasive imaging modality used to evaluate small intestine and extra-intestinal lesions. Existing scoring methods such as **MaRIA** and **sMaRIA** require manual evaluation across six bowel segments, making them time-consuming and reader-dependent.

This work aims to automate inflammatory lesion detection from MRE images as a prerequisite step for automated disease activity scoring.

---

## Method

### Data Preprocessing

- **Dataset:** MRE images from 222 patients (Severance Hospital, March 2016 – December 2018)
- **Imaging phase:** Contrast-enhanced portal phase
- **Image resolution:** 512 × 512 × 3 pixels (pixel size: 0.78mm × 0.98mm, slice thickness: 3–4mm)
- Gaussian intensity normalization applied
- Inflammatory regions manually annotated by clinicians as bounding boxes

### Single-Slice Input (Method A)
<img width="1256" height="759" alt="image" src="https://github.com/user-attachments/assets/39e320ac-469c-4bc5-86e2-715e2ae16dc0" />

### Proposed Spatial Multi-Channel Input (Method B)
<img width="1226" height="724" alt="image" src="https://github.com/user-attachments/assets/f9d6f3af-ced8-4ac5-b9c9-529fe80370dc" />

To incorporate spatial context from neighboring slices, three consecutive slices are stacked into a single RGB image:

The ground-truth annotation is assigned based on the center (current) slice.

### Model Architecture: YOLOv4

| Component | Details |
|-----------|---------|
| Backbone  | CSPDarknet53 |
| Neck      | SPP (Spatial Pyramid Pooling) + PAN (Path Aggregation Network) |
| Head      | YOLOv3 (multi-scale feature maps) |
| Pretrained weights | `yolov4-tiny.conv.29` (MS COCO) |

**Training configuration:**
- Learning rate: 0.0015 – 0.00261
- Max training batches: 2,000 – 6,000
- Input resolution: 416 – 608 (multiples of 32)
- Framework: Darknet (Google Colab, Python)

---

## Experiments

### Dataset Split

| Split      | Patients | Slices |
|------------|----------|--------|
| Train      | 111      | 8,524  |
| Validation | 56       | 4,561  |
| Test       | 55       | 4,443  |
| **Total**  | **222**  | **17,528** |

### Comparison

| Method | Description |
|--------|-------------|
| **Method A** | Single-slice YOLOv4 (baseline) |
| **Method B** | Three-slice multi-channel YOLOv4 (proposed) |

---

## Results

### Quantitative Performance (mAP, Precision, Recall in %)

|       |   | mAP@10 | mAP@50 | Prec@10 | Prec@50 | Recall@10 | Recall@50 |
|-------|---|--------|--------|---------|---------|-----------|-----------|
| **Train** | A | 73.56 | 30.47 | 10.56 | 10.64 | 97.39 | 91.26 |
|       | B | 72.66 | 59.80 | **16.63** | **14.90** | 94.34 | 84.56 |
| **Valid** | A | 32.08 | 19.04 | 8.03 | 5.93 | 73.99 | 54.71 |
|       | B | 28.88 | 17.88 | **10.13** | **7.41** | 61.97 | 45.29 |
| **Test**  | A | 35.48 | 17.80 | 12.43 | 8.65 | 71.07 | 49.50 |
|       | B | 33.56 | 17.45 | **16.13** | **10.33** | 60.68 | 38.88 |

> mAP@10 / @50 denotes mean Average Precision at IoU threshold of 10% / 50%.

### Key Findings

- **Method B improves precision by up to 6.1%p** over Method A across splits.
- Method A frequently fails to detect lesions on intermediate slices (e.g., slice 31) while detecting them on adjacent slices (30 and 32), indicating a lack of cross-slice consistency.
- Method A also produces numerous **false positives** around lesion boundaries (e.g., slice 32).
- Method B achieves **consistent detection** across consecutive slices by encoding spatial context, effectively suppressing false positives that do not persist across slices.

---

## Qualitative Results

Detection examples on three consecutive slices (ground-truth: **green**, detection: **red**):
<img width="1305" height="897" alt="image" src="https://github.com/user-attachments/assets/ee699279-e4dc-40a9-8af3-67dc7f4214d8" />

---

## Conclusion

We proposed a **multi-channel YOLOv4-based detection method** that encodes three consecutive MRE slices into a single RGB image to capture spatial context across slices. Compared to the single-slice baseline, the proposed method:

- Improves **cross-slice detection consistency**
- Reduces **false positive detections**
- Achieves up to **6.1%p improvement in precision**

---

## Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{lee2024crohns,
  title     = {Detection of Chronic Inflammatory Disease Incorporating Spatial Information of Continuous Slices Based on YOLOv4},
  author    = {Lee, Seo-Hyun and Lee, Hansang and Hong, Helen},
  booktitle = {Korea Multimedia Society Spring Conference},
  volume    = {27},
  number    = {1},
  year      = {2024}
}
```

---

## Acknowledgements

This work was supported by the SW-oriented University Project at Seoul Women's University (2024). MR data was provided by Prof. Jun-Seok Lim and Prof. Suni Eun, Department of Radiology, Severance Hospital.
