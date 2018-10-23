---
layout: post
title: Frequentism vs. Bayesianism 
date: 2016-08-04 12:00:00 -0700
categories: explanatory 
math: true
comments: true
author: Aaron
---
The distinctions between frequentism and Bayesianism are often difficult to appreciate, even though they are tremendously important in the way statistics is interpreted. Here I provide a brief primer on the biggest differences between these two schools of thoughts and how they affect statisticians’ interpretations of data and confidence measures.

Essentially, frequentists believe in the power of repeated sampling and that there is an *underlying absolute truth* that we can never truly ascertain. Bayesians, on the other hand, treat the underlying truth as a random variable, and believe that our world can always be updated.

What does this mean?

To put it in another — perhaps clearer — way, let’s take a look at a probability distribution: the Normal distribution. The Normal distribution takes two parameters: $$\mu$$ (population mean) and $$\sigma^2$$ (population variance). Frequentists will say that the underlying values for $$\mu$$ and $$\sigma^2$$ are fixed (these are intrinsic values to the population that do not change); their goal is to *repeatedly* draw random sample populations from the general population many times and use this data to estimate the actual values of $$\mu$$ and $$\sigma^2$$. In contrast, Bayesians use a set of *unchanging* data to estimate something about the parameters $$\mu$$ and $$\sigma^2$$; to them, $$\mu$$ and $$\sigma^2$$ are *random variables* that follow a probabilistic distribution. Frequentists treat $$\mu$$ and $$\sigma^2$$ as fixed values; Bayesians treat the data as fixed. The consequence of this difference in interpretation is that *the parameters for population distributions can vary from study to study in Bayesian statistics*. However, in frequentist statistics, *parameters are fixed between studies since they are considered the underlying true values for the general population*. Bayesian statistics is especially useful when it is not obvious that an experiment can be repeated (for instance, in polls, which generally occur only once); data is fixed and unrepeatable, so the frequentist approach of repeatedly drawing random samples is invalid under these circumstances.

Let’s talk about the interpretation of results under each statistical framework. The frequentist method of inference outputs a *95% confidence interval*. The definition of this confidence interval is often misunderstood by people unfamiliar with the field. Here is precisely what the confidence interval indicates: “Let’s repeat this experiment many times by drawing new random samples from the general population and applying this procedure to calculate the confidence interval. 95% of the time we do this calculation (each time with a *new random sample*), the confidence interval will include the true parameter.” In other words, if you calculate the confidence interval a bunch of times, each time with a different sample from the population, 95% of the time the confidence interval will actually encompass the true parameter of the distribution. It does *not* mean “we are 95% confident that the population mean is in the confidence interval”, though the difference in definitions when applied to science is often neglected.

The Bayesian method of inference, on the other hand, outputs a *credible interval*, which looks a lot like the confidence interval but has several important differences. We interpret a credible interval by saying “Using the data provided (with no repetitions of experiments), with probability 95%, the parameter will occur in the credible interval.”

Note the important difference here between *confidence interval* and *credible interval*. In a confidence interval, the bounds are random variables and the parameter is fixed. In a credible interval, the bounds are fixed, and the parameter is the random variable.

Here’s an interesting question: if frequentists are allowed to draw random samples a bunch of times to make their predictions more accurate, what is the analog in Bayesian inference? After all, in Bayesian statistics, the data is fixed, so what techniques can they use to improve their inference? Bayesian probability theory instead uses what’s called a *prior probability*, which is a best guess of the parameter’s distribution based on previous research. Using [Bayes’ theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem), the prior distribution is incorporated into the new data to come up with a posterior distribution. In other words, we begin with prior knowledge about the parameter, and then update our knowledge by conditioning on observed data. Priors generally come from previously published work, intuition, and expert knowledge, though they are often also chosen to make the calculation convenient. Importantly, these prior distributions can heavily skew the posterior distribution, so statisticians must carefully choose the prior and be able to argue for its inclusion in the model.

So which method is better? There is no hard answer to this question. There is a role for both techniques, and it depends on the type of problem you are trying to solve. For a highly controlled and repeatable experiment, it may be beneficial to use a frequentist analysis; however, in cases where reliable prior knowledge is available and the measurement cannot be repeated, Bayesian analysis can serve as a very powerful tool. Because statistical analysis is so heavily grounded in math, it can be easy to fall into the illusion that there is a single best method of evaluation for any scenario. However, the debate between frequentists and Bayesians illustrates that there is a component of subjectivity and preference, even in such a quantitative field of study.

For more information about the differences between these two philosophies, please check out the following resources:

1. Casella's *Bayesians and Frequentists* [presentation](http://www.stat.ufl.edu/archived/casella/Talks/BayesRefresher.pdf)
2. [XKCD](https://xkcd.com/1132/)
3. Does the debate [even matter](http://simplystatistics.org/2014/10/13/as-an-applied-statistician-i-find-the-frequentists-versus-bayesians-debate-completely-inconsequential/)?
4. Bayes Theorem in the [21st century]({{ site.url }}/download/201306_bayes.pdf)