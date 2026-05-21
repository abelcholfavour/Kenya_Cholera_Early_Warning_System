# Kenya Cholera Early Warning System (K-CEWS)
**AI-Driven Predictive Surveillance for Proactive Sub-County Intervention**

![K-CEWS Cover](Image/coverImg.png)

> *Transitioning clinical workflows from a state of reactive crisis response to proactive, anticipatory mitigation. A 14-day structural head start turns emergency logistics into a calculated containment strategy.*

[![Repository Link](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github)](https://github.com/abelcholfavour/Kenya_Cholera_Early_Warning_System/tree/main)
[![Streamlit App](https://img.shields.io/badge/Streamlit-App-FF4B4B?style=for-the-badge&logo=Streamlit)](https://kenyacholeraearlywarningsystem-hjgbvn8ghqpvex4fcxcykj.streamlit.app/)
[![Tableau Dashboard](https://img.shields.io/badge/Tableau-Dashboard-E97627?style=for-the-badge&logo=Tableau)](https://public.tableau.com/app/profile/patience.diana/viz/K-CEWS/Story1)


## **Table of Contents**

1. [Executive Summary & Problem Statement](#1-executive-summary--problem-statement)
2. [Project Innovation & Benchmarks](#2-project-innovation--benchmarks)
3. [Data Architecture & Sources](#3-data-architecture--sources)
4. [Machine Learning & Data Engineering Pipeline](#4-machine-learning--data-engineering-pipeline)
5. [The K-CEWS Interactivity Framework](#5-the-k-cews-interactivity-framework)
6. [Project Success Metrics](#6-project-success-metrics)
7. [Ethical Considerations & Governance](#7-ethical-considerations--governance)
8. [Complete Repository Structure](#8-complete-repository-structure)
9. [Project Team & Task Force](#9-project-team--task-force)


## **1. Executive Summary & Problem Statement**

### **The Critical Issue: Reactive Surveillance**
Epidemiological monitoring in Kenya under the Integrated Disease Surveillance and Response (IDSR) framework triggers alerts only *after* infected patients visit health clinics. This introduces a **10–14 day reactive lag** between environmental water contamination and real-world deployment. Consequently, vital supplies like Oral Cholera Vaccines (OCV) are distributed defensively after case spikes have already climbed, often missing the peak transmission containment window.

### **The K-CEWS Solution**
K-CEWS bridges this gap by mapping regional hydro-climatic anomalies directly onto the biological timeline of *Vibrio cholerae*. By implementing a **14-day chronological feature lag ($t-14$)**, the system acts as an automated predictive sentinel looking two weeks into the future. This grants the Ministry of Health and local logistics management teams a vital window to pre-position water-treatment assets before outbreaks manifest clinically.


## **2. Project Innovation & Benchmarks**

K-CEWS addresses systemic blind spots found in traditional global and regional surveillance networks:

* **The Science of Predictive Surveillance:** *Vibrio cholerae* proliferation spikes under clear hydro-climatic conditions: high surface temperatures, elevated humidity, and heavy rainfall anomalies (**NASA IMERG Precipitation**) that wash contaminants into unprotected local water systems. K-CEWS captures this 14-day biological incubation window to generate actionable alerts.
* **vs. NASA-UNICEF Yemen Project:** Overcomes structural "War-Data Gaps" and regional data fragmentation by localizing satellite intelligence to the precise **Sub-County level (Admin 2)** and leveraging Kenya's stable **KHIS/DHIS2** framework for high-fidelity training validation.
* **vs. Ethiopia's Climate-Sensitive Disease Framework:** Eliminates hardware vulnerabilities. Ethiopia's physical sentinel stations face operational data blackouts during severe weather or local conflict. K-CEWS is entirely **Infrastructure-Independent**, preserving 100% forecasting visibility via remote-sensing NASA satellite telemetry.
* **vs. South Sudan's WHO EWARS Framework:** Replaces reactive threshold reporting. EWARS triggers only *after* human clinical case confirmation. K-CEWS provides an anticipatory **"Lead-Time Engine"** that flags high-risk statuses 14 days prior to clinical onset, enabling vaccine prepositioning at vital border transit hubs.
* **vs. Kenya Red Cross sEAP Protocol:** Automates slow, manual observations. The Simplified Early Action Protocol checks physical rain gauges against simple linear thresholds. K-CEWS removes human reporting latency by analyzing multi-variable interactions (heat, rain, humidity, sanitation) simultaneously using machine learning.
* **vs. Regional Academic ML Studies:** Transitions past "Current Week" tracking models. K-CEWS implements a hardcoded **14-Day Feature Lag** to transform basic descriptive diagnostics into a functional, forward-looking operational forecast.

### **Innovation Matrix**

| Limitation in Previous Work | Historic Context | K-CEWS Strategic Innovation |
| :--- | :--- | :--- |
| **Reactive Tracking** | South Sudan / Nigeria | **Anticipatory AI:** Pinned 14-day predictive window for early response. |
| **Physical Infrastructure Gaps** | Ethiopia / Red Cross | **Satellite Intelligence:** Conflict-proof surveillance via NASA remote sensing. |
| **Data Fragmentation** | Yemen / Global Projects | **Centralized Dashboard:** Real-time data fusion with automated briefs. |
| **Academic Isolation** | Research Papers | **Streamlit Command Center:** Production-ready interface for field officers. |

## **3. Data Architecture & Sources**

The platform integrates four distinct environmental and clinical data streams into a continuous geospatial time-series dataset covering 10 years:

| Data Stream | Primary Source | Monitored Parameters | Strategic Value |
| :--- | :--- | :--- | :--- |
| **Hydro-Climatic** | NASA POWER API | IMERG Precipitation, Surface Temp, Relative Humidity | Isolates environmental triggers accelerating pathogen growth and water contamination. |
| **Socio-Demographic** | KNBS / Census Archives | WASH Coverage %, Sanitation Quality, Base Vulnerability Index | Weighs incoming climate triggers against historical localized community vulnerabilities. |
| **Geospatial Vector** | UN OCHA / HDX Portal | Administrative Level 2 Boundaries (`.geojson`) | Renders precise sub-county mapping boundaries for spatial visualization. |
| **Epidemiological** | WHO / Health Repositories | Historical Clinical Case Logs | Establishes the definitive historical ground-truth training vector for classification modeling. |


## **4. Machine Learning & Data Engineering Pipeline**

The predictive core targets key regional risk zones, specifically highlighting the **Lake Victoria Basin** (Kisumu, Migori counties) and **Nairobi's high-density informal settlements**.

### **Data Engineering Execution**
* **Chronological Lag Shifts:** Hydro-climatic observations are shifted forward by **14 days** (`.shift(14)`) to align environmental anomalies with biological incubation cycles and clinical reporting rates.
* **Rolling Structural Windows:** Generates rolling 14-day cumulative averages to detect sustained, high-risk environmental baseline states.
* **Imbalance Correction (SMOTE):** Addresses heavy dataset skew caused by the low frequency of active cholera outbreaks over time. The pipeline applies Synthetic Minority Over-sampling Technique (SMOTE) strictly within the historical training fold (2015–2019) to prevent model sensitivity from favoring baseline background noise.

### The Evolutionary Tournament: Empirical Model Selection
To solve the data imbalance challenge, K-CEWS deployed a multi-stage **Evolutionary Tournament**, benchmarking four distinct algorithm families across an out-of-sample validation era consisting of **13,032 rows of unseen historical data (2020–2024)**. Because the public health cost of missing an active transmission window is catastrophic, our primary optimization target was **Class 1 Recall (Sensitivity)**.

| Tournament Stage | Algorithm Family | Outbreak Precision | Outbreak Recall (Sensitivity) | Overall Accuracy | Operational Verdict & Justification |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **Stage 1: Baseline** | Negative Binomial GLM | 59.0% | 65.0% | 69.0% | **Failed:** Limited by rigid linear assumptions; missed complex climate interactions. |
| **Stage 2: Logic** | Decision Tree Classifier | 54.0% | 68.0% | 66.0% | **Failed:** Captured non-linear boundaries but exhibited extreme variance. |
| **Stage 3: Ensemble** | Random Forest Classifier | **60.0%** | 63.0% | **70.0%** | **Insufficient:** High accuracy, but too conservative to catch rapid outbreak surges. |
| **Stage 4: Champion** | **Optimized K-CEWS Engine** | 49.0% | **90.0%** | 61.0% | **🏆 WINNER:** Intentionally balanced gradient-boosted architecture. Captures 90% of out-of-sample threats. |

### Overcoming Asymmetric Public Health Risk
* **Cost-Sensitive Learning:** We implemented an internal cost-sensitive weight multiplier of **3.5** (`scale_pos_weight`) inside our gradient-boosted tree architecture. This mathematically penalizes the objective loss function for false negatives, prioritizing true outbreak windows over standard background variations.
* **Proactive Surveillance Calibration (35% Threshold):** Rather than using a standard default 50% classification cutoff, the inference layer uses an operational warning threshold of **35% (0.35 probability)**. 
  * **The Precision Trade-Off (49%):** Roughly half of the alerts may materialize as false alarms, which represents a manageable investment of preventative resources (e.g., distributing water purification tablets during heavy rain).
  * **The Sensitivity Benefit (90%):** By accepting a lower alert rate, the system successfully intercepts **90% of all out-of-sample epidemiological waves** up to 14 days in advance, directly overcoming the critical reactive lag.


## **5. The K-CEWS Interactivity Framework**

### Production Streamlit Command Center (`app.py`)
A live dashboard built for Sub-County Health Management Teams, featuring:
* **Dynamic Geotemporal Risk Map:** Renders sub-county administrative boundaries directly on an interactive map, color-coded into Critical, Moderate, and Low risk thresholds.
* **Logistics Resource Estimator:** An algorithmic calculator that cross-references local population baselines with active risk metrics to estimate real-time logistics needs (e.g., liters of chlorine compound, oral rehydration salt kits).
* **Environmental Trend Viewer:** Maps continuous 14-day weather changes alongside predicted risk scores to visualize climate-to-risk correlations.

### **Executive Analytics Dashboard (Tableau Storytelling)**
Complementing the web app, the executive [Tableau Dashboard](https://public.tableau.com/app/profile/patience.diana/viz/K-CEWS/Story1) provides comprehensive analytical storytelling workflows, historical outbreak trend comparisons, macro-level risk mapping, and executive briefings tailored for public health decision-makers.


## **6. Project Success Metrics**

The K-CEWS system is continually audited against strict public health performance standards:
1. **Empirical Sensitivity (Recall): 90.0%** – Achieved on 13,032 rows of unseen, out-of-sample historical data (2020–2024) to ensure rapid outbreak waves are captured.
2. **Actionable Horizon: 14 Days** – Delivers a minimum two-week logistics deployment window between environmental alerts and clinical presentation spikes.
3. **Operational Latency:** Ingests raw automated satellite feeds and renders system-wide geographic risk updates in under 5 minutes.


## **7. Ethical Considerations & Governance**

K-CEWS is engineered to comply fully with the **Kenya Data Protection Act of 2019**:
* **Structural De-identification:** Functions entirely at the sub-county aggregate layer. No individual patient line-lists, personal identities, or specific household coordinates are processed.
* **Surveillance Equity:** Integrates baseline demographic indices so that areas with lower clinical reporting rates remain fully visible to the predictive model, preventing regional gaps in resource routing.



## 8. Complete Repository Structure

```text
Kenya_Cholera_Early_Warning_System/
├── image/                                # Logos, documentation assets, and notebook covers
├── CSV & Data Files/                     # Core data repository (CSV & GeoJSON)
│   ├── full_geotemporal_data.csv         # Unified master dataset across space and time
│   ├── Ken_admin2.geojson                # Geographic boundaries for mapping (County/Sub-County)
│   ├── KCEDW_live_prediction.csv         # Live model outputs and risk predictions
│   ├── NASA_weather_master.csv           # Compiled historical and live weather features
│   ├── model_performance_comparison.csv  # Evaluation metrics across tested ML models
│   ├── stakeholder_summary.csv           # Aggregated high-level risk metrics for decision-makers
│   ├── sub_county_risk_factors.csv       # Granular vulnerability and risk weights
│   ├── historical_test_datasets.csv      # Baseline evaluation datasets
│   ├── train_Nairobi.csv                 # Training data - Nairobi
│   ├── train_Kisumu.csv                  # Training data - Kisumu
│   ├── train_Migori.csv                  # Training data - Migori
│   ├── test_Nairobi.csv                  # Testing data - Nairobi
│   ├── test_Kisumu.csv                   # Testing data - Kisumu
│   ├── test_Migori.csv                   # Testing data - Migori
│   ├── live_Nairobi.csv                  # Real-time inference data - Nairobi
│   ├── live_Kisumu.csv                   # Real-time inference data - Kisumu
│   └── live_Migori.csv                   # Real-time inference data - Migori
├── .gitignore                            # Standard git exclusion configurations
├── app.py                                # Core script for the Streamlit web application
├── Notebook.ipynb                        # End-to-end data science pipeline (EDA to Modeling)
├── Presentation.pdf                      # Stakeholder and project pitch deck
├── README.md                             # Project documentation (This file)
└── requirements.txt                      # Application dependencies and Python packages
## **9. Project Team & Task Force**

The **Kenya Cholera Early Warning System (K-CEWS)** was engineered and deployed by a dedicated cross-functional task force from **Moringa School, Nairobi, Kenya**, bringing together expertise across data engineering, predictive modeling, geospatial analysis, and user-experience design.

### **Task Force Breakdown**

| Name & Profile | Strategic Project Role | Core Architectural Responsibilities |
| :--- | :--- | :--- |
| **Abel Aleu Chol Garang** | **Project Lead & UI Developer** | Overall system architecture layout, end-to-end multi-page session state management, and implementation of the Streamlit live command dashboard interface. |
| **Augustine Magani** | **Technical Researcher** | Strategic data sourcing, historic case literature benchmarking, global framework review, and statutory public policy documentation. |
| **Patience Chepkosgei** | **Data Analyst** | Comprehensive geospatial exploratory data profiling, feature-to-target mapping, and orchestration of the interactive executive Tableau Storyboard. |
| **Carolyne Githenduka** | **Data Engineer** | Automated extraction pipelines for raw NASA weather files, data cleaning layers, and implementation of the critical $t-14$ chronological feature lag shifts. |
| **Marcus Kaula** | **Machine Learning Engineer** | Construction of the model evaluation tournament framework across all four analytical families, synthetic data balancing (SMOTE), and XGBoost champion engine tuning. |
