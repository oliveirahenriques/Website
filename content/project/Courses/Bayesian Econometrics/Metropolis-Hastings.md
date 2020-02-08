---
date: "2020-02-06"
diagram: true
image: 
  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  placement: 3
math: true
title: Metropolis-Hastings Algorithm 
---

The Metropolis-Hastings Algorithm 

Given $theta^{i}$, we

1. Generate $\theta^{(0)},...,\theta^{(n)} \ \mbox{where}  \ \theta^{(*)} \thicksim  q(\theta|\theta^{i-1})$ 

2. Compute the ratio 
$$\theta^{i+1} = \begin{array}{ccc} 
\theta^{(1)} = \theta^{(*)} & , \mbox{with probability} \ \alpha(x^{i},Y_{i}) \\
\theta^{(1)} = \theta^{(0)} & , \ \mbox{with probability} \ 1-\alpha(x^{i},Y_{i})
\end{array}$$ 

where $ \alpha(x,y) = \min{1,\frac{P(\theta^{(*)})}{q(\theta^{(*)|theta^{}})}}$ 




```{R}
y=geneq(x[t])
if (runif(1)<f(y)*q(y,x[t])/(f(x[t])*q(x[t],y))){
x[t+1]=y}else{x[t+1]=x[t]}

```

Now, suppose we are sampling from a bivariate 2-component mixture of Gaussians




```{R}
d2norm = function(theta){
  0.8*dnorm(theta[1])*dnorm(theta[2])+
    0.2*dnorm(theta[1],1,0.5)*dnorm(theta[2],1,0.5)
}

N = 100
thetas1 = seq(-5,5,length=N)
thetas2 = seq(-5,5,length=N)
den = matrix(0,N,N)
for (i in 1:N)
  for (j in 1:N)
    den[i,j] = d2norm(c(thetas1[i],thetas2[j]))


par(mfrow=c(3,3))
M = 10000
draws = rep(0,M)
for (sd in c(0.05,0.1,0.5)){
  contour(thetas1,thetas2,den)
  theta = c(4,-4)
  for (i in 1:M){
    theta.star = rnorm(2,theta,sd)
    nume=d2norm(theta.star)/prod(dnorm(theta.star,theta,sd))
    deno=d2norm(theta)/prod(dnorm(theta,theta.star,sd))
    alpha = min(1,nume/deno)
    if (runif(1)<alpha){
      theta = theta.star
    }
    draws[i]=theta[1]
    points(theta[1],theta[2],col=3,pch=16)
  }
  contour(thetas1,thetas2,den,add=TRUE,col=2)
  title(paste("sd=",sd,sep=""))
  ts.plot(draws)
  acf(draws)
}
```











Reference: http://hedibert.org/wp-content/uploads/2020/01/bernoulli-models-R.txt