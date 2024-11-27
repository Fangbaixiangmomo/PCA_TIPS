# Capturing Volatility Structure in the HJM Model Using PCA

In order to effectively capture the nuances of the volatility structure in the Heath-Jarrow-Morton (HJM) model, we employ **Principal Component Analysis (PCA)** to analyze the variances and correlations in the forward rate changes. PCA provides a way to decompose the volatility of interest rates into key components, offering insights into the dominant factors driving the dynamics of these rates. This analysis can reveal whether a small number of principal components can explain most of the variability, which is often the case in interest rate modeling.

## Multi-Dimensional HJM Model

The multi-dimensional HJM model assumes the following differential form for forward rates:

$$
df(t,T) = \alpha(t,T)dt + \sum_{i=1}^{m} \sigma_i(t,T)dW_i(t)
$$

We can discretize it as:

$$
\Delta f(t,T) = \alpha(t,T)\Delta t + \sum_{i=1}^{m} \sigma_i(t,T)\Delta W_i
$$

In the one-dimensional case, the drift term is uniquely determined as:

$$
\alpha(t,T) = \sigma(t,T) \int_t^T \sigma(t,u) du
$$

Repeating this for the multi-dimensional setup yields:

$$
\alpha(t,T) = \sum_{i=1}^{m} \sigma_i(t,T) \int_t^T \sigma_i(t,u) du
$$

Substituting this back, we have:

$$
\Delta f(t,T) = \left[\sum_{i=1}^{m} \sigma_i(t,T) \int_t^T \sigma_i(t,u) du \right] \Delta t + \sum_{i=1}^{m} \sigma_i(t,T) \Delta W_i
$$

Here, \( \Delta W_i \) are IID normal random variables with mean 0 and variance \( \Delta t \).

## Discrete Approximation

Given \( k \) observations from \( t \) to \( T \) with step size \( \Delta t' \), we approximate the integral with a discrete sum:

$$
\Delta f(t,T) = \left[\sum_{i=1}^{m} \sigma_i(t,T) \sum_{j=0}^{k} \sigma_i(t, t + j\Delta t') \Delta t' \right] \Delta t + \sum_{i=1}^{m} \sigma_i(t,T) \Delta W_i
$$

This allows us to study the variation in forward rate movements, which arise from the \( m \) IID Brownian covariates. Our focus is on identifying the dominant volatility factors from \( \sigma(\cdot) \).

## Organizing Forward Rate Changes

Let \( f(t_1, T), f(t_2, T), \ldots, f(t_n, T) \) represent the forward rates observed at different time points for a fixed maturity \( T \). We organize the changes into a matrix \( \mathbf{F} \) as follows:

$$
\mathbf{F} =
\begin{bmatrix}
\Delta f(t_1, T_1) & \Delta f(t_1, T_2) & \cdots & \Delta f(t_1, T_m) \\
\Delta f(t_2, T_1) & \Delta f(t_2, T_2) & \cdots & \Delta f(t_2, T_m) \\
\vdots & \vdots & \ddots & \vdots \\
\Delta f(t_n, T_1) & \Delta f(t_n, T_2) & \cdots & \Delta f(t_n, T_m)
\end{bmatrix}
$$

Here, \( \Delta f(t_i, T_j) = f(t_i, T_j) - f(t_{i-1}, T_j) \) represents the change in forward rate for maturity \( T_j \) between time steps \( t_{i-1} \) and \( t_i \).

## Covariance Analysis

The matrix \( \mathbf{F} \) encodes the forward rate changes across maturities and time steps. Define the mean vector for each column as:

$$
\mathbf{m}_j = \frac{1}{n} \sum_{i=1}^{n} \Delta f(t_i, T_j)
$$

The empirical covariance between two columns \( \mathbf{F}_i \) and \( \mathbf{F}_j \) is given by:

$$
\text{Cov}(\mathbf{F}_i, \mathbf{F}_j) = \frac{1}{n-1} (\mathbf{F}_i - \mathbf{m}_i)^\intercal (\mathbf{F}_j - \mathbf{m}_j)
$$

Equivalently, the covariance matrix for \( \mathbf{F} \) is:

$$
\text{Cov}(\mathbf{F}) = \frac{1}{n-1} \left(\mathbf{F}^\intercal \mathbf{F} - \mathbf{m} \mathbf{m}^\intercal \right)
$$

## Identifying Dominant Volatility Factors

The task now is to find the eigenvalues and eigenvectors of this positive semi-definite matrix. Sorting the eigenvalues in descending order and projecting the data onto the corresponding eigenvectors allows us to identify the dominant volatility sources across maturities and time points.

By analyzing the principal components, we can recognize the most influential factors driving variability in the forward rates, enhancing our understanding of interest rate dynamics.

