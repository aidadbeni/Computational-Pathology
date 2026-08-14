# UNI – Universal Pathology Foundation Model

**Paper:** *Towards a General-Purpose Foundation Model for Computational Pathology*  
**Authors:** Chen et al.  
**Published:** Nature Medicine, 2024

---

## Overview

UNI is a general-purpose foundation model for computational pathology designed to learn transferable visual representations from histopathology images.

Instead of being trained for one specific pathology task, UNI is pretrained on a large and diverse collection of H&E-stained whole-slide images using self-supervised learning. The pretrained model can then be reused as an image encoder for different downstream pathology tasks.

---

## Main Objective

The goal of UNI is to create a general-purpose pathology image encoder that can:

- learn from large amounts of unlabeled histopathology data
- generalize across different tissues and diseases
- reduce the need to train separate feature extractors for every pathology task
- perform well even when only a small amount of labeled downstream data is available

---

## Architecture

UNI uses a **Vision Transformer Large (ViT-L/16)** as its backbone.

The basic architecture can be represented as:

```text
Histopathology image
        ↓
Image patches
        ↓
Patch embeddings
        ↓
ViT-L/16
        ↓
Feature representation / embedding
```

The image is divided into patches, which are converted into tokens and processed by the Vision Transformer.

Through self-attention, the transformer learns relationships between different regions of the tissue image and produces a feature representation describing its visual and morphological characteristics.

UNI mainly acts as an **image encoder**, rather than directly producing a diagnosis.

---

## Pretraining

UNI was pretrained using **DINOv2**, a self-supervised learning framework.

This allows the model to learn useful representations from histopathology images without requiring manual labels during pretraining.

DINOv2 uses techniques including:

- **self-distillation**, where a student network learns from a teacher network
- **masked image modeling**, where parts of an image are masked during training

This encourages the model to learn robust visual and morphological features from the tissue images.

---

## Pretraining Dataset

UNI was trained on **Mass-100K**, a large histopathology dataset containing:

- 100,426 whole-slide images
- more than 100 million tissue patches
- approximately 77 TB of image data
- 20 major tissue types

The dataset contains normal, cancerous, and other pathological tissue samples.

The large scale and tissue diversity are important for making UNI generalizable across different pathology applications.

---

## Input and Output

### Input

A histopathology image region / tissue patch.

### Output

A numerical **feature embedding** representing the visual and morphological information contained in the image.

```text
Image patch → UNI → Feature embedding
```

The embedding can then be passed to another model for a specific downstream task.

---

## Downstream Applications

UNI was evaluated across **34 computational pathology tasks**.

The learned representations can be used for tasks such as:

- ROI-level image classification
- whole-slide image classification
- tissue segmentation
- image retrieval
- few-shot classification

For whole-slide analysis, the general pipeline is:

```text
Whole-slide image
        ↓
Patch extraction
        ↓
UNI
        ↓
Patch embeddings
        ↓
Aggregation / downstream model
        ↓
Slide-level prediction
```

Therefore, UNI can serve as the feature extraction component of a larger computational pathology pipeline.

---

## Evaluation

The authors evaluated UNI across multiple datasets, organs, diseases, and task types.

UNI was compared with other pretrained image encoders, including pathology-specific models such as **CTransPath** and **REMEDIS**, as well as models pretrained on natural images.

UNI generally showed strong performance across the evaluated tasks, particularly in transfer learning and data-efficient settings.

The results demonstrate that large-scale and diverse self-supervised pretraining can produce representations that generalize across many computational pathology problems.

---

## Key Takeaways

- UNI is a **foundation model for computational pathology**.
- It is primarily an **image encoder**, not a task-specific classifier.
- It uses a **ViT-L/16** architecture.
- It is pretrained using **DINOv2 self-supervised learning**.
- It was trained on **Mass-100K**, containing more than 100 million histopathology patches.
- UNI transforms pathology images into reusable **feature embeddings**.
- These embeddings can be used for many downstream tasks without retraining the foundation model from scratch.
- The model was evaluated across **34 downstream computational pathology tasks**.
- Its main advantage is the ability to transfer learned pathology representations across different tissues, diseases, and tasks.
