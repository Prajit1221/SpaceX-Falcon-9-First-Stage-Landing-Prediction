# SpaceX Falcon 9 First Stage Landing Prediction

## Project Overview

This project focuses on predicting the successful landing of the SpaceX Falcon 9 first stage. The ability to reuse the first stage is a cornerstone of SpaceX's cost-efficiency, making accurate landing prediction vital for competitive bidding in the rocket launch market. This repository contains the code and analysis developed as part of the IBM Data Science Professional Certificate program on Coursera, utilizing labs provided by IBM Skills Network.

## Table of Contents

- [Project Overview](#project-overview)
- [Table of Contents](#table-of-contents)
- [Problem Statement](#problem-statement)
- [Project Objectives](#project-objectives)
- [Methodology](#methodology)
  - [Data Collection](#data-collection)
  - [Data Preparation & Feature Engineering](#data-preparation--feature-engineering)
  - [Machine Learning Models](#machine-learning-models)
- [Key Findings & Results](#key-findings--results)


## Problem Statement

The core problem addressed in this project is to accurately predict whether the first stage of the SpaceX Falcon 9 rocket will land successfully. This predictive capability is vital for assessing the cost-effectiveness of a launch, as the reusability of the first stage significantly reduces overall mission expenses compared to other providers. Such information can be leveraged by alternative companies bidding against SpaceX for rocket launch contracts.

## Project Objectives

This project aims to:

1.  **Collect Comprehensive Data:** Gather historical launch records for Falcon 9 and Falcon Heavy from various online sources, including the SpaceX API and Wikipedia.
2.  **Perform Exploratory Data Analysis (EDA) & Feature Engineering:** Analyze collected data to identify patterns, understand variable relationships to landing success, and prepare the data for machine learning.
3.  **Develop and Evaluate Machine Learning Models:** Implement and fine-tune various classification algorithms to predict landing outcomes, identifying the best-performing model for the task.

## Methodology

### Data Collection

Data was sourced from:

* **SpaceX API:** Historical launch data was retrieved from the official SpaceX API (`/v4/launches/past` endpoint) using the `requests` and `pandas` libraries. Helper functions were used to extract detailed information such as rocket names, launch site coordinates, payload mass, orbit type, and core landing outcomes.
* **Wikipedia Web Scraping:** Additional historical launch records for Falcon 9 and Falcon Heavy were scraped from the "List of Falcon 9 and Falcon Heavy launches" Wikipedia page using `requests` and `BeautifulSoup`. Custom helper functions were developed to parse HTML tables and extract relevant data.

### Data Preparation & Feature Engineering

The collected raw data underwent a structured preparation process:

* **Missing Value Handling:** Missing `PayloadMass` values were imputed with the mean of existing values, while `LandingPad` `NaN` values were retained to indicate no landing pad use.
* **Class Label Creation:** A binary `Class` variable (0 for unsuccessful, 1 for successful) was created from the `Outcome` column to serve as the target variable for supervised learning.
* **Feature Selection:** Key features like `FlightNumber`, `PayloadMass`, `Orbit`, `LaunchSite`, `Flights`, `GridFins`, `Reused`, `Legs`, `LandingPad`, `Block`, `ReusedCount`, and `Serial` were selected for model training.
* **Categorical Encoding:** Categorical features (`Orbit`, `LaunchSite`, `LandingPad`, `Serial`) were converted into numerical representations using One-Hot Encoding via `pandas.get_dummies()`. All resulting numeric columns were cast to `float64`.

### Machine Learning Models

The prepared dataset was used to train and evaluate classification models:

* **Data Partitioning & Standardization:** The dataset was split into 80% training and 20% testing sets using `train_test_split` (with `random_state=2`). Features were standardized using `StandardScaler`.
* **Model Training & Hyperparameter Tuning:** `GridSearchCV` with 10-fold cross-validation (`cv=10`) was used to optimize hyperparameters for:
    * **Logistic Regression:** Tuned `C`, `penalty`, `solver`.
    * **Support Vector Machine (SVM):** Tuned `kernel`, `C`, `gamma`.
    * **Decision Tree Classifier:** Tuned `criterion`, `splitter`, `max_depth`, `max_features`, `min_samples_leaf`, `min_samples_split`.
    * **K-Nearest Neighbors (KNN):** Tuned `n_neighbors`, `algorithm`, `p`.
* **Model Evaluation:** Performance was assessed by calculating accuracy on the test data using `score()`, and confusion matrices provided detailed classification insights.

## Key Findings & Results

* **Increasing Success Rate:** SpaceX's landing success rate has shown a clear upward trajectory since 2013, indicating continuous improvements in technology and operational procedures.
* **Launch Site Influence:** Different launch sites exhibit varying success rates and payload mass limitations. VAFB SLC 4E, for instance, recorded no successful heavy payload (>10,000 kg) landings.
* **Orbit Type Impact:** Certain orbit types (e.g., HEO, GEO, ES-L1, ISS, VLEO) show higher success rates, while GTO presents more complex landing scenarios.
* **Model Performance:** All four machine learning models (Logistic Regression, SVM, Decision Tree, and KNN) achieved an identical test accuracy of 83.33%. This consistency suggests a similar predictive capability on the given dataset. The Decision Tree, despite matching others in test accuracy, showed a promising higher cross-validation score (0.8893) during training.

