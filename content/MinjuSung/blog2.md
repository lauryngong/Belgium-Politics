---
title: "Blog 2"
description: "Week 1 Blog Post, Leuven Experiences"
date: 2026-05-17
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
slug: "Blog 2"
authors:
  - "Minju Sung"
---

## Leuven Experiences
### What I found interesting
This past week we visited the attomium and the EEAS. It was really interesting to hear the different perspectives of speakers given their backgrounds and experiences with the EU. One of my favorite lectures was from Pierre Dewitte, who taught us about the relationship between AI Data Protection, Chatbots, and Law. He broke down the legal liability chain for newer technologies such as AI companions, its tendencies towards sycophancy, as well as accountability laws like the DSA (Digital Serivce Act), DMA (Digital Marketing Act), and the DFA (Digital Fairness Act). Because our program is more technology based, it was fun to hear the humanitarian aspect and impacts of our work. In the future I look forward to learning more about European regulations and laws as well as its applications on technology and AI.

Our visit to the attomium was also really fun. Unlike with the EEAS and lectures, we got a lot more time to explore on our own and learn about the city. I was surprised to learn, on that trip, that smurfs originated from Brussels, Belgium from an artist named Peyo!


### Phase 2 Contributions
I contributed to phase 2 of the project by collecting, cleaning, and creating a model for agriculture data in the Netherlands. When sorting through the data with my team, we first faced some difficulty finding strong relationships between different features that logically seemed like they should be related. For instance, 'Chlorophyll Content'-a generally good indicator of plant health-didn't seem to be strongly connected to rainfall or temperature-environmental variables that crops usually rely on for growth. We were able to address this issue by plotting out features against more direct indicators such as 'Crop Health Label' and creating a correlation table to find the strength of relationships between all of them. This also led us to using K-NN for our agriculture health indicator model later on when we found that the best predictions resulted from a combination of features and were produced more accurately when given a binary outcome.

I also ran into an issue when running the model on the dataset because the calculations were taking up too much space on the RAM, resulting in a Memory Error, or alternatively, a kernel timeout. I first tried to fix it by deleting calculations after collecting results or using an optimized loop, but the memory error persisted. It was eventually fixed by randomly dropping a percent (50%) of data from the dataset. (Approved by Dr. Gerber)


### Photos :D
---

![blogphoto1](/minb21.jpeg)
![blogphoto1](/minb22.jpeg)
![blogphoto1](/minb23.jpeg)
![blogphoto1](/minb24.jpeg)


