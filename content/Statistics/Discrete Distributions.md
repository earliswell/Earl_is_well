## [[Binomial]]
- Probability Function$$p(y) = \begin{pmatrix} n \\ y\end{pmatrix} p^y(1-p)^{n-y}; y = 0, 1, \dots, n$$
- Mean : $np$
- Variance : $np(1-p)$

## [[Geometric]]
- Probability Function$$p(y) = p(1-p)^{y-1}; \ y = 1, 2, \dots$$
- Mean : $\frac{1}{p}$
- Variance : $\frac{1-p}{p^2}$

## [[Hypergeometric]]
- Probability Function$$p(y) = \frac{\begin{pmatrix} r \\ y \end{pmatrix} \begin{pmatrix} N-r \\ n-y\end{pmatrix}}{\begin{pmatrix} N \\ n\end{pmatrix}}; \ y = 0, 1, \dots, n \ \text{if}  \ n\leq r, \ y = 0, 1, \dots, r \ \text{if}  \ n > r$$
- Mean  $$\frac{nr}{N}$$
- Variance $$n\left(\frac{r}{N}\right)\left(\frac{N-r}{N}\right)\left(\frac{N-n}{N-1}\right)$$

## [[Poisson]]
- Probability Function$$p(y) = \frac{\lambda^ye^{-\lambda}}{y!}; \ y=0,1,\dots$$
- Mean : $\lambda$
- Variance : $\lambda$

## [[Negative Binomial]]
- Probability Function$$p(y) = \begin{pmatrix} y-1 \\ r-1 \end{pmatrix}p^r(1-p)^{y-r}; \ y=r, r+1, \dots$$
- Mean $$\frac{r}{p}$$
- Variance $$\frac{r(1-p)}{p^2}$$
