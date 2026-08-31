# Rainforest Acoustic Monitoring – Audio Segmentation and MFCC Feature Extraction

## Overview

This repository contains the audio preprocessing pipeline developed for a rainforest acoustic monitoring project. The objective of the project is to process environmental audio recordings and extract meaningful acoustic features that can later be used for training a machine learning model.

The system focuses on identifying and classifying three major categories of sounds:

* **ANI** – Animal sounds
* **HUM** – Human sounds
* **ANT** – Anthropogenic sounds

The preprocessing pipeline consists of two main stages:

1. **Audio Segmentation using TextGrid annotations**
2. **MFCC Feature Extraction using Librosa**

The extracted features are intended to be used as input for a deep learning model such as a **BiLSTM** for sound classification.

---

## Project Workflow

```text
Raw Audio Files (.wav)
        │
        ▼
TextGrid Annotations
        │
        ▼
Audio Segmentation
        │
        ▼
Categorised Audio Segments
(ANT / ANI / HUM)
        │
        ▼
MFCC Feature Extraction
        │
        ▼
Feature Dataset
(X, y)
        │
        ▼
Machine Learning / BiLSTM Model
```

---

# 1. Audio Segmentation

The original audio recordings are accompanied by **TextGrid annotation files**. These annotation files contain time intervals corresponding to different sound events.

The segmentation code reads the `.wav` audio files and their corresponding `.TextGrid` files and extracts the annotated portions of the audio.

Each extracted segment is stored according to its respective class.

### Sound Classes

| Class | Description          | Label |
| ----- | -------------------- | ----: |
| ANT   | Anthropogenic Sounds |     0 |
| ANI   | Animal Sounds        |     1 |
| HUM   | Human Sounds         |     2 |

The segmented audio is organised into separate folders for each class.

### Output Structure

```text
segmented_audio/
│
├── ANT/
│   ├── segment_1.wav
│   ├── segment_2.wav
│   └── ...
│
├── ANI/
│   ├── segment_1.wav
│   ├── segment_2.wav
│   └── ...
│
└── HUM/
    ├── segment_1.wav
    ├── segment_2.wav
    └── ...
```

---

# 2. MFCC Feature Extraction

After segmentation, **Mel-Frequency Cepstral Coefficients (MFCCs)** are extracted from the audio segments.

MFCCs are widely used in audio and speech processing because they represent important frequency characteristics of an audio signal in a compact numerical form.

The implementation uses the **Librosa** library for loading audio files and extracting MFCC features.

### MFCC Configuration

```text
Number of MFCC coefficients: 40
```

The audio files are processed without intentionally changing their original sampling rate.

For each audio segment:

1. The audio file is loaded.
2. MFCC features are calculated.
3. The extracted MFCC matrix is converted into a suitable format for machine learning.
4. The corresponding numerical class label is assigned.

The final dataset consists of:

```text
X → MFCC Feature Data
y → Corresponding Class Labels
```

Example class mapping:

```python
{
    "ANT": 0,
    "ANI": 1,
    "HUM": 2
}
```

---

## Repository Structure

```text
rainforest-acoustic-feature-extraction/
│
├── Audio_Segmentation.ipynb
├── MFCC_Feature_Extraction.ipynb
│
├── README.md
├── requirements.txt
└── .gitignore
```

### Expected Dataset Structure

The code expects the dataset to be organised in a structure similar to:

```text
Audio_Project/
│
├── wav_files/
│   ├── audio_1.wav
│   ├── audio_2.wav
│   └── ...
│
├── textgrid_files/
│   ├── audio_1.TextGrid
│   ├── audio_2.TextGrid
│   └── ...
│
└── segmented_audio/
    ├── ANT/
    ├── ANI/
    └── HUM/
```

---

## Installation

Clone the repository:

```bash
git clone <your-repository-url>
cd rainforest-acoustic-feature-extraction
```

Install the required Python libraries:

```bash
pip install -r requirements.txt
```

---

## Requirements

The project uses the following Python libraries:

```text
librosa
numpy
pandas
tqdm
praatio
soundfile
```

You can include these dependencies in the `requirements.txt` file.

---

## Usage

### Step 1: Prepare the Dataset

Place the original `.wav` audio files inside:

```text
wav_files/
```

Place the corresponding TextGrid annotation files inside:

```text
textgrid_files/
```

Ensure that the audio and annotation filenames correspond correctly.

---

### Step 2: Run Audio Segmentation

Open and execute:

```text
Audio_Segmentation.ipynb
```

The notebook will:

* Read the audio files.
* Read the corresponding TextGrid annotations.
* Identify labelled time intervals.
* Extract the relevant audio segments.
* Save the segments into the `ANT`, `ANI`, and `HUM` folders.

---

### Step 3: Run MFCC Feature Extraction

After segmentation is completed, execute:

```text
MFCC_Feature_Extraction.ipynb
```

The notebook will:

* Load the segmented audio files.
* Extract 40 MFCC coefficients.
* Assign numerical labels to each sound category.
* Generate the feature dataset for machine learning.

---

## Technologies Used

* **Python**
* **Google Colab**
* **Librosa**
* **NumPy**
* **Pandas**
* **Praat / TextGrid**
* **TQDM**

---

## Future Work

The extracted MFCC features will be used for training and evaluating a deep learning model for acoustic sound classification.

The planned pipeline includes:

```text
MFCC Features
      │
      ▼
Dataset Preparation
      │
      ▼
Train / Validation / Test Split
      │
      ▼
BiLSTM Model
      │
      ▼
Sound Classification
      │
      ▼
ANI / HUM / ANT Detection
```

The final objective is to develop an **IoT-enabled rainforest acoustic monitoring system** capable of analysing environmental sounds and detecting potentially important events such as human activity and anthropogenic disturbances.

---

## Author

**Gaurav**

B.Tech – Electronics & Telecommunication Engineering

---

## License

This repository is currently intended for academic and research purposes.
