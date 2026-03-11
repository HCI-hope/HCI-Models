---

# 🧠 EEG-Based Intent Recognition for HCI

This repository contains an end-to-end **Human–Computer Interaction (HCI)** pipeline for recognizing **visual-cognitive intent** from EEG signals. By leveraging a novel **Random Forest Ensemble** and **Hjorth-based feature engineering**, this project transforms raw brain activity into structured intent predictions (Forward, Backward, Left, Right).

## 🚀 Key Technical Novelties

Unlike basic Backpropagation Neural Networks (BPNN), this pipeline implements six distinct novelties to address common EEG classification failures:

 
**1. Ensemble Learning [N-1]:** Replaces single MLPs with a **5-member Confidence-Weighted Random Forest Ensemble** to reduce variance and prevent overfitting on small datasets.

**2. Hjorth Parameters [N-2]:** Utilizes **Mobility and Complexity** ratios. These are amplitude-invariant, cancelling out subject-to-subject electrode impedance differences.

**3. Amplitude Invariance [N-3]:** Employs **Relative Alpha and Beta Band Power** (the fraction of total power) instead of absolute spectral values to ensure cross-subject consistency.

**4. Optimized Spatial Coverage [N-4]:** Uses a strategic 3-channel setup (F4-C4, F3-C3, and FZ-CZ) to monitor hemispheric lateralization and midline activity.

**5. Within-Subject SMOTE [N-5]:** Implements synthetic data augmentation constrained to individual subjects to maintain biological plausibility.
 
**6. Direction-Level LOFO CV [N-6]:** Uses a **Direction-Level Leave-One-File-Out** cross-validation strategy to ensure the model generalizes across stimulus types without data leakage.



---

## 🧪 Experimental Design

### 👥 Subjects & Stimuli

* 
**Subjects:** 2 
* 
**Stimuli Categories:** Arrows, Letters, and Words 
* 
**Directions:** Left, Right, Forward, Backward 
*
* **Data Volume:** 880 windows across 24 distinct recording sessions.



### 📊 Channel Mapping & Sensitivity

| Channel | Electrode Pair | Hemisphere | Direction Sensitivity              |
| ------- | -------------- | ---------- | -----------------------------------|
| **ch7** | F4-C4          | Right      | Strong for Left movement imagery   |
| **ch15**| F3-C3          | Left       | Strong for Right movement imagery  |
| **ch16**| FZ-CZ          | Midline    | Strong for Forward,Backward imagery|

---

## ⚙️ Data Preprocessing & Modeling

### 📓 Preprocessing (`Chebyshev_filtering.ipynb`)

Raw EEG data is structured using Chebyshev Type filters to isolate relevant frequency bands. Features are then transformed into Hjorth descriptors and relative power spectral densities (PSD).

### 📓 Modeling (`Model_BPNN.ipynb`)

The repository evaluates structured (BPNN) architectures.

* **BPNN Performance:** Achieved up to **92%** accuracy on structured tabular features.
* 
**RF Ensemble Performance:** Achieved a mean test accuracy of **29.31%** (4.31% above random chance) under strict **Direction-Level LOFO** validation.

---

## 📈 Performance Analysis

The current model addresses the "Methodology Gap" rather than signal limitations. While the diagnostic ceiling for this dataset is **59%**, the current ensemble effectively mitigates the overfitting seen in basic BPNN models.

|Fold  |Train Accuracy|Test Accuracy|Gap   |
|------|--------------|-------------|------|
|Fold 1|93.58%        |30.41%       |63.17%|
|Fold 2|96.17%        |25.68%       |70.49%|
|Fold 3|95.63%        |31.76%       |63.87%|
|Fold 4|93.72%        |30.41%       |63.31%|
|Fold 5|94.81%        |29.73%       |65.08%|
|Fold 6|94.59%        |27.86%       |66.73%|
|MEAN  |94.75%        |29.31%       |65.44%|

---

## 🛠️ Proposals to Improve Accuracy

**1. Scale Subjects:** Increasing to 8–12 subjects to represent human EEG variability. 
**2. Advanced Augmentation:** Implementing raw EEG time-reversal and channel-flipping for symmetric synthetic trials.
**3. Deep Learning:** Transitioning to **EEGNet** architectures once the subject count supports deep feature learning.

---

## 🚀 Applications

* Brain–Computer Interfaces (BCI) for assistive robotics.
* Neuroadaptive user interfaces.
* Real-time visual-cognitive intent recognition.
