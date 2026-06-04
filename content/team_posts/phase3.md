---
title: "Project - Phase III"
date: 2026-06-04
draft: false
description: "Setting up the Farmer's Market"
slug: "phase3post"
tags: ["project", "Setup"]
authors:
  - "Elise Wizemann"
  - "Lauryn Gong"
  - "Minju Sung"
  - "Nicole Stekol"
showAuthorsBadges: false
---

# Introduction


# Updates

## Data Model

## Creating Data


# ML Features
A logistic regression model was implemented in Phase III to predict Crop_Health_Label (a binary indicator of whether a crop is healthy or not) using the agriculture_dataset.csv provided in the previous phases. The cleaning and splitting of the dataset was similar to the KNN mode's process prior. Duplicates were removed, null values were accounted or, and 50% of observations were removed to efficiency before any modeling was done.
A challenge with the model itself was figuring out how to handle class imbalance. The dataset contained observations with most predictions leaning towards a crop health indicator 1, which heavily skewed the predictions an accuracy of the data. Before balancing, the model had around 70% accuracy but with a True Negative Rate near 0 (~0.98% TNR). This means it predicted every crop as healthy regardless of input. To fix this, the majority output (1) was downsampled to match the number of class (0) observations, resulting in a more balanced dataset.
Feature engineering to improve accuracy included one-hot encoding Crop_Type and Crop_Growth_Stage, then separating binary and continuous features. These continuous features were scaled with a StandardScaler, and an intercept column of ones was added to the feature matrix, before encoded columns were concatted back.
The logistic regression model was implemented manually with gradient descent. For each observation, the function computed a predicted probability, and weights were updated continuously. Weights were initialized to all zeros and the model ran for max_iter=1000 iterations.
However, even while experimenting with different values of alpha, the overall log loss remained the same throughout and accuracy stalled to around 50% after all features were leveled.This might mean the model is underfitting.

![logisticreg](/logloss.png)

- True Negative Rate is: 0.009793081661664824
- False Positive Rate is: 0.9902069183383352
- True Positive Rate is: 0.9920650931342883
- False Negative Rate is: 0.007934906865711744
- The Precision is: 0.9958150523118461
- The F1-Score is: 0.9939365357407531
- The accuracy of the model is: 0.6987548344495802

The plot of the model above shows log loss decreasing as k increases . k=5 is the best value in this scenario since a decreasng log loss indicates that the model is assigning higher probabilities to the correct classes.

In the future, I plan on experimenting with a wider range of alpha values (e.g. 0.0001 to 1.0) and increasing max_iter to give weights more time to converge.

Our second model is a KNN Crop Recommendation Model based off a new crop and soil dataset from mendeley. The crop recommendation model uses the data to predict/ reccomend what crop a farmer should grow given their soil, climate, and farming conditions.
First, the dataset is cleaned in similar ways to the previous regression and KNN models, then correlation is run on all numeric features to reveal relationships before modeling.
Categorical variables like TYPE_OF_CROP, SOIL, SOWN, HARVESTED, WATER_SOURCE, SEASON are one-hot encoded using pd.get_dummies. These binary columns are stored elsewhere so they can be appended after the rest of the numerical data is scaled. The _MAX columns (CROPDURATION_MAX, MAX_TEMP, WATERREQUIRED_MAX, RELATIVE_HUMIDITY_MAX, N_MAX, P_MAX, K_MAX) are dropped because they have near-zero variance across the dataset, making it unhelpful to the predictions.
The continuous features were scaled with StandardScaler fit on training data, and the scaled array was concatenated with the binary columns to form X_train and X_test. The target is the crop name from the CROPS column.
The KNN model uses cosine similarity and is unsupervised. For each test point, the training set is normalized, the cosine similarities are computed via dot product, and the top-k most similar training points are identified. Unlike the first KNN model however, instead of averaging neighbor values the model uses finds the most used crop name among the k neighbor crop labels to determine a prediction.
A predict() function was also built to allow predictions with outside input. It takes values for N, P, K, temperature, humidity, crop type, season, sowing month, harvest month, and water source. CROPDURATION and WATERREQUIRED, SOIL, and SOIL_PH are harcoded averages since it's not practical to collect from an user. The function scales the continuous inputs, builds the OHE vector, concatenates both, and runs the model to return a predicted crop name.

<iframe src="/plots/accuracy_plot.html" width="100%" height="450px" style="border:none;"></iframe>

The plot above reveals the accuracy of k in the KNN model with the new crop dataset. The best k values are 3 and 5 at around 99.5%. k=2 is the worst performer at around 98.4%. After k=5, accuracy gradually decreases. This suggests the model would perform best with a k value of 3 or 5.
---
### Data Source 1:
CropRecc - sourced- external CSV dataset; contains soil, climate, and crop metadata used for the KNN crop recommendation model
### Features Used for ML and Why:
Spatial_Resolution - finer resolution captures more precise field detail, making it a useful proxy for detection quality of stress patches and canopy gaps.
GPS_Coordinates - field location implicitly encodes climate zone, soil type, and regional farming conditions that influence crop health outcomes.
Elevation_Data - altitude shapes temperature ranges, frost risk, and rainfall patterns, all of which directly affect what stress conditions crops face.
Canopy_Coverage - reflects how well crops are physically establishing. Low coverage signals drought stress, poor germination, or pest damage.
NDVI - one of the strongest direct indicators of vegetation health, measuring greenness from satellite data. Low NDVI reliably precedes visible symptoms of stress or disease.
SAVI - corrects NDVI for bare soil interference, making vegetation health estimates more accurate in sparse canopy fields where raw NDVI would be misleading.
Chlorophyll_Content - photosynthetic capacity is directly tied to chlorophyll levels. Declining values are an early biochemical warning of nutrient deficiency or disease before visual symptoms appear.
Leaf_Area_Index - quantifies active leaf surface available for photosynthesis. An underdeveloped canopy strongly correlates with poor health and reduced yield.
Crop_Stress_Indicator - a composite stress score aggregating multiple stress signals, making it one of the most direct predictors of the binary health label.
Temperature - crops have narrow optimal thermal ranges; deviations in either direction cause measurable physiological stress.
Humidity - high humidity promotes fungal disease and rot while low humidity accelerates water stress, making both extremes relevant to health classification.
Rainfall - fundamental water availability driver. Too little causes drought stress; excess causes waterlogging and root disease.
Wind_Speed - strong winds cause physical crop damage, accelerate soil drying, and increase evapotranspiration, all negatively affecting crop condition.
Soil_Moisture - measures actual water availability at the root zone, which is more informative than rainfall alone since it accounts for drainage and retention differences across fields.
Soil_pH - controls nutrient solubility and uptake. Outside a crop's optimal pH range, essential nutrients become chemically unavailable even when present in the soil.
Organic_Matter - indicates soil fertility and microbial activity. Higher organic matter supports nutrient cycling and water retention, creating more favorable growing conditions.
Weed_Coverage - weeds compete directly with crops for water, light, and nutrients. Higher coverage is a consistent negative signal for crop health.
Pest_Damage - a direct severity measurement of crop damage, making it one of the most informative features for predicting an unhealthy label.
Expected_Yield - encodes agronomic projections based on field conditions. Low expected yield is strongly correlated with poor crop health and serves as an integrated summary of many field factors.
Water_Flow - measures irrigation availability independent of rainfall. Fields with insufficient flow are at drought risk even in otherwise adequate climates.

Features not included and why:
High_Resolution_RGB, Multispectral_Images, Thermal_Images, and Temporal_Images are binary availability flags, not actual measurements, so they carry no quantitative signal about field conditions. Bounding_Boxes is an object-detection artifact from image processing with no agricultural meaning. Field_Boundaries is spatial metadata with no direct health signal. Pest_Hotspots, Drainage_Features, Ground_Truth_Segmentation, Crop_Type, and Crop_Growth_Stage were retained in the model but handled separately as unscaled binary columns appended after StandardScaler, since scaling one-hot or binary features would distort their interpretation.
---
### Data Source 2:
Crop Health Dataset - sourced - external dataset used for the logistic regression model; downsampled to balance classes

### Features Used for ML and Why:
N, P, K (nitrogen, phosphorus, potassium minimums) - soil nutrient indicators that determine what crops can grow in a given soil.
TEMPERATURE - crops have specific temperature tolerances which makes this is a strong predictor of crop health.
RELATIVE_HUMIDITY - affects water stress, disease risk, and crop suitability by region and season.
CROPDURATION (avg, hardcoded) and WATERREQUIRED (avg, hardcoded) - included as continuous features using dataset averages
TYPE_OF_CROP, SEASON, SOWN, HARVESTED, WATER_SOURCE (one-hot encoded) - categorical context features that encode farming conditions.
OHE was used so cosine similarity and logistic regression weights treat each category independently

The _MAX columns (e.g. CROPDURATION_MAX, MAX_TEMP) were dropped because they held a single constant value across all rows, which makes them unvaluable for making predictions.


# REST API Matrix

Resource | GET | POST | PUT | DELETE
-------- | --- | ---- | --- | ------



# Mocked-up App

