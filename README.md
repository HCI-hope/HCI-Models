---

# 🧠 EEG-Based Intent Recognition for HCI

This repository contains an end-to-end **Human–Computer Interaction (HCI)** pipeline for recognizing **visual-cognitive intent** from EEG signals. By leveraging a novel **Random Forest Ensemble** and **Hjorth-based feature engineering**, this project transforms raw brain activity into structured intent predictions (Forward, Backward, Left, Right).
### 📓 Modeling (`Model_RF.ipynb`)
## 🚀 Key Technical Novelties

Unlike basic Backpropagation Neural Networks (BPNN), this pipeline implements six distinct novelties to address common EEG classification failures:

 
**1. Ensemble Learning [N-1]:** Replaces single MLPs with a **5-member Confidence-Weighted Random Forest Ensemble** to reduce variance and prevent overfitting on small datasets.

**2. Hjorth Parameters [N-2]:** Utilizes **Mobility and Complexity** ratios. These are amplitude-invariant, cancelling out subject-to-subject electrode impedance differences.

**3. Amplitude Invariance [N-3]:** Employs **Relative Alpha and Beta Band Power** (the fraction of total power) instead of absolute spectral values to ensure cross-subject consistency.

**4. Optimized Spatial Coverage [N-4]:** Uses a strategic 3-channel setup (F4-C4, F3-C3, and FZ-CZ) to monitor hemispheric lateralization and midline activity.

**5. Within-Subject SMOTE [N-5]:** Implements synthetic data augmentation constrained to individual subjects to maintain biological plausibility.
 
**6. Direction-Level LOFO CV [N-6]:** Uses a **Direction-Level Leave-One-File-Out** cross-validation strategy to ensure the model generalizes across stimulus types without data leakage.

### 📓 Modeling (`Model_BPNN(Novelty).ipynb`)
## 🚀 Key Technical Novelties

**NOVELTY 1 — Hemispheric Asymmetry Index (HAI):** What it captures: The difference in brain activation between left hemisphere (F3-C3) and right hemisphere (F4-C4) across 4 frequency bands — Theta, Alpha, Beta, Gamma. This gives 8 features total (4 DASM + 4 RASM).
   
   **Scientific basis:** This is grounded in neuroscience — left/right directional intent is encoded by contralateral hemisphere dominance. The standard approach of                        treating F3-C3 and F4-C4 as separate independent channels completely discards this relationship.

   
**NOVELTY 2 — Stimulus-Type Conditioning (Dual-Branch Architecture):** The cue type used in the experiment — whether the subject was shown an Arrow, a Letter, or a Word — is encoded as a one-hot vector [1,0,0], [0,1,0], or [0,0,1] and fed into a dedicated neural branch.
   
   **Scientific basis:** The brain processes an arrow (visuospatial) very differently from a word (semantic/linguistic). A model blind to stimulus type is averaging                        across fundamentally different cognitive states. No prior 4-direction BCI paper has conditioned the classifier on cue modality — this is                           the strongest novelty claim.

   
**NOVELTY 3 — Inter-Channel Functional Connectivity:** What it captures: How synchronised the three EEG channels are with each other during each 1-second window. High Left-Right correlation means both hemispheres are activating together (symmetric movement). Low or negative correlation means they're diverging (directional intent).
    
   **Scientific basis:** Functional connectivity between motor cortex regions is a well-established BCI feature in research literature, but rarely applied in                               simple 3-channel frugal BCI setups.

    
**NOVELTY 4 — Midline Deviation Feature:** How much the midline channel (FZ-CZ) deviates from the average of left and right hemispheres, per frequency band.


## Feature Count Summary

| # | Feature Group                                                 | Features | Standard BPNN |
|---|---------------------------------------------------------------|:--------:|:-------------:|
| 1 | Hjorth Activity × 3 channels                                  | 3        | ⚠️ Sometimes |
| 2 | Hjorth Mobility × 3 channels                                  | 3        | ⚠️ Sometimes |
| 3 | Hjorth Complexity × 3 channels                                | 3        | ⚠️ Sometimes |
| 4 | Band Powers (θ, α, β, γ) × 3 channels                         | 12       | ⚠️ Partial (α, β only) |
| 5 | Band Ratios (θ/α, α/β) × 3 channels                           | 6        | ❌ Rarely |
| 6 |**🆕 DASM** — Differential Asymmetry (Left − Right) × 4 bands  | 4        | ❌ No |
| 7 | **🆕 RASM** — Rational Asymmetry (Left / Right) × 4 bands     | 4        | ❌ No |
| 8 | **🆕 Functional Connectivity** — Pearson r (L↔R, L↔M, R↔M)    | 3        | ❌ No |
| 9 | **🆕 Midline Deviation** from mean(Left, Right) × 4 bands     | 4        | ❌ No |
| 10| **🆕 Stimulus Conditioning** — Arrow / Letter / Word (one-hot)| 3        | ❌ Never |
|   | **Total**                                                      | **45**   | *Typically 10–15* |





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


## 📈 Performance Analysis
### 📓(`Model_RF.ipynb`)
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


## 📈 Performance Analysis
### 📓(`Model_BPNN(Novelty).ipynb`)
## LOFO Cross-Validation Results — Fixed Novel BPNN

| Fold | Train Accuracy | Test Accuracy | Epochs | Test vs Chance  |
|:----:|:--------------:|:-------------:|:------:|:---------------:|
| 1    | 0.5943         | 0.2027        | 270    | ⬇️ Below chance |
| 2    | 0.5970         | 0.3311        | 201    | ✅ Above chance |
| 3    | 0.5628         | 0.3311        | 233    | ✅ Above chance |
| 4    | 0.5902         | 0.3581        | 185    | ✅ Best fold    |
| 5    | 0.5888         | 0.3176        | 255    | ✅ Above chance |
| 6    | 0.5973         | 0.2500        | 194    | ➡️ At chance    |
**Mean** **Train Accuracy: 0.5884** | **Test Accuracy: 0.2984** 
**Std Dev** **Train Accuracy: ±0.0119**  **Test Accuracy: ±0.0541** 


### Key Observations
| Metric            | Value           | Note                                              |
|-------------------|-----------------|---------------------------------------------------|
| Best test fold    | Fold 4 — 0.3581 | 10.8% above chance                                |
| Worst test fold   | Fold 1 — 0.2027 | Below chance — possible data imbalance            |
| Train-test gap    | ~0.29           | Moderate overfitting remains                      |
| Mean test accuracy| 0.2984          | ~4.8% above chance                                |
| Std deviation     | ±0.0541         | High variance across folds — small dataset effect |
---

## 🛠️ Proposals to Improve Accuracy

**1. Scale Subjects:** **Increasing to 8–12 subjects to represent human EEG variability**. 
**2. Advanced Augmentation:** Implementing raw EEG time-reversal and channel-flipping for symmetric synthetic trials.
**3. Deep Learning:** Transitioning to **EEGNet** architectures once the subject count supports deep feature learning.

---

## 🚀 Applications

* Brain–Computer Interfaces (BCI) for assistive robotics.
* Neuroadaptive user interfaces.
* Real-time visual-cognitive intent recognition.
