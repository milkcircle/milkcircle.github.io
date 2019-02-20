---
layout: post
title: Uncountability and Cardinality 
date: 2019-02-20 11:13:25 -0700
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

A natural question arises from this discovery: are there infinities with even higher cardinalities than the reals?  

In 1891, Cantor proved this to be true; that is, there are even higher cardinalities than $$\mathbb{R}$$. Consider for any set $$A$$ its power set $$P(A)$$. There does not exist a function $$f: A\rightarrow P(A)$$ that is onto. For if $$f$$ were to exist, then we can find some $$B\subseteq A$$ where $$B\neq f(a)$$ for any $$a\in A$$.  

For every element $$a\in A$$, $$f(a)\subseteq A$$. $$f(a)$$ may or may not contain $$a$$. If $$a\nsubseteq f(a)$$, include it in the set $$B$$. Formally, $$B=\left\{ a\in A : a\nsubseteq f(a) \right\}$$. As we have assumed our function $$f$$ to be onto, it must be the case that $$B=f(a_0)$$ for some $$a_0\in A$$. Does $$B$$ contain $$a_0$$?  

If $$a_0\in B$$, then this means that $$a_0\in f(a_0)$$, as $$B=f(a_0)$$. However, $$a_0\in B$$ necessarily must mean that $$a_0\notin f(a_0)$$ by construction! As either an element cannot both exist and not exist in a given set, we have arrived at a contradiction that proves there exists no $$a_0\in A$$ for which $$B=f(a_0)$$. Therefore, $$B$$ is an element of the power set of $$A$$ that cannot be mapped onto $$A$$ by a function $$f$$.  

This result is massive. It implies that we can start with any given set of elements, and derive another set of elements (namely, its power set) with a greater cardinality than the origin set. So you can start with $$\mathbb{R}$$ and derive a set of infinite elements with an even higher cardinality than $$\mathbb{R}$$ (such that it cannot be mapped even onto the real numbers!), and then you can take this *new* set and create yet *another* set of elements with an even *greater* cardinality, and so forth.  

With this proof, Cantor essentially demonstrated that there is no such thing as a "largest cardinal number". That is, there is no such thing as a "largest infinity", for you can take any given infinity and construct one even greater than that.  

Not bad for a one-page proof.