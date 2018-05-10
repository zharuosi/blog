---
title: 机器学习笔记 (4)
date: 2018-05-02 07:54:54
categories: 笔记篇之夙兴夜寐
tags: [Machine Learning,Artificial Intelligence,Bayesian Learning,Maximum Likelihood Estimation,Bayes Optimal Classifier]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

## Bayesian Learning

If you flip a coin, what's the probability it will fall with the head up? To solve this problem, you may flip the coin many times. <!-- more -->You will get the data:
$$ X=\{X_i\}^n_{i=1}, X_i \in {H,T} $$
where $H$ means Head and $T$ means Tail. The probability will be
$$ P(Head) = \theta, P(Tail) = 1 - \theta $$
If you flip the coin 100 times, with 57 heads and 43 tails, the probability of $P (Head)$ should be 57/100. Because the flips are **i.i.d.**, which means:
- **Independent** events
- **Identically distributed** according to Bernoulli distribution

### Maximum likelihood estimation

The aim of the maximum likelihood estimation is to choose $\theta$ that maximizes the probability of observed data. MLE of probability of head can be calculated based on **i.i.d.** as below:
$$ \hat{\theta}_{MLE} = \arg \max \limits_{\theta} P(X \mid \theta) $$
$$ = \arg \max \limits_{\theta} \prod_{i=1}^n P(X_i \mid \theta) $$
$$ = \arg \max \limits_{\theta} \prod_{i:X_i=H}^{\alpha_H} \theta \prod_{i:X_i=T}^{\alpha_T} (1-\theta)$$
$$ = \arg \max \limits_{\theta} \theta^{\alpha_H} (1-\theta)^{\alpha_T} $$

Let $J(\theta)=\theta^{\alpha_H}(1-\theta)^{\alpha_T}$. It is easy to find the $\theta$ that maximizes the probability of observed data:
$$\frac{\partial J(\theta)}{\partial \theta} \Big\vert_{\theta=\hat{\theta}_{MLE}} = 0 $$
Thus the MLE of probability of head is:
$$\hat{\theta}_{MLE} = \frac{\alpha_H}{\alpha_H+\alpha_T}$$

### Prior and posterior

If you flip a coin 5 times, it is possible that 5 heads are obtained. In this case, the probability of head will be 1.0 if you use the maximum likelihood estimation. However, it is crazy to consider that the result of fliping a coin is always head. Thus, we need to use prior probability to adjust the probability calculation.

A prior probability distribution of an uncertain quantity is the probability distribution that would express one's beliefs about this quantity before some evidence is taken into account. A prior can be determined from past information such as previous experiments, or according to some principle such as Jeffreys prior.

A posterior probability distribution of an unknow quantity is the probability distribution treated as the conditional probability that is assigned after the relevant evidence or background is taken into account. In contrasts with the likelihood function $P(X \mid \theta)$, the posterior probability is given as:
$$ P(\theta \mid X) = \frac{P(X \mid \theta)P(\theta)}{P(X)} $$
where $P(\theta)$ is probability density function of the prior probability.

In Bayesian probability theory, if the posterior distributions $P(\theta \mid X)$ are in the same probability distribution family as the prior probability distribution $P(\theta)$, the prior and posterior are then called conjugate distributions, and the prior is called a conjugate prior for the likelihood function. When a family of conjugate priors exists, choosing a prior from that family simplifies posterior form.

An example of the conjugate prior: In the problem of coin flipping,
- Likelihood is binomial distribution:
$$ P(X \mid \theta) = \left( \begin{matrix} n \\ \alpha_H \end{matrix} \right) \theta^{\alpha_H} (1-\theta)^{\alpha_T} $$
$$ = \frac{n!}{\alpha_H! (n-\alpha_H)!} \theta^{\alpha_H} (1-\theta)^{\alpha_T} $$


- Prior is assumed as Beta distribution:
$$ P(\theta) = \frac{\theta^{\beta_H-1} (1-\theta)^{\beta_T-1}}{B(\beta_H,\beta_T)} \sim Beta(\beta_H,\beta_T) $$
where $B(\beta_H,\beta_T)=\frac{\Gamma(\beta_H) \Gamma(\beta_T)}{\Gamma(\beta_H+\beta_T)}$, $\Gamma(z)$ is the gamma function as an extension of the factorial function, $\Gamma(z) = \int_0^{\infty} x^{z-1}e^{-x} dx$, and $\Gamma(n) = (n-1)!$, if $n$ is a positive integer.

- Posterior is also Beta distribution:
$$ P(\theta \mid X) \sim Beta(\beta_H + \alpha_H,\beta_T + \alpha_T) $$

For binomial distribution, the conjugate prior is Beta distribution.







