# 🧠 Human–Computer Interaction (HCI) EEG-Based Intent Recognition

This repository contains an end-to-end **Human–Computer Interaction (HCI)** pipeline built on **EEG brain signal acquisition, signal preprocessing, and machine learning / deep learning–based intent prediction**.

The core objective is to recognize **visual-cognitive intent** from EEG signals when subjects observe structured visual stimuli.

---

## 📌 Project Overview

This project investigates how **EEG brain signals** respond to different **visual semantic directions** (arrows, letters, and words) and how these signals can be transformed into structured data suitable for **ML/DL prediction models**.

The workflow includes:

- Controlled EEG data collection
- Signal preprocessing using **Chebyshev filtering**
- Temporal segmentation of EEG signals
- Classification using **LSTM** and **Backpropagation Neural Network (BPNN)**

---

## 🧪 Experimental Design

### 👥 Subjects

| Parameter | Value |
|-----------|-------|
| Number of subjects | 2 |
| Trials per subject | 5 |
| Total EEG recording sessions | 10 |

---

### 🖼️ Visual Stimuli (PPT Slideshow)

Each subject viewed a slideshow consisting of **12 distinct visual stimuli**, grouped as follows:

| Category | Direction | Label |
|----------|-----------|-------|
| Arrow | Left | Left Arrow |
| Arrow | Right | Right Arrow |
| Arrow | Forward | Forward Arrow |
| Arrow | Backward | Backward Arrow |
| Letter | Left | Left Letter |
| Letter | Right | Right Letter |
| Letter | Forward | Forward Letter |
| Letter | Backward | Backward Letter |
| Word | Left | Left Word |
| Word | Right | Right Word |
| Word | Forward | Forward Word |
| Word | Backward | Backward Word |

Each stimulus represents a **distinct cognitive-visual state**, later used as **classification labels**.

---

## 📊 EEG Data Collection

EEG signals were recorded while subjects viewed the slideshow. Raw EEG signals were noisy, high-dimensional, and temporally dense — requiring filtering and restructuring before modeling.

---

## ⚙️ Data Preprocessing

### 📓 `Chebyshev_filtering.ipynb`

This notebook performs **EEG signal preprocessing**, including noise reduction, frequency isolation, and tabular transformation of raw EEG data.

**Key Techniques Used:**

| Technique | Purpose |
|-----------|---------|
| Chebyshev Type Filter | Frequency band isolation |
| Signal smoothing | Artifact reduction |
| Temporal alignment | Synchronization across channels |
| Feature normalization | Scaling for ML ingestion |

**Output:** Cleaned and structured tabular EEG data, ready for segmentation and ML/DL model ingestion.

---

## 🧠 Machine Learning & Deep Learning Models

### 📓 `Implement_LSTM_BPNN.ipynb`

This notebook implements **two predictive models** to classify EEG segments into one of the **12 stimulus classes**.

### 🔁 Data Segmentation Strategy

One **random EEG segment** is sampled from the filtered dataset, and the model predicts **which of the 12 visual stimuli** the segment corresponds to.

---

### 🔬 Models Implemented

#### 1️⃣ Attention-Based LSTM

- Captures temporal dependencies in EEG signals
- Uses attention mechanisms for feature weighting


#### 2️⃣ Backpropagation Neural Network (BPNN)

- Fully connected neural network
- Performs strongly on structured tabular EEG data


---

## 📈 Results Summary

| Model | Accuracy |
|-------|----------|
| Attention-Based LSTM | 62.5% |
| BPNN | **92%** |

> **Observation:** BPNN outperformed LSTM significantly for this dataset, suggesting that well-filtered tabular EEG features were more effectively modeled using traditional neural networks than sequence-based architectures.

---

## 🗂️ Repository Structure

```
├── Chebyshev_filtering.ipynb       # EEG signal preprocessing and filtering
├── Implement_LSTM_BPNN.ipynb       # EEG intent classification using LSTM & BPNN
└── README.md
```

---

## 🚀 Applications

- Brain–Computer Interfaces (BCI)
- Assistive technologies
- Cognitive intent recognition
- Neuroadaptive interfaces
- EEG-driven HCI systems

---

## 🔮 Future Work

- Implementation of the Model into ROS Robotics
- Multimodal fusion (EEG + Eye Tracking)
- Real-time EEG prediction pipeline

---

## 🧠 Key Takeaway

This project demonstrates that **structured EEG preprocessing combined with appropriate neural models** can yield high-accuracy predictions of visual cognitive intent, paving the way for robust EEG-driven HCI systems.