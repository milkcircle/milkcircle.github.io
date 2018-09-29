---
layout: post
title: Notes on Simple Linear Regression
date: 2018-09-29 12:00:00 -0700
categories: notes 
math: true
comments: true
author: Aaron
---
# Introduction
A simple linear regression model can be represented by $$Y_i = \beta_0 + \beta_1X_i + \epsilon_i$$. In such a model, several important properties hold:

1. As $$E(\epsilon_i) = 0$$, $$E(Y_i) = E(\beta_0 + \beta_1X_i + \epsilon_i) = \beta_0 + \beta_1X_i + E(\epsilon_i) = \beta_0 + \beta_1X_i$$.

2. $$\sigma^2(\epsilon_i) = \sigma^2$$ (that is, variance of $$\epsilon_i$$ is importantly a constant). It follows that $$\sigma^2(\beta_0 + \beta_1X_i + \epsilon_i) = \sigma^2{\epsilon_i} = \sigma^2$$.

3. Error terms are uncorrelated, which leads to $$Y_i$$ and $$Y_j$$ being uncorrelated for all $$i\neq j$$.

Depending on the situation, it may be convenient to represent the simple linear regression model as a function of the deviation $$X_i - \overline{X}$$. Then the alternative model becomes $$Y_i = \beta_0^{*} + \beta_1(X_i-\overline{X} + \epsilon_i)$$, where $$\beta_0^{*} = \beta_0+\beta_1\overline{X}$$.

# Method of Least Squares
In simple linear regression with a set of observations $$(X_i, Y_i)$$, the goal is to identify estimators $$\beta_0$$ and $$\beta_1$$ that minimize some measure of error. In the method of least squares, we aim to minimize $$Q=\sum\limits_{i=1}^n (Y_i-\beta_0-\beta_1X_i)^2 for the set of sample observations $$(X_1,Y_1)\ldots(X_n,Y_n)$$.

These estimators can be found either numerically or analytically. The following is the analytical derivation of a closed-form solution for $$\beta_0$$ and $$\beta_1$$.

## Minimize $$Q$$
$$\frac{\partial Q}{\partial\beta_0}=-2\sum(Y_i-\beta_0-\beta_1X_i)=0$$  
$$\frac{\partial Q}{\partial\beta_1}=-2\sum X_i(Y_i-\beta_0-\beta_1X_i)=0$$.

Expand the above to get our two normal equations.  
$$\sum Y_i - nb_0 - b_1\sum(X_i) = 0$$  
$$\sum X_iY_i - b_0\sum(X_i) - b_1\sum(X_i^2) = 0$$  
Note that we have replaced $$\beta$$ with $$b$$ to denote that $$b$$ is a particular value for $$\beta$$ that minimizes $$Q$$.

We can solve these normal equations simultaneously to obtain closed-form solutions for estimators $$b_0$$ and $$b_1$$:  
$$b_1 = \frac{\sum(X_i - \overline{X})(Y_i - \overline{Y})}{\sum(X_i-\overline{X})^2}$$  
$$b_0 = \frac{1}{n}(\sum(Y_i)-b_1\sum{X_i}) = \overline{Y} - b_1\overline{X}$$