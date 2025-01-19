
# EEG Signal Denoising and Classification

## Overview

This project showcases a robust methodology for denoising EEG signals and enhancing their classification accuracy. By leveraging advanced signal processing and deep learning techniques, the approach ensures efficient preprocessing and accurate classification results.

---

## Methodology

### 1. Signal Denoising
A custom `MultiChannelEEGDenoiser` class was developed, featuring:
- **Wavelet Transform**: Utilized the `sym4` wavelet for effective decomposition and reconstruction.
- **Adaptive Thresholding**: Applied to suppress noise while preserving signal integrity.
- **Parallel Processing**: Enabled efficient batch denoising for large datasets.

**Performance**:
- Mean Peak Signal-to-Noise Ratio (PSNR): **10.62 dB**
- Channel-wise PSNR values are available in the implementation for detailed analysis.

---

### 2. Feature Extraction
- Extracted **950 features per sample**, incorporating both time-domain and frequency-domain attributes.
- Features were saved as **NumPy arrays** to ensure reproducibility and facilitate downstream analysis.

---

### 3. Neural Network Classifier
Designed a robust neural network for classification using **PyTorch**. Key training details:
- **Batch Size**: 32
- **Optimizer**: Adam
- **Loss Function**: Cross Entropy
- **Learning Rate**: 0.001

---

## Results

### Performance Metrics
The classifier's performance was evaluated on the validation set, yielding the following metrics:

#### Classification Report:
| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0     | 0.91      | 0.85   | 0.88     | 696     |
| 1     | 0.86      | 0.87   | 0.86     | 549     |
| 2     | 0.77      | 0.96   | 0.85     | 137     |
| 3     | 0.95      | 0.86   | 0.90     | 21      |

#### Summary:
| Metric                | Value       |
|-----------------------|-------------|
| **Accuracy**          | **87%**     |
| **Balanced Accuracy** | **88.57%**  |
| **Macro Average F1-Score** | **87%**     |
| **AUC-ROC Score**     | **0.973**   |

---

## Key Observations from Performance Metrics

### **Overall Accuracy and Balanced Accuracy**
- Achieved **87% accuracy** and **88.57% balanced accuracy**, demonstrating robust performance across all classes, even with imbalanced data.

---

### **Class-wise Performance**
#### Normal (Class 0)
- **Precision (0.91)** and **Recall (0.85)**: High confidence and sensitivity with an F1-score of **0.88**.
  
#### Complex Partial Seizures (Class 1)
- **Precision (0.86)** and **Recall (0.87)**: Consistent performance with an F1-score of **0.86**.

#### Electrographic Seizures (Class 2)
- **Precision (0.77)** and **Recall (0.96)**: High sensitivity ensures almost all samples are detected, with an F1-score of **0.85**.

#### Video Detected Seizures (Class 3)
- **Precision (0.95)** and **Recall (0.86)**: Excellent precision and a strong F1-score of **0.90**, even with limited data.

---

### **AUC-ROC and Imbalance Handling**
- The **AUC-ROC score of 0.973** indicates strong discrimination between classes.
- Despite imbalanced data, minority classes like Electrographic Seizures and Video Detected Seizures achieved high F1-scores of **0.85** and **0.90**, respectively.

### **Strengths**
- High precision and recall across all classes.
- Effective handling of minority classes with balanced accuracy and minimal overfitting.

### **Areas for Improvement**
- **Electrographic Seizures**: Boosting precision could reduce false positives.
- **Video Detected Seizures**: Enhancing recall may further improve performance.

---

This analysis highlights the model’s robustness, sensitivity to minority classes, and strong generalization to unseen data.