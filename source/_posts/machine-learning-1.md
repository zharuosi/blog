---
title: 机器学习笔记 (1)
date: 2018-02-28 23:48:48
categories: 笔记篇之夙兴夜寐
tags: [Machine Learning,Artificial Intelligence]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.


## Elementary concepts

### What is machine learning?

Machine Learning is a scientific discipline that explores the construction and study of algorithms that can learn from data. Machine learning enables analysis of massive quantities of data. We all know "**Machine**", i.e. computers or computer programs. Do you know what is "**Learning**"? <!-- more --> Learning denotes "Changes in the system that are adaptive in the sense that they enable the system to do same task or tasks drawn from the same population more efficiently and more effectively the next time" [[1]](http://ktubtechquestions.com/2015/06/27/cusat-b-tech-question-paper/). As a pioneer in the field of artificial intelligence, *Herbert A. Simon* created with Allen Newell the Logic Theory Machine (1956), and the General Problem Solver (GPS) (1957) programs, which was thought as the first method developed for separating problem solving strategy from information about particular problems.

*Arthur Samuel* created the world's first successful self-learning programs, the Samuel Checkers-playing. He coined the term "Machine Learning" in 1959, which was defined as "the field of study that gives computers the alility to learn without being explicitly programmed".

*Tom Mitchell* (former Chair of the Machine Learning Department at CMU) gave a modern definition: "A computer program is said to learn from experience $E$ with respect to some class of tasks T and performance measure $P$, if its performance at tasks in $T$, as measured by $P$, improves with experience $E$." 

An example: playing checkers.

- $E$ = the experience of playing many games of checkers.
- $T$ = the task of playing checkers.
- $P$ = the probability that the program will win the next game.

### What can machine learning do？

ML can be used to deal with Big Data deluge and predicitve analytics, e.g.

  - Document Classification
  - Spam Filtering
  - Weather Prediction
  - Stock Market Prediction
  - Collaborative Filtering
  - Clustering Images
  - Human Genetics
  - Decoding thoughts from brain scans
  - Medical data analysis
  - Finance
  - Robotics
  - Natural language processing, speech recognition
  - Computer vision
  - Web forensics
  - Computational biology
  - Sensor networks (new multi-modal sensing devices)
  - Social networks
  - Turbulence problem

## Three components of a machine learning algorithm

Pedro Domingos, a CS professor at the University of Washington, decomposed machine learning into three components: Representation, Evaluation, and Optimization [[2]](https://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf).

Table 1. The three components of learning algorithms

| Represnetation | Evaluation | Optimization |
| :---- | :---- | :---- |
| [Instances](https://en.wikipedia.org/wiki/Instance-based_learning) | Accuracy/Error rate | Combinatorial optimization |
| ^K-nearest neighbor | Precision and recall | ^Greedy search |
| ^Support vector machines | Squared error | ^Beam search |
| [Hyperplanes](https://www.svm-tutorial.com/2015/06/svm-understanding-math-part-3/) | Likelihood | ^Branch-and-bound |
| ^Naive Bayes | Posterior probability | Continuous optimization |
| ^Logistic regression | Information gain | ^Unconstrained (Convex) |
| [Decision trees](https://en.wikipedia.org/wiki/Decision_tree_learning) | K-L divergence | ^^Gradient descent |
| [Sets of rules](https://en.wikipedia.org/wiki/Rule-based_machine_learning) | Cost/Utility | ^^Conjugate gradient |
| ^Propositional rules | Margin | ^^Quasi-Newton methods |
| ^Logic programs |   | ^Constrained |
| [Neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network) |   | ^^Linear programming |
| [Graphical models](https://en.wikipedia.org/wiki/Graphical_model) |    | ^^Quadratic programming |
| ^Bayesian networks |    |    |
| ^Conditional random fields |    |    |


### Representation

A classifier must be represented in some formal language that the computer can handle. A learner  takes observations as inputs. The **observation language** is the language used to describe these observations. The hypotheses that a learner may produce, will be formulated in a language that is  called the **hypothesis language**. The **hypothesis space** is the set of hypotheses that can be described using this hypothesis language [[3]](https://lirias.kuleuven.be/bitstream/123456789/298291/1/hyp-space.pdf). The problem is how to represent the input, i.e. what features to use. For example, 3-layer feedforward neural networks (or computational graphs) form one type of representation, while support vector machines with RBF kernels form another.

### Evaluation

Evaluation is essentially how you judge or prefer candidate programs (hypotheses). An evaluation function (also called objective function, utility function, loss function, fitness function or scoring function) is needed. Mean squared error (of a model’s output vs. the data output) or likelihood (the estimated probability of a model given the observed data) are examples of different evaluation functions.

### Optimization

Finally, a method is needed to search among the candidate programs (in the space of represented models) for the highest-scoring one as the optimum. Stochastic gradient descent and genetic algorithms are two different ways of optimizing a model class.

## Machine Learning Tasks

Machine learning tasks are typically classified into two broad categories, depending on whether there is a learning "signal" or "feedback" available to a learning system: superivsed learning and unsupervised learning. Other tasks also include semi-supervised learning, active learning, reinforcement learning, etc.

### Supervised learning

Supervised learning (or inductive learning) is the most mature, the most studied and the type of learning used by most machine learning algorithms. Learning with supervision is much easier than learning without supervision. Training data includes desired outputs. The goal is to learn a general rule that maps inputs to outputs. Typically, the inputs are transformed into a feature vector, which contains a number of features that are descriptive of the object.

- Classification
- Regression

Reinforcement learning is concerned with how to take actions in an environment to maximize some notion of long-term reward, such as game-theory. It differs from standard supervised learning in that correct input/output pairs are not provided, nor sub-optimal actions explicitly corrected.

### Unsupervised learning

Training data does not include desired outputs. It is hard to tell what is good learning and what is not. The program own will find the feature in its inputs as "unlabeled" data.

- Clustering
- Dimensionality reduction
- Anomaly dection
- Neural Networks

## Main purpose of studying ML

To make money.

