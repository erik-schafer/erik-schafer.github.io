---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "The Bias-Variance Tradeoff, Explained"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-11-03T09:45:19-05:00
lastmod: 2020-11-03T09:45:19-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Introduction

In statistics and machine learning, *Bias* and *Variance* describe two fundamental failure modes models can have when applied to certain sets of data.  Often, when a model fails due to high bias we say it is 'underfitting', and when a model fails due to high variance we say it is 'overfitting'.  This isn't exactly wrong, but it fails to adequately capture the true meaning of the terms.

In order to get an intuitive understanding of bias and variance, it will help for me to define a few terms.

Let's call any model that improves some measure of performance when given data a *Learner*.  In this context, an elementary linear regression model is a learner, as is a decision tree, as is a deep neural network.  

Let's also call the particular configuration of tunable values for any given learner a *Hypothesis*.  Often the goal of a learner is to find a specific hypothesis that minimizes error across a data set.  As an example, $y = 3 + 5x$ is a specifc hypothesis for a linear model of form $y = \beta_0 + \beta_1 x$.  A particular set of weights and biases constitutes a hypothesis for some neural network.

Finally, let's consider the *Hypothesis Space* of a model.  The *Hypothesis Space* of a model is the set of all possible hypotheses that could be returned by it.

In machine learning we take a learner, explore its hypothesis space, and attempt to find the best hypothesis.

## Bias

***A model has high bias when its hypothesis space is inadequate to properly explain the data.***

Sometimes, this is termed 'underfitting' which is misleading, since there are many ways to underfit a model that ought to have low bias.  For example, if the cost function for a given model is noisy, and a bad optimization algorithm is used, it's possible (even likely) for the algorithm to return a false, local, minima.  Alternatively, a poor application of early stopping (a technique to address overfitting in neural networks) can yield an 'underfit' learner.  Underfitting is a much more general problem, and a model with high bias is not the only culprit.

The core idea of bias is that for any model, there are generally some relationships they are incapable of modeling.  Suppose we have a relationship like $y = \beta_0 + \beta_1 x + \beta_2 x^2$ and we apply a linear regression learner to this problem.  Unelss $\beta2=0$, this linear regression is fundamentally inadequate for explaining the data.  One way to think about this is that the linear regression learner approaches the data with a very particular preconceived notion -- that the data will have a linear relationship.

Some models, like linear regression, have high bias, while others, like a higher-order polynomial model or a neural net with many layers and neurons, have much more flexibility (low bias).  Besides, Occam's Razor, which says we should prefer the simpler models over the more complex ones, there is a deeper reason to eschew models with low bias -- models with low bias suffer from another problem: high variance.

* A model that has strong 'preconcieved notions' about the relationships in the data has high bias
* A highly flexible model, like a universal function approximator, has low bias 
* Bias relates to the implicit limitations in the Hypothesis Space of a given model

## Variance

***A model has high variance when small variations in the training data yield large variations in the selected hypothesis***

High variance is a more common problem to encounter when building a learner.  Many popular models, *especially* neural networks, are highly flexible and have high variance.  Even basic models, like a decision tree, can be impressively flexible and have high variance.  A model with high variance is often said to have 'overfit' to the training data.  In this case the model manages to minimize training error while failing to generalize well because it begins to fit the random noise inherent in the data.

Here, the classic example to understand variance is fitting a high order polynomial to a lower-order polynomial relationship.  Let's imagine we have data with *no relationship*: $y = 5$, but there is some random error $\epsilon$ in our measurements