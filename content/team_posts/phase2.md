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
![prices df](static\assets\pricesdf.png)
![prices_df #2](/pricesdf.png)

## Data EDA and Visualizations

---

# Data Models


