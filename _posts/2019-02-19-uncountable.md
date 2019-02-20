---
layout: post
title: Cantor's First Uncountability Argument 
date: 2019-02-19 11:13:25 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---


Several years ago, I described Georg Cantor's [diagonal argument](https://aaroncheng.me/explanatory/2016/08/02/cantors-diagonal-argument.html) to prove the uncountability of the reals. Although this is one of his most famous contributions to set theory, it is actually not the first proof of $$\mathbb{R}$$'s uncountability; he proved it in a much different way twenty years earlier using the Nested Interval Theorem. Below is an explanation of his first proof...it's fun to see how wildly different the two proofs are and how they arrive at the same conclusion.  

Assume that the elements of $$\mathbb{R}$$ are enumerable, and there is a 1-1 and onto function $$f: \mathbb{N}\rightarrow\mathbb{R}$$.  

Consider a closed interval $$I_1$$ which does not contain $$x_1$$ where $$x_i=f(i)$$. Then consider $$I_2$$, a closed interval contained in $$I_1$$, which does not contain $$x_2$$. We can inductively construct intervals $$I_{n+1}$$ which are contained in $$I_n$$ which do not contain element $$x_{n+1}$$.  

Now we consider the intersection $$\bigcap_{n=1}^\infty I_n$$. For some real number $$x_{n_0} \in \mathbb{R}$$, it must be the case that $$x_{n_0} \notin \bigcap_{n=1}^\infty I_n$$. This means that $$\bigcap_{n=1}^\infty I_n = \emptyset$$, which contradicts the NIP. We must therefore conclude that $$\mathbb{R}$$ is indeed an uncountable set.  

The consequences of this conclusion are huge. It means that the reals are so numerous that they cannot be mapped onto the natural numbers, since if we tried there will always be leftover numbers that have not been mapped. And since $$\mathbb{N}$$ is countable, it must be the case that $$\mathbb{I}$$ (the set of irrational numbers) is not, otherwise $$\mathbb{R}$$ would be countable. So it stands to reason that the irrational numbers form a greater subset of $$\mathbb{R}$$ than $$\mathbb{Q}$$.