---
layout: post
title: Hamilton's kin-selection equation
date: 2019-01-07 13:01:00 -0700
categories: notes 
math: true
comments: true
author: Aaron
---

To see how the covariance equation translates into Hamilton's kin-selection equation, begin with $$w\Delta g = \texttt{Cov}(w,g)$$ where $$g$$ is the breeding value that determines the level of altruism.  

The least-squares multiple regression that predicts fitness, $$w$$, can be written as $$w = \alpha + g\beta_{wg'ag'} + g'\beta_{wg''ag+\epsilon}$$ where $$g'$$ is the average $$g$$ value of an individual's social neighbors, $$\alpha$$ is a constant, and $$\epsilon$$ is the residual which is uncorrelated with $$g$$ and $$g'$$. The $$\beta$$ are partial regression coefficients that summarize costs and benefits in the following way: $$\beta_{wg'ag'}$$ is the effect of an individual's breeding value has on its own fitness in the presence of neighbors' $$g'$$, and $$\beta_{wg''ag}$$ is the effect of an individual's breeding value on the fitness of its neighbors.  

Solving for the condition under which $$w\Delta g > 0$$ gives Hamilton's inclusive fitness equation ($$rB > C$$) in the following form: $$\/beta_{wg'ag'}+\beta_{g'g}\beta_{wg''ag} > 0$$, where $$\beta_{g'g}$$ is the regression coefficient of relatedness.