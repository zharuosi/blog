---
title: 机器学习笔记 (3)
date: 2018-03-15 23:00:30
categories: 笔记篇之夙兴夜寐
tags: [Machine Learning,Artificial Intelligence,Classification,Logistic Regression]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

## Supervised Learning

In supervised learning, each example is a pair consisting of an input object (typically a vector) and a desired output value (also called the supervisory signal), like SVM, regression, decision trees, naive Bayes, etc.
<!-- more -->

### Logistic regression

In logistic regression, the dependent variable is categorical. Therefore, logistic regression is a classification model (with discrete labels). Logistic regression can be binomial, ordinal or multinomial.

The binary logistic model is used to estimate the probability of a binary response based on one or more predictor (or independent) variables (features). The output for a dependent variable can have only two values, e.g., "0" and "1". The conditional distribution $y \mid x$ is a Bernoulli distribution rather than a Gaussian distribution.
<img src="https://upload.wikimedia.org/wikipedia/commons/8/88/Logistic-curve.svg" width="50%" height="50%">
Fig. 1. The standard logistic function
The model function of LR model is called the logistic function:
$$ g(z) = \frac{1}{1+e^{-z}} $$
where $z$ is defined as:
$$ z = \theta^T x = \theta_0 + \theta_1 x_1 + ... + \theta_n x_n $$
We can obtain the generalized linear model function parameterized by $\theta$:
$$ h_{\theta}(x) = \frac{1}{1+e^{-\theta^T x}} $$
The probability of the dependent varible is:
$$p(y=1 \mid x;\theta) = h_{\theta}(x)$$
$$p(y=0 \mid x;\theta) = 1 - h_{\theta}(x)$$
$$p(y \mid x;\theta) = (h_{\theta}(x))^y (1 - h_{\theta}(x))^{1-y}$$

Maximum likehood estimation(MLE) is used to find the parameter values that maximize the likelihood function with the given observations. Define the likehood:
$$L(\theta) = p(\vec{y} \mid X;\theta) = (h_{\theta}(x))^y (1 - h_{\theta}(x))^{1-y}$$
$$ = \prod_{i=1}^m ((h_{\theta}(x^{(i)}))^{y^{(i)}} (1 - h_{\theta}(x^{(i)}))^{1-y^{(i)}} $$
In practice, it is often convenient to use the log-likelihood (natural logarithm):
$$\log L(\theta) = \sum_{i=1}{m} y^{(i)}\log h(x^{(i)}) + (1-y^{(i)})log(1 - h_{\theta}(x^{(i)}))$$
The problem is to find the independent variable at which the function values are maximized:
$${\rm argmax}_{\theta} \sum_{i=1}{m} y^{(i)}\log h(x^{(i)}) + (1-y^{(i)})log(1 - h_{\theta}(x^{(i)}))$$
After derivation, there are some optimization methods:
- Gradient descent
- Newton method
- Conjugate gradient
- etc.






