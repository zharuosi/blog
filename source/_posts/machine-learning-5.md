---
title: 机器学习笔记（5）支持向量机
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
$$ \omega^T x - b = 0 $$
Note that $\omega^T x = \vec{\omega}\cdot \vec{x}$.

Two classes can be determined by:

- $f(x) = \omega^T x - b > 0$ leads to $y = 1$
- $f(x) = \omega^T x - b < 0$ leads to $y = -1$

The problem is how to find the optimum hyperplane with a maximum margin.

### Functional margin

The functional margin is defined as:
$$ \hat{\gamma} = yf(x) = y \cdot (\omega^T x - b) $$

The result would be positive for properly classified points and negative otherwise. Hence, a large functional margin represents a confident and a correct prediction. The functional margin has a problem. For example, the two equations $\omega^T x + b =0$ and $2\omega^T x + 2b =0$ represent the same hyper plane, but the functional margins differs. 

### Geometrical margin

The geometrical margin is defined as:
$$ \tilde{\gamma} = y\gamma = y \cdot \frac{\omega^T x - b}{|| \omega ||} = \frac{\hat{\gamma}}{|| \omega ||}$$
where $\omega$ is the normal vector of the hyper plane. Here $|| \omega ||$ is the Euclidean norm (2-norm), gives the ordinary distance from the origin to the point $\omega$.

The margin in maximum margin classifier refers to the geometrical margin. Samples on the margin are called the support vectors.

<img src="https://raw.githubusercontent.com/zharuosi/blog/source/source/images/Svm_max_sep_hyperplane_with_margin.png" width="35%" height="35%">

If the training data is linearly separable, two parallel hyperplanes can be selected to separate the two classes of data so that the distance between them (called margin) is as large as possible. After normalized or standardized, these hyperplanes are:
- label 1: $\omega^T x - b = 1$
- label -1: $\omega^T x - b = -1$

The geometrical margin between these two hyperplanes is $2/|| \omega ||$. The optimization problem is to minimize $\frac{1}{2}|| \omega ||^2$ s.t. (subject to) $y_i(\vec{\omega}\cdot \vec{x_i} - b)\geq 1$ for each $i$ in data. This optimization is called the constrained optimization with the 's.t.'. It can be solved efficiently by quadratic programming ($QP$). The max-margin hyperplane is completely determined by those $\vec{x_i}$ which lie nearest to the hyperplane. These $\vec{x_i}$ are called **support vectors**. Moving other points a little does not affect the decision boundary. To predict the labels of new points, only the support vectors are necessary.

### Soft-margin for not linearly separable data

It could be very complicated or over-fitting to classify all samples correctly in practical. For the soft-margin SVM, errors in classification are allowed, which is more common. The criteria now is to minimize the number and the value of noises. E.g. A large margin causes lots of misclassified samples. A smaller margin can reduce the number of errors, but each error becomes bigger.

<img src="https://ars.els-cdn.com/content/image/1-s2.0-S0926580516301297-gr4.jpg" width="45%" height="45%">

The optimization problem is to minimize $\frac{1}{2}|| \omega ||^2 + C \sum_{i} \xi_i$ s.t. $y_i(\vec{\omega}\cdot \vec{x_i} - b)\geq 1-\xi_i$ and $\xi_i \geq 0$ for each $i$ in data. $\xi$ is pronounced as /ksi/, called "slack" variables, pay linear penalty. 
- $\xi_i = 0$ means the classification of sample $i$ is correct. The larger $x_i$ is, the bigger mistake the classification makes.
- $0 < \xi_i \leq 1$ means sample $i$ is in the margin, resulting to margin violation.
- $\xi_i > 1$ stands for sample $i$ is misclassified. 
 
$C$ is the tradeoff parameter, chosen by cross-validation. A large $C$ means we will get a small margin width and reduce the number of errors as much as possible. Contrarily, a small $C$ means the margin width can be large even though we get more errors. The product $C \xi_i$ is the penalty for misclassifying. If $C \to \infty$, the soft-margin is recovered to the hard-margin SVM.

$\xi_i$ is defined as:

$$ \xi_i = loss(sgn(\vec{\omega}\cdot \vec{x_i} - b),y_i) = 1-y_i(\vec{\omega}\cdot \vec{x_i} - b) $$

With the regularization and introducing the Hinge loss:
$$ max(0, 1-y_i(\vec{\omega}\cdot \vec{x_i} - b)) $$
the optimization problem is to minimize:
$$ \frac{1}{2}|| \omega ||^2 + \lambda \sum_{i=1}^m max(0, 1-y_i(\vec{\omega}\cdot \vec{x_i} - b)) $$
It belongs to the quadratic programming ($QP$) with a convex function $f(x)$. 

A convex function is the line segment between any two points on the graph lies above or on it, e.g., $f(x)=x^2$. For a twice differentiable function of a single variable, if the second derivative is always greater than or equal to zero for its entire domain then the function is convex. The locally optimal point of a convex problem is globally optimal.

### Lagrangian Duality

The standard form convex optimization problem can be expressed as: minimize $f(x)=\frac{1}{2}|| \omega ||^2$, subject to $g_i(x) \leq 0, i=1,...,m$ and $h_i(\omega) = 0, i=1,...,m$. The generalized Lagrange function is introduced as:
$$ \mathscr{L}(\omega, \alpha, \beta) = f(\omega) + \sum_{i=1}^k \alpha_i g_i(\omega) + \sum_{i=1}^l \beta_i h_i(\omega) $$
where $\alpha_i$ and $\beta_i$ are the Lagrangian multipliers. $\alpha_i>0$ must be satisfied.

There is a lemma: if $\omega$ satisfies the primal constraint $s$,
$$ max_{\alpha,\beta; \alpha_i \geq 0} \mathscr{L}(\omega, \alpha, \beta) = f(\omega) $$

Otherwise, the maximum of $\mathscr{L}$ can be $+\infty$, which is related to the values of $\alpha_i,\beta_i$.

The primal optimization problem $min_\omega f(x)$ is equivalent to:
$$ p* = min_\omega \ max_{\alpha,\beta; \alpha_i \geq 0} \mathscr{L}(\omega, \alpha, \beta) $$

The dual problem is:
$$ d^* = max_{\alpha,\beta; \alpha_i \geq 0} \ min_\omega \mathscr{L}(\omega, \alpha, \beta) $$

If both of the optimization solution of the primal problem $d^*$ and that of the dual problem $p^*$ exsit, we have the theorem of weak duality:
$$ d^* \leq p^* $$

Proof:
$$ min_\omega \mathscr{L}(\omega, \alpha, \beta) \leq \mathscr{L}(\omega, \alpha, \beta) \leq max_{\alpha,\beta; \alpha_i \geq 0} \mathscr{L}(\omega, \alpha, \beta) $$
$$ max_{\alpha,\beta; \alpha_i \geq 0} \ min_\omega \mathscr{L}(\omega, \alpha, \beta) \leq min_\omega \ max_{\alpha,\beta; \alpha_i \geq 0} \mathscr{L}(\omega, \alpha, \beta) $$

If we solve the primal optimization problem through the dual optimization problem, we need make sure that $$ d^* = p^* $$, which is called the strong duality. If there exist a saddle point of $\mathscr{L^*}(\omega^*, \alpha^*, \beta^*)$, we have $$ d^* = p^* $$. The saddle point satisfies the Karush-Kuhn-Tucker (KKT) conditions:











 
