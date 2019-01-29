---
layout: post
title: Visualizing pre-transfusion blood parameters
date: 2019-01-29 13:00:00 -0700
categories: explanatory
math: true
comments: true
author: Aaron
---

There is no easy way for patients who are chronically transfused to visualize important blood parameters. For patients with aplastic anemia, MDS, thalassemia, etc. who are transfused on a regular basis (typically about once a month), parameters of interest include ferritin (a proxy for total body iron levels) and hemoglobin. Beloow I share a script to generate a visual plot of both parameters, which may be useful for patients who are able to access their numbers from their hospital system.  

The generated plot looks like this, and allows for easy comparison of ferritin and pre-transfusion hemoglobin trends:  

![]({{ site.url }}/images/transfusion_ferritin_hgb.png)  

Though this image only shows data in 2018, the script may be used for as much data as a patient readily has on hand. As I had to manually input these values, I decided to use one year as a starting point. Even from this limited plot, one might notice a relationship between ferritin and pre-transfused hemoglobin (PTH); a rise in ferritin may be correlated with a low PTH. I have not yet done rigorous analysis to determine the correlation between these two variables, but one potential reason may be that a low PTH triggers a greater transfusional blood volume, which will in turn increase ferritin. If this were the case, the rise in ferritin should be offset by one measurement when compared to the PTH.  

Next steps include 1) correlating days between transfusions and change in Hgb, 2) incorporating transfused blood volume into the model, and 3) generating a predictive model for guessing the next measured Hgb based on information from prior transfusions.  