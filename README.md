# Reproducing Calibration Results for Deep Neural Networks

Reproduction study of **Guo et al. (2017), "On Calibration of Modern Neural Networks"**, investigating calibration in modern deep neural networks and comparing post-hoc calibration techniques.

## Overview

Modern neural networks can achieve high classification accuracy while producing overconfident probability estimates. This project reproduces the calibration experiments from Guo et al. (2017) and evaluates how different calibration methods reduce miscalibration.

The project compares four post-hoc calibration methods:

- **Temperature Scaling**
- **Histogram Binning**
- **Isotonic Regression**
- **Vector Scaling**

The experiments span multiple datasets and neural network architectures, using **Expected Calibration Error (ECE)** as the primary calibration metric.

The project also extends the original study by evaluating **Label Smoothing** as a training-time calibration technique.

## Datasets and Models

| Dataset | Model |
|---|---|
| CIFAR-100 | ResNet-164 |
| CIFAR-100 | ResNet-56 |
| CIFAR-100 | DenseNet-190 |
| CIFAR-100 | WideResNet-28-10 |
| CIFAR-10 | ResNet-164 |
| CIFAR-10 | ResNet-56 |
| Stanford Cars | MobileNetV2 |
| CUB-200 / Birds400 | InceptionV3 |

## Calibration Methods

### Temperature Scaling

Temperature Scaling introduces a single learnable scalar temperature `T` and divides model logits by `T` before applying softmax. It preserves the ordering of logits and therefore does not change the predicted class.

### Histogram Binning

Histogram Binning partitions predicted probabilities into fixed-width bins and replaces probabilities within each bin with the empirical accuracy of that bin.

### Isotonic Regression

Isotonic Regression learns a non-decreasing mapping from predicted probabilities to calibrated probabilities using validation data.

### Vector Scaling

Vector Scaling independently learns a weight and bias for each class and applies an affine transformation to the logits before softmax. Its larger parameterization can make it more prone to overfitting.

## Evaluation Metric

The primary metric is **Expected Calibration Error (ECE)**. ECE divides predictions into confidence bins and computes the weighted difference between empirical accuracy and average confidence. Lower ECE indicates better calibration.

## Experimental Workflow

For each model-dataset pair:

1. Prepare model predictions and labels.
2. Split the available data into validation and test sets.
3. Learn calibration parameters using the validation set.
4. Apply the calibration method to test predictions.
5. Compute ECE before and after calibration.
6. Generate reliability diagrams and visualizations.
7. Compare calibration performance across models and datasets.

## Results

The experiments reproduce the main observation from Guo et al. that modern neural networks can be substantially miscalibrated despite high classification accuracy.

For example, **ResNet-164 on CIFAR-100** has an uncalibrated ECE of **15.07%**, which is reduced to **1.36%** using Temperature Scaling.

Temperature Scaling consistently provides strong calibration improvements while retaining the model's classification behavior. Vector Scaling generally performs worse than Temperature Scaling and can suffer from overfitting due to its larger parameterization.

A notable exception is **InceptionV3 on Birds400**, where the uncalibrated model already has an ECE of **0.50%**. Applying calibration methods can therefore provide little benefit or degrade calibration.

## Label Smoothing Extension

The project additionally evaluates **Label Smoothing**, which was not part of the original post-hoc calibration comparison.

For the ResNet-56 experiment on CIFAR-10:

- Accuracy decreased from **94.12% to 92.90%**.
- ECE increased from **3.96% to 6.05%**.

This experiment suggests that, for the evaluated pretrained model, retraining with Label Smoothing was less effective than post-hoc Temperature Scaling.

## Repository Contents

The repository contains the implementation of the calibration methods, experiment workflows, result generation, and visualization code used for the reproduction study.

The accompanying report documents the methodology, implementation details, experimental results, reliability diagrams, ECE comparisons, Label Smoothing extension, analysis, and conclusions.

## Reference

> Guo, C., Pleiss, G., Sun, Y., & Weinberger, K. Q. (2017).  
> **On Calibration of Modern Neural Networks.**  
> Proceedings of the International Conference on Machine Learning (ICML).

## Author

**Monil Manish Desai**

## Acknowledgement

I would like to acknowledge the contributions of my teammates,
**Madhur Thakkar**, **Anurag Tripathi**, and **Aditya Desai**, to the development
and completion of this project. Their contributions to the implementation,
experimentation, analysis, and project discussions supported the overall
reproduction study.

The project was completed collaboratively, with individual responsibilities
distributed across different parts of the implementation and experimental
workflow. I am grateful for their cooperation, technical discussions, and
support throughout the project. The original repository is https://github.com/monil2003/FML_Project.
