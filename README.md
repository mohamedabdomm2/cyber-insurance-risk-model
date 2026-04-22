# 🛡️ Cyber Insurance Risk Prediction Pipeline

## Project Overview
This project builds an end-to-end data pipeline and machine learning model to predict the likelihood of a company filing a cyber insurance claim.
By integrating firmographic data with technical security posture metrics, the model identifies high-risk industries and key predictors of security incidents.

## 📊 Process Flow
Below is the architectural flow of the data from the raw PostgreSQL database to the final predictive model:

graph TD
    subgraph "Data Layer (PostgreSQL)"
        DB[(Cyber Insurance DB)]
        T1[Firmographics]
        T2[Security Posture]
        T3[Incident History]
        T4[Data Exposure]
    end

    subgraph "ETL & Wrangling (Python/SQLAlchemy)"
        P1[SQL JOIN via company_id]
        P2[Handle Data Leakage]
        P3[Multicollinearity Check]
        P4[Feature Engineering]
    end

    subgraph "Exploratory Data Analysis (EDA)"
        E1[Correlation Heatmaps]
        E2[Industry Risk Pivot Tables]
        E3[Class Distribution Analysis]
    end

    subgraph "Machine Learning Pipeline"
        M1[Ordinal Encoding]
        M2[Train/Val/Test Split]
        M3[Hyperparameter Tuning]
        M4[Decision Tree Model]
    end

    DB --> P1
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> E1
    E1 --> E2
    E2 --> M1
    M1 --> M2
    M2 --> M3
    M3 --> M4
    M4 --> Results[Predictive Insights]

## 🛠️ Tech Stack
- **Database:** PostgreSQL (Relational Data Storage)
- **Language:** Python 3.12
- **Libraries:** - `Pandas` & `NumPy` (Data Manipulation)
    - `SQLAlchemy` & `psycopg2` (Database Connectivity)
    - `Scikit-Learn` (Machine Learning & Pipelines)
    - `Seaborn` & `Matplotlib` (Visualization)
    - `Category Encoders` (Feature Engineering)

## 🗄️ Data Engineering
Data was extracted from 4 relational tables. The `wrangle` function implements several critical R&D steps:
- **Leakage Prevention:** Removed features known only *after* a breach (e.g., `total_loss_val`, `records_stolen`).
- **Multicollinearity:** Dropped highly correlated features like `employee_count` to stabilize the model.
- **Database Integration:** Utilized SQLAlchemy engines to bypass `PrettyTable` rendering limitations in local environments.

## 📈 Key Insights from EDA
- **Healthcare Risk:** The Healthcare sector shows a significantly higher claim rate (~58%) compared to the baseline (~35%).
- **Size vs Risk:** Annual revenue and PII count are primary drivers of claim frequency.
- **Security Posture:** Companies using specific backup types and having lower security scores showed a measurable increase in claim probability.

## 🤖 Model Performance
I implemented a **Decision Tree Classifier** within a Scikit-Learn Pipeline.
- **Validation Strategy:** 3-way split (Train/Validation/Test).
- **Optimization:** Performed hyperparameter tuning on `max_depth` to prevent overfitting.
- **Training Accuracy:** 0.78%
- **Validation Accuracy:** 0.77%

## 🚀 How to Run
1. Ensure a PostgreSQL instance is running with the `cyber_insurance` database.
2. Update the connection string in `readind_sql_data.ipynb`.
3. Run the SQL notebook to generate `cleaned_cyber_insurance_data.csv`.
4. Run `insurance_model.ipynb` to train and evaluate the model.

---
**Author:** Muhammed Abdo 
**Field:**  Data Science
