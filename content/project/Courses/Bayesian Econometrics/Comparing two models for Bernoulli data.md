---
date: "2020-02-06"
diagram: true
image: 
  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  placement: 3
math: true
title: Comparing two models using Bernoulli data 
---

We are interest in comparing two differt models. For the sake of our example, let us generate a random sample $x_{i} \thicksim \mbox{Bernoulli}(\theta)$


```R
set.seed(01123581321)
n <- 50              # the sample length
x <- rep(0,n)        # vector of zeros
alpha_t <- 0.0         # the "true" alpha we imagined
beta_t  <- 1.0          # the "true" beta we imagined
for (t in 2:n){                 
  theta_t <- 1/(1+exp(-(alpha_t + beta_t*x[t-1]))) # the transformed version
  x[t] <- rbinom(1,1,theta_t) }

```

**Model 1**

Suppose $X_{1},...,X_{n}$ is a sequence of random variables independent and identically distributed, where $X_{1} \thicksim \mbox{Bernoulli}(\theta)$. Then, the probability function $(f.p)$ associated is:

$$ p(x|\theta) = \theta^{x}(1-\theta)^{x-1}$$  

Since the random variables are independents, the likelihood function can be written as:

$$ p(x_{1},...,x_{n}|\theta) = \Pi_{i=1}^{n}P(x_{i}) = \Pi_{i=1}^{n}\theta^{x_{i}}(1-\theta)^{1-x_{i}} = \theta^{\sum_{i=1}^{n}x_{i}}(1-\theta)^{n-\sum_{i=1}^{n}}$$


```R
theta_M1 = seq(0,1,length=n) # the parameter theta
likelihood_M1 = rep(0,n)
for (i in 1:n){
likelihood_M1[i] = prod(dbinom(x,1,theta_M1[i])) }
likelihood_M1 = likelihood_M1/max(likelihood_M1)

par(mfrow=c(1,1))
plot(theta_M1,likelihood_M1,xlab=expression(theta),
     ylab="Likelihood",type="l")
title("Likelihood for model 1")
```


**Model 2**

Now, consider the logit model given by:

$$g(\theta_{t}) = \alpha + \beta x_{t-1}$$

where $g(\theta_{t}) = log\Big(\frac{\theta_{t}}{1-\theta_{t}}\Big)$

Clearly, we don't know the distribution of $g$. Since the function is monotonic on $\theta > 1$, we can apply the transform:

$$\begin{eqnarray} \frac{\theta_{t}}{1-\theta_{t}} = e^{\alpha + \beta x_{t-1}} \\\\\\ \frac{\theta_{t}}{\frac{1}{\theta_{t}}-1} = e^{\alpha + \beta x_{t-1}} \\\\\\
\frac{1}{\theta_{t}} = 1 + e^{\alpha + \beta x_{t-1}} \\\\\\ 
\theta_{t} = \frac{1}{1+e^{-(\alpha + \beta x_{t-1})}} \end{eqnarray}$$


The likelihood is given by

$$p(x_{1},...,x_{n}|\theta) =  \theta^{\sum_{i=1}^{n}x_{i}}(1-\theta)^{n-\sum_{i=1}^{n}} \\
= $$
First, let us define $\alpha = \begin{cases}-2,...,1\end{cases}$ and  $\beta = \begin{cases}0,...,3\end{cases}$ an $M \times 1$ vector, 

```R
alpha = seq(-2,1,length=n)          # Vector 
beta = seq(0,3,length=n)            #
likelihood_M2 = matrix(0,n,n)       #
densid = rep(0,n-1)                 
for (i in 1:n){                         
  for (j in 1:n){                   
    for (t in 2:n){                 
      theta_M2 = 1/(1+exp(-(alpha[i]+beta[j]*x[t-1])))
      densid[t-1] = dbinom(x[t],1,theta_M2)
    }
    likelihood_M2[i,j] = prod(densid)
  }
}
likelihood_M2 = likelihood_M2/max(likelihood_M2) # normalizing the likelihood

par(mfrow=c(1,2))
plot(theta_M1,likelihood_M1,xlab=expression(theta),
     ylab="Likelihood",type="l")
title("Likelihood for model 1")

contour(alpha,beta,likelihood_M2,xlab=expression(alpha),
        ylab=expression(beta))
points(alpha_t,beta_t,pch=16,col=2)
title("Likelihood for model 2")
```

Sometimes, inference based on likelihood makes our lives much more difficult, once we are interested on numeric integration. One explicit way to comput is the aproximation using Monte Carlo Integration.

More interesting, we are going to use the method known as **Sampling Importance Resampling**, which is much more powerfull in terms of convergence.

Again


```R
N = 50000                       # size of draws
theta.draw = runif(N)           # 
w_M1 = rep(0,N)                    #
for (i in 1:N){
  w[i] = prod(dbinom(x,1,theta.draw[i]))} 
ind = sample(1:N,size=N,replace=TRUE,prob=w)
theta.draw = theta.draw[ind]

par(mfrow=c(1,2))
plot(theta,like.M1,xlab=expression(theta),
     ylab="Likelihood",type="l")
title("Likelihood for model 1")
hist(theta.draw,prob=T,xlim=c(0,1),main="Posterior for model 1",xlab=expression(theta))

alpha.draw = runif(N,-4,3)
beta.draw  = runif(N,-2,5)
w_M2 = rep(0,N)
like = rep(0,n-1)
for (i in 1:N){
  for (t in 2:n){
    eta = alpha.draw[i]+beta.draw[i]*x[t-1]
    thetat = 1/(1+exp(-eta))
    like[t-1] = dbinom(x[t],1,thetat)
  }
  w[i] = prod(like)
}
ind = sample(1:N,size=N,replace=TRUE,prob=w)
alpha.draw = alpha.draw[ind]
beta.draw = beta.draw[ind]
eta0 = 1/(1+exp(-(alpha.draw)))
eta1 = 1/(1+exp(-(alpha.draw+beta.draw)))

par(mfrow=c(1,1))
plot(alpha.draw,beta.draw,xlab=expression(alpha),
     ylab=expression(beta))
contour(alphas,betas,like.M2,col=2,add=TRUE,lwd=3,drawlabels=F)
title("Likelihood & posterior for model 2")
```

This example was extracted from 

Reference: http://hedibert.org/wp-content/uploads/2020/01/bernoulli-models-R.txt