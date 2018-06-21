---
title: 机器学习笔记（5）理解支持向量机
date: 2018-06-09 22:33:35
categories: 笔记篇之夙兴夜寐
tags: [Machine Learning,Artificial Intelligence,Support Vector Machine,Hyperplane]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

## Support Vector Machine

### Linear classifier

A classification decision is made by the linear classifier based on the value of a linear combination of the characteristics (features), which is typically presented to the machine by a feature vector.

- Input: $\vec{x}$
- Output: $y = f(\vec{w} \cdot \vec{x}) = f(\sum_j w_j x_j)$

<!-- more -->
In the case of support vector machines, a data point is viewed as a $p$-dimensional vector (a list of $p$ numbers). The points was supposed to separated by a $p-1$ dimensional hyperplane. Many hyperplanes exists. The best choice is the one that represents the largest margin between the two classes, which is known as the maximum-margin hyperplane. This classifier is also called as a maximum margin classifier. 

Kernel machines are used to compute a non-linearly separable functions into a higher dimension linearly separable function.