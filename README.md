# 🚀 ETL Data Pipeline
# Import required libraries
import pandas as pd
from sklearn.preprocessing import LabelEncoder, StandardScaler

# Define ETL pipeline function
def etl_pipeline(input_file, output_file):

    # STEP 1: Extract → Load data
    df = pd.read_csv(input_file)

    # STEP 2: Select required columns
    df = df[['title', 'type', 'country', 'release_year', 'rating']]

    # STEP 3: Handle missing values
    df['country'] = df['country'].fillna("Unknown")
    df['rating'] = df['rating'].fillna("Not Rated")

    # STEP 4: Remove duplicates
    df.drop_duplicates(inplace=True)

    # STEP 5: Encode categorical columns
    le = LabelEncoder()
    df['type'] = le.fit_transform(df['type'])
    df['country'] = le.fit_transform(df['country'])
    df['rating'] = le.fit_transform(df['rating'])

    # STEP 5.5: Handle outliers using IQR method
    q1 = df['release_year'].quantile(0.25)
    q3 = df['release_year'].quantile(0.75)
    iqr = q3 - q1

    lower = q1 - 1.5 * iqr
    upper = q3 + 1.5 * iqr

    df = df[(df['release_year'] >= lower) & (df['release_year'] <= upper)]

    # STEP 6: Scale numerical column
    scaler = StandardScaler()
    df[['release_year']] = scaler.fit_transform(df[['release_year']])

    # STEP 7: Load → Save processed data
    df.to_csv(output_file, index=False)

    return df

# Run the pipeline
etl_pipeline("data.csv", "processed_data.csv")

## 📌 Project Overview
This project implements an automated **ETL (Extract, Transform, Load)** pipeline using Python, Pandas, and Scikit-learn.  
It processes raw data into a clean, structured format ready for analysis or machine learning.

---

## ⚙️ Features
- 📥 Data extraction from CSV file  
- 🧹 Data cleaning (missing values, duplicates)  
- 🔄 Feature transformation using Label Encoding  
- 📊 Scaling numerical data using StandardScaler  
- 🚫 Outlier detection and removal (IQR method)  
- 🤖 Fully automated pipeline using function  

---

## 🛠️ Technologies Used
- Python  
- Pandas  
- Scikit-learn  
- Jupyter Notebook  

---

## 📂 Project Structure
