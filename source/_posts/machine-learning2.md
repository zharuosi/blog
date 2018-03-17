---
title: 机器学习笔记 (2)
date: 2018-03-06 20:23:44
categories: 笔记篇之夙兴夜寐
tags: [Machine Learning,Artificial Intelligence,Linear Regression]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

## The Learning Problem

  - Input variables / features : $x^{(i)}$
  - Target variable : $y^{(i)}$
  - A training example: ($x^{(i)}$,$y^{(i)}$)
  - A training set: {($x^{(i)}$,$y^{(i)}$); $i=1,2,...,m$}
  - The space of input values: $X$
  - The space of output values: $Y$

The goal is addressed as: <!-- more -->

Given a training set, learn a function $h: X->Y$ so that $h(x)$ is a good predictor for the corresponding value of $y$. If there are $n$ features, like $x=[x_1,x_2,...,x_n]^T$, the training set will be $X_{m\times n}$ and $Y_{m\times 1}$. **Note that $m$ is the number of samples and $n$ is the number of features of each sample.**

## Linear Regression

Linear regression is a very simple approach for supervised learning. We use linear regression to predict a quantitative response $Y$ from the predictor variable $X$. The relationship between $X$ and $Y$ is assumed to be linear. A linear function $h(x)$ can be used to map from $X$ to $Y$:
$$ h(x) = \sum_{i=0}^n \theta_i x_i = \theta ^T \phi(x)$$

The intercept term is: $x_0 = 1$. Note that here $i$ is not the number of samples.

- multiple linear regression

Multiple linear regression is a generalization of linear regression by considering more than one independent variable ($X$: $n>1$).

- multivariate linear regression (General linear model)

The multivariate linear regression is a generalization of multiple linear regression model to the case of more than one dependent variable ($Y$ will be a matrix).



### Linear basis function models

In polynomial curve fitting, the feature extraction is represented as the polynomial basis functions: $\phi_j(x) = x^j$, $j$ is the polynomial order ($1\leq j\leq M$). A small change in $x$ affects all basis functions. Thus using a polynomial basis function is global.

Consider the sigmoidal basis functions: $\phi_j(x) = \sigma (\frac{x - \mu_j}{s})$, where $\sigma(x) = \frac{e^x}{1+e^x}$. A small change in $x$ only affects nearby basis functions (near $\mu_j$).

### The Least Mean Square (LMS) method

The cost function is defined as:
$$ J(\theta) = \frac{1}{2} \sum_{i=1}^m (h_{\theta}(x^{(i)}) - y^{(i)})^2 $$

The problem is how to get a minimum $J(\theta)$.

#### Gradient descent

Apply a gradient descent algorithm for every $j$:
$$ \theta_j:= \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta) $$
where $j$ is the number of basis function ($1\leq j\leq M$).

For a single sample, the LMS update rule (or Widro-Hoff learning rule) can be obtained as:
$$ \theta_j:= \theta_j - \alpha (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)} $$

- Batch gradient descent

For many samples, we use the batch gradient descent as:
$$\theta_j :=\theta_j - \alpha \sum_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)}$$
in the iteration:

    Repeat until convergence {
      ... (for every j)
    }


The iteration can be written as:
$$\theta_j^{k+1}=\theta_j^k - \alpha \sum_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)}$$
where $i$ is the number of samples ($1\leq i\leq m$), $k$ is the iteration step. Note that $x_j^{(i)}$ is the feature extraction, $x^{(i)}$ is the sample.

until $|\theta_j^{k+1} - \theta_j^{k}|<\epsilon$, then $\theta_j$ reaches convergence. For every iteration, all samples will be traversed. If a new sample is added, the iteration has to start from the first sample again.

- Stochastic gradient descent

When the training set is large, instead, we use Stochastic gradient descent as:
$$ \theta_j^{k+1} = \theta_j^k - \alpha (h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)} $$
in

    Loop {
      for i = 1,m {
      ... (for every j)
      }
    }

until $|\theta_j^{k+1} - \theta_j^{k}|<\epsilon$, then $\theta_j$ reaches convergence. For each $\theta$, only one sample is used for iteration until the convergence. If a new sample is added, the iteration will continue only using the new sample. Finally, the new $\theta$ and $h_{\theta}$ are obtained.

[Feature scaling](https://en.wikipedia.org/wiki/Feature_scaling) is a method used to standardize the range of independent variables or features of data (data normalization). Feature scaling can be used to improve the iteration rate in gradient descent, e.g., *rescaling* and *standardiztion*. Feature *standardization* makes the values of each feature in the data have zero-mean (when subtracting the mean in the numerator) and unit-variance. Thus each feature contributes approximately proportionately to the final values.

#### The normal equations

To avoid interations, we can use the normal equations. Consider $X_{m\times n}$ and $Y_{m\times 1}$ in matrix, the derivative of $J(\theta)$ equals zero:
$$ \nabla_\theta J(\theta) = X^T X\theta - X^T \vec{y} = 0 $$

Finally,
$$\theta = (X^T X)^{-1}X^T\vec{y}$$

In most situations of practical interest, the number of data points $m$ is larger than the dimensionality $n$ of the input space, and the matrix $X$ is of full column rank. A matrix is full column rank when each of the columns of the matrix are linearly independent. Then $X^TX$ is necessarily invertible and therefore positive definite. The minimum $J(\theta)$ can be obtained at the critical point when $\nabla_\theta J(\theta)=0$.

If $m \leq n$ or $X$ is not of full column rank, $X^TX$ is not invertible. [Regularization](https://en.wikipedia.org/wiki/Tikhonov_regularization) is a process of introducing additional information in order to solve an ill-posed problem or to prevent overfitting. E.g. In Quadratic ridge regression, the cost function is rewritten as:
$$ J(\theta) = \frac{1}{2} \sum_{i=1}^m (h_{\theta}(x^{(i)}) - y^{(i)})^2 + \frac{\lambda}{2} \sum_{j=1}^n |\theta_j|^2$$
According to $\nabla_\theta J(\theta)=0$, $\hat{\theta}$ can be calculated as:
$$\hat{\theta} = (X^TX+\lambda I)^{-1}X^T\vec{y}$$

**Direct methods**: Solve the normal equations by Gaussian elimination or QR decomposition
- Benefit: in a single step or very few steps
- Shortcoming: not feasible when data are streaming in real time or of very large amount

**Iterative methods**: use the stochastic or gradient descent
- Benefit: converging fast, more attactive in large practical problems
- Shortcoming: the learning rate $\alpha$ should be carefully chosen.

For probalilistic interpretation of Least Mean Square, by the independence assumption, LMS is equivalent to maximum likehood estimation [(MLE)](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation) of $\theta$.

### Locally weighted linear regression (LWR)

The problem is how to get a minimum $J(\theta)$ given as:
$$ J(\theta) = \sum_{i=1}^m w^{(i)}(h_{\theta}(x^{(i)}) - y^{(i)})^2 $$
where
$$ w^{(i)} = exp(-frac{(x^{(i)} - x)^2}{2\tau^2})$$
where $x$ is the query point for which we'd like to know its $y$. Essentially higher weights will be put on the training examples close to $x$.

### Parametric and Nonparametric

A learning model that summarizes data with a set of parameters of fixed size (independent of the number of training examples) is called a **parametric model**. No matter how much data you throw at a parametric model, it won’t change its mind about how many parameters it needs. **Nonparametric methods** are good when you have a lot of data and no prior knowledge, and when you don’t want to worry too much about choosing just the right features. (Artificial Intelligence: A Modern Approach)

[Benefits](https://machinelearningmastery.com/parametric-and-nonparametric-machine-learning-algorithms/) of parametric models:
- Simpler: These methods are easier to understand and interpret results.
- Speed: Parametric models are very fast to learn from data.
- Less Data: They do not require as much training data and can work well even if the fit to the data is not perfect.

Limitations of parametric models:
- Constrained: By choosing a functional form these methods are highly constrained to the specified form, because a parametric model has a fixed and finite number of parameters ($\theta$). 
- Limited Complexity: The methods are more suited to simpler problems.
- Poor Fit: In practice the methods are unlikely to match the underlying mapping function.

Benefits of nonparametric models:
- Flexibility: Capable of fitting a large number of functional forms.
- Power: No assumptions (or weak assumptions) about the underlying function.
- Performance: Can result in higher performance models for prediction.

Limitations of non-parametric models:
- More data: Require a lot more training data to estimate the mapping function.
- Slower: A lot slower to train as they often have far more parameters to train.
- Overfitting: More of a risk to overfit the training data and it is harder to explain why specific predictions are made.

Locally weighted linear regression is a non-parametric learning algorithm. The term non-parametric means the model structure is not specified a priori but is instead determined from data. It is not meant to imply that such models completely lack parameters but that the number and nature of the parameters are flexible and not fixed in advance. If we use unweighted linear regression, Once $\theta$ is determined, we no longer need to keep the training data around to make future predictions. In contrast, if the weighted linear regression is applied, the entire training set should be kept around.


