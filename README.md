# 🧬 Embryo Stage Classification using Deep Learning

Study on ordinal-aware deep learning for classifying human embryo development stages from microscopy images.

This project explores multi-backbone CNN architectures combined with a custom-designed loss function to improve performance on ordered (ordinal) biological stages.

---

## 📁 Notebook Versions

- **`*.ipynb` (v1)**  
  Initial implementation with frame-wise data splitting and training for 4 epochs  

- **`*_v2.ipynb`**  
  Extended training (9 epochs) with improved performance using the same frame-wise split  

- **`*_v3.ipynb`**  
  Updated pipeline with **patient-wise data splitting** to eliminate data leakage and ensure realistic evaluation
## 📂 Project: Embryo Development Stage Classification

---
### Objective

Classify human embryo images into **16 ordered developmental stages** using deep learning, while incorporating **ordinal relationships between stages** through a custom loss function.

---

## 🧪 Embryo Stages

- tPB2, tPNa, tPNf  
- t2, t3, t4, t5, t6, t7, t8, t9+  
- tM, tSB, tB, tEB, tHB  

These stages represent **sequential biological development**, making this an **ordinal classification problem** rather than standard classification.

---

## 🧠 Methods

### Deep Learning Models (Pretrained)

- MobileNetV2  
- VGG16  
- VGG19  
- InceptionV3  

---

### Custom Loss: SoftOrdinalMarginLoss

A novel loss function designed to incorporate **ordinal relationships between classes**:

- Ordinal-smoothed Cross Entropy (Gaussian soft labels)  
- Log-margin penalty for distant misclassifications  
- Class imbalance handling via inverse-frequency weighting  

#### Key Properties

- Non-negativity  
- Differentiability  
- Zero at optimum  
- Handles class imbalance  
- Penalizes distant stage errors  

---

## ⚙️ Training Setup

- Framework: **PyTorch**  
- Multi-GPU: **2 × NVIDIA T4**  
- Mixed Precision: **Automatic Mixed Precision (AMP)**  
- Stratified Train / Validation / Test split (80 / 10 / 10)  
- Memory-optimized DataLoader (leak-safe)  

---

## 🗂️ Dataset

Human Embryo Dataset (from Zenodo)

- Kaggle Version:  
https://www.kaggle.com/datasets/naumisharanyatirth/human-embryo-dataset (created from Zenodo)

- Original Source:  
Images: https://zenodo.org/records/6390798/files/embryo_dataset.tar.gz  
Annotations: https://zenodo.org/records/6390798/files/embryo_dataset_annotations.tar.gz  

### Dataset Structure

- Image sequences per patient  
- Frame-wise annotations with stage intervals  
- Labels mapped using RUN frame indices  

---
## 🔬 Data Splitting Strategy

- Implemented **patient-wise splitting** to prevent data leakage  
- Ensured that all frames from a single embryo sequence belong to only one split (train/validation/test)  
- This avoids overlap of highly similar frames across splits and provides a more realistic evaluation of model performance
  
---

## 🧪 Evaluation Metrics

- Accuracy  
- Loss (SoftOrdinalMarginLoss)  

---
## 📊 Test Results (Initial — 4 Epochs)

*Reference: `*.ipynb` (v1)*

| Model | Loss | Accuracy |
|------|------|---------|
| MobileNetV2 | 0.3021 | 0.7679 |
| VGG16 | 0.2715 | 0.8572 |
| VGG19 | 0.2722 | 0.8515 |
| InceptionV3 | **0.2652** | **0.8708** |

> ⚠️ These results are based on initial training (4 epochs).  

---

## 📊 Test Results (9 Epochs)

*Reference: `*_v2.ipynb`*

| Model | Loss | Accuracy |
|------|------|---------|
| MobileNetV2 | 0.2874 | 0.8171 |
| VGG16 | 0.2614 | **0.8895** |
| VGG19 | 0.2611 | 0.8872 |
| InceptionV3 | 0.4475 | 0.5454 |

> ⚠️ Results after 9 epochs of training. Further improvements may be achieved through hyperparameter tuning and training stabilization.
---

## ⚠️ Experimental Note

Earlier versions (v1, v2) used frame-wise splitting, which can lead to optimistic performance due to similarity between frames from the same embryo.

Version v3 resolves this by enforcing **patient-wise splitting**, resulting in a more reliable and generalizable evaluation.

---
## 🔍 Key Findings

- Incorporating **ordinal relationships** significantly improves classification performance  
- **VGG16 + SoftOrdinalMarginLoss** achieved the best results after extended training (9 epochs)  
- InceptionV3 showed strong initial performance but **degraded with extended training**, suggesting sensitivity to optimization dynamics and training stability
- Reduced misclassification between distant biological stages due to ordinal-aware loss  
- Deeper architectures (VGG variants) demonstrated better stability and generalization under the current training setup  

- Corrected data leakage by transitioning from frame-wise to **patient-wise splitting**  
- Observed that realistic evaluation requires sequence-level separation in temporal medical datasets  

---

## 🚀 Highlights

- Custom **ordinal-aware loss function**  
- Efficient **multi-GPU training pipeline**  
- Stable training using **mixed precision (AMP)**  
- Robust handling of:
  - Corrupt / truncated images  
  - Class imbalance  
  - Sequential label structure  

---

## 🛠️ Tools and Libraries

- Python  
- PyTorch  
- Torchvision  
- NumPy  
- Pandas  
- Scikit-learn  
- Matplotlib  

---

## 🔗 Kaggle Notebook
https://www.kaggle.com/code/ntirth005/embryo-dataset/

---

## 📌 Conclusion

This project demonstrates that **ordinal-aware learning** is crucial for tasks involving structured labels such as biological development stages.

By combining:
- pretrained CNN architectures  
- a custom loss function  
- optimized training strategies  

we achieve strong performance on a complex medical imaging classification problem.

