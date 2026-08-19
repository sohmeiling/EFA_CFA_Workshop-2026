# Factor Analysis Notes

## Factor Analysis: General Goal

**Goal:** Find common factors $f$ that affect a set of observed variables $x$.

Factor analysis can be viewed as a form of **dimensionality reduction**, similar to PCA, but the goal differs.

### PCA vs. Factor Analysis

- **PCA is data-oriented**
  - Reduces the dimensionality of the data.
  - Projects the observed variables into a lower-dimensional space.
  - The new dimensions do not necessarily need to represent meaningful latent constructs.
  - Main goal: obtain a lower-dimensional representation of the data.

- **Factor Analysis is model-oriented**
  - Reduces dimensionality by modelling common latent factors.
  - Focuses on the meaning of the underlying dimensions.
  - Main goal: identify factors that can explain the covariance among observed variables.

### Different mathematical goals

- **PCA**
  - Finds new axes that maximise the variance in the data.
  - Uses total variance.

- **Factor Analysis**
  - Identifies latent variables or common factors that explain the shared variance among observed variables.
  - Uses a hypothesised common-factor model.

PCA and FA may sometimes produce similar loading patterns, especially when communalities are high, but they are not the same model.

---

## PCA vs. Factor Analysis

| Feature | Principal Component Analysis (PCA) | Factor Analysis (FA/EFA) |
|---|---|---|
| **Primary goal** | Reduce many observed variables into fewer components | Identify latent factors that explain relationships among observed variables |
| **Main question** | How can I summarise these variables with fewer dimensions? | What underlying constructs could explain why these variables correlate? |
| **Orientation** | Data-oriented | Model-oriented |
| **What is extracted?** | Components | Latent factors |
| **Variance analysed** | Total variance of each variable | Primarily common/shared variance |
| **Unique/error variance** | Included in the components | Modelled separately from common variance |
| **Model** | Observed variables → components | Latent factors → observed variables |
| **Typical purpose** | Data reduction, composite scores, reduction of multicollinearity | Identification of psychological constructs, scale development, construct validation |
| **Interpretation required?** | Components may be interpreted, but interpretation is not central to the mathematical objective | Interpretation of factors is central |
| **Rotation** | Can be used to improve interpretation | Commonly used to obtain interpretable factor structure |

### Important distinction

It is common to see PCA used in workflows described as "factor analysis", especially in older literature or software menus. However:

> PCA is not technically a common-factor model.

For exploratory factor analysis, common extraction methods include:

- Principal axis factoring (PAF)
- Maximum likelihood (ML)
- Minimum residual (MINRES)

A more precise statement is:

> PCA and EFA follow similar analytical workflows, but PCA is not itself a factor-analysis model.

---

## Conceptual Difference Between PCA and FA

### PCA

PCA constructs weighted combinations of observed variables:

$$
PC_1 = w_1X_1 + w_2X_2 + \cdots + w_pX_p
$$

The observed variables are combined to produce a component:

$$
\text{Observed variables} \rightarrow \text{Component}
$$

### Factor Analysis

Factor analysis assumes that latent factors generate or explain variation in observed variables:

$$
X_i = \lambda_iF + \epsilon_i
$$

where:

- $F$ = common latent factor
- $\lambda_i$ = factor loading
- $\epsilon_i$ = unique variance and error

Conceptually:

$$
\text{Latent factor} \rightarrow \text{Observed variables}
$$

---

## Examples of Factor Analysis

- $x$ = scores on different intelligence tests  
  $f$ = underlying dimensions of intelligence

- $x$ = responses to psychological questionnaire items  
  $f$ = latent personality traits, such as the Big Five

- $x$ = prices or returns of different stocks  
  $f$ = underlying market or industry factors

---

# Linear Factor Model

Each observed variable is modelled as a linear combination of common factors plus a variable-specific component.

$$
x_j - \mu_j
=
l_{j1}f_1 + \cdots + l_{jm}f_m + \epsilon_j,
\qquad j = 1,\ldots,p
$$

where:

- $x_j$ = observed variable $j$
- $\mu_j$ = mean of observed variable $j$
- $f_1,\ldots,f_m$ = common factors
- $l_{j1},\ldots,l_{jm}$ = factor loadings
- $\epsilon_j$ = specific or unique component for variable $j$

---

## Vector/Matrix Notation

$$
\mathbf{x} - \boldsymbol{\mu}
=
L\mathbf{f} + \boldsymbol{\epsilon}
$$

where:

- $\mathbf{x}$ = random vector containing $p$ observed variables
- $\boldsymbol{\mu}$ = mean vector of $\mathbf{x}$
- $\boldsymbol{\epsilon}$ = vector of specific factors / unique errors
- $\mathbf{f}$ = vector of $m$ common factors
- usually $m \ll p$
- $L$ = factor-loading matrix of size $p \times m$, also called the **pattern matrix**

---

## Interpretation of the Factor Model

Each observed variable is determined by:

1. its mean,
2. one or more common factors,
3. the loading of the variable on each factor,
4. variance that is unique to that variable.

For example:

$$
x_1 - \mu_1
=
l_{11}f_1 + l_{12}f_2 + \epsilon_1
$$

where:

- $l_{11}$ = loading of $x_1$ on Factor 1
- $l_{12}$ = loading of $x_1$ on Factor 2
- $\epsilon_1$ = unique component of $x_1$

---

# How to Draw a Factor-Analysis Diagram

The conventional notation is:

- **Oval or circle** = latent variable
- **Rectangle** = observed/measured variable
- **Small circle** = residual/error or unique factor

A simple diagram can be represented as:

```text
             (Factor 1)                   (Factor 2)
              /   |   \                    /   |   \
             ↓    ↓    ↓                  ↓    ↓    ↓

          [ x1 ] [ x2 ] [ x3 ]   ...   [xp-1] [ xp ]
             ↑      ↑      ↑                ↑      ↑
            (ε1)   (ε2)   (ε3)            (εp-1) (εp)

```
The arrows generally go:

$$
\text{Factor} \rightarrow \text{Observed variable}
$$

because the factor model assumes that latent factors account for variation in the observed variables.

Cross-loadings can be represented by additional arrows from one factor to variables associated primarily with another factor.

---

# Factor Analysis and Regression

The factor model resembles a regression model because both represent a variable as a linear combination of other quantities plus an error term.

### Factor model

$$
x_j - \mu_j
=
l_{j1}f_1 + \cdots + l_{jm}f_m + \epsilon_j
$$

### Regression model

$$
y - \beta_0
=
\beta_1x_1 + \cdots + \beta_mx_m + \epsilon
$$

The equations look similar, but the interpretation differs.

In regression:

- $y$ is an observed outcome variable.
- $x_1,\ldots,x_m$ are observed predictors.
- $\beta_1,\ldots,\beta_m$ are regression coefficients.

In factor analysis:

- $x_j$ is an observed variable.
- $f_1,\ldots,f_m$ are unobserved latent factors.
- $l_{j1},\ldots,l_{jm}$ are factor loadings.
- $\epsilon_j$ represents variance unique to the observed variable.

---

## Factor Analysis vs. Regression

| Feature | Regression | Factor Analysis |
|---|---|---|
| **Outcome** | $y$ is usually a single observed outcome variable | $\mathbf{x}$ contains multiple observed variables |
| **Coefficients / loadings** | Regression coefficients $\beta$ quantify the relationship between predictors and the outcome; they usually form a vector | Factor loadings $L$ quantify the relationship between latent factors and observed variables; they are arranged in a $p \times m$ loading matrix |
| **Predictors** | Uses observed predictors or covariates $x$ | Uses common latent factors $f$ |
| **Are predictors/factors observed?** | Predictors $x$ are known and observed | Factors $f$ are unobserved latent variables |
| **Goal** | Predict or explain an observed outcome from observed predictors | Explain covariance among observed variables using latent factors |
| **Machine-learning analogy** | Supervised: the outcome $y$ is observed | Unsupervised: latent factors are inferred from the observed variables |

### Important terminology

Instead of saying:

> FA coefficients form a matrix.

It is better to say:

> Factor loadings are arranged in a $p \times m$ loading matrix $L$.

Each loading $l_{jk}$ represents the relationship between observed variable $j$ and latent factor $k$.

---

# Orthogonal Factor Model

An **orthogonal factor model** assumes that the common factors are uncorrelated with one another.

Several additional assumptions are made.

---

## Assumption 1: Common factors have mean zero

$$
E[\mathbf{f}] = 0
$$

This means that the expected value of each common factor is zero.

---

## Assumption 2: Common factors are orthogonal and have unit variance

$$
V[\mathbf{f}]
=
E[\mathbf{f}\mathbf{f}^T]
=
I
$$

where $I$ is the identity matrix.

This assumption implies that:

- each factor has variance equal to 1;
- different common factors are uncorrelated.

For two different factors $f_j$ and $f_k$:

$$
Cov(f_j,f_k)=0
$$

for $j \neq k$.

---

## Assumption 3: Specific factors have mean zero

$$
E[\boldsymbol{\epsilon}] = 0
$$

The unique or specific components are assumed to have an expected value of zero.

---

## Assumption 4: Specific factors are uncorrelated with one another

$$
V[\boldsymbol{\epsilon}]
=
E[\boldsymbol{\epsilon}\boldsymbol{\epsilon}^T]
=
\Psi
$$

where $\Psi$ is a diagonal matrix.

For example:

$$
\Psi
=
\begin{bmatrix}
\psi_1 & 0 & 0 \\
0 & \psi_2 & 0 \\
0 & 0 & \psi_3
\end{bmatrix}
$$

The diagonal elements represent the unique variance of each observed variable.

The off-diagonal elements are zero because the specific factors are assumed to be uncorrelated with one another.

---

## Assumption 5: Common factors and specific factors are uncorrelated

$$
Cov(\boldsymbol{\epsilon},\mathbf{f})
=
E[\boldsymbol{\epsilon}\mathbf{f}^T]
=
0
$$

For an individual specific factor and common factor:

$$
Cov(\epsilon_j,f_h)
=
0
$$

Therefore, the variance associated with the common factors is assumed to be independent of the variance unique to each observed variable.

---

# Covariance Structure of the Orthogonal Factor Model

Recall the basic factor model:

$$
\mathbf{x}-\boldsymbol{\mu}
=
L\mathbf{f}+\boldsymbol{\epsilon}
$$

The covariance matrix of the observed variables is:

$$
\Sigma
=
V[\mathbf{x}]
$$

Since:

$$
V[\mathbf{x}]
=
E[
(\mathbf{x}-\boldsymbol{\mu})
(\mathbf{x}-\boldsymbol{\mu})^T
]
$$

substitute the factor model:

$$
\Sigma
=
E[
(L\mathbf{f}+\boldsymbol{\epsilon})
(L\mathbf{f}+\boldsymbol{\epsilon})^T
]
$$

Expanding gives:

$$
\Sigma
=
E[
L\mathbf{f}\mathbf{f}^TL^T
+
L\mathbf{f}\boldsymbol{\epsilon}^T
+
\boldsymbol{\epsilon}\mathbf{f}^TL^T
+
\boldsymbol{\epsilon}\boldsymbol{\epsilon}^T
]
$$

Using the orthogonal factor-model assumptions:

$$
E[\mathbf{f}\mathbf{f}^T]=I
$$

$$
E[\mathbf{f}\boldsymbol{\epsilon}^T]=0
$$

$$
E[\boldsymbol{\epsilon}\mathbf{f}^T]=0
$$

$$
E[\boldsymbol{\epsilon}\boldsymbol{\epsilon}^T]=\Psi
$$

therefore:

$$
\Sigma
=
LIL^T+\Psi
$$

Since multiplying by the identity matrix does not change $L$:

$$
\boxed{
\Sigma
=
LL^T+\Psi
}
$$

Thus:

$$
\boxed{
V[\mathbf{x}]
=
LL^T+\Psi
}
$$

---

## Interpretation of the Covariance Decomposition

The covariance structure of the observed variables can be divided into two parts:

$$
\text{Observed variance/covariance}
=
\text{Common-factor variance}
+
\text{Unique variance}
$$

More specifically:

$$
\Sigma
=
LL^T+\Psi
$$

where:

- $LL^T$ represents variance and covariance explained by the common factors;
- $\Psi$ represents unique variance associated with individual observed variables.

This distinction is central to factor analysis.

---

# Variance of an Individual Observed Variable

Looking at the diagonal elements of the covariance matrix:

$$
\sigma_j^2
=
l_{j1}^2
+
\cdots
+
l_{jm}^2
+
\psi_j
$$

This means that the total variance of observed variable $j$ can be divided into:

$$
\text{Total variance}
=
\text{Common variance}
+
\text{Unique variance}
$$

The common variance is called the **communality**.

---

# Communality

The **communality** of a variable is the amount of its variance accounted for by the common factors.

For observed variable $j$:

$$
h_j^2
=
l_{j1}^2
+
l_{j2}^2
+
\cdots
+
l_{jm}^2
$$

or equivalently:

$$
h_j^2
=
\sum_{k=1}^{m}l_{jk}^2
$$

Therefore:

$$
\sigma_j^2
=
h_j^2+\psi_j
$$

where:

- $h_j^2$ = communality;
- $\psi_j$ = uniqueness.

---

## Example of Communality

Suppose an observed variable has the following loadings on two orthogonal factors:

$$
l_1=.70
$$

$$
l_2=.30
$$

Its communality is:

$$
h^2
=
(.70)^2+(.30)^2
$$

$$
=
.49+.09
$$

$$
=
.58
$$

Therefore, the common factors account for 58% of the variance in this variable.

---

# Uniqueness

**Uniqueness** is the portion of an observed variable's variance that is not accounted for by the common factors.

$$
\psi_j
=
\sigma_j^2-h_j^2
$$

Uniqueness may contain two sources of variance:

$$
\text{Uniqueness}
=
\text{Specific variance}
+
\text{Measurement error/noise}
$$

### Specific variance

Specific variance represents systematic variance that belongs only to that particular observed variable and is not shared with the other variables.

### Measurement error

Measurement error represents random variation or unreliability in the observed measure.

Factor analysis does not usually separate these two sources directly; both are contained in the uniqueness term $\psi_j$.

---

# Communality and Uniqueness for Standardised Variables

If the observed variables have been standardised, each variable has variance:

$$
\sigma_j^2=1
$$

Therefore:

$$
1
=
h_j^2+\psi_j
$$

and:

$$
\boxed{
\psi_j=1-h_j^2
}
$$

Likewise:

$$
\boxed{
h_j^2=1-\psi_j
}
$$

For example, if:

$$
h_j^2=.70
$$

then:

$$
\psi_j
=
1-.70
=
.30
$$

This means that:

- 70% of the variance is explained by the common factors;
- 30% is unique variance.

---

# Covariance Between Observed Variables and Factors

We can also calculate the covariance between the observed variables $\mathbf{x}$ and the common factors $\mathbf{f}$.

Start with:

$$
Cov(\mathbf{x},\mathbf{f})
$$

Because subtracting a constant does not affect covariance:

$$
Cov(\mathbf{x},\mathbf{f})
=
Cov(\mathbf{x}-\boldsymbol{\mu},\mathbf{f})
$$

Recall:

$$
\mathbf{x}-\boldsymbol{\mu}
=
L\mathbf{f}+\boldsymbol{\epsilon}
$$

Therefore:

$$
Cov(\mathbf{x},\mathbf{f})
=
Cov(L\mathbf{f}+\boldsymbol{\epsilon},\mathbf{f})
$$

This can be written as:

$$
=
E[
(L\mathbf{f}+\boldsymbol{\epsilon})\mathbf{f}^T
]
$$

Expanding:

$$
=
LE[\mathbf{f}\mathbf{f}^T]
+
E[\boldsymbol{\epsilon}\mathbf{f}^T]
$$

Under the orthogonal factor-model assumptions:

$$
E[\mathbf{f}\mathbf{f}^T]=I
$$

and:

$$
E[\boldsymbol{\epsilon}\mathbf{f}^T]=0
$$

Therefore:

$$
Cov(\mathbf{x},\mathbf{f})
=
LI+0
$$

and hence:

$$
\boxed{
Cov(\mathbf{x},\mathbf{f})
=
L
}
$$

---

# Interpretation of Factor Loadings

Under the orthogonal factor model:

$$
Cov(\mathbf{x},\mathbf{f})
=
L
$$

Therefore, each element $l_{jk}$ of the loading matrix represents the covariance between:

- observed variable $x_j$; and
- common factor $f_k$.

If both the observed variables and factors are standardised, then:

$$
l_{jk}
=
Cor(x_j,f_k)
$$

Thus, under these assumptions, a factor loading can be interpreted as the correlation between an observed variable and a factor.

For example:

$$
l_{jk}=.80
$$

indicates a strong positive relationship between observed variable $j$ and factor $k$.

A loading close to zero indicates that the observed variable has little relationship with that factor.

The sign indicates the direction of the relationship:

- positive loading: higher factor scores are associated with higher observed scores;
- negative loading: higher factor scores are associated with lower observed scores.

---

# Key Relationships to Remember

### Basic factor model

$$
\mathbf{x}-\boldsymbol{\mu}
=
L\mathbf{f}+\boldsymbol{\epsilon}
$$

### Covariance structure

$$
\boxed{
\Sigma
=
LL^T+\Psi
}
$$

### Variance of variable $j$

$$
\boxed{
\sigma_j^2
=
h_j^2+\psi_j
}
$$

### Communality

$$
\boxed{
h_j^2
=
\sum_{k=1}^{m}l_{jk}^2
}
$$

### For standardised variables

$$
\boxed{
1
=
h_j^2+\psi_j
}
$$

### Covariance between observed variables and factors

$$
\boxed{
Cov(\mathbf{x},\mathbf{f})
=
L
}
$$

### For standardised observed variables and orthogonal standardised factors

$$
\boxed{
l_{jk}
=
Cor(x_j,f_k)
}
$$