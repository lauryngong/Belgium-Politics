---
title: "Project - Phase IV"
date: 2026-06-11
draft: false
description: "Finishing up the Farmer's Market"
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
The Farmers Market application was designed around three primary user personas: Farmers, Researchers, and Policymakers. Each persona has access to specialized pages and tools tailored to their needs. Farmers can view crop predictions, farm information, and community discussions. Researchers can analyze environmental and soil data and compare datasets across regions. Policymakers can explore regional maps, environmental reports, and future predictions to support planning and decision-making. Throughout Phase IV, the user interface was refined based on feedback from professors, testing activities, and team discussions to improve usability and navigation.

---

## Software Architecture
The application follows a three-layer architecture consisting of a Streamlit frontend, a Flask REST API backend, and a MySQL database. The Streamlit frontend handles user interaction and visualization, while the REST API serves as the communication layer between the user interface, machine learning models, and database. The database stores user information, farm data, crop information, saved analyses, community discussions, and model-related data. Machine learning models are integrated through API routes that receive user inputs, perform predictions, and return results to the frontend for display. This separation of responsibilities improves maintainability and allows different components of the application to be developed and updated independently.

---

## Database
The final database model consists of several related tables that support the application's functionality. Core tables include users, farms, crops, posts, comments, reactions, saved reports, saved graphs, and saved analyses. Additional model-specific tables store machine learning information such as training data, scaling parameters, and saved prediction results. Relationships between tables allow users to create posts, comment on discussions, react to content, save analyses, and manage farm-related information. The database structure was refined throughout the project to support all three personas while maintaining data integrity and efficient access through the REST API.

---

## Data Models

---

## ML Model1: Linear Regression
### Logic
A linear regression model is used to predict crop selling prices for farmers and policymakers based on regional weather conditions and recent price history. The dataset combines Eurostat crop price data with Open-Meteo historical weather records, covering 25 EU member states and 5 crop types (soft wheat, durum wheat, barley, rye, and feed barley) from 2015 to 2024.
The target variable is selling price in euros per 100kg. Features include mean annual temperature, total annual precipitation, a quadratic precipitation term to capture non-linear weather effects, and two lag price features representing the previous two years' selling prices for the same crop and country. Categorical variables — crop type and country — are one-hot encoded using pd.get_dummies with drop_first=True to avoid multicollinearity. Numeric features are standardised using StandardScaler fitted on training data only to prevent data leakage.
The lag price features were the most impactful addition to the model. Grain prices are highly autocorrelated — last year's price is a strong predictor of this year's price — and including price_lag1 and price_lag2 significantly improved predictive performance. 

### Frontend

The model is connected to Streamlit via a Flask REST API route (GET /prices_model/prediction/<crop>/<country>). The policymaker prediction page fetches the prediction and displays it alongside the historical average for that crop/country combination, the percentage difference from the historical average, and a trend line showing all historical years plus the model's prediction as the next point. A choropleth map page displays predicted prices across all 25 EU countries simultaneously for a selected crop type, with results cached for one hour to avoid repeated model calls.

### Model DB

## Model Assumptions

Prior to modelling, a correlation heatmap was generated to verify that meaningful relationships existed between the input features and the target variable, confirming that the selected features had predictive value before proceeding to model training.

The final model achieved an R² of 0.47 on the test set, meaning the model explains approximately 47% of the variation in crop selling prices using weather conditions, lag prices, and regional indicators. 

Three diagnostic plots were produced to verify model assumptions:

### Residuals vs Predicted Values
The residuals vs fitted values plot was used to check the linearity and homoscedasticity assumptions. Random scatter around zero with no systematic pattern indicates the linear relationship assumption is reasonably satisfied and that variance of the residuals is approximately constant across predicted values. As seen on our graph, there is no clear pattern in the data, meaning that the data is now linear and the residuals have constant variance.
![Res vs pred](/new_res_vs_yhat.png)

### Residuals vs Order
The residuals vs order plot checks for autocorrelation — whether residuals follow a pattern based on the order observations appear in the data. There is a random scatter with no trend on our data points around 0, suggesting that residuals are independent, satisfying the independence assumption.
![Res vs order](/new_res_vs_order.png)


### Predicted vs Actual Values

The predicted vs actual values plot shows how closely model predictions align with true selling prices. Points clustering along the diagonal line of perfect prediction indicate the model is capturing the general price level well. The points on our graph stay relatively along the line, meaning that the data is captured well. Originally, there was a curve in the data compared to the line, which showed us that the data was not exactly linear, and this was fixed by adding the quadratic precipitation sum squared column.
![Predicted vs Actual](/pred_vs_act.png)

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

![correlation_img](/corrmod.png)

For predictive checks, I used the accuracy_score metric from sklearn and used a loop to determine the bet k. The library compares the predicted labels to the true labels to determine how good a model is.

{{< plot src="plots/accuracy_plot.html" height="450px" >}}

The model uses finds the most used crop name among the k neighbor crop labels to determine a prediction. In this instance, k=3, and k=5 produced the most accurate results.
A predict() function was also built to allow predictions with outside input. It takes values for N, P, K, temperature, humidity, crop type, season, sowing month, harvest month, and water source. CROPDURATION and WATERREQUIRED, SOIL, and SOIL_PH are harcoded averages since it's not practical to collect from an user. The function scales the continuous inputs, builds the OHE vector, concatenates both, and runs the model to return a predicted crop name.

