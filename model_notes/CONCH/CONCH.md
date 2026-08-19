# CONCH

**Paper:** A Visual-Language Foundation Model for Computational Pathology  
**Authors:** Ming Y. Lu, Bowen Chen, Drew F. K. Williamson, et al.  
**Published:** Nature Medicine, 2024

## Overview

CONCH (CONtrastive learning from Captions for Histopathology) is a visual-language foundation model developed specifically for computational pathology.

Unlike vision-only pathology foundation models, CONCH learns jointly from histopathology images and natural-language descriptions. This allows the model to connect visual tissue morphology with pathological concepts expressed in text.

The model was pretrained in a task-agnostic manner using diverse histopathology data, including more than 1.17 million image-caption pairs.

The resulting model can be transferred to several downstream tasks with little or no task-specific training.

---

## Motivation

Most computational pathology models are trained for a specific task, such as:

- cancer classification
- tumor grading
- metastasis detection
- molecular alteration prediction

This approach requires large amounts of labeled pathology data, which are expensive and time-consuming to obtain.

Another limitation is that most pathology models use only images, even though natural language is central to pathology through reports, textbooks, publications, and descriptions of histological findings.

CONCH addresses these limitations by learning a shared representation between histopathology images and text.

---

## Pretraining Data

CONCH was trained using histopathology images and corresponding textual descriptions collected from multiple sources.

The authors first created an unfiltered dataset of approximately **1.79 million image-text pairs**.

After filtering out non-human samples, the final CONCH pretraining dataset contained approximately:

**1.17 million human histopathology image-caption pairs.**

The dataset covers a wide variety of tissues, diseases, and pathological findings.

Examples include:

- gastrointestinal pathology
- lung
- skin
- liver
- hematopathology
- central nervous system
- breast
- kidney
- reproductive systems
- musculoskeletal pathology

The dataset contains both H&E images and images obtained using immunohistochemistry and special stains.

---

## Architecture

CONCH is based on the **CoCa (Contrastive Captioner)** visual-language pretraining framework.

It contains three main components:

### 1. Image Encoder

The image encoder receives a histopathology image and converts it into visual tokens.

A Vision Transformer (ViT) is used as the visual backbone.

The image passes through transformer blocks to produce representations describing its visual features.

An attention pooling mechanism is then used to obtain representations that can interact with the language components of the model.

---

### 2. Text Encoder

The corresponding pathology caption is tokenized and processed by a transformer-based text encoder.

The text encoder converts the caption into a representation in the same embedding space as the image representation.

This makes it possible to directly compare images and textual descriptions.

---

### 3. Multimodal Fusion Decoder

CONCH also contains a multimodal text decoder.

The decoder combines:

- visual information from the image encoder
- textual information from previously generated tokens

Cross-attention layers allow the decoder to attend to image features while processing language.

This component enables generative visual-language tasks such as image captioning.

---

## Pretraining Objectives

CONCH combines two major learning objectives.

### Contrastive Learning

The model learns to align matching images and captions in a shared embedding space.

For a correct image-caption pair, their representations are encouraged to have high cosine similarity.

Incorrect image-text combinations are pushed farther apart.

Conceptually:

Image → Image Encoder → Image Embedding

Caption → Text Encoder → Text Embedding

The model learns:

Matching image + caption → similar embeddings

Non-matching image + caption → dissimilar embeddings

This shared embedding space enables tasks such as zero-shot classification and image-text retrieval.

---

### Captioning Objective

CONCH is also trained to generate the caption associated with an image.

The multimodal decoder predicts the next text token based on:

- the image representation
- previously generated text tokens

Therefore, CONCH does not only learn whether an image and text description match. It also learns how visual pathology information relates to natural-language descriptions.

The combination of contrastive learning and caption generation follows the CoCa framework.

---


## Zero-Shot Classification

One important capability of CONCH is zero-shot classification.

Instead of training a new classifier for every pathology task, class names can be expressed as text prompts.

For example:

"An image of invasive ductal carcinoma."

"An image of invasive lobular carcinoma."

The image is encoded using the image encoder, while each text prompt is encoded using the text encoder.

The model compares their representations using similarity in the shared embedding space.

The class whose text representation is most similar to the image representation becomes the prediction.

This allows CONCH to perform classification without task-specific labeled training examples.

---

## Whole-Slide Image Classification

Whole-slide images are too large to process directly as a single image.

For WSI classification, the slide is divided into smaller image tiles.

Each tile is encoded independently by CONCH.

The similarity between each tile and the text prompts representing the possible classes is calculated.

Tile-level scores are then aggregated to obtain a slide-level prediction.

This approach also allows similarity heatmaps to be generated across the WSI, showing which regions contributed most strongly to a particular prediction.

---

## Downstream Tasks

The paper evaluates CONCH across 14 benchmarks involving several types of computational pathology tasks.

### Classification

CONCH supports:

- ROI-level classification
- whole-slide classification
- zero-shot classification
- few-shot classification
- supervised classification using pretrained image representations

### Image-Text Retrieval

Because images and text share the same representation space, CONCH can perform:

**Text-to-image retrieval**

A natural-language pathology description can be used to search for histology images with similar characteristics.

**Image-to-text retrieval**

A histology image can be used to retrieve relevant textual descriptions.

### Segmentation

The similarity between image regions and textual prompts can be used to localize particular pathological concepts.

This allows CONCH to perform coarse zero-shot tissue segmentation without directly training a segmentation model for every target class.

### Image Captioning

The multimodal decoder allows CONCH to generate textual descriptions of histopathology images.

---

## Evaluation

CONCH was evaluated across diverse pathology datasets and tasks.

It was compared with other visual-language models including:

- PLIP
- BiomedCLIP
- OpenAI CLIP

Across many of the evaluated classification, retrieval, and segmentation tasks, CONCH substantially outperformed these visual-language baselines.

The pretrained image encoder was also shown to provide strong representations for conventional supervised and few-shot learning.

---

## Key Idea

The central idea behind CONCH is to connect **histopathological morphology with natural language**.

Instead of learning only:

Image → Diagnosis

CONCH learns a more general relationship:

Image ↔ Pathology Language

This shared visual-language representation allows the same pretrained model to support many different computational pathology tasks.

---

## Summary

CONCH is a visual-language foundation model for computational pathology based on the CoCa framework.

Its main characteristics are:

- pretrained on over 1.17 million histopathology image-caption pairs
- combines an image encoder, text encoder, and multimodal decoder
- uses both contrastive learning and captioning objectives
- learns a shared embedding space between pathology images and text
- supports zero-shot and few-shot learning
- can process both ROIs and tiled whole-slide images
- enables image-text and text-image retrieval
- can perform zero-shot segmentation
- supports image caption generation

Overall, CONCH demonstrates how combining histopathology images with natural-language descriptions can produce a general-purpose representation that transfers across multiple computational pathology tasks.
