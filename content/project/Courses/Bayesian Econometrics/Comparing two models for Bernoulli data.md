---
date: "2020-02-06"
diagram: true
image: 
  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  placement: 3
math: true
title: Comparing two models for Bernoulli data 
---

We are interest in comparing two differt models for a given Bernoulli data. For the sake of our example, let us generate a random sample $x_{i}$ according to our model


```R
set.seed(01123581321)
n <- 50              # the sample length
x <- rep(0,n)        # vector of zeros
alpha_t <- 0.0         # the "true" alpha we imagined
beta_t  <- 1.0          # the "true" beta we imagined
for (t in 2:n){                 
  theta_t <- 1/(1+exp(-(alpha_t + beta_t*x[t-1]))) # the transformed version
  x[t] <- rbinom(1,1,theta_t)
  x[t] <- rbinom 
}

```

**Model 1**

Suppose $X_{1},...,X_{n}$ is a sequence of random variables independent and identically distributed, where $X_{1} \thicksim \mbox{Bernoulli}(\theta)$. Then, the probability function $(f.p)$ associated is:

$$ p(x_{i}|\theta) = \theta(1-\theta) \quad , \ i = 0,1,2,...,n$$  

Since the random variables are independents, the likelihood function can be written as:

$$ p(x_{1},...,x_{n}|\theta) = \Pi_{i=1}^{n}P(x_{i}) = \Pi_{i=1}^{n}\theta^{x_{i}}(1-\theta)^{1-x_{i}} = \theta^{\sum_{i=1}^{n}x_{i}}(1-\theta)^{n-\sum_{i=1}^{n}}$$


```R
theta_M1 = seq(0,1,length=n) # the parameter theta
likelihood.M1 = rep(0,M)
for (i in 1:M)
likelihood.M1[i] = prod(dbinom(x,1,theta[i]))
likelihood.M1 = likelihood.M1/max(likelihood.M1)

par(mfrow=c(1,1))
plot(theta,like.M1,xlab=expression(theta),
     ylab="Likelihood",type="l")
title("Likelihood for model 1")
```


**Model 2**

Consider the logit model given by

$$g(\theta_{t}) = log\Big(\frac{\theta_{t}}{1-\theta_{t}}\Big)$$


$$ log\Big(\frac{\theta_{t}}{1-\theta_{t}}\Big) = \alpha + \beta x_{t-1}$$  

Clearly, we don't know the distribution of $g$. Since the function is monotonic and(...), we can apply the transform:

$$\begin{eqnarray} \frac{\theta_{t}}{1-\theta_{t}} = e^{\alpha + \beta x_{t-1}} \\\\\\ \frac{\theta_{t}}{\frac{1}{\theta_{t}}-1} = e^{\alpha + \beta x_{t-1}} \\\\\\
\frac{1}{\theta_{t}} = 1 + e^{\alpha + \beta x_{t-1}} \\\\\\ 
\theta_{t} = \frac{1}{1+e^{-(\alpha + \beta x_{t-1})}} \end{eqnarray}$$


The likelihood is given by

$$p(x_{1},...,x_{n}|\theta) =  \theta^{\sum_{i=1}^{n}x_{i}}(1-\theta)^{n-\sum_{i=1}^{n}} \\
= $$

For the sake of our example, let us simulate a sample $x_{i}$ according to our model

```R
set.seed(01123581321)
n <- 50              # the sample length
x <- rep(0,n)        # vector of zeros
alpha_t <- 0.0         # the "true" alpha we imagined
beta_t  <- 1.0          # the "true" beta we imagined
for (t in 2:n){                 
  theta_t <- 1/(1+exp(-(alpha_t + beta_t*x[t-1]))) # the transformed version
  x[t] <- rbinom(1,1,theta_t)
  x[t] <- rbinom 
}


```


First, let us define $\alpha = \begin{cases}-2,...,1\end{cases}$ and  $\beta = \begin{cases}0,...,3\end{cases}$ an $M \times 1$ vector, 

```R
alpha = seq(-2,1,length=M)    # Vector 
beta = seq(0,3,length=M)      #
like.M2 = matrix(0,M,M)       #
like = rep(0,n-1)
for (i in 1:M){
  for (j in 1:M){
    for (t in 2:n){
      eta = alphas[i]+betas[j]*x[t-1]
      thetat = 1/(1+exp(-eta))
      like[t-1] = dbinom(x[t],1,thetat)
    }
    like.M2[i,j] = prod(like)
  }
}
like.M2 = like.M2/max(like.M2) # normalizing the likelihood

```




Credits: http://hedibert.org/wp-content/uploads/2020/01/bernoulli-models-R.txt