---
date: "2020-02-06"
diagram: true
image: 
  caption: 'Image credit: [**John Moeses Bauan**](https://unsplash.com/photos/OGZtQF8iC0g)'
  placement: 3
math: true
title: The normal-normal distribution 
---

Suppose for a givena simple linear regression such as,  

$$ y_{i} = \alpha + \beta x_{i} + \varepsilon_{i} \quad , \ \varepsilon_{i} \$$

we simple denote $\textbf{y}$ as a $n\times 1$ vector of

$$ p(\textbf{y} |\alpha,\beta,\textbf{x}) = \frac{1}{\sqrt{2\pi}}\exp((-\textbf{y} - 1_{n}\alpha -\beta \textbf{x})^{T}(\textbf{y} - 1_{n}\alpha -\beta \textbf{x})/2)$$ 

Now, suppose that we have some knowledge about the distribution of the parameters $\alpha$ and $\beta$

$$\begin{eqnarray} P(\alpha,\beta) = P(\alpha)P(\beta) \\ 
\alpha \thicksim \mathrm{N}(0,\tau_{\alpha}^{2}) end{eqnarray}$$ 
