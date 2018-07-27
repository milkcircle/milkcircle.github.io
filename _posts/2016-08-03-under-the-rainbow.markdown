---
layout: post
title: Under the Rainbow 
date: 2016-08-03 12:00:00 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---
Here I present my short solution to Project Euler problem 493. The problem goes as follows:

>70 colored balls are placed in an urn, 10 for each of the seven rainbow colors. What is the expected number of distinct colors in 20 randomly picked balls? Give your answer with nine digits after the decimal point (a.bcdefghij).

Solution: Use indicator variables, where $$I_i$$ represents the success of drawing a ball of the  $$i^{th}$$ color. We want to find $$E(I_1+\ldots+I_7)$$. By linearity of expectation (since the event for each color is independent), this equates to $$7\timesE(I_1)$$. Now, we can reduce this problem to a problem of 2 colors. Consider a bucket with 10 balls of color 1 and 60 balls of all other colors. The probability of selecting at least 1 ball of color 1 in 20 draws (without replacement) is given by $$1-P(\textrm{no ball of color 1}) = 1-HGeom(0,10,60,20)$$, where HGeom represents the hypergeometric distribution. Multiply this by 7 to get the final answer.

The following code in R immediately retrieves the answer.

{% highlight R %}
7*(1-phyper(0,10,60,20))
{% endhighlight %}