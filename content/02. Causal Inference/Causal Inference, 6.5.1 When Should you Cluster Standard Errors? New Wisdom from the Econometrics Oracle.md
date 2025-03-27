---
title: 6.5.1 When Should you Cluster Standard Errors? New Wisdom from the Econometrics Oracle
draft: false
tags:
  - 가설검증
  - "#Test"
  - "#Robustness"
---

## When should you cluster standard errors.
- This is definitely one of life's most important questions, as any keen player of seminar bingo can surely attest.
- The authors argue that there are two reasons for clustering standard errors: 
	- a _sampling design_ reason, which arises because you have sampled data from a population using clustered sampling, and want to say something about the broader population; 
	- and and _experimental design_ reason, where the assignment mechanism for some causal treatment of interest is clustered. 
##### The Sampling Design reason for clustering
- Consider running a simple Mincer earnings regression of the form:
$$
\ln(\text{Wages})_{i} = \beta_{0} + \beta_{1}\text{Year of schooling}_{i} + \beta_{2}\text{Experience}_{i} + \beta_{3}\text{Experience}^2_{i} + \epsilon_{i}
$$
- Your present this model, and are deciding whether to cluster the standard errors. 
	- Referee 1 tells you "the wage residual is likely to be correlated within ==local labor markets==, so you should cluster your standard errors by state or village."
	- But Referee 2 argues "The wage residual is likely to be correlated for ==people working in the same industry==, so you should cluster your standard errors by industry",
	- and referee 3 argues that "the wage residual is likely to be correlated by ==age cohort==, so you should cluster your standard errors by cohort".  (cohort: 특성을 공유하는 주제 그룹)
- What should you do?
- you could try estimating your model with these three different clustering approaches, and see what difference this makes.
- Their advice: Whether or not clustering makes a difference to the standard errors should not be the basis for deciding whether or not to cluster.

- Instead, under the sampling perspective, what matters for clustering is **how the sample was selected** and whether there are clusters in the population of interest that are not represented in the sample.