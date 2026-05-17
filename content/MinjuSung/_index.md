---
title: "Minju Sung"
description: "Week 1 Blog Post, Leuven Experiences"
date: 2026-05-17
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
authors:
  - "Minju Sung"
---
<style>
  body:has(.strawberry-banner) {
    background: linear-gradient(to bottom, #fff5f8 0%, #ffe4ec 100%) fixed;
  }
  body:has(.strawberry-banner) header h1.text-4xl,
  body:has(.strawberry-banner) header h1.text-4xl + div {
    display: none;
  }
  .strawberry-banner {
    position: relative;
    width: 100%;
    height: 260px;
    margin: 1.5rem 0 2.5rem 0;
    border-radius: 14px;
    overflow: hidden;
    background-image: url('/strawberrybanner.webp');
    background-size: cover;
    background-position: center;
    box-shadow: 0 6px 24px rgba(255, 105, 135, 0.25);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .strawberry-banner::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, rgba(0,0,0,0) 35%, rgba(0,0,0,0.4) 100%);
  }
  .strawberry-banner h1 {
    position: relative;
    z-index: 1;
    margin: 0;
    color: #ffffff;
    font-size: 3.25rem;
    font-weight: 800;
    letter-spacing: 0.01em;
    text-shadow: 0 2px 14px rgba(0,0,0,0.45);
  }
  /* Center everything in the page content */
  body:has(.strawberry-banner) section .max-w-prose {
    text-align: center;
    margin-left: auto;
    margin-right: auto;
  }
  body:has(.strawberry-banner) section .max-w-prose h2,
  body:has(.strawberry-banner) section .max-w-prose h3 {
    text-align: center;
  }
  /* Polaroid scrapbook for the photos */
  .polaroid-row {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: flex-start;
    gap: 1.75rem;
    margin: 2.5rem 0;
  }
  .polaroid {
    background: #ffffff;
    padding: 14px 14px 44px 14px;
    box-shadow: 0 6px 18px rgba(0,0,0,0.18);
    border-radius: 3px;
    width: 240px;
    transition: transform 0.25s ease, box-shadow 0.25s ease;
  }
  .polaroid:nth-child(4n+1) { transform: rotate(-3deg); }
  .polaroid:nth-child(4n+2) { transform: rotate(2deg); }
  .polaroid:nth-child(4n+3) { transform: rotate(-1.5deg); }
  .polaroid:nth-child(4n+4) { transform: rotate(2.5deg); }
  .polaroid:hover {
    transform: rotate(0deg) scale(1.06);
    box-shadow: 0 10px 28px rgba(255, 105, 135, 0.35);
    z-index: 5;
  }
  .polaroid img {
    display: block;
    width: 100%;
    height: 220px;
    object-fit: cover;
    margin: 0;
    border-radius: 1px;
  }
  .polaroid figcaption {
    text-align: center;
    margin-top: 12px;
    font-family: 'Bradley Hand', 'Caveat', 'Segoe Script', 'Comic Sans MS', cursive;
    font-size: 1.15rem;
    color: #5a3a44;
  }
</style>

<div class="strawberry-banner">
  <h1>Minju Sung</h1>
</div>

## Leuven Experiences
### What I found interesting
I found our visit to the EU Parliament this past week especially interesting. It was helpful to visit after hearing from the guest speaker earlier in the schedule because it provided context for its history and made the experience much more meaningful. Despite its similarities to organizations such as NATO and the United Nations, the European Union has a different governmental structure that allows it to remain democratic while also giving smaller member states more influence within the union.

Additionally, I enjoyed learning about the EU’s direct impact on citizens’ daily lives, such as how trade regulations can influence taxes, inflation, and economic stability. It’s intriguing that some countries choose to leave the union despite its perceived benefits because of political ideologies, leadership decisions, or broader national disagreements. At the same time, other countries have been rejected from candidacy for failing to meet economic requirements. These discussions raised important questions about bias, political influence, and how national cooperation affects economic prosperity, trade, and currency regulation.


### Phase 1 Contributions
I contributed to Phase 1 by researching different topic areas for our project and identifying areas where our team could make meaningful contributions. This included speaking with guest speakers and local residents to better understand the challenges citizens and the country face in their everyday lives. These conversations helped me gain a stronger understanding of important social and economic issues, which allowed our team to better shape potential solutions.

Additionally, I contributed to the development of the user personas and helped ensure that the dataset we plan to use aligns with both the ethical and technical requirements of the project. This included evaluating factors like accessibility, rate limits, potential biases, and whether the dataset contained features that were both relevant and large enough to support the goals of our project.

![beguines](/IMG_0691.jpeg)
![EUParl](/IMG_0806.jpeg)
![EUOffice](/IMG_0803.jpeg)
![BrusselsCentral](/IMG_0831.jpeg)