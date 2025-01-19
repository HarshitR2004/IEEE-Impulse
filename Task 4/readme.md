

# EEG Signal Analysis for Seizure Detection

## Overview
This project aims to analyze Electroencephalogram (EEG) signals to detect and classify various seizure types. The analysis pipeline includes data loading, preprocessing, visualization, and statistical evaluation of EEG signals across multiple seizure categories, with a focus on identifying distinct patterns that characterize different seizure events.

## Project Steps

### 1. Data Loading and Preprocessing
- Automated loading of `.npy` files from categorized seizure folders for efficient data handling.
- Organization of EEG data into numpy arrays for easier manipulation and analysis.
- Label encoding applied to categorize different seizure types, facilitating machine learning model training.

### 2. Visualization Capabilities
The project provides comprehensive visualization tools to understand EEG signals better:
- **Individual Channel Plotting**: Visualizes EEG signal data from each channel for different seizure types.
- **Superimposed Channel Visualization**: Combines data from multiple channels to visualize their interactions and behaviors during seizures.
- **Multi-channel Comparison**: Allows for side-by-side comparisons of multiple channels across different seizure categories, helping to identify channel-specific patterns.
- **Time-Series Visualization**: Plots time-series data with customizable parameters, enabling the inspection of specific time windows of interest.

### 3. Statistical Analysis
A detailed set of statistical metrics is computed for each EEG channel, helping to quantify seizure patterns:
- **Mean Amplitude**: Measures the average signal strength across time.
- **Zero Crossing Rate (ZCR)**: Calculates how frequently the signal changes polarity, indicating activity level.
- **Signal Range**: Represents the difference between the maximum and minimum values of the signal, reflecting its dynamic range.
- **Energy**: Measures the total energy in the signal, highlighting active periods during seizures.
- **Root Mean Square (RMS)**: Calculates the average magnitude of the signal, useful for identifying intensity during seizures.
- **Variance**: Measures the dispersion of the signal values, reflecting the consistency or fluctuation of the EEG data.

## Key Findings

### Normal EEG Characteristics
- **Lower Zero Crossing Rate (ZCR)**: Indicates fewer signal transitions, reflecting calm brain activity.
- **Reduced Energy and RMS**: Normal EEG signals typically exhibit low energy and RMS values, representing stable and less intense activity.
- **Minimal Variance and Range**: The signal exhibits low variance and a narrow range, showing steady brain function without significant fluctuations.

### Seizure Patterns
1. **Complex Partial Seizures**
   - **Higher ZCR values**: These seizures display frequent polarity changes, indicating irregular neural firing.
   - **Increased energy**: Notable spikes in energy levels in channels 8, 9, 11, and 14 suggest active brain regions during the seizure.
   - **Significant signal fluctuations**: The signal exhibits larger fluctuations, corresponding to abnormal neural activity.

2. **Electrographic Seizures**
   - **High energy and RMS**: Channels 1, 2, and 12 show notable energy increases, signifying intense brain activity.
   - **Distinct activity in channels 12 and 13**: These channels exhibit strong signal changes during seizure events.
   - **Lower ZCR**: Compared to other seizure types, Electrographic Seizures have a lower ZCR, indicating a more stable signal pattern.

3. **Video-detected Seizures**
   - **High mean values in channels 1 and 2**: These channels display higher signal amplitudes during seizure events.
   - **Elevated energy levels**: Specific channels show increased energy during seizures, reflecting intense brain activity.
   - **Frequent polarity transitions**: The signal experiences frequent transitions, suggesting erratic brain function during the seizure.








