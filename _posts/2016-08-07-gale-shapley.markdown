---
layout: post
title: How a Computer Determines the Fate of Hundreds of Thousands of Medical Students
date: 2016-08-07 12:00:00 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---
The process for entering medical residency is unlike any other application process in the journey to become a physician. Unlike college and medical school applications, residency applications culminate in Match Day, where newly-minted M.D.s receive their fate in an envelope that names the hospital they have been assigned to for the next phase of their training. This matching process is often likened to a marriage, which is fitting because the algorithm used to conduct the match is a solution to a widely-known problem called the Stable Marriage Problem.

The Problem
===
The problem is stated in the following way: Assume there are two equally-sized sets of people, n men ($$m_1, \ldots, m_n$$) and n women ($$w_1, \ldots, w_n$$), who all have a preference list of the opposite sex. How do we pair up the men and women in such a way that they all stay with each other (nobody cheats on each other)?

To understand this question further it helps to consider an example where the arrangement is not stable. Consider a very small system with 2 men and 2 women. Let’s say $$m_1$$ prefers $$w_1$$ to $$w_2$$, and $$w_1$$ prefers $$m_1$$ to $$m_2$$. Then the following arrangement $$m_1$$—$$w_2$$, $$m_2$$—$$w_1$$ is unstable, because $$m_1$$ and $$w_1$$ would simply elope and leave their partners in the dust! We definitely don’t want that.

The Solution
===
It turns out that our intuitive way of thinking about how marriages work in Western society actually comes pretty close to solving this problem. The following solution is known as the Gale-Shapley algorithm, and it is deceptively simple:

1. Take a man who is still single, and match him up with whoever’s highest on his preference list. Now he is *engaged* with the highest-ranked possible woman on his list.
2. If the man is matched to a woman who is already engaged by another man, the woman chooses whichever man is highest on her preference list. The losing man becomes single and goes back into the pile; the next time he selects a woman, he chooses the next woman on his preference list.
3. Repeat this algorithm until there are no remaining single people.

Sounds familiar, right? Very reminiscent of our society’s marriage algorithm (except I think women generally are not as quick to call off engagements when a better man comes along, nor do men generally propose to women who are already in engagements).

The G-S algorithm is actually a fantastic solution to the Stable Marriage problem. But it is clearly unfair. Indeed, if you look carefully at how the algorithm is constructed, men end up with a better outcome than women, because men move *down* their preference list, whereas women move *up* theirs. This one-sidedness is actually quite relevant to the medical school match and will be discussed in a little bit, after we skim through some other interesting characteristics of the G-S algorithm.

Run-time
===
First let’s talk a bit about algorithm bounding. When computer scientists generate new algorithms, one of the most important things they want to find out about the algorithm is whether it’s any better than preexisting algorithms for the same problem. An algorithm is generally considered better if 1) it works faster, or 2) it requires less storage space. To compare algorithms, then, it is important to have a measure of speed or storage efficiency. We use “Big-O” notation to determine worst-case time as a function of the size of the problem. For instance, consider the simple problem of searching through an unsorted array of size $$n$$. The worst-case scenario is if we start from the left and the element that we want is all the way at the other end of the array; we would have to traverse through all $$n$$ elements. So this algorithm is O($$n$$). Importantly, the worst-case time grows linearly as the array grows. Generally speaking, we care less about the absolute time that it takes for an algorithm to operate (since the absolute time depends so heavily on the technology used) than how the time grows as the problem gets larger.

What is the worst case running time for the G-S algorithm? This is pretty simple to answer: we can imagine a scenario where each of the $$n$$ men have to traverse all the way down their respective preference lists (which are $$n$$ elements long) before we reach a stable arrangement. This makes the problem O($$n^2$$).

Proof of correctness
===
How do we know that the algorithm works and will end in a stable arrangement? We can prove this in a couple steps.

*How do we know that no man will “run out” of women on his preference list?* In other words, will any man remain single by the time he has proposed to every single woman available? To show the answer is no, first assume that there is a man $$m$$ who is single even after he has proposed to all women. Then this suggests that each woman is already in engaged. However, since there are $$n$$ men and $$n$$ women, if each woman is engaged, then each man must be in an engagement as well, so m cannot be single! Therefore we know that if a man is single at any point in the algorithm, there must be at least one woman he hasn’t proposed to yet. (Note that this is slightly different than how true courtship works; in our society, a man may very well be rejected by every woman he proposes to and remain unfortunately single at the end!) Now we know that when the algorithm terminates, everybody man and woman will be in a pair.

*How do we know that the algorithm ends in a stable arrangement?* Again proof by contradiction gets us the answer here. Assume that there is in fact an instability; that is, there is a scenario where $$m_1$$ prefers $$w_1$$ to $$w_2$$, and $$w_1$$ prefers $$m_1$$ to $$m_2$$…but $$m_1$$—$$w_2$$ and $$m_2$$—$$w_1$$. Now let’s think of how this scenario could have arisen. We know that in order to end in this pairing, $$m_1$$‘s last proposal was to $$w_2$$. Did he propose to $$w_1$$ before or not? If he didn’t, then this would contradict the fact that he prefers $$w_1$$ over $$w_2$$. If he did, then this means $$w_1$$ must have rejected him for some other man who we’ll call $$m'$$ (who may or may not at this point be $$m_2$$). Now, since we know that in the final pairing $$w_1$$ is matched with $$m_2$$, this must mean that either $$m' = m_2$$, or $$w_1$$ rejected $$m'$$ in favor of $$m_2$$. But according to our algorithm $$w_1$$ would not have rejected $$m_1$$ over the others because she prefers him! So we run into a contradition, demonstrating that the algorithm must not contain an instability.

Interesting behaviors of the G-S algorithm
===
Some natural follow-up questions arise from our analysis of the G-S algorithm. For instance:

1. We didn’t specify the order to select men. Does it matter in what order we loop through the set of men? Does every execution of the G-S algorithm yield the same stable arrangement?
2. We mentioned before that the algorithm is unfair. Just how unfair is it?

Surprisingly, the answer to Question 1 turns out to be: every execution, regardless of loop order (through men), results in the same stable arrangement! And the resulting stable arrangement turns out to be the best possible outcome for every man simultaneously (our answer for Question 2)! These results are not intuitive at all but can be proved quite simply using the strategies laid out before.

*1) The resulting stable arrangement yields the best possible outcome for every man.* That is, assume that there exist several stable arrangements. The G-S algorithm outputs a stable arrangement in which each man is paired with the best possible partner he could have in any stable arrangement! To prove this, let’s assume that this is not the case; that is, let’s say that the G-S algorithm matches $$m—w'$$, but there exists another stable arrangement (which we’ll call $$S'$$) in which $$m—w$$, whom he prefers over $$w'$$. So with this assumption the G-S algorithm outputs a stable arrangement that is suboptimal for $$m$$. Can we find a contradiction?

Well, for this pairing $$m—w'$$ to have occurred, this means that $$w$$ must have at some point rejected $$m$$ prior to his current engagement. Now, let’s call this rejection the very first rejection to occur during the algorithm’s runtime. In other words, let’s assume that at no point before this rejection (when $$w$$ rejected $$m$$) did a woman turn down a man’s proposal. Let’s say $$w$$ goes on to be engaged to $$m'$$ who she prefers over $$m$$. Then, since there were no prior rejections, $$m'$$ must prefer $$w$$ to all other possible stable partners. But $$m'—w$$ is not a possible pairing in $$S'$$ because we defined $$S'$$ to be the stable arrangement containing $$m—w$$, and a single woman cannot be simultaneously engaged to two different men! Indeed, this contradiction allows us to conclude that *(1) the G-S algorithm outputs a stable arrangement in which every man ends up with the most preferred woman he could have in any stable arrangement*. This is not an intuitive result, but is quite convenient (especially for the men, in our scenario).

It is also possible to prove that conversely, *2) the resulting stable arrangement yields the worst possible outcome for every woman*. Again, let’s call the G-S output $$S$$. Assume that (2) is false; that is, assume that in $$S$$, there exists a pairing $$m—w$$ in which $$m$$ is not the worst possible partner for $$w$$. Then there must exist a stable arrangement $$S'$$ with the pair $$m'—w$$ such that $$m'$$ is lower on $$w$$‘s preference list than $$m$$. In $$S'$$, m is paired with a woman $$w'$$ who he likes more than $$w$$ (by our proof of statement (1)). But if $$m$$ prefers $$w'$$ to $$w$$, then he should have been paired with $$w'$$ rather than $$w$$ in $$S$$, yielding a contradiction! Therefore, *(2) the G-S algorithm outputs a stable arrangement in which every woman ends up in the worst possible stable pair*. Yikes.

In summary, in this section we showed that (1) G-S outputs one and only one stable arrangement, regardless of how you loop through the men, and (2) G-S outputs a stable arrangement that favors the proposers.

The medical residency matching process
===
The G-S algorithm above isn’t exactly the solution to the residency match. First, there are far fewer hospitals than there are residents. You can bypass this by considering residency *spots* rather than hospitals, in which case the residency match starts to resemble an $$n$$ by $$n$$ problem as described above. However, there are still typically more applicants than spots available. When you throw in the constraint of “couples matching” (where applicants in long-term relationships look to stay in the same geographic area), then things start to look a lot more complicated.

In the 1950s, Gale and Shapley’s algorithm was adopted by the National Residency Matching Program. During this time, most applicants were male, and the “couples matching” problem had not arisen to its full force. In the 1980s, when the couples problem became more prominent, Alvin Roth *et. al.* further improved on the G-S algorithm to include couples matching. The new algorithm executes the G-S algorithm for single applicants first, then incorporates couples afterward; couples may displace certain single applicants. After this step, the displaced single applicants are again brought back into the matching game. This process repeats until the program terminates. One new problem generated by this modification to the G-S algorithm is that the resulting arrangement is no longer guaranteed to be stable! However, historically speaking, every arrangement output by the Roth-Peranson algorithm has been stable, likely because the market is so large. Indeed, Roth showed in a later paper that the likelihood of arriving at a stable arrangement approaches 1 as the size of the market approaches infinity.

Another interesting historical note: recall that the proposing side always gets the best possible outcome in the G-S algorithm, and the other side always got the worst possible stable arrangement. Until 1997, the NRMP implemented a *pro-hospital* version of the algorithm that favored the hospital’s preferences over the residents’ preferences. Fortunately, today the algorithm is *pro-resident*; this change has presumably improved the quality of life for many residents quite drastically.

Resources
===
1. A medical student [runs a simulation](https://medium.com/@vishnuravi/how-long-does-the-residency-match-algorithm-take-to-run-c38c06cd4d57#.w24fixu0i) of the match and finds that it takes less than half a minute to determine the fate of tens of thousands of residents.
2. A nice high-level [overview and history](https://www.simonsfoundation.org/mathematics-and-physical-science/playing-with-matches/) of the medical residency match
3. The [Roth-Peranson algorithm](https://web.stanford.edu/~alroth/papers/rothperansonaer.PDF)
4. A walk-through of the [Gale-Shapley algorithm](http://mathsite.math.berkeley.edu/smp/smp.html)