# EEG-Based Motor Imagery Classification Using MNE-Python

A beginner-friendly EEG machine learning project that explores classifying imagined left- and right-fist movements using EEG signals, signal processing, feature extraction, and logistic regression.

##  Project Overview

The goal of this project is to build a basic machine learning pipeline that distinguishes between two types of motor imagery:

- **Left:** Imagining moving the left fist.
- **Right:** Imagining moving the right fist.

The project uses EEG data from the PhysioNet EEG Motor Movement/Imagery Dataset and Python libraries to process the signals and train a classification model.

This project was developed as a learning exercise to understand the fundamentals of EEG processing and machine learning.


## 📊 Dataset

**Dataset:** [EEG Motor Movement/Imagery Dataset — PhysioNet](https://physionet.org/content/eegmmidb/1.0.0/)

The dataset contains EEG recordings collected during different motor and motor imagery tasks.

For this project, the imagined left- and right-fist movement task was selected.

- **Participant:** S025
- **Recording:** S025R04
- **File format:** EDF (European Data Format)
- **Sampling frequency:** 160 Hz
- **Selected channels:** C3, Cz, and C4
- **Number of epochs used:** 15
- **Class distribution:** 8 left trials and 7 right trials in the initial feature-label setup

The dataset was accessed through a Kaggle copy of the PhysioNet dataset.



## Project Workflow

### 1. Load the EEG Data

The EDF recording is loaded using MNE-Python to access the EEG signals, channel information, sampling frequency, and event annotations.

### 2. Extract Events and Create Epochs

Event annotations are converted into events. Epochs are then created around the imagined left- and right-fist movement markers.

- `T1`: Imagined left-fist movement
- `T2`: Imagined right-fist movement

Each epoch covers 0 to 4 seconds after the event marker.

### 3. Filter the EEG Signals

A band-pass filter from **8 to 30 Hz** is applied to focus on the mu and beta frequency bands, which are commonly studied in motor imagery research.

The filtered epochs are used for feature extraction.

### 4. Select EEG Channels

Three channels are selected:

- **C3:** Located over the left sensorimotor region.
- **Cz:** Located near the central midline.
- **C4:** Located over the right sensorimotor region.

These channels are relevant to studying sensorimotor activity during imagined hand movements.

### 5. Extract Features Using Mean-Squared Amplitude

Each epoch contains multiple voltage samples for each selected channel.

For each channel in each epoch, the mean squared amplitude is calculated:

\[
P = \frac{1}{N}\sum_{i=1}^{N}x_i^2
\]

Where:

- \(P\) is the mean-squared amplitude feature.
- \(N\) is the number of samples in the epoch.
- \(x_i\) is an EEG voltage sample.

This calculation produces one feature per channel per epoch.

Therefore, each epoch is represented by three features:

- C3 mean-squared amplitude
- Cz mean-squared amplitude
- C4 mean-squared amplitude

The resulting feature matrix has three columns, with one row per epoch.

**Note:** These features are mean-squared amplitudes of the filtered EEG signals, not averaged evoked responses.

### 6. Prepare Labels

Each epoch is assigned a class label:

- `0`: Imagined left-fist movement
- `1`: Imagined right-fist movement

The features and labels are combined into the dataset used by the classifier.

### 7. Split the Dataset

The feature dataset is split into training and testing sets.

- **Training set:** 10 epochs
- **Testing set:** 5 epochs
- **Test size:** Approximately 33%
- **Random state:** 42
- **Stratification:** Used to preserve class proportions as closely as possible

### 8. Train the Classification Model

A logistic regression classifier from Scikit-learn is trained using the training features and their corresponding labels.

The model learns patterns in the extracted features that may help distinguish imagined left- and right-fist movements.

### 9. Evaluate the Model

The trained model predicts the labels of the held-out testing epochs.

Accuracy is calculated by comparing the predicted labels with the actual labels.
.
