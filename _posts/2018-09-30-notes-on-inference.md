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

(1) Analytically, $$b_1 = \frac{\sum(X_i-\overline{X})(Y_i-\overline{Y}){\sum(X_i-\overline{X})^2}$$.
(2) We know $$\sum(X_i-\overline{X})=0$$. Therefore, $$\sum(X_i-\overline{X})\overline{Y}=\overline{Y}\sum(X_i-\overline{X})=0$$.
(3) From (1) and (2), we can simplify $$b_1=\frac{\sum(X_i-\overline{X})Y_i}{\sum(X_i-\overline{X})^2}$$.
(4) For simplicity, fixed variables can be wrapped into a constant $$k$$, such taht $$b_1=\sum k_iY_i$$.