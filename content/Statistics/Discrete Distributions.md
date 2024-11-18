### DEFINITION
- A random Variable $Y$ is said to be discrete if it can assume only a finite or Countably infinite number of distinct values
- 랜덤 변수 Y를 이산(discrete)라고 말하는 것은 유한하거나 고유한 값만 가질 수 있는 수를 지칭한다.
- EX) 주사위를 3번 돌려 3이 나온 횟수, 동전을 돌려 나온 앞면의 횟수
- 우리는 따라서 확률의 집합은 Probability distribution(확률 분포)이다.
- 이에 따라 아래의 분포가 나뉘는데 특징을 알아보도록 하자 !

우선, 우리는 대문자 Y를 Random Variable이라 두고, 소문자 y를 특정한 값으로 사용할 것이다.
이에 따라, $P(Y=y)$를 해석하면, Random 변수 Y가 특정한 수 y일 확률을 의미한다.
우리는 Probability Distribution for discrete variable Y에서 확률을 다음과 같이 표기한다. 
- $p(y) = P(Y=y)$, $p(y) \geq 0$ for all y.

주요 성질
1. $0\leq p(y) \leq 1$ for all y, 확률의 범위는 0과 1사이어야 하고
2. $\sum_y p(y) = 1$ 모든 확률을 더하면 1이라는 값이 나와야 한다.

Expected Value of Y
- $E(Y) = \sum_y yp(y) = \mu$
- $E(c) = c$, c is constant. 
- $E[g(Y)] = \sum_y g(y)p(y)$ : 이는 Y에 대한 g라는 함수를 대입했을 때 구하는 평균이다.
	- Y에 대한 linear combinations이라고 이해해도 될까?
- $E[cg(Y)] = cE[g(Y)] = c\sum_y g(y)p(y)$
- $V(Y) = E[(Y-\mu)^2] = E[Y^2] - (E[Y])^2 = E[Y^2] - \mu^2$ 


## [[Binomial]]
- p : 성공할 확률 (Succes)
- 1 - p : 실패할 확률 (Failure), q로 표기하기도 함.
- Probability Function$$p(y) = \begin{pmatrix} n \\ y\end{pmatrix} p^y(1-p)^{n-y}; y = 0, 1, \dots, n$$
- Mean : $np$
- Variance : $np(1-p)$

## [[Geometric]]
- n번 째 성공학 확률 (즉, 실패를 유지하다 n번째 한 번 성공해야함)
- 즉, n-1번 실패하다 n번 째 성공 !![[Pasted image 20241118103637.png]]
- Probability Function$$p(y) = p(1-p)^{y-1}; \ y = 1, 2, \dots$$
- Mean : $\frac{1}{p}$
- Variance : $\frac{1-p}{p^2}$

## [[Hypergeometric]]
- 
- Probability Function$$p(y) = \frac{\begin{pmatrix} r \\ y \end{pmatrix} \begin{pmatrix} N-r \\ n-y\end{pmatrix}}{\begin{pmatrix} N \\ n\end{pmatrix}}; \ y = 0, 1, \dots, n \ \text{if}  \ n\leq r, \ y = 0, 1, \dots, r \ \text{if}  \ n > r$$
- Mean  $$\frac{nr}{N}$$
- Variance $$n\left(\frac{r}{N}\right)\left(\frac{N-r}{N}\right)\left(\frac{N-n}{N-1}\right)$$

## [[Poisson]]
- Probability Function$$p(y) = \frac{\lambda^ye^{-\lambda}}{y!}; \ y=0,1,\dots$$
- Mean : $\lambda$
- Variance : $\lambda$

## [[Negative Binomial]]
- N번의 시행 중 r번의 성공할 확률 !
- 확률의 식을 보면, r번째 이전의 실패와 성공할 확률의 조합을 결정하면 마지막 r번째의 성공이 결정됨. 
- Probability Function$$p(y) = \begin{pmatrix} y-1 \\ r-1 \end{pmatrix}p^r(1-p)^{y-r}; \ y=r, r+1, \dots$$
- Mean $$\frac{r}{p}$$
- Variance $$\frac{r(1-p)}{p^2}$$
