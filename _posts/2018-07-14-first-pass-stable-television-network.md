---
layout: post
title: Is First-Pass Stable Television Network Programming Possible?
date: 2018-07-14 12:00:00 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---
Two television networks, Network A and Network B, each have a set of $$n$$ television shows. Each television show $$A_1 \ldots A_n and B_1 \ldots B_n/$$ has its own rating, and no two shows share the same rating. There are $$n$$ programming slots, each of which can accomodate a single television show. Network A and Network B both wish to maximize the number of shows they are able to schedule into these slots. A network wins a slot if the show it schedules for that slot has a higher rating than the show the other network proposes for that slot.

A pair of proposed schedules $$(A, B)$$ is stable if neither network can unilaterally change their proposed schedule to win more time slots.

Question: Does there exist an algorithm which, for every possible set of television shows and associated ratings, can output a set of schedules $$(A, B)$$ that is stable?


The answer is, surprisingly, no. A simple list of 4 television shows (2 by network A, and 2 by network B) provides a counterexample in which one network will always be able to propose an alternate schedule to win more time slots.

Consider the following set of television shows and associated ratings: $$(A_1, A_2): (100, 80)$$ and $$(B_1, B_2): (90, 70)$$. There are two available prime-time slots for the 4 shows.

In scenario 1, $$A_1$$ competes with $$B_1$$ for a slot, and $$A_2$$ competes with $$B_2$$ for the other. In this scenario, Network A wins both slots; however, Network B can switch its proposed schedule and gain a slot (since when $$A_2$$ competes against $$B_1$$, $$B_1$$ wins).

In scenario 2, $$A_1$$ competes with $$B_2$$ for a slot, and $$A_2$$ competes with $$B_1$$ for the other. This time, Network A stands to gain a prime-time slot by switching its proposed schedule and thereby out-competing Network B for both slots.

This demonstrates that there exist show-rating configurations which are impossible to arrange into a stable pair of schedules.