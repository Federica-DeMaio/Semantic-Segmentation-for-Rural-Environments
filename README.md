
# Semantic Segmentation for Rural Environments - DeepLabV3+ Stable

This repository contains the final project for the Machine Learning course, focused on the **semantic segmentation** of images acquired by vehicles in rural and off-road environments.

## 🎯 Project Objective

The goal is the pixel-wise classification of images captured by a mobile camera under varying lighting conditions. The model must distinguish between different types of terrain, obstacles, and vegetation while adhering to strict hardware constraints for execution on Google Colab.

### Project Constraints

* **GPU Memory (Training):** < 5 GB
* **GPU Memory (Inference):** < 4 GB
* **Dataset:** 931 images (original format 1280x720, resized for training).

## 🧠 Architecture: DeepLabV3+ Stable

The implemented model is an optimized version of **DeepLabV3+** that integrates:

* **Backbone:** ResNet-101 (Pre-trained) with a reduced Learning Rate for fine-tuning.
* **ASPP (Atrous Spatial Pyramid Pooling):** To capture multi-scale context.
* **Decoder:** A refinement module to recover spatial details from low-resolution layers.
* **Class Weighting:** Strategic weight balancing (e.g., Background reduced to 10%) to prioritize critical classes such as "Puddle" and "Obstacle".

## 📊 Classes and Mapping

The model manages the following 9 classes:

| ID | Class Name | Description |
| --- | --- | --- |
| 0 | Background | Ignored class/Background |
| 1 | Smooth Trail | Flat/beaten path |
| 2 | Grass | Grass |
| 3 | Rough Trail | Uneven/rough path |
| 4 | Puddle | Puddles |
| 5 | Obstacle | Obstacles |
| 6 | Non-traversable Vegetation | Impassable vegetation |
| 7 | High Vegetation | High vegetation |
| 8 | Sky | Sky |

## 📈 Final Results

After an iterative optimization process (including the use of adaptive schedulers and combined loss), the model achieved:

* **mIoU (Macro Avg):** ~0.646 (64.6%)
* **Mean F1-Score:** ~0.773 (77.3%)
* **Pixel Accuracy:** Extremely high, especially on dominant classes like Sky (IoU 0.901) and High Vegetation (IoU 0.855).

## 🛠️ Training Pipeline

1. **Preprocessing:** Normalization (ImageNet stats) and resizing.
2. **Loss:** `CombinedLoss` consisting of Weighted Cross-Entropy + Dice Loss (0.5/0.5).
3. **Optimizer:** Adam with differentiated Learning Rates ($10^{-5}$ for the backbone, $10^{-4}$ for the ASPP/Decoder modules).
4. **Regularization:** Early Stopping with a patience of 15 epochs and a `ReduceLROnPlateau` scheduler.

## 🚀 Usage

1. **Data:** Place the images in `./dataset/train` and `./dataset/val`.
2. **Training:** Execute the notebook cells to start the training loop.
3. **Evaluation:** Use the final module to generate the confusion matrix and visualize comparisons between Ground Truth and Predictions.

