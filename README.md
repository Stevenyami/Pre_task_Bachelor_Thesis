# Nuclei Segmentation on MoNuSeg

**Computer vision for healthcare · Histopathology · TensorFlow/Keras**

A bachelor-thesis preparation project exploring U-Net segmentation of cell nuclei in MoNuSeg histopathology images. The notebook covers annotation processing, image preparation, model training, visual inspection of predictions and an exploratory approach to cell counting.

## Explore the project

- [Notebook: preparation, training and visual results](Pretask_for_Bachelor_Thesis.ipynb)
- [MoNuSeg dataset information](https://monuseg.grand-challenge.org/Data/)

The notebook includes saved outputs, so the workflow and example predictions can be inspected without retraining the model.

## Implemented workflow

1. Load tissue images and create binary masks from the XML nucleus annotations.
2. Convert RGB images to grayscale and normalize each image.
3. Split the loaded images into training, validation and test sets **before** extracting patches.
4. Extract non-overlapping 128 × 128 image and mask patches.
5. Train a U-Net in TensorFlow/Keras using Adam and binary cross-entropy.
6. Threshold predicted masks and compare them visually with ground truth.
7. Label connected components and visualize selected component centroids for exploratory counting.

The saved run loaded 37 images and split them into 25 training, 6 validation and 6 test images. Training ran for 10 epochs with a batch size of 16. These figures describe this notebook run rather than an official benchmark split.

## What the results show

The notebook contains training and validation curves, example test-mask predictions and centroid visualizations. Its recorded accuracy metric is **pixel accuracy**; it is not a measure of cell-count accuracy or clinical performance.

Dice, IoU and quantitative counting-error evaluation are not currently reported. The project is best read as a segmentation learning experiment with an exploratory counting stage.

## Inspecting or rerunning the notebook

Open the notebook on GitHub to inspect the saved outputs, or open a copy in Google Colab. The existing workflow uses Colab's file-upload interface for a MoNuSeg ZIP archive and expects tissue images and XML annotations beneath `monuseg/MoNuSeg 2018 Training Data/`.

Before rerunning, obtain the dataset from its source and check that the archive layout matches the paths in the data-loading cell. Installation cells are included, but dependencies are not pinned; the recorded runtime is not a reproducible environment specification.

Core tools used in the implementation include Python, TensorFlow/Keras, NumPy, scikit-image, scikit-learn, Matplotlib and imagecodecs.

## Current limitations

- **Counting is exploratory:** the current function selects connected components near any patch edge and omits components wholly inside the patch. It does not implement a validated rule for counting each nucleus exactly once across patches.
- **Touching nuclei:** binary connected components can merge neighboring nuclei, so a component is not necessarily one nucleus.
- **Image coverage:** the patch extractor discards remainder strips that do not fit a complete 128 × 128 patch.
- **Evaluation:** the notebook does not report quantitative test segmentation or counting metrics, and no trained model artifact is included in this repository.

Useful next steps are to correct and validate the counting rule, retain full image coverage, and report segmentation and count errors on a fixed held-out image split.
