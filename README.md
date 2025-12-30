# Online Writer Identification using Gaussian Mixture Models  
*A Bachelor’s Thesis Project (2020–2021) — IIT Guwahati*

This repository documents my bachelor’s thesis on **online writer identification**, completed under the supervision of **Dr. Suresh Sundaram** at IIT Guwahati. The aim of the project is to automatically determine *who* wrote a piece of handwriting based on writing style alone, using **Gaussian Mixture Models (GMMs)** trained on online pen-trajectory data.

This repository includes the **final project report** and **presentation poster**, which describe the system design, methodology, experiments, and analysis.

---

## 🌟 Introduction

Each person has a unique handwriting style, shaped by their physical motion patterns, writing speed, pen pressure, stroke directions, and personal habits.  
This project focuses on identifying a writer using **online handwriting**, where the raw pen movement is captured as:

- **x-coordinate**
- **y-coordinate**
- **timestamp**

Online handwriting is richer than offline scanned images because it preserves **temporal dynamics**, making it especially suitable for writer identification tasks.

The system determines the most likely writer by analyzing stroke-level and point-level features and comparing them against statistical models built for each writer.

---

## 📚 Dataset  

The system is developed and evaluated using the **IAM-OnDB** online handwriting dataset, which contains:

- **196 unique writers**
- **8 sessions per writer**
- Each session contains multiple strokes defined as sequences of `(x, y, t)` points  
- The raw `.xml` files were processed into `.mat` files for easier handling

---

## 🛠 Data Preprocessing  

Raw online handwriting often contains noise due to:

- Infrared pen occlusions  
- Transmission errors  
- Irregular movement sampling  

Preprocessing steps included:

### ✔ Stroke segmentation  
Splitting a stroke whenever its direction changes by **more than 90°**, ensuring clearer modeling of local writing behavior.

### ✔ Stroke resampling  
Normalizing all strokes to **30 equally spaced points** to ensure fixed-size feature vectors.

---

## ✨ Feature Engineering  

Two feature sets were designed to capture writing characteristics.

---

### ### **1. Point-Based Features (for each of the 30 sampled points)**

These capture local movement patterns:

- **Speed** of pen movement  
- **Writing direction** using  
  - `cos(θ)` and `sin(θ)`  
- **Curvature** using  
  - `cos(ϕ)` and `sin(ϕ)`

These characterize how the pen moves at every tiny segment of the stroke.

---

### **2. Stroke-Level Features**

These describe global stroke shape:

- Accumulated stroke length  
- Total turning angle  
- Stroke **height**  
- Stroke **width**  
- Stroke **duration**

Together, these features form a **high-dimensional feature vector** (118–140 dimensions) for each stroke.

---

## 🧠 Modeling with Gaussian Mixture Models  

Each writer’s handwriting is modeled as a **probability distribution** of the extracted features.

The modeling process has two major stages:

---

### **1. Universal Background Model (UBM)**

- Train a single large GMM using **all writers' data**
- Captures general handwriting variability
- Provides stable starting parameters (means, covariances, mixture weights)

---

### **2. Writer-Specific GMMs via Adaptation**

Each writer receives a personalized GMM by **adapting** the UBM using only that writer’s strokes.

This adaptation is based on the Maximum A Posteriori (MAP) approach:

- Means are shifted towards the writer’s feature distribution  
- Covariances and mixture weights remain stable  

This produces distinct statistical “signatures” for each writer.

---

## 🔍 Identification Strategy  

During testing:

1. Extract features from a test paragraph  
2. Compute the **log-likelihood** of these features under each writer's GMM  
3. Choose the writer with the highest likelihood score

---

## 📊 Experimental Setup  

Three writer groups were tested:

- 5 writers  
- 15 writers  
- 30 writers  

Two feature sets were evaluated:

| Feature Set | Writers | Accuracy |
|-------------|---------|----------|
| Set 1 | 5 | 100% |
| Set 1 | 15 | 65% |
| Set 1 | 30 | 50% |
| Set 2 | 5 | 100% |
| Set 2 | 15 | 70% |
| Set 2 | 30 | 57% |

### Key Findings

- Accuracy decreases as the number of classes increases (expected).
- Feature Set 1 performs slightly better due to inclusion of stroke-level features.
- System reaches up to **100% accuracy** for small writer groups.

---

## 🧩 Conclusion  

- The GMM–UBM framework proved effective for distinguishing writer styles.
- Carefully engineered features capture both micro (point-wise) and macro (stroke-wise) handwriting behavior.
- The approach is robust, scalable, and generalizes well without requiring textual content.

---

## 📄 References  

1. Schlapbach, A., Liwicki, M., & Bunke, H. (2007). *A Writer Identification System for On-line Whiteboard Data.*  
2. Namboodiri, A., & Gupta, S. (2006). *Text Independent Writer Identification from Online Handwriting.*
