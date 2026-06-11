---
title: "Blog 4"
description: "Week 4 Blog Post, Leuven Experiences"
date: 2026-05-17
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
slug: "Blog 4 yay!!!!"
authors:
  - "Minju Sung"
---

## Leuven Experiences
### What I found interesting
This week, we vistied the KU Leuven Energy Facility and C-mine in Genk. I got to see a lot of hardware used in machines and learn about the facilities' sustainability goals. At the mine, we did a vr experience and climbed 300 steps to get to the top of a tower; the view was great and it was cool to see the maze from above. Unfortunately, there were no diamond remnants in the mine...


### Phase 3 Contributions
This week, I contributed to Phase IV of the project by finishing up the KNN model for a crop recommender for farmers based on available agricultural resources and completing the all farms page. I integrated the knn model into streamlit, created a database to store past predictions the farmers made (if they chose to save), and made all the routes to connect the database to the frontent. After predicting, it shows at most 5 crop recommendations along with photos of each crop. 
I also changed some of the databases since Phase III by deleting an ohe table (which stored all categories + subcategories of one hot encoded values) and added a table to store certain values from the predictions that a farmer might find useful. When integrating into streamlit my biggest challenge was figuring out how to encode input categorical values for predicting since they can't be scaled with the StandardScaler. My solution was to store all categories (now in an array) in the same order as it's stored in the training data, so that for all categories (set default to [0 0 0 ...]), I can flip the index of the correct column to 1 based on the index of the category in the array.
On the UI end, I adjusted the theme of our app to be more agriculture centered and added descriptions/help buttons for each input feature for the crop predictor model.