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

## Logistic regression

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
$$\log L(\theta) = \sum_{i=1}^m y^{(i)}\log h_{\theta}(x^{(i)}) + (1-y^{(i)})\log(1 - h_{\theta}(x^{(i)}))$$
A log-likelihood can be interpreted as a measure of "encoding length" - the number of bits you expect to spend to encode this information, which is called [cross-entropy](https://rdipietro.github.io/friendly-intro-to-cross-entropy-loss/).

The problem is to find the independent variable at which the function values are maximized:
$${argmax}_{\theta} \sum_{i=1}^m y^{(i)}\log h_{\theta}(x^{(i)}) + (1-y^{(i)})\log(1 - h_{\theta}(x^{(i)}))$$
By derivation, we can find the $\theta$. there are some optimization methods:
- Gradient <u>ascent</u> (to find a local maximum)
- Newton method
- Conjugate gradient
- etc.

### Gradient ascent
Apply a gradient ascent algorithm for every $j$:
$$ \theta_j:= \theta_j + \alpha \frac{\partial}{\partial \theta_j} \log L(\theta) $$
where $+$ means the direction is the same as the gradient.
From the definition of $g(z)$, we have:
$$ \frac{\partial g(z)}{\partial z} = g(z) (1 - g(z)) $$
$$ \frac{\partial h_{\theta}(x^{(i)})}{\partial \theta_j} = h_{\theta}(x^{(i)}) (1 - h_{\theta}(x^{(i)})) x_j $$
$$ \frac{\partial \log L(\theta)}{\partial \theta_j} = \sum_{i=1}^m (y^{(i)} - h_{\theta}(x^{(i)}))x_j $$
Thus,
- batch gradient ascent method:
$$ \theta_j:= \theta_j + \alpha \sum_{i=1}^m (y^{(i)} - h_{\theta}(x^{(i)})) x_j$$
- stochastic gradient ascent method:
$$ \theta_j:= \theta_j + \alpha (y^{(i)} - h_{\theta}(x^{(i)})) x_j$$
where $i$ is a randomly chosen sample. Then we can repeat choosing the sample and update the gradient and weights.

### Newton's method (Newton-Raphson method)

In numerical analysis, Newton's method is a iterative method for finding successively better approximations to the solution $x$ of a real-valued function in the form of $f(x) = 0$ based on Taylor expansion. The process is repeated as:
$$ \theta^{k+1}:=\theta^k - \frac{f(\theta^k)}{f'(\theta^k)}$$

Now we need to solve $\nabla \log L(\theta) = 0$, the process can be written as:
$$ \theta^{k+1}:=\theta^k - H^{-1} \nabla_{\theta} \log L(\theta) $$
where $H$ is the Hessian matrix with $n+1$ by $n+1$, whose entries are given by:
$$ H_{ij} = \frac{\partial^2 \log L(\theta)}{\partial \theta_i \partial \theta_j} $$

The problem is: (why 1/m?)
$$ argmin_{\theta} J(\theta) = \frac{1}{m} \sum_{i=1}^m -y^{(i)}\log h_{\theta}(x^{(i)}) - (1-y^{(i)})\log(1 - h_{\theta}(x^{(i)})) $$
The Newton's method is:
$$ \theta^{(t+1)} = \theta^{(t)} - H^{-1} \nabla J(\theta^{(t)}) $$
where the gradient of the cost function is:
$$ \nabla J(\theta) = \frac{1}{m} \sum_{i=1}^m (h_{\theta} (x^{(i)}) - y^{(i)} )x_j $$ 
and the Hessian Matrix is:
$$ H = \frac{1}{m} \sum_{i=1}^m h_{\theta}(x^{(i)})( 1 - h_{\theta}(x^{(i)}) )x^{(i)}(x^{(i)})^T $$
(H is a matrix, where is $j$?)

Logistic regression has a linear decision boundary (hyperplane).

### Case study









