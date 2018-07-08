---
title: 机器学习笔记（5）理解支持向量机
date: 2018-06-09 22:33:35
categories: 笔记
tags: [Machine Learning,Artificial Intelligence,Support Vector Machine,Hyperplane]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

In this note, let's discuss about the support vector machine (SVM).

## Linear classifier

A classification decision is made by the linear classifier based on the value of a linear combination of the characteristics (features), which is typically presented to the machine by a feature vector.

- Input: $\vec{x}$
- Output: $y = f(\vec{w} \cdot \vec{x}) = f(\sum_j w_j x_j)$

$f$ is such a function, that maps the values above a threshold to the first class and other values to the second class, or give the probability. $\vec{w}$ is a vector of weights, which can be learned from a set of labeled samples. There are two classes of methods for determining the parameters of $\vec{w}$:
<!-- more -->
- Generative model: a model of the joint probability distribution on $P(X,Y)$
    - Linear discriminant analysis (LDA)
    - Naive Bayes classifier
    - Hidden Markov model
- discriminative model: a model of the conditional probability of $Y$ with an observation $x$ as $P(Y \mid X=x)$
    - Logistic regression: maximum likelihood estimation of $\vec{w}$
    - Perceptron: an algorithm that attempts to fix all errors encountered in the training set
    - SVM:  maximizes the margin between the decision hyperplane and the examples in the training set.
    - Neural networks

Generative algorithms try to learn $P(X,Y)$ which can be transformed into $P(Y \mid X=x)$ later to classify the data. Discriminative algorithms try to learn $P(Y \mid X)$ directly from the data and then classify data, does not care how the data was generated ($P(X)$). Here is the ralation:

$$P(Y \mid X) = P(X,Y) / P(X) = P(X \mid Y) P(Y) / P(X) $$ 

Generative algorithms are suitable for unsupervised learning. Discriminative models usually proceeds in a supervised way.

## Maximum margin classifier

In the case of support vector machines, a data point is viewed as a $p$-dimensional vector (a list of $p$ numbers). The points was supposed to separated by a $p-1$ dimensional hyperplane. Many hyperplanes exists. The best choice is the one that represents the largest margin between the two classes, which is known as the maximum-margin hyperplane. This classifier is also called as a maximum margin classifier. 

The equation of a hyperplane can be written as:
$$\omega^T x + b =0 $$
Two classes can be determined by:

- $f(x) = \omega^T x + b > 0$ leads to $y = 1$
- $f(x) = \omega^T x + b < 0$ leads to $y = -1$

The problem is how to find the optimum hyperplane with a maximum margin.

### Functional margin

The functional margin is defined as:
$$ \hat{\gamma} = yf(x) = y \cdot (\omega^T x + b) $$

The result would be positive for properly classified points and negative otherwise. Hence, a large functional margin represents a confident and a correct prediction. The functional margin has a problem. For example, the two equations $\omega^T x + b =0$ and $2\omega^T x + 2b =0$ represent the same hyper plane, but the functional margins differs. 

### Geometrical margin

The geometrical margin is defined as:
$$ \tilde{\gamma} = y\gamma = y \cdot \frac{\omega^T x + b}{|| \omega ||} = \frac{\hat{\gamma}}{|| \omega ||}$$
where $omega$ is the normal vector of the hyper plane. Here $|| \omega ||$ is the Euclidean norm (2-norm), gives the ordinary distance from the origin to the point $\omega$.






