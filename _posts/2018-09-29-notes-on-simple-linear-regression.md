---
layout: post
title: Notes on Simple Linear Regression
date: 2018-09-29 12:00:00 -0700
categories: notes 
math: true
comments: true
author: Aaron
---
*This post is a work in progress, and thus will not be listed in the Archive until complete.*

## Introduction
A simple linear regression model can be represented by $$Y_i = \beta_0 + \beta_1X_i + \epsilon_i$$. In such a model, several important properties hold:

1. As $$E(\epsilon_i) = 0$$, $$E(Y_i) = E(\beta_0 + \beta_1X_i + \epsilon_i) = \beta_0 + \beta_1X_i + E(\epsilon_i) = \beta_0 + \beta_1X_i$$.

2. $$\sigma^2(\epsilon_i) = \sigma^2$$ (that is, variance of $$\epsilon_i$$ is importantly a constant). It follows that $$\sigma^2(\beta_0 + \beta_1X_i + \epsilon_i) = \sigma^2{\epsilon_i} = \sigma^2$$.

3. Error terms are uncorrelated, which leads to $$Y_i$$ and $$Y_j$$ being uncorrelated for all $$i\neq j$$.

Depending on the situation, it may be convenient to represent the simple linear regression model as a function of the deviation $$X_i - \overline{X}$$. Then the alternative model becomes $$Y_i = \beta_0^{*} + \beta_1(X_i-\overline{X} + \epsilon_i)$$, where $$\beta_0^{*} = \beta_0+\beta_1\overline{X}$$.

## Method of Least Squares
In simple linear regression with a set of observations $$(X_i, Y_i)$$, the goal is to identify estimators $$\beta_0$$ and $$\beta_1$$ that minimize some measure of error. In the method of least squares, we aim to minimize $$Q=\sum\limits_{i=1}^n (Y_i-\beta_0-\beta_1X_i)^2$$ for the set of sample observations $$(X_1,Y_1)\ldots(X_n,Y_n)$$.

These estimators can be found either numerically or analytically. The following is the analytical derivation of a closed-form solution for $$\beta_0$$ and $$\beta_1$$.

# Minimize $$Q$$
$$\frac{\partial Q}{\partial\beta_0}=-2\sum(Y_i-\beta_0-\beta_1X_i):=0$$  
$$\frac{\partial Q}{\partial\beta_1}=-2\sum X_i(Y_i-\beta_0-\beta_1X_i):=0$$.

Expand the above to get our two normal equations. Note that we have replaced $$\beta$$ with $$b$$ to denote that $$b$$ is a particular value for $$\beta$$ that minimizes $$Q$$.  

$$\sum Y_i - nb_0 - b_1\sum(X_i) = 0$$  
$$\sum X_iY_i - b_0\sum(X_i) - b_1\sum(X_i^2) = 0$$  

We can solve the equations simultaneously to obtain closed-form solutions for estimators $$b_0$$ and $$b_1$$:  

$$b_1 = \frac{\sum(X_i - \overline{X})(Y_i - \overline{Y})}{\sum(X_i-\overline{X})^2}$$  
$$b_0 = \frac{1}{n}(\sum Y_i-b_1\sum{X_i}) = \overline{Y} - b_1\overline{X}$$

# Properties of least squares estimators
1. $$b_0$$ and $$b_1$$ are unbiased. That is, $$E(b_0)=\beta_0$$ and $$E(b_1)=\beta_1$$.
2. $$b_0$$ and $$b_1$$ have minimum variance among all unbiased linear estimators, where linear estimators are defined as those estimators which can be expressed as a linear combination of the $$Y_i$$.

# Residuals
We define $$e_i = Y_i - \hat{Y}$$, the distance of the observed $$Y_i$$ from the fitted regression line. Importantly, $$e_i$$ is different from $$\epsilon_i$$, which is defined $$\epsilon_i = Y_i - E(Y_i)$$ which involves the unknown true regression line.

# Properties of fitted regression lines
1. The sum of the residuals $$\sum\limits_{i=1}^n e_i = 0$$.
2. The sum of the squared residuals $$\sum e_i^2$$ is minimized.
3. The sum of the observed values $$\sum Y_i$$ is the sum of the fitted values $$\sum\hat{Y}_i$$.
4. The sum of the weighted residuals $$\sum X_ie_i$$ is zero.
5. $$\sum\hat{Y}_ie_i = 0$$.
6. The regression line passes through $$(\overline{X},\overline{Y})$$.

## Estimating error term variance $$\sigma^2$$

# Point estimator of $$\sigma^2$$
In a single population, the estimator $$s^2 = \frac{\sum\limits_{i=1}^n(Y_i-\hat{Y})^2}{n-1}$$, where dividing by $$n-1$$ is necessary to adjust for the degrees of freedom. This value comes from the fact that we are using $$\hat{Y}$$ as an estimate of $$\mu$$, and once we know $$n-1$$ values we can find the last value given our knowledge of $$\hat{Y}$$.

In our regression model, the sum of squares $$SSE = \sum\limits_{i=1}^n(Y_i-\hat{Y}_i)^2 = \sum\limits_{i=1}^n e_i^2$$. 

The appropriate estimator in this case becomes $$s^2 = \frac{\sum e_i^2}{n-2}$$, where the denominator comes from estimating both $$\beta_0$$ and $$\beta_1$$ to obtain the estimated means $$\hat{Y}_i$$. Importantly, $$E(s_\textrm{regression}^2) = \sigma^2$$.

## Assumption of normality in error terms
It is often reasonable to assume that $$\epsilon_i\sim N(0,\sigma^2)$$. This is justifiable in many situations, since error terms represent effects of unmeasured factors without reference to $$X$$. The composite effect usually tends to follow the central limit theorem and approach normality.

# Maximum likelihood method
Assume we have the following problem. We have a single population with standard deviation $$\sigma$$, but we do not know the true population mean $$\mu$$ and therefore wish to estimate $$\mu$$. We have some number of samples $$Y_1\ldots Y_n$$. The maximum likelihood estimator takes the product of the probability density function at each $$Y_i$$ assuming various $$\mu$$'s to determine the $$\mu$$ with the greatest likelihood. 

The product of densities viewed as a function of the unknown parameters is called the *likelihood function*, and is referred to as $$L(\mu)$$. It can be shown that for a normal population, the maximum likelihood estimator of $$\mu$$ is $$\overline{Y}$$, the sample mean.

This concept can be carried over to our normal error regression model. Recall that in this model, each $$Y_i\sim N(\beta_0+\beta_1X_i, \sigma^2)$$. We can fit our observations $$Y_i$$ into our likelihood function $$L(\beta_0,\beta_1,\sigma)$$ to identify the $$(b_0, b_1,s)$$ that best maximizes $$L$$.

$$L(\beta_0,\beta_1,\sigma)=\prod\limits_{i=1}^n f_i$$ where $$f_i = \frac{1}{\sigma\sqrt{2\pi}}\exp\left[-\frac{1}{2}\left(\frac{Y_i-(\beta_0+\beta_1X_i)}{\sigma}\right)^2\right]$$

Analytically, the MLE solutions are:

$$\hat{\beta}_0 = b_0$$
$$\hat{\beta}_1 = b_1$$
$$\hat{\sigma}^2 = \frac{\sum{(Y_i-\hat{Y_i})^2}}{n}$$