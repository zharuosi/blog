---
title: 机器学习笔记（3）常见分类
date: 2018-03-15 23:00:30
categories: 笔记
tags: [Machine Learning,Artificial Intelligence,Classification,Logistic Regression,LDA]
---

## Introduction

Here is a not so short note (Rolling Update) for machine learning and artificial intelligience. Main concepts and some references are presented. This note is based on Wikipedia, Andrew Ng's Lecture Notes [CS229 of Standford](http://cs229.stanford.edu/), Machine Learning course of SJTU [IEEE-yangyang](http://bcmi.sjtu.edu.cn/%7Eyangyang/ml/#), and [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) of Google.

## Supervised Learning

In supervised learning, each example is a pair consisting of an input object (typically a vector) and a desired output value (also called the supervisory signal), like SVM, regression, decision trees, naive Bayes, etc. Classification means the output takes discrete values. Three common classification methods are Logistic Regression, Fisher Linear Discriminant Analysis and Nearest Neighbor Classification.
<!-- more -->

## Logistic Regression

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

## Linear discriminant analysis (LDA)

Linear discriminant analysis (LDA), which is also known as Fisher's linear discriminant, is a simple way for dimensionality reduction in linear classification. By LDA, we can find a linear combination of features that classifies objects in two or more groups.
- Compared with logistic regression, LDA assumes that the independent variables are normally distributed (Gaussian distribution). Logistic regression is more preferable in applications when the assumption is not valid.
- Compared with analysis of variance (ANOVA), ANOVA uses categorical independent variables and a continuous dependent variable while LDA adopts continuous independent variables and a categorical dependent variable.
- Compared with principle component analysis (PCA), PCA is an unsupervised learning method, which  does not take into account any difference in class, only for dimensionality reduction. LDA attemps to model the difference between the classes of data using labeled data.
- Compared with factor analysis, factor analysis builds the feature combinations based on differences. LDA deals with similarities. LDA is a dependence technique.

A dependence method is one in which a variable of set of variables is identifies as the dependent variable to be predicted or explained by other, independent variables. Dependence techniques include multiple regression analysis, discriminant analysis, and conjoint analysis. An interdependence method is one in which no single variable or group of variables is defined as being independent or dependent. The goal of interdependence methods is data reduction, or grouping things together. Cluster analysis, factor analysis, and multidimensional scaling are the most commonly used interdependence methods.

Consider the $n$ dimensional input vector $x$ and project it down to one dimension:
$$ y = w^Tx $$
where $w$ is the weight vector for linear transformation. Thus we can place a threshold on $y$ and then classify $y$ as $C_1/C_2$.

The dimensionality reduction leads to a considerable loss of information, and classes may become strongly overlapping in the low dimensional space. The goal is to find a projection that maximizes the class separation by adjusting $w$. Therefore, we need to maxmimize the between-class scatter (use difference of mean values) and minimize the within-class scatter (use covariance matrix).

<img src="https://helloacm.com/wp-content/uploads/2016/03/linear-discriminant-analysis.png" width="50%" height="50%">
Fig. 1. Comparison of the good projection and the bad projection in LDA

### Algorithm of LDA

#### Two-class cases

The purpose of LDA considers maximizing the "Rayleigh quotient":
$$ J(w) = \frac{w^T S_B w}{w^T S_W w} $$
where $S_B$ is the between class scatter matrix and $S_W$ is the within class scatter matrix. 

the means of every class is:
$$ \mu_1 = \frac{1}{N_1} \sum_{x\in c_1} \mathbf{x} $$
$$ \mu_2 = \frac{1}{N_2} \sum_{x\in c_2} \mathbf{x} $$
$$ \mu = \frac{1}{N_1+N_2} \sum_{x\in all} \mathbf{x} $$
where $x_k (k=1,..,m)$ is a vector in $X_{m\times n}$.

The scatter matrix for every class is defined as $S_1$ and $S_2$:
$$ S_1 = \sum_{x\in c_1} (x - \mu_1) (x - \mu_i)^T $$
$$ S_2 = \sum_{x\in c_2} (x - \mu_2) (x - \mu_2)^T $$
$$ S_W = S_1 + S_2 $$
$$ S_B = N_1(\mu_1 - \mu) (\mu_1 - \mu)^T + N_2(\mu_2 - \mu) (\mu_2 - \mu)^T = (\mu_2 - \mu_1) (\mu_2 - \mu_1)^T $$

The solution to maximize the $J(w)$ can be obtained by derivation and Lagrange multiplier. Let $||w^T S_w w||=1$ to find one solution for $w$.
$$ \nabla_w c(w) = \nabla_w (w^T S_B w - \lambda (w^T S_w w - 1)) = 0 $$
The Fisher linear discrimination can be obtained as:
$$ S_w^{-1} S_B w = \lambda w $$
Thus $w$ is the eigenvector of the matrix $S_w^{-1} S_B$. Solution is based on solving a generalized eigenvalue problem. In two-class case, $w$ is calculated as:
$$ w = S_w^{-1} (\mu_2 - \mu_1) $$

#### Multi-class cases

The purpose is to maximize the "Rayleigh quotient":
$$ J(w) = \frac{\mid A^T S_B A \mid}{\mid A^T S_W A \mid} $$
where $A$ is the projection matrix.

$$ \mu_j = \frac{1}{N_j} \sum_{x\in c_j} \mathbf{x} $$
$$ \mu = \frac{1}{\sum_{j=1}^C N_j} \sum_{x\in all} \mathbf{x} $$
where $c_j$ means the group of class $j$. $C$ is the number of classes. $N_j$ is the  sample number in the class $c_j$.

The scatter matrix is generalized as:
$$ S_j = \sum_{x\in c_j} (x - \mu_j) (x - \mu_j)^T $$

$$ S_W = \sum_{j=1}^C S_j = \sum_{j=1}^C \sum_{x_i\in c=j} (x_i - \mu_{c=j}) (x_i - \mu_{c=j})^T $$
$$ S_B = \sum_{j=1}^C N_j(\mu_j - \mu) (\mu_j - \mu)^T $$
where $N_j$ is used as the weight. Determinants are the product of all eigenvalues of the matrix.

We also have:
$$ S_w^{-1} S_B A_k = \lambda A_k $$

To solve $A$, we need to use the eigenvalues of $S_w^{-1} S_B$ and obtain the matrix $A$ by $K$ eigenvectors. $K$ is the number of base vectors, namely the dimension of projection matrix $A$ as $A=[A_1\mid A_2 \mid ... A_K]$. The maximum of $K$ is $C-1$. The classification will be better for the eigenvectors with respect to the larger eigenvalues.

Multi-class classification can be transferred to binary classification. The techniques can be categorized into One vs Rest and One vs One.

#### One vs Rest

OVR (or one-vs.-all, OvA, one-against-all, OAA) involves training a single classifier per class, with the samples of that class as positive samples and all other samples as negatives. The base classifiers are required to produce a real-valued confidence score for its decision, rather than just a class label, e.g., a new label vector $z$ where $z_i = 1$ if $y_i = k$ and $z_i = 0$ otherwise. We can use $C - 1$ binary classifiers. Each binary classifier solves the problem of separating samples in a particular class from samples not in that class. This popular strategy suffers from several problems. Firstly, the scale of the confidence values may differ between the binary classifiers. Second, even if the class distribution is balanced in the training set, the binary classification learners see unbalanced distributions because typically the set of negatives they see is much larger than the set of positives (?).

- Benefits: Small storing cost and short test time.
- Shortcomings: Long training time and imbalanced Samples.

#### One vs One

OVO involves using $C(C-1)/2$ binary classifiers for every possible pairs of classes and learning to distinguish these two classes. At prediction time, a voting scheme is applied: all $C(C-1)/2$ classifiers are applied to an sample. The class that got the highest number of vote amongst the discriminant functions gets predicted by the combined classifier. Sometimes it suffers from ambiguities in that some regions of its input space may receive the same number of votes.

- Benefits: Short training time.
- Shortcomings: Too many classifiers, large storing cost and long test time.


## Nearest Neighbor Classification

The basic idea of the nearest neighbor classification is: a new sample is classified by calculating the distance to the nearest training case; the sign of that point then determines the classification of the sample.

The k-NN classifier extends this idea by taking the k nearest neighbors and assigning the sign of the majority. It is common to select k small and odd to break ties (typically 1, 3 or 5). Larger k values help reduce the effects of noisy points within the training data set, and the choice of k is often performed through cross-validation. KNN classifier is non-parametric, which means it does not make any assumptions on the underlying data distribution. The model structure is determined from the data. KNN is a good choice for a classification study when there is little or no prior knowledge about the distribution data. There is no explicit training phase for KNN. KNN makes predictions just-in-time by calculating the similarity between an input sample and each training instance. A sample is classified by a majority vote of its neighbors, with the sample being assigned to the class most common among its k nearest neighbors.


 




