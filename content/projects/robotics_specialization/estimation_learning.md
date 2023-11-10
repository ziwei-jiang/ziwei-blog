---
title: "Robotics Specialization: Estimation and Learning"
date: 2018-07-13T11:30:03+00:00
weight: 5
# aliases: ["/first"]
tags: ["Robotics", "Machine Learning"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: true
hidemeta: false
comments: true
description: "Learning and estimation for robots "
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
math: true
cover:
    # image: "<image path/url>" # image path/url
    image: "/projects/robotics_specialization/imgs/course5/estimation_learning.png"
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: true # when using page bundles set this to true
    hidden: false # only hide on current single page
    hiddenInList: false
    hiddenInSingle: false

---

Here's the content from the *Robotics Specialization: Estimation and Learning* offered by UPenn on Coursera. 

This is the fifth course in the robotics specialization. This course is about topics related to machine learning and estimation in robotics.


# Gaussian Model Learning

##  1D Gaussian Distribution

The Gaussian distribution is a widely used probability distribution defined by two parameters, the mean (μ) and standard deviation (σ), which represent its central value and spread. It is characterized by a bell-shaped probability density function (PDF) and has some nice mathematical properties such as the product of two Gaussian distributions is also Gaussian. Additionally, the **Central Limit Theorem** states that the sum or average of a large number of independent and identically distributed random variables converges to a Gaussian distribution, making it suitable for modeling noise in measurement or uncertainty.

$$ P(x) = \frac{1}{\sqrt{2\pi}\sigma}\exp{\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)} $$
{{< figure align=center src="/projects/robotics_specialization/imgs/course5/1d_gaussian.png" width="90%" title="1D Gaussian Distribution">}}

## Multivariate Gaussian Distribution

The Multivariate Gaussian is an extension of the 1D Gaussian distribution, allowing us to model complex multidimensional data by specifying a vector of means (μ) and a covariance matrix (Σ) that describes both the central tendency and interrelationships between variables. Just as the one-dimensional Gaussian distribution is characterized by its mean and standard deviation, the multivariate version is defined by these parameters and exhibits a bell-shaped, elliptical probability density function. 

$$P(\mathbf{x}) = \frac{1}{(2\pi)^\frac{D}{2}|\Sigma|^\frac{1}{2}}\exp{\left(-\frac{1}{2}\mathbf{(x-\mu)^T\Sigma^{-1}(x-\mu)}\right)}$$

The covariance matrix is a symmetric matrix that describes how pairs of random variables in the distribution are related to each other. It describes both the variances (spread) of individual variables along the diagonal and the covariances (relationships) between pairs of variables off the diagonal. A covariance of zero implies that the variables are uncorrelated.
$$ \Sigma = \begin{bmatrix} \sigma_{x_1}^2 && \sigma_{x_1}\sigma_{x_2} \\\ \sigma_{x_2}\sigma_{x_1} && \sigma_{x_2}^2 \end{bmatrix}$$

{{< figure align=center src="/projects/robotics_specialization/imgs/course5/2d_gaussian.png" width="90%" title="2D Gaussian Distribution">}}


## Maximum Likelihood Estimation (MLE)

To determine the most probable parameters for the Gaussian distribution, **Maximum Likelihood Estimation (MLE)** is often used. For the 1D Gaussian, we want to estimate the mean (μ) and standard deviation (σ). More specifically, we want to find the parameters that maximize the conditional probability over the observational data.
$$ \hat{\mu}, \hat{\sigma} = \argmin_{\mu,\sigma} P(\\{x_i\\}|\mu, \sigma)$$

If we assume each $x_i$ is independently sampled, it becomes:

$$ \hat{\mu}, \hat{\sigma} = \argmin_{\mu,\sigma} \prod_{i=1}^n P(x_i|\mu, \sigma)$$

Next, we can take the natural log of the objective function, since the natural log is a monotonically non-decreasing function.

$$ \hat{\mu}, \hat{\sigma} = \argmin_{\mu,\sigma} \left(\ln \prod_{i=1}^n P(x_i|\mu, \sigma)\right) =  \argmin_{\mu,\sigma} \sum_{i=1}^n \left(\ln  P(x_i|\mu, \sigma)\right)$$

Then we get the objective function in terms of the following summation.

$$ \hat{\mu}, \hat{\sigma} =  \argmin_{\mu,\sigma} \sum_{i=1}^n \left( \frac{(x_i-\mu)^2}{2\sigma^2} + \ln\sigma + \text{const} \right)  $$

Take the derivative of the objective function with respect to $\mu$ and $\sigma$, and set them to zero, we get

$$ \hat{\mu} = \frac{1}{N}\sum_{i=1}^N x_i ,\qquad \hat{\sigma}^2 = \frac{1}{N}\sum_{i=1}^N (x_i-\hat{\mu})^2. $$

With similar steps, we can get the MLE solution of multivariate Gaussian distribution.

$$ \hat{\mathbf{\mu}} = \frac{1}{N}\sum_{i=1}^N \mathbf{x_i}, \qquad \hat{\mathbf{\sigma}}^2 = \frac{1}{N}\sum_{i=1}^N (\mathbf{x_i}-\hat{\mathbf{\mu}})(\mathbf{x_i}-\hat{\mathbf{\mu}})^T $$

## Mixture of Gaussians

The **Gaussian Mixture Model (GMM)** extends the Gaussian (normal) distribution to represent complex data distributions by combining multiple Gaussian components. Each component is defined by its own set of parameters, including mean and covariance, allowing GMMs to capture non-symmetric patterns and multimodal data structures. 

$$ P(\mathbf{x}) = \sum_{k=1}^K w_k g_k (\mathbf{x}|\mathbf{\mu_k}, \mathbf{\Sigma_k}) $$

The $g_k$ is the Gaussian component with $\mathbf{\mu_k}$ and $ \mathbf{\Sigma_k}$. The $w_k$ is the mixing coefficient with $w_k>0$ and $\sum_{k=1}^K w_k = 1$. 

{{< figure align=center src="/projects/robotics_specialization/imgs/course5/gmm.png" width="60%" title="The black curve illustrates an example of Gaussian Mixture Model.">}}

One primary weakness is that GMMs can become computationally demanding when dealing with a large number of components or clusters, as each component introduces additional parameters to estimate, potentially leading to high computational complexity. 

## Expectation-Maximization (EM) Algorithm

The Expectation-Maximization (EM) algorithm is a powerful computational technique used to estimate the parameters of a Gaussian Mixture Model (GMM). The EM algorithm iteratively updates estimation for the means, covariances, and mixing coefficients of the Gaussian components. In the Expectation step (E-step), the algorithm computes the expected values of the missing data, given the current parameter estimates. The Maximization step (M-step), then optimizes these parameter estimates based on the expected data. EM iterates between these two steps until convergence. The flowchart below summarizes the updates for the E-step and M-step for estimating GMM parameters.

| ![Picture 1](/projects/robotics_specialization/imgs/course5/e-step.png) | ![Picture 2](/projects/robotics_specialization/imgs/course5/m-step.png) |
|:---:|:---:|

## Color Learning and Target Detection
In this assignment, we learn to use GMM for target detection using GMM. 

The process of colored ball detection involves two steps. Firstly, we collect color samples of the yellow ball from training images and label them. 
| ![Picture 1](/projects/robotics_specialization/imgs/course5/training1.png) | ![Picture 2](/projects/robotics_specialization/imgs/course5/training2.png) |
|:---:|:---:|

Subsequently, we use this training data to estimate the model parameters, specifically the mean and covariance. Then, during the detection stage, given the model parameters and a test image, we calculate the likelihood of each pixel. Applying an appropriate threshold allows us to obtain a segmentation map of the yellow balls.
| ![Picture 1](/projects/robotics_specialization/imgs/course5/testing1.png) | ![Picture 2](/projects/robotics_specialization/imgs/course5/testing1_mask.png) |
|:---:|:---:|

| ![Picture 1](/projects/robotics_specialization/imgs/course5/testing2.png) | ![Picture 2](/projects/robotics_specialization/imgs/course5/testing2_mask.png) |
|:---:|:---:|


# Bayesian Estimation

## Kalman Filter Model

## MAP Estimate of KF

## Kalman Filter Tracking


# Mapping

## Occupancy Grid Mapping

## 3D Mapping


# Localization




## Belief state in 2D

## Map Registration

## Particle Filters

## Iterative Closest Point


(Images and codes are from [Robotics: Estimation and Learning](https://www.coursera.org/learn/robotics-learning/).)