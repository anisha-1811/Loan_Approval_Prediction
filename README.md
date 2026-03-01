# LoanIQ — Loan Approval Prediction

A machine learning project that predicts whether a loan application will be approved, paired with a polished web-based frontend for interactive predictions.

---

## Project Structure

| File | Description |
|------|-------------|
| `PROJECT.ipynb` | Jupyter Notebook containing full ML pipeline — EDA, data cleaning, model training & evaluation |
| `loan_approval_app.html` | Standalone frontend web app (LoanIQ) for submitting loan applications and viewing predictions |

---

## Notebook — `PROJECT.ipynb`

A complete end-to-end machine learning pipeline built on a loan dataset.

### Steps Covered

1. **Exploratory Data Analysis (EDA)**
   - Dataset overview (`info`, `describe`, null checks)
   - Target variable distribution (Loan Status)
   - Feature analysis: Credit History vs Loan Status, Applicant Income distribution
   - Correlation heatmap (before encoding)

2. **Data Cleaning**
   - Categorical missing values filled using **mode** (Gender, Married, Dependents, Self_Employed)
   - Numerical missing values filled using **median** (LoanAmount) and **mode** (Loan_Amount_Term, Credit_History)

3. **Feature Engineering & Encoding**
   - Created `Total_Income` from `ApplicantIncome + CoapplicantIncome`
   - Label Encoding applied to all categorical columns

4. **Train-Test Split & Scaling**
   - Stratified 80/20 train-test split
   - StandardScaler applied after splitting to prevent data leakage

5. **Model Training — Classification Algorithms**
   - Logistic Regression
   - Decision Tree
   - Random Forest
   - K-Nearest Neighbors (KNN)
   - Support Vector Machine (SVM)

6. **Evaluation**
   - Accuracy score & confusion matrix per model
   - ROC Curve comparison
   - 5-Fold Cross Validation

7. **Result**
   - **Random Forest Classifier** achieved the best accuracy and generalization across all evaluation metrics.

### Dependencies

```
pandas, numpy, matplotlib, seaborn, scikit-learn
```

Install with:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Dataset

The notebook reads from `loan.csv`. Update the file path in the notebook to match your local setup:
```python
df = pd.read_csv("path/to/loan.csv")
```

---

## Web App — `loan_approval_app.html`

A fully self-contained, single-file frontend application called **LoanIQ**.

### Features

- Clean dark-themed UI with animated background
- Input form collecting all relevant applicant details:
  - Gender, Marital Status, Dependents, Education, Employment type
  - Applicant & Co-applicant Income, Loan Amount & Term
  - Credit History, Property Area
- Instant loan approval prediction with visual feedback
- No backend or server required — runs directly in any browser

### How to Use

1. Open `loan_approval_app.html` in any modern web browser
2. Fill in the applicant details
3. Click **Predict** to see the loan approval result

> **Note:** The frontend is a UI demo. To connect it to the trained ML model, integrate it with a Python backend (e.g., Flask or FastAPI) that serves predictions from the saved model.

---

## How They Fit Together

```
loan.csv  →  PROJECT.ipynb  →  Trained Model
                                     ↓
                          loan_approval_app.html  (UI)
```

The notebook handles all data science work and produces the best model (Random Forest). The HTML app provides the user-facing interface that can be wired up to serve real-time predictions.
