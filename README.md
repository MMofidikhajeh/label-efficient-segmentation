# Label-Efficient Segmentation with DINOv2 Distillation

This repository contains an ongoing research project on label-efficient binary segmentation using a ConvNeXt-Tiny encoder with a U-Net-style decoder, optional lightweight denoising reconstruction, and direct feature distillation from a frozen DINOv2 teacher.

The main research question is whether auxiliary self-supervised signals can improve segmentation performance when only a small fraction of training images have pixel-level labels.

## Current Focus

The current training pipeline focuses on:

- Semi-supervised / label-efficient segmentation
- Direct DINOv2 feature distillation
- Lightweight denoising reconstruction as an auxiliary regularizer
- Evaluation under limited label fractions
- Oxford-IIIT Pet for diagnostic experiments
- ISIC as the target dataset for the main experiments

Boundary loss has been removed from the current pipeline after experiments showed that it destabilized training and reduced segmentation performance.

## Method Overview

The model predicts:

1. A binary segmentation map.
2. An optional reconstructed RGB image.

During training:

- The segmentation loss is applied only to labeled images.
- The reconstruction loss is applied to all images.
- The DINOv2 distillation loss is applied to all images.
- Stronger photometric perturbations are applied to the model input.
- The reconstruction target remains clean and spatially aligned with the input.

The segmentation loss combines Dice and BCE.

## Supported Datasets

### Oxford-IIIT Pet

Oxford-IIIT Pet is used for diagnostic experiments and is downloaded automatically.

To use it, set:

    DATASET = "oxford"

### ISIC

ISIC is intended for the main label-efficiency experiments.

To use ISIC, place the data in the following structure:

    data/ISIC/images/*.jpg
    data/ISIC/masks/*.png

Mask filenames should match image filenames, for example:

    data/ISIC/images/ISIC_0000000.jpg
    data/ISIC/masks/ISIC_0000000.png

Common mask filename variants such as the following are also supported:

    ISIC_0000000_segmentation.png
    ISIC_0000000_mask.png

Then set:

    DATASET = "isic"

## Current Experiments

The current experiment suite evaluates the effect of lightweight reconstruction under limited labels.

Configs include:

- Segmentation-only baseline
- Segmentation + reconstruction with weight 0.010
- Segmentation + reconstruction with weight 0.005
- Segmentation + DINOv2 distillation
- Segmentation + DINOv2 distillation + reconstruction with weight 0.010
- Segmentation + DINOv2 distillation + reconstruction with weight 0.005

Label fractions are configurable. For example:

    LABEL_FRACTIONS = [0.10]

For a fuller low-label study, use:

    LABEL_FRACTIONS = [0.02, 0.05, 0.10]

Multiple seeds can be configured as:

    SEEDS = [42, 43, 44]

## Usage


The notebooks for the previous experiment featuring reconstruction, boundry, distillation, and segmentation loss can be found in the v1 folder.
The code for current experiments may be found in v2 folder.

*after deciding the best path for research, this section will be updated.*

## Metrics

The repository reports:

- IoU
- Dice
- SSIM, PSNR, and LPIPS for reconstruction-enabled models

## Project Status

This project is under active development.

Current priorities:

1. Determine whether very lightweight reconstruction helps segmentation.
2. Move the main experiments to ISIC.
3. Evaluate DINOv2 distillation under stronger label scarcity.
4. Add additional baselines and statistical evaluation over multiple seeds.
