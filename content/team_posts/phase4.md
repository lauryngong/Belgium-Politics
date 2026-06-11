---
title: "Project - Phase IV"
date: 2026-05-19
draft: false
description: "Setting up the Farmer's Market"
slug: "phase4post"
tags: ["project", "Setup"]
authors:
  - "Elise Wizemann"
  - "Lauryn Gong"
  - "Minju Sung"
  - "Nicole Stekol"
showAuthorsBadges: false
---

## User Interface + Persona Pages
describe software architecture

---

## Database
Descrbe final version of database model

---

## Data Models

---

## ML Model1: Linear Regression
fundamental understanding of ML models and their implementation in the software architecture:

verifying checking of all model assumptions and predictive checks (things not included in the web app):

---

## ML Mode2: K-Nearest Neighbors
## Logic
A K-Nearest Neighbors Model is used to recommend crops to plant (for farmers) based on their available agricultural resources.
The dataset in from mendeley and consists of features such as Potassium, Nitrogren, Crop Type, Water Source, and more. (For the full list of features, reference phase3.md). 
An l2 Knn model was also considered, however with 71 dimensions, of which only 7 are scaled continuous features (crop duration, temp, water, humidity, N, P, K) and the other 64 are one-hot binary columns. This means the binary columns would likely overpower the numerical features, and since l2 measures distance, most points would appear to be of equal distance away, resulting in meaningless predictions. Thus, a cosine similarity approach was taken to compare the angle between the vectors (direction/pattern).
The dataset is cleaned to standardize, then remove, all null values, and correlation is run on all numeric features to reveal relationships before modeling. Additionally, I accounted for false confidence predictions by ensuring that the training data contained a similar number of observations for each crop. There were 57 total types of crops and 1000 observations of each crop.
Categorical variables like TYPE_OF_CROP, SOIL, SOWN, HARVESTED, WATER_SOURCE, SEASON are one-hot encoded using pd.get_dummies. These binary columns are stored elsewhere so they can be appended after the rest of the numerical data is scaled. The _MAX columns (CROPDURATION_MAX, MAX_TEMP, WATERREQUIRED_MAX, RELATIVE_HUMIDITY_MAX, N_MAX, P_MAX, K_MAX) are dropped because they have near-zero variance across the dataset, making it unhelpful to the predictions.
The continuous features were scaled with StandardScaler fit on training data, and the scaled array was concatenated with the binary columns to form X_train and X_test. The target is the crop name from the CROPS column.
The KNN model uses cosine similarity and is unsupervised. For each test point, the training set is normalized, the cosine similarities are computed, and the top-k most similar training points are identified. 

### Frontend
The model is connected to streamlit via routes. The first route is connected to function get_model3_prediction which takes the input features (N, P, K, type_of_crop, temperature, season, sown, harvested, water_source, relative_humidity, crop_duration, water_required) and runs the model to return recommended up to 5 crops.

A second function, connected to preds_routes, connects a table from the farmers database which stores past predictions, should the farmer choose to save them. This allows users to view and save past predictions so they can make better informed decisions.
Additionally, a small help button is present beside each feature so users are able to view the functionalities of each feature in the model.

Both functionalities are stored as tabs on the crop-prediction page.

### Model DB
The model03 database consists of model3_scaler, which stores the feature means and stds of numerical values so we can properly scale the inputs; saved_crop_preds, which stores the saved predictions made by farmers (this includes the PK, farmer_id, type_of_crop, sown date, harvested, date, water_source, predicted_crop, and time of creation); and a model3_training_data table which consists of a PK, feature vector as a JSON tpye, and crop_label for the actual output. These are used in the predictor function to determine which point is closest to the input values. There is a total of 39,899 observations for training.

### Model Assumptions + Predictive Checks
KNN (K-Nearest Neighbors) does not have or require linearity, homoscedasticity, or the absence of autocorrelation. These are strictly foundational assumptions for parametric models like Linear Regression, whereas KNN is a non-parametric algorithm, meaning it makes virtually no assumptions about the underlying distribution or structure of the data.

I verified the features had a relationship prior to doing any modeling by using .corr() which is a pandas library that calculates the pairwise correlation between numerical columns in a dataset. It evaluates how strongly two variables change together. Out of the features, our chosen input features had the strongest correlation to crop type.

[!correlation_img](/corrmod.png)

For predictive checks, I used the accuracy_score metric from sklearn and used a loop to determine the bet k. The library compares the predicted labels to the true labels to determine how good a model is.

{{< plot src="plots/accuracy_plot.html" height="450px" >}}

The model uses finds the most used crop name among the k neighbor crop labels to determine a prediction. In this instance, k=3, and k=5 produced the most accurate results.
A predict() function was also built to allow predictions with outside input. It takes values for N, P, K, temperature, humidity, crop type, season, sowing month, harvest month, and water source. CROPDURATION and WATERREQUIRED, SOIL, and SOIL_PH are harcoded averages since it's not practical to collect from an user. The function scales the continuous inputs, builds the OHE vector, concatenates both, and runs the model to return a predicted crop name.

