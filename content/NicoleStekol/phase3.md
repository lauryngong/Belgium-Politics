---
title: "Phase 3 Post"
description: "This post is about my contributions to Phase 3"
date: 2026-06-05
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
slug: "Blog3"
authors:
  - "Nicole Stekol"
---

# Phase 3 Contributions
My contributions to Phase 3 were mainly all in the form of the predicting crop prices linear regression model. As documented in the team blog post, I fit many different iterations of models and did feature engineering to find the model that output the optimal r2, which is the model that included temp, precipitation, precipitation squared, and 2 lag prices (to make it a time series model). I then created a few databases and input my features, and moved my model into the python script in the predict() function. I then created a route for the model so it would return the predicted price when given the url with the crop and country. However, this is what gave me many issues because I kept getting the error that the country column could not be found in my weather database, when it was actually there. None of the faculty could help with this as well so this will need to be fixed so that I can route my model and get it connected with the front end.

# My Belgium Experience
Last week, we visited Strasbourg and Luxembourg, which were both very fun and interesting to explore! I really enjoyed walking around Strasbourg and even get some shopping done, and Luxembourg really shocked me with how the public transport is both completely free and the cleanest transport I've ever seen! I'm happy to be back in Leuven and get to walk around now that it is so familiar.