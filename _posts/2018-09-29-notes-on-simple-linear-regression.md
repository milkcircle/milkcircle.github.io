---
layout: post
title: Notes on Simple Linear Regression
date: 2018-09-29 12:00:00 -0700
categories: notes 
math: true
comments: true
author: Aaron
---
A simple linear regression model can be represented by $$Y_i = \beta_0 + \beta_1X_i + \epsilon_i$$. In such a model, several important properties hold:

1. As $$E(\epsilon_i) = 0$$, $$E(Y_i) = E(\beta_0 + \beta_1X_i + \epsilon_i) = \beta_0 + \beta_1X_i + E(\epsilon_i) = \beta_0 + \beta_1X_i$$.

2. $$\sigma^2(\epsilon_i) = \sigma^2$$ (that is, variance of $$\epsilon_i$$ is importantly a constant). It follow that 
$$\sigma^2(\beta_0 + \beta_1X_i + \epsilon_i) = \sigma^2{\epsilon_i} = \sigma^2$$.

3. Error terms are uncorrelated, which leads to $$Y_i$$ and $$Y_j$$ being uncorrelated for all $$i\neq j$$.

Depending on the situation, it may be convenient to represent the simple linear regression model as a function of the deviation $$X_i - \overline{X}$$. Then
the alternative model becomes $$Y_i = \beta_0^{*} + \beta_1(X_i-\overline{X} + \epsilon_i)$$, where $$\beta_o^{*} = \beta_0+\beta_1\overline{X}$$.