# Paper 4: Semi-Supervised, Attention-Based Deep Learning for Predicting TMPRSS2:ERG Fusion Status in Prostate Cancer Using Whole Slide Images

## Citation

**Authors:** Mohamed Omar, Zhuoran Xu, Sophie B. Rand, Mohammad K. Alexanderani, Daniela C. Salles, Itzel Valencia, Edward M. Schaeffer, Brian D. Robinson, Tamara L. Lotan, Massimo Loda, Luigi Marchionni

**Journal:** Molecular Cancer Research

**Year:** 2024

**DOI:** 10.1158/1541-7786.MCR-23-0639

---

## Research Question

Can a semi-supervised, attention-based deep learning model predict TMPRSS2:ERG fusion status directly from routine H&E whole-slide images without requiring pixel-level annotations?

---

## Motivation

TMPRSS2:ERG fusion is one of the most common genomic alterations in prostate cancer, occurring in approximately 40–50% of cases. Detecting this fusion typically requires molecular assays such as FISH, RT-PCR, or immunohistochemistry (IHC), which may not always be available, particularly in resource-limited settings. The authors investigate whether the morphological information contained in routine H&E slides is sufficient for predicting this molecular alteration using deep learning.

---

## Contribution

The paper presents a semi-supervised, attention-based deep learning framework capable of predicting TMPRSS2:ERG fusion directly from whole-slide images.

Its major contributions include:

- Development of an attention-based Multiple Instance Learning (MIL) model using CLAM.
- Prediction using only slide-level labels (no pixel-level annotations).
- Validation on an independent external cohort from another institution.
- Biological interpretation of model attention using HoVer-Net nuclei segmentation.
- Demonstration that the cellular composition of highly attended regions is associated with patient survival.

---

## Dataset

- **Dataset(s):**
  - TCGA Prostate Adenocarcinoma (TCGA-PRAD)
  - Johns Hopkins Natural History Cohort

- **Number of slides/images:**
  - TCGA:
    - 436 whole-slide images
    - 393 patients
  - External cohort:
    - 314 whole-slide images
    - 314 patients

- **Cancer type / tissue:**
  - Prostate adenocarcinoma
  - Radical prostatectomy specimens
  - H&E-stained whole-slide images

- **Annotation type:**
  - Slide-level TMPRSS2:ERG fusion status
  - No pixel-level annotations

---

## Method

### Overall Pipeline

1. Whole-slide image preprocessing.
2. Tissue segmentation.
3. Extraction of 2048 × 2048 image patches.
4. Downsampling to 512 × 512 patches.
5. Feature extraction using pretrained ResNet-50.
6. Attention-based Multiple Instance Learning (CLAM).
7. Slide-level prediction of TMPRSS2:ERG fusion.
8. Extraction of top 15 highest-attention patches.
9. HoVer-Net segmentation and nuclei classification.
10. Survival analysis based on cellular composition.

---

### Model Architecture

The framework consists of:

- Preprocessing and tissue segmentation.
- Pretrained ResNet-50 feature extractor.
- CLAM (Clustering-constrained Attention Multiple Instance Learning).
- Attention pooling for slide representation.
- Two slide-level classifiers:
  - ERG positive
  - ERG negative

Unlike conventional MIL, CLAM learns which image regions deserve the highest attention during prediction.

---

### Training Strategy

- Weak / semi-supervised learning.
- Slide-level supervision only.
- Cross-entropy loss.
- Adam optimizer.
- Learning rate = 0.0001.
- Weight decay = 0.00001.
- Maximum 150 epochs.
- Early stopping after 20 epochs without validation improvement.
- Ten independent train/validation/test splits.

---

### Inference

During inference:

1. Each whole-slide image is divided into patches.
2. ResNet-50 extracts features from every patch.
3. CLAM assigns an attention score to every patch.
4. Highly weighted patches determine the final slide prediction.
5. The top 15 attention patches are further analyzed using HoVer-Net to segment and classify nuclei.

---

## Results

### Main Findings

The best-performing model achieved:

**Training**

- AUC: 0.84
- Accuracy: 0.77

**TCGA Test Set**

- AUC: 0.72
- Accuracy: 0.70

**Independent External Cohort**

- AUC: 0.73
- Accuracy: 0.69

Biological interpretation showed that:

- ERG-positive slides contained more neoplastic nuclei.
- ERG-negative slides showed more stromal and inflammatory cells.
- The cellular composition of the highly attended regions predicted progression-free, overall, and metastasis-free survival.

---

## Key Takeaways

- Weakly supervised learning enables molecular prediction directly from routine H&E slides.
- Attention mechanisms identify biologically meaningful regions without manual annotation.
- HoVer-Net is used as a downstream analysis tool rather than the primary prediction model.
- Highly attended tissue regions contain prognostic cellular information.
- Combining attention-based MIL with nuclei segmentation improves interpretability.
- This work demonstrates how computational pathology can connect tissue morphology with genomic alterations and clinical outcomes.
