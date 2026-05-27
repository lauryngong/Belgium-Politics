---
title: "Project - Phase II"
date: 2026-05-27
draft: false
description: "Setting up the Farmer's Market"
slug: "phase2post"
tags: ["project", "Setup"]
authors:
  - "Elise Wizemann"
  - "Lauryn Gong"
  - "Minju Sung"
  - "Nicole Stekol"
showAuthorsBadges: false
---

# Updates

## User stories and personas
Since the submission of Phase I, our team made several updates to the project plan, particularly involving the policy maker and researcher personas. We revised both personas and their associated user stories to make them more distinct from one another and to better reflect realistic use cases within the application. Initially, many of the user stories were heavily focused on data visualization and reading information, but we expanded them to include a wider range of CRUD objectives such as creating analyses, updating reports or recommendations, and managing saved datasets and alerts. This helped make the application feel more interactive and practical rather than only functioning as a visualization tool.

For the policymaker persona, we added features involving agricultural demographics such as crop types, crop prices, farm productivity, community-reported complaints, and environmental alerts. We also expanded the application to include predictive recommendations for future policy planning instead of only showing historical data. For the researcher persona, we refined the focus toward combining datasets from multiple sources with different scales and methodologies while also allowing researchers to upload and manage their own soil data. These updates helped improve the overall scope of the project and made the personas more specialized and realistic.

## Data sets for ML modeling

---

# Data Handling

## Data Curation and Cleaning
Aside from the model that will be used to predict if a farmers’ crops will be successful or not, our second model will be a linear regression that will predict the selling price of a farmers’ crop based on the crop type, country, yearly precipitation, and average yearly temperature. In order to get a start on this, we performed the data EDA and visualizations on the data for this second model.
I found a Eurostat database that includes yearly prices for different crops for each country in the EU, and combined that with a weather API I found that I got yearly precipitation sum from and average yearly precipitation. This left me with a dataset with columns for country, selling price, crop, year, temperature mean, and precipitation sum. To finish cleaning it, I saw that some of the price values were na, so I replaced them with the median of those countries’ respective prices. I then removed all of the National currency values because they are duplicates of the columns in euros, just in the different currencies. 
This is how the dataset looks:
![prices_df](/pricesdf.png)

## Data EDA and Visualizations
To start analyzing and visualizing the data, I made a correlation heatmap, which I specifically focused on the correlations between the factors and selling price.
![correlation heatmap](/corheatmap.png)
This heatmap shows the positive correlation (0.31) between temperature and selling price. This means that a higher temperature means a higher selling price. A possible reason for this could be that since higher temperatures are correlated with less precipitation because of the -0.43, it results in a drier climate and less crop yield, driving the prices up. This can be very helpful for our regression model because it is clearly a factor influencing the price. This is similar and related to the precipitation sum, which has a negative (-0.17) correlation with selling price, meaning that with more precipitation, the prices go down. This could be because more rainfall means a better crop yield, allowing the prices to decrease. This will also be included as a factor in our linear regression model for these reasons of the negative correlation. 

![temp scatter](/tempprice.png)
This scatter plot visualises the relationship between mean annual temperature and crop selling price across all countries and crop types in our dataset. The plot shows a wide spread of points, reflecting that temperature alone does not determine price — however a loose upward trend is visible, consistent with the 0.31 correlation identified in the heatmap. The majority of prices cluster between €10–25 at temperatures of 8–15°C, which represents the typical range for central and northern European growing conditions. Points at higher temperatures tend to show more price variation, which could reflect the drought stress effect discussed above — in warmer years and regions, unpredictable yields lead to more volatile prices. This scatter plot confirms that temperature carries useful signal for our regression model, even if the relationship is not perfectly linear.

![precip scatter](/precipprice.png)
This scatter plot shows the relationship between total annual precipitation and crop selling price. The bulk of observations are concentrated between 400–1000mm of rainfall and prices of €10–25, with a slight downward tendency as precipitation increases — consistent with the weak -0.17 correlation from the heatmap. The wide spread of points indicates that precipitation is not a strong standalone predictor of price, which is expected given the low correlation value. However, combined with temperature and the categorical features of crop type and region, it contributes additional signal to the model. The negative trend supports the interpretation that higher rainfall is associated with better growing conditions and increased supply, putting downward pressure on prices.


---

# Data Models


