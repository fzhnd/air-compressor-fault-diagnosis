# Air Compressor Fault Detection System using XGBoost
This project implements a Machine Learning pipeline to detect and classify faults in air compressors based on acoustic/vibration signals. It utilizes advanced Digital Signal Processing (DSP) techniques for noise reduction and feature extraction, followed by an optimized **XGBoost** classifier.

## Overview
The system processes raw acoustic signals sampled at **50 kHz**. It is designed to classify the compressor into one of 8 states:
1. **Healthy**
2. **Bearing** (Fault)
3. **Flywheel** (Fault)
4. **LIV** (Leakage Inlet Valve)
5. **LOV** (Leakage Outlet Valve)
6. **NRV** (Non-Return Valve)
7. **Piston** (Fault)
8. **Riderbelt** (Fault)

## Dataset Structure
The script expects a folder named `AirCompressor` in the root directory. Inside, there should be sub-folders for each class containing `.dat` files.
```text
AirCompressor/
│   ├── Bearing/
│   │   ├── file1.dat
│   │   └── ...
│   ├── Healthy/
│   ├── ... (other classes)
│
├── air-compressor.ipynb
└── README.md
```

## Features & Methodology
### 1. Preprocessing (DSP)
Before analysis, raw signals undergo rigorous cleaning to remove environmental noise:
* **Detrending:** Removes DC offset.
* **Notch Filtering:** Specifically removes **50 Hz** (Powerline noise) and **300 Hz** (Cooling fan noise) using an IIR Notch filter.
* **Lowpass Filtering:** Removes high-frequency noise (> 12 kHz) using a 6th-order Butterworth filter.
### 2. Feature Extraction (Frequency Domain)
The signal is transformed using **FFT** (Fast Fourier Transform). 13 statistical features are extracted:
* Mean Frequency, Standard Deviation Frequency
* Spectral Skewness, Spectral Kurtosis
* Spectral Entropy, Spectral Flatness
* Spectral Rolloff (85% energy)
* Peak Frequency, **Median Frequency** (50% energy)
* RMSF, Crest Factor, Max High Band Power, Total Energy
### 3. Feature Selection & Modeling
* **RFECV (Recursive Feature Elimination):** Automatically selects the most important features to reduce dimensionality and overfitting.
* **XGBoost Classifier:** The core classification engine.
* **GridSearchCV:** Performs hyperparameter tuning (Learning rate, Max depth, Gamma, etc.) to maximize accuracy.

## Installation
1. **Clone the repository:**
```bash
git clone https://github.com/fzhnd/air-compressor-fault-detection.git
cd air-compressor-fault-detection
```
2. **Install dependencies:**
It is recommended to use a virtual environment.
```bash
pip install numpy scipy matplotlib scikit-learn xgboost
```

## Usage
1. Ensure your dataset is placed in the `AirCompressor` folder as described above.
2. Run the main script:
```bash
python air-compressor.ipynb
```
3. The script will output:
* Preprocessing status.
* Optimal features selected by RFECV.
* Best Hyperparameters found.
* Final Accuracy and Classification Report.
* A Confusion Matrix plot.

## Results
The model achieves a high accuracy on the test set.
* **Final Accuracy:** ~90.00%
* **Selected Features:** 11 out of 13 features were deemed optimal.
* **Performance:**
* **Healthy** state detection is highly accurate (Low False Alarm rate).
* **Riderbelt** faults are detected with 100% recall.
