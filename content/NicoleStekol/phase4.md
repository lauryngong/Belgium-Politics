---
title: "Phase 4 Post"
description: "This post is about my contributions to Phase 4"
date: 2026-06-12
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
slug: "Blog4"
authors:
  - "Nicole Stekol"
---

# Phase 4 Contributions
Since Phase 3, I resolved the country column routing error that was blocking my model from being connected to the frontend by adding the Insert table with the actual price data, and successfully got the prediction route working end to end. I built out the full policymaker suite of pages including a crop price prediction page with a trend line showing historical prices alongside the model's prediction as the next data point, auto-generated context comparing the prediction to the historical average, and three metric cards; a choropleth map page showing both historical average prices and model predictions across all 25 EU countries with a year range slider; a side-by-side comparison page where policymakers can compare two countries, crops, or time periods across historical and predicted price tabs; and a policy report builder that allows policymakers to fill in findings and recommendations and download the report as a PDF or plain text file, or save it to the database. I also built the entire discussion board feature for all three personas, creating a shared community feed module with role-based filtering, reply threads, and post creation and deletion, connected to Flask routes for posts and comments. For the farmer persona, I implemented address-based farm location input using the geopy Nominatim library, replacing the previous lat/lon number inputs with a two-step flow where the farmer enters an address, sees a map preview, and confirms before saving. For the researcher persona, I built the Explore Trends page with filterable box plots and a crop distribution chart, and the Compare page where researchers can compare two seasons, crops, water sources, or countries side by side across temperature, humidity, and crop distribution charts. Finally, I improved the overall UI with a green agricultural theme, a redesigned home page with persona cards, and an About page with team section and data source descriptions.