---
layout: post
title: Notes on Inference and Correlation Analysis
date: 2018-09-30 12:00:00 -0700
categories: notes 
math: true
comments: true
author: Aaron
---
*This post is unfinished and in progress.*

## Introduction
The following derivations assume a normal error regression model $$Y_i=\beta_0+\beta_1X_i+\epsilon_i$$.

## Inferences concerning $$\beta_1$$
# Sampling distribution of $$\beta_1$$
Here, we show that $$\beta_1$$ is expressible as a linear combination of observations $$Y_i$$.  

(1) Analytically, $$b_1=\frac{\sum(X_i-\overline{X})(Y_i-\overline{Y})}{\sum(X_i-\overline{X})^2}$$  
(2) We know $$\sum(X_i-\overline{X})=0$$. Therefore, $$\sum(X_i-\overline{X})\overline{Y}=\overline{Y}\sum(X_i-\overline{X})=0$$  
(3) From (1) and (2), we can simplify $$b_1$$ to $$b_1=\frac{\sum(X_i-\overline{X})Y_i}{\sum(X_i-\overline{X})^2}$$  
(4) For simplicity, fixed variables can be wrapped into a constant $$k$$, such that $$b_1=\sum k_iY_i$$.  

Since $$b_1$$ is a linear combination of $$Y_i$$, and $$Y_i \sim N(\mu,\sigma^2)$$, $$b_1$$ is also normally distributed.  

$$k_i$$ has several important properties which I will not derive but are important to know about:  
(1) $$\sum k_i = 0$$  
(2) $$\sum k_iX_i = 1$$  
(3) $$\sum k_i^2 = \frac{1}{\sum(X_i-\overline{X})^2}$$  

