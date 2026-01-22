# Air Compressor Fault Diagnosis

## Overview
This project presents a **frequency-domain signal processing and machine learning pipeline** for air compressor fault diagnosis using acoustic signals. The dataset contains one healthy condition and multiple fault types collected from an air compressor system.

The raw signals are contaminated by:
* **50 Hz powerline noise**
* **300 Hz cooling fan noise**
* **High-frequency noise above 12 kHz**

To address these issues, signal preprocessing, frequency-domain feature extraction, dimensionality reduction, and classification are applied.

## Objectives
* Remove real-world noise from acoustic signals using digital filtering
* Transform signals from **time domain to frequency domain**
* Extract **13 meaningful frequency-domain features**
* Reduce feature dimensionality **without significant loss of accuracy**
* Classify air compressor operating conditions using **Support Vector Machine (SVC)**

## Dataset Structure
The dataset directory must be placed in the same folder as the Jupyter Notebook.
```
AirCompressor/
├── Bearing/
│   ├── preprocess_Reading1.dat
│   ├── preprocess_Reading2.dat
│   └── ...
├── Flywheel/
├── Healthy/
├── LIV/
├── LOV/
├── NRV/
├── Piston/
└── Riderbelt/
```

Each subfolder represents a **machine condition or fault class**.
Each `.dat` file contains a **single-channel acoustic signal**.

## Signal Processing Pipeline

### 1. Preprocessing
* Detrending
* Notch filtering:
  * **50 Hz** (powerline interference)
  * **300 Hz** (cooling fan noise)
* Low-pass filtering:
  * Cutoff frequency: **12,000 Hz**
* Sampling frequency: **50,000 Hz**

### 2. Frequency Transformation
* Fast Fourier Transform (FFT)
* Only positive frequency components are used

## Frequency-Domain Feature Extraction (13 Features)
The following features are extracted from the FFT magnitude spectrum:
1. Mean
2. Standard Deviation
3. Maximum
4. Minimum
5. Spectral Energy
6. RMS
7. Skewness
8. Kurtosis
9. Peak Frequency Index
10. Variance
11. Median
12. Spectral Entropy
13. Spectral Roll-off (75%)

## Feature Reduction
* **Principal Component Analysis (PCA)**
* Retains **95% of total variance**
* Reduces feature dimension from **13 → 5**
* Prevents overfitting and improves generalization

## Classification
* Classifier: **Support Vector Machine (SVC)**
* Kernel: Radial Basis Function (RBF)
* Data normalization using **StandardScaler**
* Evaluation:
  * Train/Test split (80/20)
  * 5-fold cross-validation
  * Confusion matrix and accuracy metrics

## Results

| Metric                    | Value  |
| ------------------------- | ------ |
| Training Accuracy         | ~88%   |
| Cross-Validation Accuracy | ~87%   |
| Test Accuracy             | ~88%   |
| Number of Features        | 13 → 5 |

## Requirements
* Python ≥ 3.8
* NumPy
* SciPy
* scikit-learn
* matplotlib

Install dependencies:
```bash
pip install numpy scipy scikit-learn matplotlib
```

## How to Run
1. Place the `AirCompressor` dataset folder next to the notebook file
2. Open the Jupyter Notebook
3. Run all cells sequentially
4. Review classification results and confusion matrix
