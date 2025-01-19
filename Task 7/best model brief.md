
# EEG Seizure Classification Model

This project implements an advanced neural network model for classifying different types of seizures from EEG data. The model achieves high accuracy while maintaining computational efficiency through careful feature engineering and architectural choices.

## Model Architecture

The model uses a custom neural network architecture designed for EEG signal detection, incorporating the following components:

1. **Attention Mechanisms:**
   - The model applies attention mechanisms to selectively focus on important time-frequency features within the EEG signal data. EEG signals contain temporal dependencies and spatial correlations across multiple channels, and the attention mechanism enables the model to weigh more important signals for classification, helping the model focus on the most relevant segments of the EEG time series.

2. **Residual Blocks:**
   - The model leverages residual blocks to improve gradient propagation, which is essential for deep learning models dealing with long-range dependencies in EEG data. These residual connections allow the model to maintain gradient flow and avoid vanishing gradient problems, making it easier to train deeper networks and capture both short-term and long-term patterns in EEG signals.

3. **Fully Connected Layers (Dense Layers):**
   - Multiple fully connected layers are used to map the high-dimensional features extracted from EEG signals to the final output classes. Each fully connected layer learns abstract representations of the EEG signals.
   - After each fully connected layer, **Batch Normalization** is applied to stabilize training by normalizing the activations. This helps the model converge faster and improves its generalization performance on unseen EEG data.

4. **Batch Normalization:**
   - Batch normalization is applied after each fully connected layer to prevent the issue of internal covariate shift, where the distribution of the activations changes during training. This ensures that the network can better generalize to different EEG signals and increases the stability of the training process.

5. **Dropout Layers:**
   - Dropout is implemented in multiple layers to mitigate overfitting. EEG signals can contain noise, and dropout prevents the model from overly relying on any particular feature, forcing it to generalize better. By randomly setting some of the activations to zero during training, dropout encourages the model to learn more robust representations of the EEG data.

6. **Output Layer:**
   - The final output layer uses a **Softmax activation** function, producing a probability distribution over the possible classes (e.g., detecting specific mental states or identifying seizure events). This is suitable for multi-class classification tasks, where each output class corresponds to a different EEG event or pattern.

7. **Activation Functions:**
   - **ReLU** (Rectified Linear Unit) is used in the hidden layers to introduce non-linearity, allowing the model to learn more complex patterns in the EEG data. This is crucial for capturing the non-linear nature of brain signals.
   - **Softmax** is used in the output layer to normalize the network’s outputs into probabilities, ensuring that the sum of the output values is 1. This is suitable for multi-class classification tasks, where each class corresponds to a different EEG event or condition.

---

This architecture is optimized for processing EEG signals, utilizing attention to focus on relevant features, residual connections to maintain gradient flow, and robust regularization techniques (batch normalization and dropout) to ensure generalization and performance in detecting EEG patterns.


Total Parameters:
- Trainable Parameters: 1,445,764
- Non-Trainable Parameters: 0
- Total Parameters: 1,445,764

## Feature Engineering

The model employs a multi-faceted feature extraction approach, incorporating a variety of time-domain, frequency-domain, connectivity, and wavelet-based features to capture the complex dynamics of EEG signals.

### 1. Time Domain Features
These features focus on the raw EEG signal and its statistical properties over time.
- **Basic statistics**: Mean, standard deviation, maximum, and minimum values that provide a basic overview of the signal’s amplitude distribution.
- **Zero crossing rate**: The rate at which the signal crosses the zero line, useful for detecting the frequency of events in the signal.
- **RMS value**: Root Mean Square value to quantify the energy of the EEG signal.
- **Activity measures**: The amount of signal activity over time, helping to detect periods of significant brain activity.
- **Hjorth parameters**: These include mobility (a measure of frequency content) and complexity (a measure of the waveform's irregularity).
- **Shape statistics**: Measures like kurtosis (peakedness of the signal distribution) and skewness (asymmetry of the signal distribution).

### 2. Frequency Domain Features
These features capture the frequency characteristics of the EEG signal by transforming it into the frequency domain.
- **Power spectral density**: The distribution of power across different frequency components of the EEG signal.
- **Band powers**: The power of the signal in specific frequency bands—delta (0.5-4 Hz), theta (4-8 Hz), alpha (8-13 Hz), beta (13-30 Hz), and gamma (30-40 Hz). These bands are relevant for identifying different types of brain activity.
- **Spectral edge frequency**: The frequency below which a certain percentage (usually 95%) of the total spectral power is contained. It is a marker for the signal's overall frequency distribution.

### 3. Connectivity Features
These features capture the relationship between different EEG channels, helping to reveal network-like interactions in the brain.
- **Cross-correlation between channels**: Measures the similarity of signals between two different EEG channels to identify synchrony or co-activation between brain regions.
- **Signal coherence**: Measures the degree of similarity or correlation between two signals in the frequency domain, providing insights into how well different brain regions communicate.
- **Inter-channel relationships**: A broader set of connectivity metrics that explores how signals from different channels relate to one another, which is useful for detecting abnormal brain activity patterns associated with seizures.

### 4. Wavelet Features
Wavelet transforms are used to decompose the EEG signal into different frequency components at various scales.
- **Multi-level wavelet decomposition**: Decomposes the signal into components at multiple frequency scales using wavelets, allowing for the extraction of both high and low-frequency information.
- **Statistical features from wavelet coefficients**: Statistical measures (mean, variance, etc.) of the wavelet coefficients at each level, which provide insight into the signal’s structure at different frequencies.
- **Energy measures at different frequency bands**: The energy content of the signal in each frequency band, providing additional context for the classification task.

All features are normalized using **StandardScaler** to ensure consistent scaling across different feature types, enabling the model to train efficiently and make accurate predictions.

## Performance

### Classification Report:

| Class              | Precision | Recall | F1-Score | Support |
|--------------------|-----------|--------|----------|---------|
| 0 (Normal)         | 0.98      | 0.99   | 0.98     | 696     |
| 1 (Complex Partial)| 0.99      | 0.97   | 0.98     | 549     |
| 2 (Electrographic) | 0.99      | 0.99   | 0.99     | 137     |
| 3 (Video-detected) | 1.00      | 1.00   | 1.00     | 21      |

- Accuracy: 0.98 (1403 samples)
- Macro avg: 0.99 | 0.99 | 0.99 | 1403 samples
- Weighted avg: 0.98 | 0.98 | 0.98 | 1403 samples

- **Balanced Accuracy**: 0.9876
- **AUC-ROC Score**: 0.9983

## Interpretation of Model Results

### Classification Report:
- **Precision**: Indicates how many of the predicted positive instances were actually positive. For example, the model's precision for the **Normal** class (0) is 0.98, meaning that 98% of the samples predicted as normal were correctly identified.
- **Recall**: Shows how many of the actual positive instances were identified by the model. For **Complex Partial** (class 1), the recall is 0.97, meaning the model identified 97% of the true complex partial seizures.
- **F1-Score**: The harmonic mean of precision and recall. For **Electrographic** (class 2), the F1-score is 0.99, indicating a strong balance between precision and recall, which is important for detecting rare seizure types.
- **Support**: The number of actual occurrences of each class in the dataset, showing the distribution of seizure types.

- **Accuracy (0.98)**: The model correctly predicted the class for 98% of the samples, which is a strong indicator of its generalization capability.
- **Macro average**: Averages the precision, recall, and F1-score across all classes. It gives an overall measure of model performance without considering class imbalance.
- **Weighted average**: Takes into account the support of each class, providing a performance metric that is more reflective of the true distribution of the dataset.
  
### **Balanced Accuracy (0.9876)**:
This metric accounts for class imbalance by calculating the average of recall rates for each class. A balanced accuracy of 0.9876 indicates that the model performs well across all classes, especially in handling minority classes (like **Video-detected**, class 3).

### **AUC-ROC Score (0.9983)**:
The AUC-ROC score represents the area under the Receiver Operating Characteristic curve, which plots the true positive rate against the false positive rate. A value of 0.9983 is very close to 1, showing that the model is excellent at distinguishing between the different seizure classes and non-seizure data, even in cases of imbalance between the classes.

This combination of high precision, recall, F1-score, balanced accuracy, and AUC-ROC score suggests that the model is robust, effective, and highly reliable for classifying seizures based on EEG data.
