---
title: "Project - Phase I"
date: 2026-05-19
draft: false
description: "Setting up the Farmer's Market"
slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "Elise Wizemann"
  - "Lauryn Gong"
  - "Minju Sung"
  - "Nicole Stekol"
showAuthorsBadges: false
---

# Our Plan

Agricultural productivity and land development are heavily affected by environmental conditions, including soil quality, weather patterns, elevation, and erosion risk. However, farmers, policymakers, and researchers often lack a centralized system that analyzes these factors together to support informed decision-making. This can lead to reduced crop success, poor land-use planning, environmental damage, and missed opportunities for sustainable agriculture.

The main part of this project will allow farmers to input information about their land, such as sunlight, precipitation, elevation, and more, and a regression model will output a predicted crop success rate for their land. For farmers, there will also be an interactive map that breaks the EU into different sectors and has a 0–100 percent scale of how ideal each part is for farming specific crops, which gives farmers insight into where and what to start farming.

The platform will also support Policy Makers and Agriculture Researchers. Agricultural researchers can use the system to analyze environmental trends, study crop performance across different conditions, and evaluate the long-term impacts of climate and land-use changes on agriculture. Policy Makers will also be able to get insight on how they can help support farmers and their crops in their regions in order to help their citizens.

---

# Personas

Ever look at an application and think *who is actually gonna use this?* Well lucky for you, we have three candidates for this application!

---

## Joe Hudson — Farmer, age 45

Joe Hudson is a 45-year-old farmer who manages agriculture and crop production on his mid-sized farm in a small agricultural community. He depends on weather conditions, soil quality, and the year's crop performances to plan his harvests. Joe wants to be able to find the best locations for farming from a model that uses data like crop requirements, weather conditions, soil quality, erosion risk, and past crop performances from different geographical locations to predict farming zones. Because environmental states vary largely depending on geography, this app will help reduce crop risks and improve overall productivity.

**User story 1:** As a farmer, I want to create and manage farm records so that I can track crop performance, soil conditions, and farming history over time.

**User story 2:** As a farmer, I want to input location information to see my crop success rate, and use the data regarding my farm's weather and soil conditions so I can see what crops would best grow in my area.

**User story 3:** As a farmer, I want to update the environment and harvest data throughout the season so that the system can continuously improve predictions.

**User story 4:** As a farmer, I want to view visual maps and reports of my data so that I can compare different farming locations and make decisions.

---

## Sarah Baker — Policy Maker, age 52

Sarah Baker is a 52-year-old Regional Agricultural Policy Director who makes land-use and sustainability decisions for her region. She uses environmental datasets such as weather patterns, soil quality, elevation, and erosion risk to determine which areas should be preserved for farming, protected from environmental damage, or approved for development. Because this data is often spread across multiple sources, Sarah needs a platform that can combine and visualize information clearly. This application helps her make more accurate, data-driven decisions that support sustainable agriculture and urban planning.

**User story 1:** As a policy maker, I want to identify highly fertile farming regions so that I can prioritize them for agricultural preservation and funding.

**User story 2:** As a policy maker, I want to analyze soil erosion and flood-risk data so that I can prevent construction projects in environmentally unstable areas.

**User story 3:** As a policy maker, I want to compare weather, elevation, and soil quality across different regions so that I can make informed land-use decisions.

**User story 4:** As a policy maker, I want to view long-term environmental trends and predictions so that I can create sustainable agricultural policies for future development.

**User story 5:** As a policy maker, I want to generate visual reports and maps from environmental datasets so that I can clearly present findings to government officials and stakeholders.

---

##  Carrie Miller — Soil Researcher, age 28

Carrie Miller is a 28-year-old academic who focuses on researching environmental health, specifically soil. She looks at the chemical and biological properties of soil in the areas around her, and while she does take her own samples, she often relies on external databases to see the soil conditions of the larger overall area. The data she does look at is often outdated, has different methodologies, or covers largely different scales of area, so she cannot easily combine them all together. Carrie hopes to find an application that can bring all this data together in one area and take into account the different scales and times that the data was taken to give a conclusive answer to the health of soil in specific areas.

**User story 1:** As a researcher, I want to be able to filter data by when it was collected or its location, so I can find data relevant to what I want.

**User story 2:** As a researcher, I want to be able to export data into one clear place so I can use it for purposes beyond the app.

**User story 3:** As a researcher, I want to add my own data for personal use so I can get more accurate and custom data for specific areas.

**User story 4:** As a researcher, I want a way to estimate how certain properties of soil will change in the future based on current and past data, so I can research the possible future trends of soil in an area and its possible effects.

---

# Datasets + Descriptions
![cropHealth](/dataSoil.png)

The agriculture_dataset (csv) from Kaggle includes data on crop growth, chlorophyll statistics, GPS locations, and the underlying environmental factors necessary for crop optimization. It will be used to predict the success rates and percentages for crops based on the factors via a model and display statistics on each measure on a user website. The data is downloaded locally and will be read through python pandas for the model.

![weatherApi](/forecastApi.png)

The forecast api from Open-Meteo gives us weather data from across Europe including latitude, longitude, precipitation, elevation, temperature, and other forecast data. This will be used to locate which areas in Europe have ideal farming conditions. The API on default provides forecasts for 7 days, but the user is able to access forecasts for up to 16 days. Past weather data can be accessed via a Past Days feature to access archived forecasts. Additionally, non-commercial use is free and has a limit of less than 10,000 daily API calls.