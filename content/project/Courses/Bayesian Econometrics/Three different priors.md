---
date: "2020-02-06"
diagram: true
image: 
  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  placement: 3
math: true
title: Comparing three distinct priors for the same model 
---

Suppose $X_{1},...,X_{n}$ is a sequence of random variables independent and identically distributed, where $X_{1} \thicksim \mbox{Bernoulli}(\theta)$. Then, the probability function $(f.p)$ associated is:

$$p(x|\theta) = \theta^{x}(1-\theta)^{x-1}$$  

Since the random variables are independents, the likelihood function can be written as:

$$ p(x_{1},...,x_{n}|\theta) = \Pi_{i=1}^{n}P(x_{i}) = \Pi_{i=1}^{n}\theta^{x_{i}}(1-\theta)^{1-x_{i}} = \theta^{\sum_{i=1}^{n}x_{i}}(1-\theta)^{n-\sum_{i=1}^{n}}$$


Now, suppose we have three difrente


$$\theta \thicksim U_{[0,1]} \quad \mbox{where} \quad P(\theta) = 1 \ , \ \theta \in (0,1)$$

$$\theta \thicksim Beta(a,b) \quad \mbox{where}  \quad P(\theta) = \frac{\Gamma(a,b)}{\Gamma(a)\Gamma(b)}\theta^{a-1}(1-\theta)^{b-1} \ , \ \theta \in (0,1)$$

$$\log{\frac{\theta}{1-\theta}} \thicksim \mathrm{N}(\mu,\sigma^2) \quad \mbox{where}  \quad P(\theta) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\Big(\log{(\frac{\theta}{1-\theta}}-\mu)^2/2\sigma^2\Big)}\frac{1}{\theta(1-\theta)} \ , \ \theta \in (0,1) $$


By Bayes rule, we have:

$$  $$ 


This example was extracted from 

Reference: http://hedibert.org/wp-content/uploads/2020/01/bernoulli-models-R.txt