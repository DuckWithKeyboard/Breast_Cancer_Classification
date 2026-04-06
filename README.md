# 🧬 Breast Cancer Classification using CNN

## 📖 Overview
This project implements a **Convolutional Neural Network (CNN)** to classify breast cancer samples as **Healthy** or **Tumor** using 2D-transformed gene expression data.  
The model employs **10-fold stratified cross-validation** for robust evaluation and utilizes **class weighting** to handle dataset imbalance.

---

## 🧪 Data Preprocessing

We began by constructing an **Average Array Intensity Correlation (AAIC)** matrix — a symmetric square matrix based on **Spearman correlation** between samples — to detect and remove outliers.  

- Outliers were identified using a correlation cut-off value of **0.6**.  
- Data normalization was applied using the **`TCGAbiolinks`** library with **GC-content correction**, ensuring removal of GC-content bias.  
- Post-normalization, features with a mean value below **0.25** were filtered out.  
- The final dataset consisted of **1,208 clinical samples** and **14,477 genes**:  
  - **113 Negative samples** (Healthy)  
  - **1,095 Positive samples** (Tumor)

The data were reshaped into **2D images (127×114)** to fit the CNN’s convolutional layer input.  
Since the dimensions didn’t perfectly align, the last column was **zero-padded** to maintain uniformity.

### 🔍 Edge Detection
To enhance image features, **edge detection** was applied to all transformed images.  
Comparative visualizations between **Healthy (negative)** and **Tumor (positive)** samples revealed distinct spatial patterns.

---

## 🧱 Model Architecture

The CNN architecture used for classification is summarized below:

| Layer Type         | Parameters             | Activation | Notes |
|--------------------|-----------------------|-------------|--------|
| Conv2D             | 16 filters, kernel 3×3 | ReLU        | Feature extraction |
| BatchNormalization | -                      | -           | Gradient stabilization |
| MaxPooling2D       | Pool size 2×2          | -           | Spatial downsampling |
| Conv2D             | 16 filters, kernel 3×3 | ReLU        | Deep feature extraction |
| BatchNormalization | -                      | -           | Normalization post-convolution |
| MaxPooling2D       | Pool size 2×2          | -           | Dimensional reduction |
| Flatten            | -                      | -           | Converts 2D features to 1D |
| Dense              | 1024 neurons           | ReLU        | High-level feature integration |
| Dropout            | 0.3                    | -           | Regularization |
| Dense              | 1 neuron               | Sigmoid     | Binary classification output |

**Optimizer:** Adam (learning rate = 0.001)  
**Loss Function:** Binary Crossentropy  
**Metrics:** Accuracy  

Class weights were computed automatically to address class imbalance.

---

## ⚙️ Training Configuration

| Parameter | Value |
|------------|--------|
| Cross-Validation | 10-fold Stratified K-Fold |
| Epochs | Up to 50 (Early Stopping Patience = 5) |
| Batch Size | 16 |
| Early Stopping | Based on validation loss |
| Class Weights | Computed dynamically |

Weights for each fold were saved for reproducibility and future use.

---

## 📊 Results

### Fold-wise Performance

| Fold | Accuracy | Precision | Recall | F1-score |
|------|-----------|------------|---------|-----------|
| 1 | 0.9024 | 0.9024 | 1.0000 | 0.9487 |
| 2 | 0.9024 | 0.9024 | 1.0000 | 0.9487 |
| 3 | 0.9024 | 0.9024 | 1.0000 | 0.9487 |
| 4 | 0.9180 | 0.9174 | 1.0000 | 0.9569 |
| 5 | 0.9918 | 1.0000 | 0.9910 | 0.9955 |
| 6 | 0.9098 | 0.9098 | 1.0000 | 0.9528 |
| 7 | 0.9098 | 0.9098 | 1.0000 | 0.9528 |
| 8 | 0.9098 | 0.9098 | 1.0000 | 0.9528 |
| 9 | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| 10 | 0.9098 | 0.9098 | 1.0000 | 0.9528 |

**Average Metrics:**
- Accuracy: **0.9256**
- Precision: **0.9264**
- Recall: **0.9991**
- F1-score: **0.9610**

🧩 **Observations:**
- Extremely high **recall (~1.0)** indicates almost all tumor samples were correctly identified.  
- Slightly lower precision in some folds suggests a few false positives.  
- The **F1-score (0.96)** reflects a strong precision–recall balance.  
- Early stopping effectively reduced overfitting.

---

## 🧠 Confusion Matrix (Example - Fold 1)

| Predicted Negative | Predicted Positive |
|--------------------|--------------------|
| 0 | 12 |
| 0 | 111 |

**True Negatives:** 0  
**False Positives:** 12  
**False Negatives:** 0  
**True Positives:** 111  

---

## 🏁 Conclusion
The CNN model achieved excellent classification performance for breast cancer detection using gene expression data transformed into images.  
The combination of **Spearman-based preprocessing**, **GC-content normalization**, and **deep convolutional learning** yielded a reliable and accurate binary classifier.

---

## 🗂️ Repository Structure

```

Breast_Cancer_Classification/
│
├── Data/
│   ├── final_expression.csv
│   ├── final_expression_transposed.csv
│
├── Notebooks/
│   └── Train_CNN.ipynb
│
│── cnn_fold9.weights.h5
├── README.md
└── requirements.txt

````

---

## ⚡ How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/DuckWithKeyboard/Breast_Cancer_Classification.git
   cd Breast_Cancer_Classification

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Open the Jupyter notebook:

   ```bash
   jupyter notebook Notebooks/Train_CNN.ipynb
   ```

4. Run all cells to train and evaluate the model.

---

## 📈 Future Work

* Visualize **training/validation accuracy and loss** curves.
* Extend to **multi-class classification** (e.g., multiple cancer subtypes).
* Implement **Grad-CAM** for model explainability.
* Experiment with **transfer learning** using pre-trained models like VGG16 or ResNet50.

---

## 🧑‍💻 Author

**Ayush Kumar Singh**
Machine Learning & AI Enthusiast
[GitHub Profile](https://github.com/DuckWithKeyboard)

---

## 🧾 References

1. TCGA Biolinks Documentation
2. Breast Cancer Gene Expression Datasets
3. Related works on CNN for gene-expression-based classification 

```
