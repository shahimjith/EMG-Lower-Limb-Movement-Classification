# EMG-Based Lower Limb Movement Classification Using Random Forest

A machine-learning project for classifying lower-limb movements from electromyography (EMG) signals.

## Project Overview

This project develops an end-to-end EMG classification pipeline for three lower-limb movement classes:

- **Leg Down**
- **Leg Move**
- **Leg Up**

The workflow includes signal cleaning, preprocessing, fixed-length windowing, time-domain feature extraction, Random Forest classification, and model evaluation.

## Assignment

The project was developed for the task **Lower-Limb Movement Classification Using EMG Signals**.

The selected dataset is:

**Electromyography Signal Dataset for Controlling Lower Limb Prostheses — Mendeley Data**

The dataset contains EMG recordings associated with Leg Up, Leg Down, and Leg Move movements.

## Methodology

```text
Raw EMG Signal
      ↓
Data Cleaning
      ↓
Detrending
      ↓
Full-Wave Rectification
      ↓
200-Sample Windowing
      ↓
Feature Extraction
      ↓
80/20 Stratified Train/Test Split
      ↓
Random Forest
      ↓
Evaluation
      ↓
Movement Prediction
```

### Preprocessing

The raw EMG signals are converted to numerical arrays. Invalid values are handled using interpolation, followed by:

1. Baseline/DC removal using detrending
2. Full-wave rectification using the absolute value

### Windowing

The preprocessed signals are divided into **non-overlapping 200-sample windows**.

### Extracted Features

Seven time-domain EMG features are extracted from every window:

| Feature | Description |
|---|---|
| MAV | Mean Absolute Value |
| RMS | Root Mean Square |
| Variance | Signal variation |
| Waveform Length | Cumulative absolute waveform change |
| Zero Crossings | Number of signal sign changes |
| Slope Sign Changes | Changes in the sign of successive slopes |
| Willison Amplitude | Significant amplitude changes |

## Model

The final classifier is a **Random Forest** with:

- 300 decision trees
- Balanced class weighting
- Fixed random seed for reproducibility

## Results

The final model was evaluated on a held-out test set.

| Metric | Result |
|---|---:|
| **Test Accuracy** | **94.44%** |
| **Macro F1-score** | **0.8491** |
| Test Samples | 162 |

### Class-wise Performance

| Class | Precision | Recall | F1-score |
|---|---:|---:|---:|
| Leg Down | 0.94 | 0.98 | 0.96 |
| Leg Move | 0.97 | 1.00 | 0.99 |
| Leg Up | 0.86 | 0.46 | 0.60 |

The main classification difficulty is **Leg Up vs. Leg Down**.

### Confusion Matrix

| True / Predicted | Leg Down | Leg Move | Leg Up |
|---|---:|---:|---:|
| Leg Down | 109 | 1 | 1 |
| Leg Move | 0 | 38 | 0 |
| Leg Up | 7 | 0 | 6 |

Seven Leg Up test windows were classified as Leg Down.

## Feature Importance

The Random Forest feature-importance ranking was:

| Feature | Importance |
|---|---:|
| MAV | 0.279 |
| Waveform Length | 0.198 |
| RMS | 0.189 |
| Variance | 0.148 |
| Willison Amplitude | 0.142 |
| Slope Sign Changes | 0.044 |
| Zero Crossings | 0.000 |

MAV was the most important feature for this Random Forest.

## Repository Contents

Recommended repository structure:

```text
EMG-Lower-Limb-Movement-Classification/
│
├── README.md
├── requirements.txt
├── .gitignore
├── EMG_Lower_Limb_Movement_Classification.ipynb
├── final_emg_random_forest.pkl
├── emg_feature_names.pkl
│
├── report/
│   └── EMG_Lower_Limb_Movement_Classification_Report.docx
│
└── results/
    ├── confusion_matrix.png
    └── feature_importance.png
```

The notebook and trained model files should be added from the Google Colab session after downloading them.

## Requirements

The project uses:

- Python
- NumPy
- Pandas
- SciPy
- Matplotlib
- scikit-learn
- Joblib
- openpyxl

Install them with:

```bash
pip install -r requirements.txt
```

## Running the Project

1. Download or obtain the permitted copy of the Mendeley dataset.
2. Open `EMG_Lower_Limb_Movement_Classification.ipynb` in Google Colab or Jupyter.
3. Upload the dataset.
4. Run the notebook cells in order.
5. The notebook performs preprocessing, windowing, feature extraction, training, evaluation, and prediction.
6. The trained Random Forest is saved as `final_emg_random_forest.pkl`.

## Limitations

- The movement classes are not equally represented.
- Leg Up has substantially lower recall than the other classes.
- The current system uses time-domain features only.
- Evaluation uses a held-out test split rather than an external dataset.
- Real-time EMG acquisition and prosthetic hardware control were not implemented in the core project.

## Future Improvements

With additional time, the project could be extended by:

- Increasing the number of Leg Up recordings
- Using a more balanced dataset
- Adding frequency-domain and time-frequency EMG features
- Performing subject-independent evaluation
- Testing real-time EMG acquisition
- Integrating the classifier into a real-time prosthetic-control system
- Implementing the **bonus EMG-based exoskeleton assistance architecture**

### Bonus Concept

```text
EMG Signal
    ↓
Preprocessing + Features
    ↓
ML Classifier
    ↓
Predicted Movement
    ↓
Assistance Decision
    ↓
Exoskeleton Actuator
```

## Report

The project report is available in the `report/` directory.

## Disclaimer

The trained model is a project/research prototype and is not intended for clinical or safety-critical use.

## Acknowledgements

Dataset: *Electromyography Signal Dataset for Controlling Lower Limb Prostheses*, Mendeley Data.

