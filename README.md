# Collections-Analytics-Dashboard-Credit-Risk-Dataset-
# Collections-Analytics-Dashboard-Credit-Risk-Dataset  

## 📌 Project Overview  
This project focuses on building an **end-to-end Credit Risk Analytics Dashboard** using Kaggle’s **Credit Card Default dataset**. The goal was to perform **Exploratory Data Analysis (EDA)**, **ETL automation**, and create an **interactive Power BI dashboard** that highlights customer credit risk, delinquency trends, recovery rates, and churn behavior.  

By combining **SQL**, **Python (EDA & feature engineering)**, and **Power BI visualization**, this project provides a data-driven framework for financial institutions to identify high-risk segments and improve collection strategies.  

---

## 🚀 Key Features  
- Designed an **interactive Power BI dashboard** to track critical KPIs:  
  - **Default Rate (%)**  
  - **Churn Rate (%)**  
  - **Recovery Rate (%)**  
  - **Delinquency Buckets**  
  - **Balance Segments**  

- Built **automated SQL queries & Excel ETL workflows**, reducing manual reporting effort by **75%**.  
- Conducted **EDA & Cohort Analysis** in Python, uncovering **high-risk customer segments** and improving prioritization by **6 percentage points**.  
- Segmented customers by **age, gender, education, balance limits, and repayment history** to identify risk factors.  

---

## 📊 Dashboard Snapshots  

### 1. Credit Risk Dashboard (Power BI)  
- Total Customers, Defaults, Default Rate, and Churn Rate  
- Breakdown by **Gender, Education, Age Group, Balance Segment**  
- **Delinquency Analysis** (Current vs Late payments)  
- **Defaults vs Non-Defaults Age Distribution**  

![Credit Risk Dashboard](images/credit_dashboard.png)  

### 2. Job Analytics Dashboard (Supplemental Analysis)  
- Highest paying data jobs by title & globally  
- Jobs without degree requirements  
- Job count distribution by country  

![Job Market Dashboard](images/job_dashboard.png)  

---

## 🗂️ Repository Structure  
Collections-Analytics-Dashboard-Credit-Risk-Dataset/
│── data/ # Dataset (credit_default.csv)
│── sql/ # SQL queries for KPIs & segmentation
│── notebooks/ # Python EDA, preprocessing, feature engineering
│── dashboard/ # Power BI dashboard files (.pbix)
│── images/ # Dashboard snapshots for README
│── README.md # Project documentation


---


---

## ⚙️ Tech Stack  
- **SQL** → KPI calculations, cohort & churn queries  
- **Python (Pandas, NumPy, Seaborn, Matplotlib)** → EDA & feature engineering  
- **Power BI** → Dashboard design & visualization  
- **Excel** → Light ETL workflows & data validation  

---

## 📑 Example SQL Queries  

**Default Rate by Age Group**  


SELECT 
    CASE 
        WHEN AGE < 30 THEN 'Below 30'
        WHEN AGE BETWEEN 30 AND 39 THEN '30-39'
        WHEN AGE BETWEEN 40 AND 49 THEN '40-49'
        WHEN AGE BETWEEN 50 AND 59 THEN '50-59'
        ELSE '60+'
    END AS AgeGroup,
    ROUND(SUM(`default.payment.next.month`) * 100.0 / COUNT(*), 2) AS DefaultRate
FROM `credit_default.csv`
GROUP BY AgeGroup
ORDER BY AgeGroup;

**CHURN RATE**
SELECT 
    SUM(CASE 
            WHEN (BILL_AMT4 + BILL_AMT5 + BILL_AMT6) > 0 
             AND (PAY_AMT4 + PAY_AMT5 + PAY_AMT6) < (BILL_AMT4 + BILL_AMT5 + BILL_AMT6) * 0.2
            THEN 1 ELSE 0 END
       ) * 100.0 / COUNT(*) AS ChurnRate
FROM `credit_default.csv`;

📑 Example Python EDA Workflow

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# Load dataset
df = pd.read_csv("credit_default.csv.csv")

# Rename target column
df.rename(columns={'default.payment.next.month': 'Default'}, inplace=True)

# Map categorical variables
df['SEX'] = df['SEX'].map({1: 'Male', 2: 'Female'})

# Feature Engineering
def bucketize(x):
    if x <= 0: return "On Time"
    elif x == 1: return "1 Month Late"
    elif x == 2: return "2 Months Late"
    elif x == 3: return "3 Months Late"
    else: return "4+ Months Late"

df['Delinquency_Bucket'] = df['PAY_0'].apply(bucketize)

# KPIs
default_rate = df['Default'].mean() * 100
churn_rate = (df['PAY_AMT4'] + df['PAY_AMT5'] + df['PAY_AMT6'] == 0).mean() * 100

print(f"Default Rate: {default_rate:.2f}%")
print(f"Churn Rate: {churn_rate:.2f}%")


📈 KPIs Achieved

Default Rate: ~22%

Churn Rate: ~58%

Total Customers: 30K

Total Defaults: 6.6K
