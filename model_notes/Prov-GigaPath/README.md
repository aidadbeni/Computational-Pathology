# Prov-GigaPath – Whole-Slide Pathology Foundation Model

**Paper:** *A Whole-Slide Foundation Model for Digital Pathology from Real-World Data*  
**Authors:** Xu et al.  
**Published:** Nature, 2024

---

## Overview

Prov-GigaPath is a foundation model for digital pathology designed to learn representations from entire whole-slide images (WSIs).

Unlike models that only learn representations from individual image patches, Prov-GigaPath models both:

- **local information** within individual tissue tiles
- **global information** across the entire whole-slide image

This is achieved using a two-stage architecture consisting of a **tile encoder** and a **slide encoder**.

The model was pretrained on a large real-world pathology dataset called **Prov-Path**.

---

## Main Objective

The goal of Prov-GigaPath is to create a general-purpose pathology foundation model capable of understanding gigapixel whole-slide images.

Whole-slide images can contain tens of thousands of image tiles. Many previous approaches analyze individual tiles or only sample part of the slide, which can result in the loss of important global tissue information.

Prov-GigaPath addresses this by:

- learning representations of individual pathology tiles
- modelling relationships between tiles across the entire slide
- capturing both local and global morphological patterns
- producing reusable whole-slide representations for different downstream pathology tasks

---

## Architecture

Prov-GigaPath consists of two main components:

1. **Tile Encoder**
2. **Slide Encoder**

The overall architecture is:

```text
Whole-Slide Image (WSI)
        ↓
Divide into 256 × 256 tiles
        ↓
Tile Encoder
        ↓
Tile embeddings
        ↓
Slide Encoder (LongNet)
        ↓
Contextualized tile embeddings
        ↓
Aggregation
        ↓
Slide-level representation
        ↓
Downstream task
```

### Tile Encoder

The tile encoder processes each image tile independently and converts it into a feature embedding.

It uses a **Vision Transformer (ViT)** architecture pretrained using **DINOv2 self-supervised learning**.

Its role is to capture **local morphological features**, such as tissue structures and cellular patterns.

```text
Image tile → Tile Encoder → Tile embedding
```

### Slide Encoder

After the tiles have been converted into embeddings, the sequence of tile embeddings is passed to the **slide encoder**.

The slide encoder is based on **LongNet**, a transformer architecture designed for extremely long sequences.

Its role is to model relationships between tiles across the whole slide and capture **global tissue context**.

```text
Tile embeddings
      ↓
   LongNet
      ↓
Contextualized tile embeddings
```

LongNet uses **dilated attention**, allowing the model to process very long sequences more efficiently than standard transformer self-attention.

This is important because a single WSI may contain tens of thousands of tiles.

---

## Pretraining

Prov-GigaPath uses a **two-stage self-supervised pretraining strategy**.

### Stage 1 – Tile-Level Pretraining

The tile encoder is pretrained using **DINOv2**.

Each pathology tile is treated as an individual training sample.

```text
Pathology tiles
      ↓
DINOv2 self-supervised learning
      ↓
Pretrained tile encoder
```

This stage teaches the model to recognize useful local morphological patterns without requiring manual labels.

### Stage 2 – Slide-Level Pretraining

After the tile encoder has been pretrained, it is used to generate embeddings for all tiles within each WSI.

These embeddings are then used to train the LongNet slide encoder.

The slide encoder is pretrained using **masked autoencoder (MAE) learning**.

```text
WSI
 ↓
Tiles
 ↓
Pretrained tile encoder
 ↓
Tile embeddings
 ↓
Masked Autoencoder + LongNet
 ↓
Pretrained slide encoder
```

During this stage, the tile encoder is frozen while the slide encoder learns relationships between tile representations across the entire slide.

---

## Pretraining Dataset

Prov-GigaPath was pretrained on **Prov-Path**, a large real-world pathology dataset collected from the Providence health network.

The dataset contains:

- **171,189 whole-slide images**
- approximately **1.3 billion image tiles**
- tiles of size **256 × 256 pixels**
- more than **30,000 patients**
- **31 major tissue types**
- data collected across **28 cancer centers**

The dataset includes both:

- H&E-stained slides
- immunohistochemistry (IHC) slides

Unlike datasets composed mainly of curated research samples, Prov-Path contains large-scale **real-world clinical pathology data**.

---

## Input and Output

### Input

The input is a **whole-slide pathology image**.

The WSI is first divided into 256 × 256 image tiles.

### Output

Prov-GigaPath produces contextualized representations of the slide that contain information about both individual tissue regions and their relationships across the whole WSI.

```text
Whole-Slide Image
       ↓
Prov-GigaPath
       ↓
Slide-level representation
```

These representations can then be used for different downstream pathology tasks.

---

## Downstream Applications

Prov-GigaPath was evaluated on **26 downstream prediction tasks**, consisting of:

- **9 cancer subtyping tasks**
- **17 pathomics tasks**

### Cancer Subtyping

The model can use WSI morphology to distinguish between different cancer subtypes.

Examples include subtyping cancers involving:

- lung
- breast
- kidney
- colorectal tissue
- brain
- ovarian tissue

### Pathomics

Prov-GigaPath was also evaluated on predicting molecular information from pathology images.

These tasks include predictions related to:

- gene mutations
- molecular biomarkers
- tumor mutation burden

The idea is that some molecular changes produce morphological patterns that may be detectable from histopathology images.

---

## Vision-Language Extension

The authors also explored combining Prov-GigaPath with pathology reports.

The slide representation was aligned with text representations generated from pathology reports using contrastive learning.

The general idea is:

```text
Whole-Slide Image
       ↓
Prov-GigaPath
       ↓
Slide embedding
       ↕
Contrastive learning
       ↕
Pathology report
       ↓
Text encoder
       ↓
Text embedding
```

This enables vision-language applications such as **zero-shot prediction**.

The paper demonstrates zero-shot experiments for:

- cancer subtyping
- gene mutation prediction

This part of the work shows that Prov-GigaPath could potentially be extended beyond image-only analysis toward multimodal pathology models.

---

## Evaluation

Prov-GigaPath was evaluated using data from both:

- Providence
- The Cancer Genome Atlas (TCGA)

It was compared with existing pathology foundation models including:

- HIPT
- CTransPath
- REMEDIS

Prov-GigaPath achieved state-of-the-art performance on **25 out of 26 evaluated tasks**.

It significantly outperformed the second-best method on **18 tasks**.

The results suggest that combining large-scale real-world pretraining data with whole-slide modelling improves performance across different computational pathology applications.

---

## Key Takeaways

- Prov-GigaPath is a **whole-slide pathology foundation model**.
- It is designed to model entire gigapixel WSIs rather than treating every image tile independently.
- It contains two major components: a **tile encoder** and a **slide encoder**.
- The tile encoder captures **local morphological information**.
- The slide encoder captures **global relationships across the WSI**.
- The tile encoder is pretrained using **DINOv2**.
- The slide encoder is based on **LongNet** and pretrained using masked autoencoding.
- LongNet uses **dilated attention** to efficiently process very long sequences of tile embeddings.
- Prov-GigaPath was pretrained on **Prov-Path**, containing approximately 1.3 billion tiles from 171,189 WSIs.
- It was evaluated on **26 downstream tasks**, including cancer subtyping and pathomics.
- Prov-GigaPath achieved state-of-the-art performance on **25 of the 26 evaluated tasks**.
- The model was also extended to vision-language learning using pathology reports.
- The main contribution of Prov-GigaPath is combining **local tile-level representation learning with global whole-slide context modelling**.
