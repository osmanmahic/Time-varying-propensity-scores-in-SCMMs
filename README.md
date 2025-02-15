# Time-varying-propensity-scores-in-SCMMs
<p ">Author: Osman Mahic<br>Date: 15-02-2025</p>

This repository presents a mathematical formulation of the **Sequential Conditional Mean Model (SCMM)** with **Time-Varying Propensity Scores**—a Generalized Estimating Equations (GEE)-based approach to modeling time-varying treatment effects, as applied in my study (https://doi.org/10.1093/ndt/gfae069.1616).

This method was first described by: 

Keogh, R. H., et al. (2017). Analysis of longitudinal studies with repeated outcome measures: Adjusting for time-dependent confounding using conventional methods. American Journal of Epidemiology, 187(5), 1085–1092. https://doi.org/10.1093/aje/kwx311

## Notation
Let $X$ denote the exposure and $Y$ the outcome, both measured repeatedly at times $t = 1, \dots, T$. We define the notation $X_t$ to denote the time-varying exposure at time $t$, and $Y_t$ the outcome at time $t$. Similarly, let $\boldsymbol{L}_t$ denote a vector of (time-varying) covariates at time $t$, and $\bar{\boldsymbol{L}}\_{t}$ represent the history up to time $t$.

## Estimator for the conditional expectation
We aim to estimate the following conditional expectation at a given time:

$$
\text{E} [Y_t|{\bar{X}}\_{t},{\bar{Y}}\_{t-1},{\bar{\boldsymbol{L}}}\_{t}]  = \theta_0 + \theta_1{{X}}\_{t} + \theta_2{{X}}\_{t-1} + \theta_3{{Y}}\_{t-1} + \theta_4^{\top}{\bar{\boldsymbol{L}}}\_{t}
$$

Because adjustment is made for ${\bar{Y}}\_{t-1}$, the parameters of such SCMM can be estimated using a GEE with an identity working correlation matrix:

$$
\text{Corr}(Y_{ij}, Y_{ik}) =
\left\lbrace
\begin{array}{ll}
1, & \text{if } j = k \\
0, & \text{if } j \neq k
\end{array}
\right.
$$

## Time-varying propensity scores

A SCMM is a doubly-robust estimator when a time-varying propensity score is included as a covariate. Given that $X \in \lbrace 1, \dots, K \rbrace $, we computed the following generalized propensity scores using multinomial logistic regression:

$$
\text{GPS}_t = \text{Pr}[X_t = k | {\bar{X}}\_{t-1},{\bar{Y}}\_{t-1},{\bar{\boldsymbol{L}}}\_{t}]
$$


Where the class probabilities were estimated using the softmax function: 

$$
\text{Pr}[X_t = k | \boldsymbol{V}] = \frac{1}{\sum_{j=1}^{K} e^{\theta_j' \boldsymbol{V}}}  e^{\theta_k' \boldsymbol{V}}
$$

With $\boldsymbol{V}$ representing the covariate vector = $({\bar{X}}\_{t-1},{\bar{Y}}\_{t-1},{\bar{\boldsymbol{L}}}\_{t})$. Given that $\sum_{k=1}^{K} \Pr(X_t = k | \boldsymbol{V}) = 1$ holds, it is sufficient to condition on $K-1$ of the $\widehat{\text{GPS}_t}$ in the SCMM. 

## Software
R version 4.1.3 or higher with the following packages: 
- `nnet`: Feed-Forward Neural Networks and Multinomial Log-Linear Models
- `geepack`: Generalized Estimating Equation Package









