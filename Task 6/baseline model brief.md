

# Classification using SVM as a Baseline Model

## Overview
This project implements a machine learning model for classifying EEG (Electroencephalogram) signals into different seizure categories using a Support Vector Machine (SVM). The model extracts both frequency-domain features (Fourier Transform) and time-domain features (Zero Crossing Rate) from the EEG signals for classification.

## Dataset
The dataset consists of EEG recordings categorized into four classes:
- **0**: Normal
- **1**: Complex Partial Seizures
- **2**: Electrographic Seizures
- **3**: Video-detected Seizures with no visual change over EEG

## Feature Extraction
Two main features are extracted from the EEG signals:

1. **Fourier Transform Features**
   - Transforms time-domain signals to frequency domain
   - Captures frequency components of the EEG signals
   - Implemented using `scipy.fft`
   - Mean values are used as features

2. **Zero Crossing Rate (ZCR)**
   - Measures the rate at which the signal changes from positive to negative
   - Provides information about signal frequency characteristics
   - Calculated using sign changes in the signal
   - Mean values are used as features

## Model Architecture
- **Algorithm**: Support Vector Machine (SVM)
- **Kernel**: Linear
- **Probability**: Enabled for ROC-AUC calculation
- **Feature Preprocessing**: StandardScaler for feature normalization

## Implementation Details

### 1. Data Loading
- Training and validation data are loaded separately.
- Class labels are maintained consistently across the data.

### 2. Feature Processing
- Features are extracted independently for the training and validation sets.
- Features are combined using `numpy.hstack`.
- Standardization is applied using `StandardScaler`.

### 3. Model Training
- SVM is trained on the preprocessed features.
- Probability estimates are enabled to compute the ROC-AUC score.

## Model Evaluation
The model's performance is evaluated using the following metrics:

# Classification Report

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0     | 0.63      | 0.92   | 0.75     | 696     |
| 1     | 0.76      | 0.52   | 0.61     | 549     |
| 2     | 0.00      | 0.00   | 0.00     | 137     |
| 3     | 0.61      | 0.52   | 0.56     | 21      |
| **Accuracy** |           |        | 0.67     | 1403    |
| **Macro avg** | 0.50      | 0.49   | 0.48     | 1403    |
| **Weighted avg** | 0.62      | 0.67   | 0.62     | 1403    |


    
 **Precision**: Indicates the accuracy of positive predictions for each class.
- **Recall**: Shows the model's ability to identify all positive samples for each class.
- **F1-score**: Harmonic mean of precision and recall, providing a balanced evaluation.
- **Accuracy**: The overall percentage of correct predictions.
- **Macro Average**: The average performance across all classes.
- **Weighted Average**: Accounts for the class imbalance by considering the support of each class.

 **ROC-AUC Score**
- **Score**: 0.770
- Interpretation: The ROC-AUC score of 0.770 indicates that the model has a good ability to distinguish between classes. A score above 0.5 means the model performs better than random chance. A score of 0.770 suggests moderately strong discriminative ability.

 **Balanced Accuracy**
- **Score**: 0.490
- Interpretation: The balanced accuracy of 0.490 indicates that the model's performance is affected by class imbalance. It shows the average recall per class, which can be further improved by tuning the model or addressing data imbalance.

## Results Interpretation
- **ROC-AUC Score**: The model has a good ability to differentiate between the classes, with a score of 0.770 indicating good discriminative power.
- **Balanced Accuracy**: While the score is moderate (0.490), it highlights that the model has room for improvement, particularly in handling class imbalance.
- **Classification Report**: This detailed breakdown shows that the model performs reasonably well for some classes, especially Class 0 and Class 1, but struggles with Class 2 due to its lower recall and F1-score.



