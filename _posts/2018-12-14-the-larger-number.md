---
include: true
excerpt: "An unexpected result of a simple problem"
layout: post
title: The Larger Number
date: 2018-12-14 12:00:00 -0700
categories: explanatory
math: true
comments: true
author: Aaron
---

Consider the following puzzle:

A friend comes to you and says that he holds two different randomly generated numbers, one in either hand. You choose one hand, and he reveals the number he holds in that hand. You then have the option of either staying with that number or switching to the other hand, with the goal of picking the bigger number. You are given no other information other than that your friend is non-adversarial.

What is the best strategy to maximize your chance of picking the bigger number?

## Solution:

Let the two numbers be $$A$$ and $$B$$, where $$B>A$$. Arbitrarily select a number $$C$$. If the number you select is less than $$C$$, switch hands. If the number you select is greater than $$C$$, stay.

In the case where $$C<A<B$$ (the number you imagine is smaller than either number), you will end up switching, and there will be a 50% chance that you end with the greater number. Similarly, when $$C>B>A$$, you will stay regardless of whether you select $$B$$ or $$A$$, and you will have a 50% chance of ending with $$B$$.

However, following this algorithm ensures that if $$A<C<B$$, you will always arrive at $$B$$. 

Given that there is no information about the distribution from which your friend selects the two numbers, $$C$$ can theoretically be any number and confer the same advantage, as there is an infinite number of possible numbers on either side of $$C$$.

If your friend is adversarial, one way in which he can minimize your advantage using this strategy is to choose $$A$$ and $$B$$ such that the difference between the two numbers is extremely small, to minimize the chance that your chosen $$C$$ falls between them.