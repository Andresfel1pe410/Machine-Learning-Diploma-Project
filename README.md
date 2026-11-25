# Machine Learning & Data Science — Applied Healthcare Portfolio

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![MongoDB Atlas](https://img.shields.io/badge/MongoDB-Atlas-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/atlas)
[![GeoPandas](https://img.shields.io/badge/GeoPandas-Spatial%20Analytics-139C5A)](https://geopandas.org/)
[![Plotly](https://img.shields.io/badge/Plotly-Interactive%20Viz-3F4F75?logo=plotly&logoColor=white)](https://plotly.com/)

An end-to-end **Data Science, Big Data, and Machine Learning** portfolio developed for the **Machine Learning and Data Science Diploma** (*Facultad de Ingeniería, Universidad Nacional de Colombia*).

Across three applied projects in **public health and preventive medicine**, this repository demonstrates production-minded data engineering, statistical inference, NoSQL cloud pipelines, geospatial analytics, and multi-class supervised learning.

---

## Projects at a Glance

| Module | Project | Core Stack | Key Outcome | Artifacts |
| :--- | :--- | :--- | :--- | :--- |
| **1. Data Analytics & Viz** | Cardiovascular Mortality in Bogotá (2015–2025) | `Pandas`, `SciPy`, `Statsmodels`, `GeoPandas`, `Plotly` | Analyzed **47,542** mortality records across 20 districts; built animated choropleth maps & interactive treemaps | [Notebook](Notebooks/FINALPROYECTO_MLDS_Aldana_Martinez_Nieto.ipynb) · [Slides](Presentation/PROYECTO%20MLDS_%20ANALISIS%20Y%20VISUALIZACION%20DATOS.pptx) |
| **2. Big Data Engineering** | Cloud NoSQL Epidemiological Pipeline | `MongoDB Atlas`, `PyMongo`, `Pandas`, `Matplotlib` | Ingested **47,542** records into MongoDB Atlas and built server-side Aggregation Framework pipelines | [Notebook](Notebooks/PROYECTO_BIG_DATA__Aldana_Nieto_Martinez%20(1).ipynb) · [Slides](Presentation/_PROYECTO%20MLDS_%20BIG%20DATA.pptx) |
| **3. Machine Learning** | Multi-Class Obesity Level Prediction | `scikit-learn`, `GridSearchCV`, `Pipelines`, `SciPy` | Benchmarked **7 classifiers** across 7 obesity classes; achieved **94.32% accuracy** & **0.9419 F1-macro** (SVM) | [Notebook](Notebooks/PROYECTO_ML__Aldana_Nieto_Martinez%20(2).ipynb) · [Slides](Presentation/_PROYECTO%20MLDS_%20ML.pptx) |

---

## 1. Data Analysis & Geospatial Visualization — Cardiovascular Mortality in Bogotá

**Objective:** Deliver a reproducible epidemiological analysis of premature cardio-cerebrovascular disease (CVD) mortality (ages 30–70) in Bogotá D.C. to support territorial public health prioritization.

- **Data Source:** Bogotá Health Observatory (SaluData / DANE / RUAF-ND) — **47,542 records**, **12 features** (2015–2025).
- **Data Engineering & Standardization:** Normalized text encodings, accents, and categorical schemas (`SEXO_STD`, `LOCALIDAD_STD`); detected 957 exact duplicates and 351 unmapped districts; mapped raw ICD-10 codes into actionable clinical groups.
- **Statistical & Spatial Analysis:**
  - Computed 95% confidence intervals (Student's *t* for mean age; Wilson score intervals for cause proportions).
  - Ran hypothesis tests (Welch's *t*-test, $\chi^2$, Cramér's *V*) and OLS/ANOVA regression models.
  - Built static/animated **choropleth maps** (`GeoPandas`, `Shapely`) and **interactive treemaps** (`Plotly`).

**Key Findings:**
- **Leading Causes (ICD-10):** Ischemic heart diseases (`I20–I25`) account for **63.27%** of deaths, followed by cerebrovascular (`I60–I69`, **27.06%**) and hypertensive diseases (`I10–I15`, **8.96%**).
- **Demographic Profile:** **62.13% male** vs. **37.87% female** ($\chi^2 = 1461.2$, $p < 0.001$, Cramér's $V = 0.184$ between sex and cause); mean age at death is **60.12 years** (peak age bracket: 65–69 at 31.03%).
- **Temporal & Territorial Trends:** Mortality spiked sharply in **2020–2021** before declining through 2024; excluding the city-wide aggregate (`00 - Bogotá`), the highest burden concentrates in **Kennedy, Suba, Engativá, Ciudad Bolívar, and Bosa**.

---

## 2. Big Data — Cloud NoSQL Pipeline with MongoDB Atlas

**Objective:** Replace flat-file epidemiological processing with a flexible, scalable NoSQL cloud architecture capable of multi-year ingestion and server-side analytical querying.

- **Architecture:** **MongoDB Atlas** (`southamerica-east` cluster) connected to Python via `PyMongo`, decoupling storage and aggregation from local notebook memory.
- **Pipeline Highlights:**
  - Bulk-ingested **47,542 JSON documents** into collection `cronicas_ecv`.
  - Executed server-side data curation (`delete_many`) to remove **23,771** aggregated city-level records (`00 - Bogotá`), isolating **23,771** district-level records for unbiased spatial analysis.
  - Built **MongoDB Aggregation Framework** pipelines (`$addFields`, `$switch`, `$group`, `$sort`) to dynamically classify ICD-10 groups inside the database and run multidimensional rollups by year, health subnetwork, district, sex, and age group.

---

## 3. Machine Learning — Multi-Class Obesity Level Prediction

**Objective:** Build and benchmark supervised classification models to predict an individual's obesity level across **7 balanced categories** based on eating habits, physical condition, and demographics.

- **Dataset:** [UCI ML Repository (ID 544)](https://archive.ics.uci.edu/dataset/544/estimation+of+obesity+levels+based+on+eating+habits+and+physical+condition) — **2,111 survey records** from Mexico, Peru, and Colombia (**2,087** after removing 24 duplicates; 0 missing values; 16 predictors).
- **Feature & Association Analysis:** Identified `Weight` ($r = 0.39$) and `Age` ($r = 0.24$) as top numerical correlates, and `Gender` (Cramér's $V = 0.56$) and `family_history_with_overweight` ($V = 0.54$) as the strongest categorical predictors ($p < 0.001$).
- **Modeling Pipeline:** End-to-end `scikit-learn` `Pipeline` + `ColumnTransformer` (`StandardScaler` + `OneHotEncoder`) tuned via **`GridSearchCV`** with **`StratifiedKFold`** cross-validation.

### Model Benchmark Results

| Classifier | Best CV F1-Macro | Test Accuracy | Test F1-Macro |
| :--- | :---: | :---: | :---: |
| **SVM (RBF Kernel)** | 0.9460 | **94.32%** | **0.9419** |
| **Neural Network (`MLPClassifier`)** | **0.9530** | 94.01% | 0.9382 |
| **Decision Tree** | 0.9374 | 93.38% | 0.9331 |
| **Random Forest** | 0.9432 | 91.80% | 0.9180 |
| **Logistic Regression** | 0.9322 | 91.80% | 0.9168 |
| **K-Nearest Neighbors (KNN)** | 0.8667 | 88.33% | 0.8785 |
| **Gaussian Naive Bayes** | 0.4802 | 52.05% | 0.4698 |

- **Interactive Inference:** Includes an interactive CLI survey (`encuesta_obesidad()`) that feeds live user inputs directly into the trained Neural Network pipeline for real-time prediction.

---

## Repository Structure

```text
Machine-Learning-Diploma-Project/
├── Notebooks/
│   ├── FINALPROYECTO_MLDS_Aldana_Martinez_Nieto.ipynb        # Module 1: Data Analysis & Geospatial Viz
│   ├── PROYECTO_BIG_DATA__Aldana_Nieto_Martinez (1).ipynb    # Module 2: MongoDB Atlas & Aggregation Pipelines
│   └── PROYECTO_ML__Aldana_Nieto_Martinez (2).ipynb          # Module 3: Supervised ML Classification
├── Presentation/                                             # Executive slide decks (.pptx) for each module
├── Certificados/                                             # Diploma & course completion certificates (.pdf)
└── README.md
```

---

## Quick Start

```bash
git clone https://github.com/Andresfel1pe410/Machine-Learning-Diploma-Project.git
cd Machine-Learning-Diploma-Project
pip install numpy pandas matplotlib seaborn plotly geopandas shapely scipy statsmodels scikit-learn pymongo ucimlrepo jupyter
```

- **Modules 1 & 2 (CVD Bogotá):** Download `osb_cronicas_mpcardiocerebrovasculares.csv` from [Bogotá Open Data](https://datosabiertos.bogota.gov.co/dataset/mortalidad-prematura-por-enfermedad-cardiocerebrovascular-en-bogota). For Module 2, provide your own MongoDB Atlas connection string via environment variables.
- **Module 3 (Obesity ML):** Automatically fetches the dataset via `ucimlrepo` (`fetch_ucirepo(id=544)`).

---

## Contributors & Credentials

- **Andrés Felipe Nieto Gutiérrez** — [GitHub](https://github.com/Andresfel1pe410)
- **Mariana Aldana Castellanos**
- **Steven Alejandro Martínez Martínez**

**Institution:** Universidad Nacional de Colombia — *Faculty of Engineering*  
**Program:** Machine Learning and Data Science Diploma ([View Certificates](Certificados/))
