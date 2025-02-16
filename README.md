# Hospital-Readmission

# **Step-by-Step Summary of the Report**

## **1. Feature Analysis**
The dataset was analyzed to understand key attributes and potential biases.

### **1.1 Race Distribution**
- Majority of patients are **Caucasian (~75,000)**, followed by **African American (~20,000)**.
- Other race categories such as **Asian, Hispanic, and "Other"** had significantly fewer records.
- Presence of missing values and placeholder **"?"** entries.
- This imbalance could skew model predictions, especially for underrepresented groups.

### **1.2 Gender Distribution**
- Slight imbalance in gender representation:
  - **Female patients (~55,000)**
  - **Male patients (~50,000)**
- The dataset includes more female patients, which might introduce gender-based bias in predictions.

### **1.3 Age Distribution**
- Majority of patients belong to **older age groups**:
  - **70-80 years** (largest category)
  - **60-70 years**
  - **80-90 years**
- The skew toward older patients suggests that findings and predictions may be **less applicable to younger populations**.

### **1.4 Weight Information**
- Most weight values are **missing or unknown**, making it difficult to use weight as a predictive feature.

### **1.5 Admission-Related Variables**
- **Admission Type (admission_type_id):** Majority of admissions are **emergency cases**, followed by urgent and elective.
- **Discharge Disposition (discharge_disposition_id):** Most patients are **discharged to home**.
- **Admission Source (admission_source_id):** The majority of admissions come from **Emergency Rooms**, followed by **physician referrals**.

---

## **2. Data Cleaning**
To ensure data quality, multiple preprocessing steps were performed.

### **2.1 Handling Missing Values**
- **Categorical variables** (e.g., medical_specialty, payer_code) were imputed with **most frequent category (mode)** or assigned an **"Unknown"** value.
- **Numerical variables** (e.g., time_in_hospital, num_lab_procedures) were imputed using the **mean/median**.
- **Weight** had too many missing values, so it was labeled as "Unknown."  

### **2.2 Outlier Detection and Treatment**
- Used **Interquartile Range (IQR) method** to detect extreme values in **time_in_hospital, num_lab_procedures, num_medications**.
- Unusual records (e.g., **hospital stays of 0 days**) were analyzed for possible data entry errors.

### **2.3 Removing Duplicates**
- Duplicate records were identified based on **encounter_id** and **patient_nbr**.
- Duplicates were removed to prevent data redundancy and bias.

---

## **3. Data Transformation**

### **3.1 Categorical Encoding**
- Applied **one-hot encoding** to categorical variables such as **race, gender, and admission type**.
- Used **label encoding** for ordinal variables like **age groups** (e.g., [0-10) → 0, [10-20) → 1).

### **3.2 Normalization and Standardization**
- Applied **Min-Max Scaling** to scale numerical variables like **time_in_hospital**.
- Used **Standardization (Z-score normalization)** for variables like **num_medications**.

---

## **4. Feature Engineering**

### **4.1 Interaction Features**
- Created new variables capturing interactions between features, e.g.:
  - **Age Group × Num_Medications**.
  - **Number of Inpatient Visits × Diabetes Medication Status**.

### **4.2 Readmission Label Binarization**
- Converted **readmission categories (<30, >30, No)** into a **binary classification** problem:
  - **Readmitted within 30 days → 1**
  - **Not readmitted → 0**

---

## **5. Dimensionality Reduction**

### **5.1 Correlation Analysis**
- Generated a correlation matrix to remove highly correlated features.
- **Example:** num_lab_procedures and num_procedures were found to be redundant.

### **5.2 Principal Component Analysis (PCA)**
- Used PCA to reduce dimensionality while retaining most variance.

---

## **6. Data Splitting**
- Split dataset into **training (70%)**, **validation (15%)**, and **test (15%)**.
- Used **stratified sampling** to maintain class balance.

---

## **7. Predictive Analytics & Model Training**

### **7.1 Models Tested**
1. **Logistic Regression** (Baseline)
2. **Random Forest**
3. **Gradient Boosting Models (XGBoost, LightGBM, CatBoost)**

### **7.2 Hyperparameter Tuning**
- Used **GridSearchCV** to optimize model parameters.
- Performed **10-fold cross-validation**.

---

## **8. Model Evaluation**

### **8.1 Performance Metrics**
- **Accuracy, Precision, Recall, F1-Score, ROC-AUC**.

### **8.2 Model Comparison**
| Model | Accuracy | Precision | Recall | F1-Score | AUC |
|--------|---------|----------|--------|----------|------|
| Logistic Regression | 61.6% | 63.9% | 54.2% | 58.6% | 0.618 |
| Random Forest | 60.5% | 60.6% | 61.1% | 60.8% | 0.668 |
| XGBoost | 60.1% | 60.7% | 58.3% | 59.5% | 0.655 |
| LightGBM | 61.4% | 62.1% | 59.3% | 60.7% | 0.673 |
| **CatBoost** | **61.6%** | **62.3%** | **59.0%** | **60.6%** | **0.674** |

---

## **9. Feature Importance Analysis**
The **most influential features** identified by CatBoost were:
1. **Num_lab_procedures (28%)**
2. **Num_medications (24%)**
3. **Age (18%)**
4. **Time_in_hospital (15%)**
5. **Medical_specialty (10%)**

---

## **10. Final Model Selection & Conclusion**
- **CatBoost was chosen** as the final model due to **highest AUC, precision-recall balance, and robustness**.
- Achieved **65% accuracy** in identifying readmissions.
- The model insights can be used to **develop intervention strategies** for high-risk patients.

---
