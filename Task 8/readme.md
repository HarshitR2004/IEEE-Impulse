# EEG Channel Interpretability Analysis for Seizure Detection  

## Overview  
By attributing model decisions to specific channels, this study enhances transparency, enabling clinicians to better understand and trust AI-driven diagnostic tools.

## Methodology: Channel Importance Detection  

To identify the critical EEG channels contributing to seizure classification, the following steps were undertaken:  

### 1. **Attention Weight Extraction**  
- Neural network attention weights, obtained using PyTorch hooks, highlight the importance of input features during inference.  
- Focus was on the outputs of primary attention layers to capture spatial and temporal dependencies.

### 2. **Channel Mapping**  
- Raw attention weights were averaged to derive channel-specific importance scores.  
- Padding and reshaping were handled automatically, ensuring precise alignment with EEG channel indices.

### 3. **Class-Specific Analysis**  
- Samples were grouped by seizure type, and average channel importance scores were computed.  
- Top-K channel selection was performed to identify the most influential channels for each class.  

### 4. **Validation Through Masking**  
- Critical channels were masked (set to zero), and the model's performance was re-evaluated.  
- Performance drops were used to quantify the importance of these channels.

## Results and Reasoning  

### Top 3 Channels per Seizure Class  

| **Seizure Class**              | **Rank 1**       | **Rank 2**       | **Rank 3**       |  
|--------------------------------|------------------|------------------|------------------|  
| **Normal**                     | C3 (0.1727)      | C11 (0.1706)     | C13 (0.1646)     |  
| **Complex Partial Seizures**   | C1 (0.1581)      | C3 (0.1336)      | C5 (0.1318)      |  
| **Electrographic Seizures**    | C9 (0.1829)      | C3 (0.1674)      | C4 (0.1545)      |  
| **Video-Detected Seizures**    | C18 (0.6428)     | C1 (0.4668)      | C12 (0.4164)     |  

**Reasoning:**  
- Channels with the highest weights contribute the most to the model's classification decisions.  
- The model captures seizure-specific patterns, such as localized disruptions in specific brain regions (e.g., Complex Partial Seizures) or large-scale cortical activity (e.g., Video-Detected Seizures).  

## Performance Metrics  

#### **Original Model Performance**  
- **Accuracy:** 98%  
- **Balanced Accuracy:** 0.9876  
- **AUC-ROC Score:** 0.9983  

**Classification Report:**  
| Class                          | Precision | Recall | F1-Score | Support |  
|--------------------------------|-----------|--------|----------|---------|  
| **Normal**                     | 0.98      | 0.99   | 0.98     | 696     |  
| **Complex Partial Seizures**   | 0.99      | 0.97   | 0.98     | 549     |  
| **Electrographic Seizures**    | 0.99      | 0.99   | 0.99     | 137     |  
| **Video-Detected Seizures**    | 1.00      | 1.00   | 1.00     | 21      |  

#### **Performance Post-Masking**  
- **Accuracy:** 82% (16% drop)  
- **Balanced Accuracy:** 0.8266 (16.1% drop)  
- **AUC-ROC Score:** 0.9813 (1.71% drop)  

**Masked Channels:**  
- **Normal:** C3, C11, C13  
- **Complex Partial Seizures:** C1, C3, C5  
- **Electrographic Seizures:** C9, C3, C4  
- **Video-Detected Seizures:** C18, C1, C12  

**Classification Report After Masking:**  
| Class                          | Precision | Recall | F1-Score | Support |  
|--------------------------------|-----------|--------|----------|---------|  
| **Normal**                     | 0.97      | 0.67   | 0.80     | 696     |  
| **Complex Partial Seizures**   | 0.71      | 0.98   | 0.82     | 549     |  
| **Electrographic Seizures**    | 0.85      | 0.94   | 0.90     | 137     |  
| **Video-Detected Seizures**    | 1.00      | 0.71   | 0.83     | 21      |  

## Observed Performance Changes  

### 1. **Accuracy Decrease**  
Post-masking accuracy dropped significantly by **16%**, indicating the strong reliance of the model on the top-ranked channels for accurate classification.  
- **Key Observations by Class:**  
  - The **Normal** class exhibited the largest performance drop, with recall plummeting from **99% to 67%**. This highlights that the masked channels (C3, C11, and C13) play a crucial role in the model’s ability to identify normal EEG patterns accurately.

  - This steep decline in recall demonstrates that the Normal class depends heavily on specific global EEG patterns or regions, and the removal of these features severely impacts the model's confidence.  

  - Other classes also showed drops, but the magnitude of performance degradation varied based on how dependent each seizure type was on the selected critical channels.  

### 2. **AUC-ROC Score Observations**  
Despite the significant drop in accuracy, the **AUC-ROC score** decreased by only **1.71%**. This seemingly small change can be attributed to the nature of the metric:

- **AUC-ROC (Area Under the Receiver Operating Characteristic Curve)** measures the model's ability to rank positive cases higher than negative ones, rather than its absolute classification accuracy.  

- The metric is less sensitive to class imbalances and reflects the model's overall separability across classes.  

- Even with the removal of the most important channels, the model retained its ability to distinguish between seizure and non-seizure patterns to a considerable extent, showing its robustness to perturbations.  

### 3. **Seizure-Specific Observations**  
The impact of masking varied depending on the seizure class:  
- **Video-Detected Seizures:**  
  - The performance drop for this class was relatively smaller, suggesting that the model can compensate for the masked channels (C18, C1, and C12) by relying on alternative features or patterns in the EEG data.  

  - This resilience may stem from the unique nature of video-detected seizures, where large-scale cortical activity could manifest in multiple channels, allowing the model to infer the class using redundant information.  

- **Complex Partial Seizures and Electrographic Seizures:**  
  - The performance drop for these classes was more pronounced, indicating that their classification relies heavily on specific localized brain regions.  

  - The masked channels for these seizure types (e.g., C1, C3, C5 for Complex Partial Seizures and C9, C3, C4 for Electrographic Seizures) are likely critical for capturing fine-grained spatial patterns and disruptions in brain activity specific to these conditions.  

  - Removing these channels significantly impacted the model's ability to capture and utilize these patterns, leading to a noticeable degradation in performance.  

### Key Insights  
- The steep decline in accuracy, especially for the **Normal** class, highlights the criticality of the selected channels for capturing essential EEG patterns. 

- The relatively minor drop in AUC-ROC demonstrates the model's robustness and ability to maintain separability, even when deprived of its most influential features.  

- Seizure-specific dependencies on localized regions reinforce the importance of interpretability techniques, as they can identify the distinct neural regions critical for diagnosing different seizure types.  

## Conclusion  

This interpretability framework can aid clinicians in understanding EEG patterns, improving diagnostic precision, and enhancing trust in AI-driven tools.


