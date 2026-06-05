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

Welcome back to the Farmer's Market! During this past week, a lot of time was spent trying to work through database and ML issues, along with figuring out exactly what we wanted our project to do. Our overall goal is to still help farmers see what crops they should grow depending on their environment, but instead of doing this through a crop success rate, we are now doing this through the farmer putting in their environmental information, and then the ML model will give them the crop that best fits their environment.


# Updates

## Data Model

- Separated the ideal_crop_data table into multiple different tables (user_crop_data, with the main goal of separating the ML data (what is scraped and stored) and user generated data)
- Changed the attributes of user_crop_data to better represent the new database for crop type prediction ML


## Creating Data

*Sourced data:*
- All data used to train ML models, crop_price_model_coefficients, crop_health_model_coefficients
Generated data:

*Mockaroo data*
- Users
- Farms
- Farms_Locations

*Manually generated data since we need specific parameters or themes*
- Posts, Comments, Reactions, user_crop_data, saved_data, saved_graphs


# ML Features

# Prices Linear Regression Model Updates
- Refined the best linear regression model that we could find to predict prices of crops in countries. The final model includes average temperature, precipitation sum, precipitation sum squared, price lag 1 and price lag 2. We included the price lags because the years were interfering, so the price lags take in the prices in the years before and after making it more accurate. We also included precipitation sum squared because the precipitation sum vs price scatter plot was not completely linear. These allowed us to get to a r2 around 0.43
- We transfered this model from the jupyter notebook into a python script in a few steps. First, we created databases for the weather data, prices data, and scaled prices. We also input the coefficients and scaled mean and standard deviations into the appropriate databases. I then input the code for the model into the predict() function of the price prediction model python script.
- We routed the prices model so that when the url is called with the crop and country, the predict() function should be called and output the predicted price

## The Problem
We loaded the data from the csv in and input all of the appropriate INSERT statements (specifically for the weather df). However, when we try to do crud /prices_model/prediction/Rye/Austria , we get errors telling us that the 'geo' column cannot be found in the weather database. I worked with all of the professors and Seamus on this, and no one could figure it out since the geo column very clearly was present in the data. This means that we were not able to get the routing to work and therefore could not connect the prices model to any front end screen.

# Logistic Regressor - New Model
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

---
# KNN Crop Reccomender-New Model
Our second model is a KNN Crop Recommendation Model based off a new crop and soil dataset from mendeley. The crop recommendation model uses the data to predict/ reccomend what crop a farmer should grow given their soil, climate, and farming conditions.
First, the dataset is cleaned in similar ways to the previous regression and KNN models, then correlation is run on all numeric features to reveal relationships before modeling.
Categorical variables like TYPE_OF_CROP, SOIL, SOWN, HARVESTED, WATER_SOURCE, SEASON are one-hot encoded using pd.get_dummies. These binary columns are stored elsewhere so they can be appended after the rest of the numerical data is scaled. The _MAX columns (CROPDURATION_MAX, MAX_TEMP, WATERREQUIRED_MAX, RELATIVE_HUMIDITY_MAX, N_MAX, P_MAX, K_MAX) are dropped because they have near-zero variance across the dataset, making it unhelpful to the predictions.
The continuous features were scaled with StandardScaler fit on training data, and the scaled array was concatenated with the binary columns to form X_train and X_test. The target is the crop name from the CROPS column.
The KNN model uses cosine similarity and is unsupervised. For each test point, the training set is normalized, the cosine similarities are computed via dot product, and the top-k most similar training points are identified. Unlike the first KNN model however, instead of averaging neighbor values the model uses finds the most used crop name among the k neighbor crop labels to determine a prediction.
A predict() function was also built to allow predictions with outside input. It takes values for N, P, K, temperature, humidity, crop type, season, sowing month, harvest month, and water source. CROPDURATION and WATERREQUIRED, SOIL, and SOIL_PH are harcoded averages since it's not practical to collect from an user. The function scales the continuous inputs, builds the OHE vector, concatenates both, and runs the model to return a predicted crop name.

---

<iframe src="/plots/accuracy_plot.html" width="100%" height="450px" style="border:none;"></iframe>

The plot above reveals the accuracy of k in the KNN model with the new crop dataset. The best k values are 3 and 5 at around 99.5%. k=2 is the worst performer at around 98.4%. After k=5, accuracy gradually decreases. This suggests the model would perform best with a k value of 3 or 5.

---
## Data Source 1:
CropRecc - sourced - external CSV dataset; contains soil, climate, and crop metadata used for the KNN crop recommendation model

### Features Used for ML
Spatial_Resolution — finer resolution captures more precise field detail, making it a useful proxy for detection quality of stress patches and canopy gaps.

GPS_Coordinates — field location implicitly encodes climate zone, soil type, and regional farming conditions that influence crop health outcomes.

Elevation_Data — altitude shapes temperature ranges, frost risk, and rainfall patterns, all of which directly affect what stress conditions crops face.

Canopy_Coverage — reflects how well crops are physically establishing. Low coverage signals drought stress, poor germination, or pest damage.

NDVI — one of the strongest direct indicators of vegetation health, measuring greenness from satellite data. Low NDVI reliably precedes visible symptoms of stress or disease.

SAVI — corrects NDVI for bare soil interference, making vegetation health estimates more accurate in sparse canopy fields where raw NDVI would be misleading.

Chlorophyll_Content — photosynthetic capacity is directly tied to chlorophyll levels. Declining values are an early biochemical warning of nutrient deficiency or disease before visual symptoms appear.

Leaf_Area_Index — quantifies active leaf surface available for photosynthesis. An underdeveloped canopy strongly correlates with poor health and reduced yield.

Crop_Stress_Indicator — a composite stress score aggregating multiple stress signals, making it one of the most direct predictors of the binary health label.

Temperature — crops have narrow optimal thermal ranges; deviations in either direction cause measurable physiological stress.

Humidity — high humidity promotes fungal disease and rot while low humidity accelerates water stress, making both extremes relevant to health classification.

Rainfall — fundamental water availability driver. Too little causes drought stress; excess causes waterlogging and root disease.

Wind_Speed — strong winds cause physical crop damage, accelerate soil drying, and increase evapotranspiration, all negatively affecting crop condition.

Soil_Moisture — measures actual water availability at the root zone, which is more informative than rainfall alone since it accounts for drainage and retention differences across fields.

Soil_pH — controls nutrient solubility and uptake. Outside a crop's optimal pH range, essential nutrients become chemically unavailable even when present in the soil.

Organic_Matter — indicates soil fertility and microbial activity. Higher organic matter supports nutrient cycling and water retention, creating more favorable growing conditions.

Weed_Coverage — weeds compete directly with crops for water, light, and nutrients. Higher coverage is a consistent negative signal for crop health.

Pest_Damage — a direct severity measurement of crop damage, making it one of the most informative features for predicting an unhealthy label.

Expected_Yield — encodes agronomic projections based on field conditions. Low expected yield is strongly correlated with poor crop health and serves as an integrated summary of many field factors.

Water_Flow — measures irrigation availability independent of rainfall. Fields with insufficient flow are at drought risk even in otherwise adequate climates.

### Features not included and why:
High_Resolution_RGB, Multispectral_Images, Thermal_Images, and Temporal_Images are binary availability flags, not actual measurements, so they carry no quantitative signal about field conditions. Bounding_Boxes is an object-detection artifact from image processing with no agricultural meaning. Field_Boundaries is spatial metadata with no direct health signal. Pest_Hotspots, Drainage_Features, Ground_Truth_Segmentation, Crop_Type, and Crop_Growth_Stage were retained in the model but handled separately as unscaled binary columns appended after StandardScaler, since scaling one-hot or binary features would distort their interpretation.

---
## Data Source 2:
Crop Health Dataset - sourced - external dataset used for the logistic regression model; downsampled to balance classes

### Features used for ML
CROPS — the name/identifier of the crop being grown.

TYPE_OF_CROP — categorical classification of the crop category (e.g. cereal, legume, vegetable). Encodes broader agronomic groupings that share similar growing requirements.

SOIL — the soil type associated with the crop or field. Different soils have different drainage, nutrient retention, and aeration properties that affect crop suitability.

SEASON — the growing season (e.g. Kharif, Rabi, Zaid). Encodes climate and timing context since crops grown in different seasons face fundamentally different temperature and rainfall conditions.

SOWN — the month or period when the crop is planted. Planting timing affects frost exposure, rainfall alignment, and how long the crop has to mature.

HARVESTED — the month or period when the crop is harvested. Together with SOWN it defines the full growing window and can imply total growing duration.

WATER_SOURCE — the irrigation or water supply type (e.g. rain-fed, canal, groundwater). Determines water reliability and stress risk independent of rainfall levels.

SOIL_PH / SOIL_PH_HIGH — the minimum and maximum soil pH range suitable for the crop. pH controls nutrient availability; most crops have a narrow optimal range outside of which uptake is impaired.

CROPDURATION / CROPDURATION_MAX — the minimum and maximum number of days the crop takes to mature. Encodes how long the crop is exposed to environmental conditions and stress risks.

TEMP / MAX_TEMP — the minimum and maximum temperature tolerance of the crop in °C. Defines the thermal window within which the crop can grow without stress.

WATERREQUIRED / WATERREQUIRED_MAX — the minimum and maximum water requirement of the crop in mm. Encodes how drought-sensitive or water-intensive the crop is.

RELATIVE_HUMIDITY / RELATIVE_HUMIDITY_MAX — the minimum and maximum relative humidity range the crop tolerates. High humidity promotes disease; low humidity increases water stress.

N / N_MAX — the minimum and maximum nitrogen requirement of the crop. Nitrogen is the primary driver of vegetative growth and is the most commonly deficient macronutrient in agricultural soils.

P / P_MAX — the minimum and maximum phosphorus requirement. Phosphorus supports root development, flowering, and energy transfer within the plant.

K / K_MAX — the minimum and maximum potassium requirement. Potassium regulates water uptake, disease resistance, and overall plant strength, making it critical to crop health and yield.

### Features Not Included:
N, P, K (nitrogen, phosphorus, potassium minimums) - soil nutrient indicators that determine what crops can grow in a given soil.
TEMPERATURE - crops have specific temperature tolerances which makes this is a strong predictor of crop health.
RELATIVE_HUMIDITY - affects water stress, disease risk, and crop suitability by region and season.
CROPDURATION (avg, hardcoded) and WATERREQUIRED (avg, hardcoded) - included as continuous features using dataset averages
TYPE_OF_CROP, SEASON, SOWN, HARVESTED, WATER_SOURCE (one-hot encoded) - categorical context features that encode farming conditions.
OHE was used so cosine similarity and logistic regression weights treat each category independently

The _MAX columns (e.g. CROPDURATION_MAX, MAX_TEMP) were dropped because they held a single constant value across all rows, which makes them unvaluable for making predictions.


# REST API Matrix

| Resource | GET | POST | PUT | DELETE |
| -------- | --- | ---- | --- | ------ |
| `/farms` | Shows list of all farms | Add a new farm (request body contains farm details) | — | — |
| `/farms/{farm_id}` | Shows details of a specific farm | — | Update farm information | Delete farm |
| `/user_crop_data` | — | Add new farm conditions (request body contains farm condition data) | — | — |
| `/user_crop_data/{farm_id}` | Shows all conditions for a specific farm | — | -- | -- |
| `/user_crop_data/{user_crop_data_id}` | Shows specific entry of crop data | — | Update specific entry | Delete specific entry |
| `/savedData` | — | Add saved ML model output data | — | — |
| `/savedData/{saved_id}` | Shows specific saved data | — | Update saved data | Delete saved data |
| `/savedGraphs` | — | Add a saved graph (request body contains graph and ML data) | — | — |
| `/savedGraphs/{id}` | Shows a specific saved graph | — | Update saved graph | Delete saved graph |
| `/savedReports` | — | Add a saved report (request body contains report and ML data) | — | — |
| `/savedReports/{id}` | Shows a specific saved report | — | Update saved report | Delete saved report |
| `/users` | Shows all users | Add a new user (request body contains user information) | — | — |
| `/users/{user_id}` | Shows details for a specific user | — | Update user information | Delete user |
| `/posts` | Shows all posts | Add a new post (request body contains title and text) | — | — |
| `/posts/{post_id}` | Shows a specific post | — | Update post | Delete post |
| `/comments/post/{post_id}` | Shows all comments under a post | Add a comment under a post (request body contains comment text) | — | — |
| `/comments/user/{user_id}` | Shows all comments made by a user | — | — | — |
| `/comments/{comment_id}` | Shows a specific comment | — | Update comment | Delete comment |
| `/reactions/post/{post_id}` | Shows all reactions under a post | Add a reaction (request body contains like/dislike value) | — | — |
| `/reactions/{reaction_id}` | Shows a specific reaction | — | Update reaction | Delete reaction |

## Relation to Wireframes and User Stories

- Farmers and Politicians must be able to see a list of all farms and specific details about a farm in order to compare crop statistics. Farmers can do this to get a better understanding of what the farmers around them are growing, while policy makers can use this to see how many farmers there are, and any general trends between the farmers.
- Farmers should be able to access the user_crop_data in order to upload their information, they should be able to view all of their user_crop_data enteries based on their farm, and should be able to access an individual entry in order to view, update, and delete it
- Farmers, Policy Makers, and Reseachers should be able to save the data generated by a ML. This saved data will also be built off in the future for graphs and reports
- Policy Makers and Researchers should be able to generate and save graphs and reports in order to use the data beyond data visualization, and should be able to access, add, edit, and delete these saved graphs and reports as they collect more saved data
- We should be able to access all users and specific user information mainly for wireframes, such as the home page which shows all users grouped by their user group, along with giving users the ability to update their profile.
- All personas should be able to post, comment, and react to posts on the discussion board. This board serves as a way for all parties to be more aware of the issues they are facing, such as farmers letting policy makers know of some issues that are beyond crop metrics, and researchers reporting on trends they found that could impact farmers' ability to farm in the future and the direction of policies.

# Mocked-up App

## Farmer Discussion Board Page:
![Farmer Discussion Board Page](FarmerDiscussionBoardPage.png)

## Policymaker Crop Price Predictions Page:
![Policymaker Crop Price Predictions Page](PolicymakerCropPricePredictionsPage.png)

## Policymaker Crop Map Page:
![Policymaker Crop Map Page](PolicymakerCropMapPage.png)

## Policymaker Report Maker Page:
![Policymaker Report Maker Page](PolicymakerReportMakerPage.png)
