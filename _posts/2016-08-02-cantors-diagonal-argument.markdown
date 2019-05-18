---
layout: post
title: Cantor's Diagonal Argument 
date: 2016-08-02 11:13:25 -0700
categories: explanatory 
include: true
math: true
comments: true
author: Aaron
excerpt: "Here I write up my favorite metamathematical proof."
---

Below I describe an elegant proof first presented by the brilliant Georg Cantor. Through this argument Cantor determined that the set of all real numbers ($$\mathbb{R}$$) is uncountably — rather than countably — infinite. The proof demonstrates a powerful technique called “diagonalization” that heavily influenced the field of metamathematics; indeed, Gödel’s incompleteness theorems employ a similar technique to prove that no set of mathematical axioms can be both complete and consistent, thereby establishing one of the most important limitations in mathematics.

Cantor’s diagonal argument answers the question: is the set of all real numbers (meaning, all rational and irrational numbers) countably infinite? For a set of infinite objects to be countably infinite, there must exist a one-to-one correspondence of that set with the set of integers ($$\{\ldots, -1, 0, 1, 2, 3, \ldots \}$$). Put another way, for any n in the set, there must exist a system that one can employ to enumerate n. For example, the set of integers $$\mathbb{Z}$$ is countably infinite because for any integer, you can simply enumerate the integers with this pattern —$$0, -1, 1, -2, 2, -3, 3, \ldots$$ — to get to that integer. With this simple algorithm, given enough time you will be able to reach any integer. This characteristic makes the set of integers countably infinite. If you can create a bijection between a given set of infinite objects and the set of integers $$\mathbb{Z}$$, then that set must also be countably infinite.

Now we have the necessary terminology to pursue with Cantor’s proof. What follows is a proof by contradiction, a strategy mathematicians employ that assumes a statement to be disproven is true, then describes why the statement cannot be true. To begin, we assume that the set of all reals is countably infinite. It must follow then that we can create an infinite list of all numbers that exist in $$\mathbb{R}$$. The following example attempts to visualize what this infinite list might look like.

$$1 \rightarrow 0.500000000\ldots$$<br>
$$2 \rightarrow 0.333333333\ldots$$<br>
$$3 \rightarrow 3.141592653\ldots$$<br>
$$4 \rightarrow 4.56497494138\ldots$$<br>
$$5 \rightarrow 0.676767676767\ldots$$<br>
$$\vdots$$

Cantor shows that there must exist at least one real number that cannot be contained within this list. That is: the most foolproof algorithm for enumerating all the elements in $$\mathbb{R}$$ — simply list all of them! — must fail because there exists real numbers that the algorithm can never access.

What is this elusive real number that can’t even be contained within an infinitely ongoing list of all the elements in the set? To generate this number, take the diagonal of the list and add 1 to each digit. In the above example, the diagonal is $$0.3447 \ldots$$ (simply take the first digit of the first element, which is 0, then the second digit of the second element, which is 3, and so on). We add 1 to each digit to get $$1.4558 \ldots$$. By construction, this number never shows up in our infinite list! Therefore we have proven that this list cannot contain all real numbers. The set of reals must therefore be uncountably infinite.