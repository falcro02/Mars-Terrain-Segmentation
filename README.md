# Martian Terrain Semantic Segmentation

This repository contains the code and resources for a semantic segmentation project focused on segmenting images of Martian terrain. The goal was to develop a neural network capable of accurately segmenting unseen images into distinct classes: Background, Soil, Bedrock, Sand, and Big Rocks.

## Table of Contents

- [Introduction](#introduction)
- [Problem Analysis](#problem-analysis)
- [Methodology](#methodology)
- [Experiments](#experiments)
- [Results](#results)
- [Files in this Project](#files-in-this-project)
- [Team](#team)

## Introduction

In this project, a semantic segmentation model was developed to segment images of Martian terrain. The primary objective was to create a neural network that could accurately segment unseen images into specific classes: Background, Soil, Bedrock, Sand, and Big Rocks. A U-Net architecture was designed from scratch to effectively learn the necessary features for precise segmentation. The dataset comprised approximately 2,500 images and their corresponding masks after cleaning, which were used for training and evaluation.

## Problem Analysis

During the problem analysis, outliers were removed from the dataset to ensure data consistency. An analysis of the class distribution revealed that most classes occupied about 20% of the dataset area, with the exception of the "Big Rocks" class, which accounted for only 0.13%. This indicated a significant class imbalance. The main challenges identified were the small size of the dataset and the absence of transfer learning options, necessitating the U-Net to learn solely from this limited data. To address this, geometric data augmentation was implemented, with careful preservation of mask integrity to maintain recognizability. It was assumed that careful augmentation strategies and mechanisms to prevent overfitting would be essential for training an effective model due to the class imbalance.

## Methodology

The approach began with designing a U-Net architecture specifically for Martian terrain segmentation. A simple U-Net served as the baseline and was systematically refined to overcome dataset challenges. Key parameters such as dropout rates and regularization terms were adjusted to balance model capacity and generalization. More complex approaches were explored through trial-and-error, focusing only on techniques that consistently yielded better results. The methodology also involved a detailed examination of the model's predictions post-training to identify underperforming areas, providing insights into weaknesses and enabling targeted improvements. These iterative refinements, guided by output observations, led to significant performance improvements and more consistent results across validation and test datasets.

## Experiments

After augmenting the dataset with geometric transformations (horizontal flip, vertical flip, safe rotate), more complex techniques like CutMix from the Albumentations library were explored. A foundational U-Net architecture was implemented with three encoding levels, a bottleneck, and three decoding levels, utilizing skip connections to preserve spatial information. Dropout layers, including both standard and spatial dropout, were incorporated to enhance generalization and mitigate overfitting.

Further enhancements to the U-Net included modifications to the bottleneck design and integration of advanced components such as attention mechanisms, squeeze-and-excitation blocks, residual blocks, Atrous Spatial Pyramid Pooling (ASPP) modules, and inception blocks. These additions aimed to improve the model's capacity to capture multi-scale features and complex patterns. Various loss functions, including Dice, Weighted Sparse Categorical Crossentropy, Tversky, and Focal Tversky, were evaluated to address class imbalance and emphasize hard-to-segment regions. Targeted data augmentation strategies were used to generate more samples for minority classes to tackle their underrepresentation. This comprehensive approach addressed class imbalance at both the dataset level and through loss function design, ensuring better optimization for precise segmentation.

A "Full-Inception UNet" was developed by incorporating inception blocks into the encoder, bottleneck, and skip connections for multi-scale feature extraction, though this design did not yield the expected improvement in segmentation performance. Training duration was extended by adjusting early stopping patience (from 10 to a range of 20-40 epochs), allowing the model additional time to converge. Similarly, the patience of the learning rate scheduler was extended from 5 to a range of 10-15 epochs to provide a more gradual adaptation of the learning rate during training.

A significant breakthrough came when the background class was revisited in the loss function. Excluding the background class from the loss computation proved to be a key refinement, leading to a substantial jump in performance. Implementing this adjustment with the Weighted Sparse Categorical Crossentropy loss function resulted in a marked improvement in results, which validated the robustness of the framework and reinvigorated efforts. This discovery underscored the importance of iterative experimentation and attention to detail for final success.

## Results

Initially, a simple U-Net architecture achieved a mean IoU of 45% on the test set, but showed significant overfitting with a local validation set mean IoU of approximately 70%. Modifications like adding squeeze-and-excitation blocks and spatial dropout effectively reduced overfitting and improved test set performance to a much higher 51% IoU.

The "Big Rock" class was identified as significantly underrepresented in the dataset, with only 62 images containing this class. While targeted data augmentation was applied for the big rock class to partially address this imbalance, much of the focus during this phase was on exploring more complex architectures and advanced techniques to improve overall performance. Despite these efforts, performance eventually reached a plateau, where additional changes often degraded performance rather than yielding further gains.

Upon re-evaluating the approach, a critical issue was identified: the improper handling of class 0 during training. Initially, class 0 was excluded only from the mean IoU metric, but further analysis revealed that since the test set also ignored class 0, it would be beneficial to exclude it from the loss calculation as well. Once this change was implemented, the performance plateau that had previously hindered progress was overcome, achieving a significant improvement to 67% mean IoU. This breakthrough reinvigorated efforts, leading to experimentation with various new techniques, ultimately pushing the performance further to 71.13%.

The model's main strengths lie in its ability to generalize well across all classes, particularly its improved performance in predicting class 4, which was initially underrepresented. This improvement is attributed to the application of targeted augmentations and the use of a weighted sparse categorical cross-entropy loss function. However, the model has a notable limitation: it does not predict any label corresponding to the background class.

**Metrics Per Classes On Local Test Set**

| Class | Precision | Recall | F1-score | Mean IoU |
| :--- | :--- | :--- | :--- | :--- |
| Soil | 81.90% | 96.52% | 88.61% | 79.55% |
| Bedrock | 70.55% | 85.68% | 77.38% | 63.11% |
| Sand | 69.13% | 94.95% | 80.01% | 66.68% |
| Big Rock | 74.07% | 95.31% | 83.36% | 71.46% |

**Overall Mean IoU**: 76.16%

## Files in this Project

- `aliendRemoval.ipynb`
- `Attention_RES_Unet.ipynb`
- `Augmentation.ipynb`
- `best.ipynb`
- `inception_Unet.ipynb`
- `report.pdf`

## Team

*   Denis Gabriel Sanduleanu (denis2404)
*   Alessio Caggiano (falcro02)
*   Mattia Repetti (mattiarepettil)
*   Jonatan Sciaky (jonatansciakyy)

**Contributions:**
*   Jonatan worked on augmentations, creating an adequate training set, and setting up the network.
*   Denis focused on different networks and architectures, integrating various blocks into the network.
*   Alessio worked on augmentations and network refinements.
*   Mattia concentrated on testing different loss functions and other network models.
