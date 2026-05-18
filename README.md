# 🇰🇪 Kenya Cholera Early Warning System (K-CEWS)
**PROMAX Edition: AI-Driven Predictive Surveillance & Logistics Command**

> *"In public health, time is the only currency that matters. A 24-hour delay in response can result in an exponential surge in infections; a 14-day head start can save a city."*

---

## 📑 Table of Contents
1. [Executive Summary & Problem Statement](#1-executive-summary--problem-statement)
2. [Project Innovation & Benchmarks](#2-project-innovation--benchmarks)
3. [Data Architecture & Sources](#3-data-architecture--sources)
4. [Machine Learning & Data Engineering Pipeline](#4-machine-learning--data-engineering-pipeline)
5. [The ProMax Command Center (Streamlit App)](#5-the-promax-command-center-streamlit-app)
6. [Project Success Metrics](#6-project-success-metrics)
7. [Ethical Considerations & Governance](#7-ethical-considerations--governance)
8. [Complete Project Structure](#8-complete-project-structure)
9. [Project Team](#9-project-team)

---

## 1. Executive Summary & Problem Statement
### The Current Failure: Reactive Surveillance
Currently, epidemiological surveillance in Kenya relies on the Integrated Disease Surveillance and Response (IDSR) framework. While robust for tracking, it lacks predictive foresight. Emergency response (vaccine deployment and WASH kits) is triggered by "Case Spikes," meaning resources arrive after the peak of the transmission curve. 

**Core Issues Addressed:**
* **The Lag Gap:** Known correlations between hydro-climatic triggers (heavy rainfall/flooding) and cholera surges are not integrated into real-time health alerts.
* **Data Silos:** Environmental data (NASA), health data (WHO), and demographic data (KNBS) exist in isolation.
* **Resource Inefficiency:** Reactive responses are significantly more expensive and deadly than proactive intervention.

### The K-CEWS Solution
K-CEWS utilizes spatiotemporal machine learning to bridge the gap between "Climate Shocks" and the biological transmission cycle of *Vibrio cholerae*. By implementing **Feature Lagging (t-14 days)**, K-CEWS acts as a "Sentinel" that looks 14 days into the future, empowering the Ministry of Health to pre-position Oral Cholera Vaccines (OCV) and medical supplies before an outbreak peaks.

---

## 2. Project Innovation & Benchmarks
K-CEWS was designed by analyzing and improving upon global epidemiological predictive models:
* **vs. NASA-UNICEF Yemen Project:** Overcame the "War-Data Gap" by utilizing Kenya's highly stable KHIS/DHIS2 reporting systems for superior model verification.
* **vs. Kenya Red Cross sEAP Protocol:** Automates the "Human-Heavy" manual checking of rain gauges using continuous API and satellite data feeds.
* **vs. Nigeria ML Study (2026):** Introduces the crucial **14-Day Lead Time**, moving beyond predicting the *current* week to providing a true actionable foresight window.

---

## 3. Data Architecture & Sources
The system integrates four isolated data streams into a continuous geographic time-series dataset:

| Data Stream | Source | Local Files | Parameters Used |
| :--- | :--- | :--- | :--- |
| **Hydro-Climatic** | NASA POWER | `_raw.csv` files | IMERG Precipitation, T2M (Temp), RH2M (Humidity) |
| **Socio-Demographic**| KNBS | `subcounty_risk_factors.csv` | WASH Improved Water/Sanitation %, Baseline Risk Score (1-10) |
| **Geospatial** | HDX / OCHA | `ken_admin2.geojson` | Sub-County Administrative Boundaries (Admin 2) |
| **Epidemiological** | WHO / ReliefWeb| `actual_cases.csv` | Weekly Confirmed Cases, Binary Outbreak Trigger |

---

## 4. Machine Learning & Data Engineering Pipeline
*(Found in `K-CEWS.ipynb`)*

The core intelligence evaluates high-risk reservoirs, particularly the **Lake Victoria Basin** (Kisumu, Migori, Homa Bay) and **Nairobi’s informal settlements**.

### Data Engineering
* **Temporal Shifting:** Weather variables are lagged backward by exactly 14 days (`rainfall_lag_14`, `temp_lag_14`).
* **Rolling Windows:** Incorporates 14-day cumulative rainfall and average humidity to capture prolonged environmental saturation leading to waterborne spread.

### Modeling Strategy
The system evaluates multiple algorithms to handle epidemiological overdispersion (sudden case spikes):
1.  **Negative Binomial Regression:** Establishes the baseline for handling sparse count data.
2.  **Random Forest Regressor/Classifier:** Evaluates non-linear environmental relationships.
3.  **Optimized XGBoost (Production Engine):** Uses `scale_pos_weight=10` to heavily penalize the model for missing an outbreak, prioritizing **100% Outbreak Recall**.
4.  **Explainable AI (SHAP):** Transitions the model from a "Black Box" to a transparent tool, allowing the dashboard to explain *why* an area is flagged (e.g., rainfall exceeding the 90th percentile).

---

## 5. The ProMax Command Center (Streamlit App)
*(Found in `app.py`)*

The Streamlit frontend is engineered as a high-availability dashboard for Sub-County Health Management Teams (SCHMTs).

### Key Features:
* **Live Regional Risk Map:** An interactive Folium map dynamically color-coded by AI risk scores (Critical, Moderate, Low ).
* **Medical Logistics Estimator:** An algorithmic logistics calculator. It uses baseline risk scores to generate a population proxy, then multiplies it by the live AI Risk Score to instantly estimate required liters of Chlorine and units of ORS Kits.
* **14-Day Signature Trend:** Visualizes historical 14-day rainfall trends against predicted risk spikes.
* **Automated AI Memos:** Generates downloadable intelligence text reports for rapid sharing among health workers.
* **Data Engineering View:** Exposes model tournament results and environmental variable distributions to data scientists and stakeholders.

---

## 6. Project Success Metrics
The project is evaluated on strict quantitative and qualitative matrices:
* **Statistical:** Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE) to capture accuracy, with a primary focus on **Recall** (Sensitivity) to ensure zero missed outbreaks.
* **User Utility:** A health officer must be able to interpret the Risk Map in under 10 seconds.
* **Latency:** The system is designed to ingest new NASA data and update forecasts within a 5-minute window.

---

## 7. Ethical Considerations & Governance
K-CEWS adheres strictly to the **Data Protection Act (2019) of Kenya**:
* **De-identification:** Operates entirely at the Sub-County aggregate level. It does not ingest, process, or map individual household GPS coordinates or patient line lists.
* **Anti-Stigmatization:** Designed as a restricted Decision-Support Tool for health officials, not a public-facing fear-mongering map.
* **Algorithmic Equity:** Incorporates KNBS socio-demographic baselines so that rural areas with low historical reporting rates are not ignored by the model.

---

## 8. Complete Project Structure

```text
Kenya_Cholera_Early_Warning_System/
│
├── app.py                              # Streamlit Command Center Application
├── K-CEWS.ipynb                        # Master Data Engineering & ML Pipeline
├── requirements.txt                    # Python dependencies
├── README.md                           # Project documentation
├── .gitignore                          # Git ignore rules                  
│
└── csv/                                # Comprehensive Data Repository
    ├── full_geotemporal_dataset.csv    # Final engineered dataset (Notebook output)
    ├── kcews_live_predictions.csv      # Main prediction outputs driving the Streamlit app
    ├── ken_admin2.geojson              # Spatial mapping data for Folium
    ├── model_performance_comparison.csv# Algorithm tournament evaluation metrics
    ├── subcounty_risk_factors.csv      # KNBS Baseline WASH/Epi risk scores
    ├── historical_test_dataset.csv     # Historical baseline for evaluation
    ├── stakeholder_summary.csv         # High-level data for stakeholder reporting
    ├── NASA_Weather_Master.csv         # Consolidated historical weather data
    ├── Train_*_raw.csv                 # Raw training data sets (Nairobi, Migori, Kisumu)
    ├── Test_*.csv                      # Isolated testing data sets
    └── live_*.csv                      # Live/Recent data feeds
```
---

## 9. project-team
**Role & Core Responsibility**
* **Abel Aleu Chol Garang** - Project Lead & UI Developer|"Project architecture, Streamlit deployment, UI/UX logic."
* **Augustine Magani** - Technical Researcher,"Data sourcing, literature review, technical documentation."
* **Patience Chepkosgei** - Data Analyst,Exploratory Data Analysis (EDA) and Tableau Storytelling.
* **Carolyne Githenduka** - Data Engineer,"ETL processes, data cleaning, 14-day feature lagging."
* **Marcus Kaula** - Machine Learning Engineer,"Lead modeler (Negative Binomial, Random Forest, XGBoost)."
