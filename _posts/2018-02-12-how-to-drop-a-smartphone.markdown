---
layout: post
title: How to Drop a Smartphone 
date: 2018-02-12 12:00:00 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---
Here I share the solution to a puzzle I came across on [538](https://fivethirtyeight.com/features/whats-the-best-way-to-drop-a-smartphone/). The puzzle is stated below:

>You work for a tech firm developing the newest smartphone that supposedly can survive falls from great heights. Your firm wants to advertise the maximum height from which the phone can be dropped without breaking.
>
>You are given two of the smartphones and access to a 100-story tower from which you can drop either phone from whatever story you want. If it doesn’t break when it falls, you can retrieve it and use it for future drops. But if it breaks, you don’t get a replacement phone.
>
>Using the two phones, what is the minimum number of drops you need to ensure that you can determine exactly the highest story from which a dropped phone does not break? (Assume you know that it breaks when dropped from the very top.) What if, instead, the tower were 1,000 stories high?

My first approach in solving this problem was to consider the worst-case, brute-force solution. This is easy to think of: simply start from the first floor, and drop the phone on every story until it breaks. In this solution though, you would only need one phone, and the worst-case scenario is if the answer is the 99th floor; you would have to traverse every floor of the entire building to get there.

What if you bisect the building? With two phones, you could drop one phone on the 50th floor (halfway between the ground floor and the top floor). If it breaks, then you start on the bottom-most floor and work your way up every floor until the second floor breaks. If the phone survives on the 50th floor, then you could maybe bisect the remaining floors again and repeat. This gets us closer to the solution. If the answer is the 99th floor, you would throw the phone from floor 50, then 75, then 88, then … until you hit the 99th floor. If the answer is the 49th floor, you would need 50 throws in total.

This answer appears to get us a more efficient solution, but can we do better? What if we trisect the building? That is, we start with floor 33, then $$(100-33)/3 + 33$$, etc.? Indeed, this gets us an even better solution. But how do we know what floor to start on to get us the most efficient solution?

I considered what seemed to me to be an obvious solution. Take $$n = \sqrt{f}$$ where $$f$$ is the number of floors in the building. Then traverse the building in increments of $$n$$ from the bottom until the first phone breaks. When the first phone breaks, start from the highest floor from which you know the phone did not break, and go floor by floor until the second phone breaks (since in order to determine the floor at which the phone breaks, you need to throw a phone from that floor and the floor below).

This solution seemed pretty good. The worst case scenario in this solution is if the phone breaks no lower than the 99th floor; you would throw the phone 9 times to reach the 90th floor, then 9 more times to get to the 99th floor, totalling 18 throws. Indeed, the number of throws in the worst case scenario becomes $$2(n-1)$$. So in the case of a 1000-story building, the worst case scenario would be 62. Not too bad.

Can we do better?

Turns out, we can. If we can assume that the floor at which the phone first breaks is uniformly distributed throughout the building, then perhaps we can distribute the floors in such a way that instead of there being only one worst-case scenario, there are many worst-case scenarios. In other words, instead of incrementing by 10 floors every time, maybe we should adjust our increment with every phone throw, so that the worst-case scenario (floor 99) doesn’t take so many more throws than if the phone were to first break at any of the other floors.

Consider incrementing in the following way. $$I_1$$ is our first increment from the ground floor. If the first phone survives that throw, then you will have thrown the phone 1 time, so the next increment should be $$I_2 = I_1 - 1$$ to account for the throw we just did. Then $$I_3 = I_2 - 1$$ and so on and so forth.

To find the first increment, then, we must identify the number $$I_1$$ such that $$I_1 + (I_1-1) + (I_2-1) + \ldots \geq f$$ where $$f$$ is the number of floors in the building. Ultimately we solve for $$I_1^2 + I_1 - 2f \geq 0$$, since the sum $$1+2+\ldots+n = \frac{n(n+1)}{2}$$. A generalized formula for $$I_1$$ becomes $$I_1 = [\frac{\sqrt{8f+1} - 1}{2}]$$ (the brackets here indicate to round up to the nearest integer). This number, $$I_1$$, turns out to be the maximum number of throws you’ll need to determine the first floor at which the phone breaks.

Imagine a scenario with m phones. This allows us, after the first phone breaks, to reduce the problem to a building of $$I_1$$ size for $$m-1$$ times, until you have one phone left. You would simply recalculate $$I_1$$ with the following equation: $$I_1' = [\frac{\sqrt{8I_1+1} - 1}{2}]$$.