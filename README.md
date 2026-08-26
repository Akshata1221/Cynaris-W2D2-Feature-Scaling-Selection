# W2D2: Feature Scaling & Selection

## Overview

This project implements **feature engineering, categorical encoding, feature scaling, and feature selection** using Python and Scikit-learn.

The objective is to prepare student-performance data for machine learning by handling missing values, converting categorical features into numerical representations, scaling numerical features, and selecting the most relevant features.

---

## Learning Objectives

* Handle missing values using imputation.
* Check and remove duplicate records.
* Apply different categorical encoding techniques.
* Compare different feature scaling methods.
* Visualize feature distributions before and after scaling.
* Use `SelectKBest` to identify the top 5 features.
* Generate reusable CSV and visualization outputs.

---

## Project Structure

```text
W2D2 Feature Scaling & Selection/
│
├── data/
│   └── indian_student_performance.csv
│
├── src/
│   └── feature_scaling_selection.py
│
├── output/
│   ├── encoded_features.csv
│   ├── standard_scaled_features.csv
│   ├── minmax_scaled_features.csv
│   ├── robust_scaled_features.csv
│   ├── select_k_best_feature_scores.csv
│   ├── top_5_features.csv
│   └── *.png
│
└── README.md
```

---

## Dataset

The dataset contains **100 student records** with the following features:

* `Student_ID`
* `Gender`
* `City`
* `Study_Hours`
* `Attendance_Percent`
* `Math_Score`
* `Science_Score`
* `English_Score`
* `Performance_Level`

Missing values were found in:

* `Study_Hours`
* `Attendance_Percent`
* `Science_Score`

These numerical missing values were handled using **median imputation**.

Duplicate checking was also performed.

**Rows before:** 100
**Rows after:** 100
**Duplicates removed:** 0

---

## Encoding Techniques

### 1. LabelEncoder

Applied to:

`Gender`

Example:

```text
Female → 0
Male   → 1
```

**Advantage:** Simple and compact for binary or target-like categorical variables.

**Trade-off:** The numerical values can incorrectly suggest an order between categories. Therefore, it should not normally be used for nominal features with many categories.

---

### 2. OneHotEncoder

Applied to:

`City`

Generated features such as:

```text
City_Ahmedabad
City_Bengaluru
City_Chennai
City_Delhi
City_Hyderabad
City_Jaipur
City_Kolkata
City_Mumbai
City_Pune
```

**Advantage:** Does not impose an artificial numerical ordering.

**Trade-off:** Can significantly increase the number of columns when a categorical feature has many unique values.

---

### 3. OrdinalEncoder

Applied to:

`Performance_Level`

Mapping:

```text
Average   → 1
Good      → 2
Excellent → 3
```

**Advantage:** Appropriate when categories have a meaningful natural order.

**Trade-off:** Should not be used for nominal categories because the numerical ordering may introduce false relationships.

---

## Feature Scaling

Three scaling techniques were applied to the numerical features:

```text
Study_Hours
Attendance_Percent
Math_Score
Science_Score
English_Score
```

### StandardScaler

Transforms features to approximately:

```text
Mean = 0
Standard deviation = 1
```

Useful when features follow a relatively normal distribution.

**Limitation:** Sensitive to outliers because mean and standard deviation are affected by extreme values.

---

### MinMaxScaler

Transforms values to a range between:

```text
0 and 1
```

**Advantage:** Preserves the relative distribution and gives all features the same bounded range.

**Limitation:** Sensitive to outliers.

---

### RobustScaler

Uses the:

```text
Median
Interquartile Range (IQR)
```

**Advantage:** More resistant to outliers than StandardScaler and MinMaxScaler.

**Best choice:** When numerical data contains significant outliers.

---

## Visualization

The project generates plots showing:

* Feature distributions before scaling.
* Feature distributions after StandardScaler.
* Comparison of StandardScaler, MinMaxScaler, and RobustScaler.

Generated visualizations are stored in the `output/` directory.

---

## Feature Selection

`SelectKBest` was used to select the **top 5 numerical features** based on statistical scores.

### Feature Scores

| Feature            |  Score |
| ------------------ | -----: |
| Science_Score      | 796.69 |
| Attendance_Percent | 591.35 |
| Math_Score         | 530.29 |
| English_Score      | 350.16 |
| Study_Hours        | 226.43 |

### Top 5 Selected Features

```text
Study_Hours
Attendance_Percent
Math_Score
Science_Score
English_Score
```

### Why These Features Matter

* **Science_Score:** Highest statistical relationship with the target in the feature-selection test.
* **Attendance_Percent:** Indicates student participation and consistency.
* **Math_Score:** Represents academic performance in mathematics.
* **English_Score:** Provides another measure of academic achievement.
* **Study_Hours:** Represents the amount of time dedicated to studying and can contribute to academic performance.

---

## Output Files

The following outputs are generated:

```text
encoded_features.csv
standard_scaled_features.csv
minmax_scaled_features.csv
robust_scaled_features.csv
select_k_best_feature_scores.csv
top_5_features.csv
```

Visualization files include:

```text
*_before_scaling.png
*_after_standard_scaling.png
*_scaler_comparison.png
```

---

## Technologies Used

* Python
* NumPy
* Pandas
* Matplotlib
* Seaborn
* Scikit-learn
* Git
* GitHub

---

## How to Run

Activate the virtual environment:

```powershell
.\.venv\Scripts\Activate.ps1
```

Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

Run the feature engineering pipeline:

```powershell
python src\feature_scaling_selection.py
```

The processed files and visualizations will be generated in:

```text
output/
```

---

## Viva Questions

### 1. When should you use OneHotEncoder vs OrdinalEncoder?

**OneHotEncoder** should be used for nominal categories where there is no natural order, such as city names.

**OrdinalEncoder** should be used when categories have a meaningful order, such as Average < Good < Excellent.

---

### 2. Why does StandardScaler not work well with outliers?

StandardScaler uses the **mean and standard deviation**. Extreme values can strongly influence both, causing the scaled data to be distorted.

For datasets with outliers, **RobustScaler** is generally more suitable because it uses the median and IQR.

---

### 3. What is feature leakage and how do you prevent it?

Feature leakage occurs when information that would not be available at prediction time is accidentally used during model training.

To prevent leakage:

* Split the data into training and testing sets before preprocessing.
* Fit encoders and scalers only on training data.
* Apply the fitted transformations to validation/test data.
* Avoid using future or target-derived information as input features.

---

## Self-Review Checklist

* [x] Missing values handled
* [x] Duplicate records checked
* [x] LabelEncoder applied
* [x] OneHotEncoder applied
* [x] OrdinalEncoder applied
* [x] StandardScaler applied
* [x] MinMaxScaler applied
* [x] RobustScaler applied
* [x] Before/after scaling visualizations created
* [x] Scaler comparison visualizations created
* [x] SelectKBest applied
* [x] Top 5 features identified
* [x] CSV outputs generated
* [x] Code executed successfully
* [x] Git branch created
* [x] Changes committed
* [x] Changes pushed to GitHub

---

## Conclusion

The W2D2 task successfully demonstrates a complete feature preprocessing workflow. Missing numerical values were imputed, categorical variables were encoded using appropriate techniques, numerical features were transformed using three scaling methods, and `SelectKBest` was used to identify the most relevant features.

The resulting processed datasets and visualizations provide evidence that the feature scaling and selection pipeline was executed successfully.
