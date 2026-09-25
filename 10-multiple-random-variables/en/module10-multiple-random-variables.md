# Module 10 — Multiple Random Variables

## Learning Objectives

After completing this module, students will be able to:
1. Extend joint distributions to $N$ random variables
2. Define and work with random vectors
3. Compute mean vectors and covariance matrices
4. Apply the multivariate Gaussian distribution
5. Perform linear transformations of random vectors

---

## 10.1 Introduction: From 2 to $N$ Variables

Real systems often involve many uncertain quantities simultaneously:
- **Sensor array:** $N$ sensors each producing a noisy measurement
- **MIMO system:** $M \times N$ channel coefficients
- **Parameter estimation:** $p$ unknown parameters
- **Signal samples:** $N$ consecutive noise samples

We need a framework for **N-dimensional** random vectors.

---

## 10.2 Joint Distributions for $N$ Random Variables

### Joint PDF

For $X_{1},\, X_{2},\, \ldots,\, X_{n}$:
$$f_{X_1,...,X_n}(x_1,...,x_n) \geq 0, \quad \int\cdots\int f_{X_1,...,X_n} \, dx_1 \cdots dx_n = 1$$

### Marginal Distributions

Integrate (or sum) out unwanted variables:
$$f_{X_1}(x_1) = \int\cdots\int f_{X_1,...,X_n}(x_1,...,x_n) \, dx_2 \cdots dx_n$$

### Conditional Distributions

$$f_{X_1|X_2,...,X_n}(x_1|x_2,...,x_n) = \frac{f_{X_1,...,X_n}(x_1,...,x_n)}{f_{X_2,...,X_n}(x_2,...,x_n)}$$

### Mutual Independence

$X_{1},\, \ldots,\, X_{n}$ are mutually independent if:
$$f_{X_1,...,X_n}(x_1,...,x_n) = \prod_{i=1}^n f_{X_i}(x_i)$$

---

## 10.3 Random Vectors

### Definition

A **random vector** is an ordered collection of random variables:

$$\mathbf{X} = \begin{bmatrix} X_1 \\ X_2 \\ \vdots \\ X_n \end{bmatrix}$$

### Engineering Examples

| Random Vector | Components | Context |
|--------------|-----------|---------|
| Sensor array output | $X_{1},\,\ldots,\,X_{n}$ = sensor readings | Array processing |
| MIMO channel | $h_{1},\,\ldots,\,h_{n}$ = channel gains | Wireless communications |
| Noise vector | $N_{1},\,\ldots,\,N_{n}$ = noise samples | Signal processing |
| State vector | Position, velocity, acceleration | Kalman filtering |
| Feature vector | Extracted measurements | Pattern recognition |

---

## 10.4 Mean Vector

### Definition

$$\boldsymbol{\mu} = E[\mathbf{X}] = \begin{bmatrix} E[X_1] \\ E[X_2] \\ \vdots \\ E[X_n] \end{bmatrix} = \begin{bmatrix} \mu_1 \\ \mu_2 \\ \vdots \\ \mu_n \end{bmatrix}$$

### Properties
- $E[AX + b]$ = AE[X] $+ b = A\mu + b$
- Linearity extends to vectors and matrices

---

## 10.5 Covariance Matrix

### Definition

$$\boldsymbol{\Sigma} = E[(\mathbf{X} - \boldsymbol{\mu})(\mathbf{X} - \boldsymbol{\mu})^T]$$

Element $(i,\,j)$: $\Sigma_{ij} = \operatorname{Cov}(X_{i},\, X_{j})$

$$\boldsymbol{\Sigma} = \begin{bmatrix} \sigma_1^2 & \text{Cov}(X_1,X_2) & \cdots & \text{Cov}(X_1,X_n) \\ \text{Cov}(X_2,X_1) & \sigma_2^2 & \cdots & \text{Cov}(X_2,X_n) \\ \vdots & & \ddots & \vdots \\ \text{Cov}(X_n,X_1) & \cdots & \cdots & \sigma_n^2 \end{bmatrix}$$

### Properties

1. **Symmetric:** $\Sigma = \Sigma^{T}$
2. **Positive semi-definite:** $v^{T}\Sigma v \ge 0$ for all $v$
3. **Diagonal = variances:** $\Sigma_{ii} = \operatorname{Var}(X_{i})$
4. **Off-diagonal = covariances:** $\Sigma_{ij} = \operatorname{Cov}(X_{i},\, X_{j})$
5. **Independent components → diagonal $\Sigma$**

### Alternative Formula

$$\boldsymbol{\Sigma} = E[\mathbf{X}\mathbf{X}^T] - \boldsymbol{\mu}\boldsymbol{\mu}^T$$

---

## 10.6 Multivariate Gaussian Distribution

### Definition

$X \sim N(\mu,\, \Sigma)$ has PDF:

$$f_\mathbf{X}(\mathbf{x}) = \frac{1}{(2\pi)^{n/2}|\boldsymbol{\Sigma}|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)$$

where $\lvert \Sigma\rvert$ = determinant of $\Sigma$.

### Key Properties

1. **Marginals are Gaussian:** Any subset of components is jointly Gaussian
2. **Conditionals are Gaussian:** $X_{1} \mid X_{2} = x_{2}$ is Gaussian
3. **Linear transformations:** $Y$ = AX $+ b \sim N(A\mu +b,\, A\Sigma A^{T})$
4. **Uncorrelated = Independent** (unique to Gaussian!)
5. **Fully specified** by $\mu$ and $\Sigma$ (only 2 parameters needed per variable plus correlations)

### Why So Important?

- Central Limit Theorem: sums of many RVs → multivariate Gaussian
- Thermal noise vectors are multivariate Gaussian
- Analytically tractable (closed-form marginals, conditionals)
- Optimal filters (Kalman, Wiener) assume Gaussian

### $2D$ Case (Bivariate Gaussian)

For $n = 2$ with $\mu = [0,\,0]^{T},\, \sigma_{1} = \sigma_{2} = 1$:

$$f(x,y) = \frac{1}{2\pi\sqrt{1-\rho^2}} \exp\left(-\frac{x^2 - 2\rho xy + y^2}{2(1-\rho^2)}\right)$$

Contours are **ellipses** (tilted when $\rho \ne 0$).

---

## 10.7 Linear Transformations of Random Vectors

### Transformation $Y$ = AX $+ b$

If $X$ is a random vector with mean $\mu_{X}$ and covariance $\Sigma_{X}$:

$$E[\mathbf{Y}] = A\boldsymbol{\mu}_X + \mathbf{b}$$
$$\boldsymbol{\Sigma}_Y = A\boldsymbol{\Sigma}_X A^T$$

### Engineering Applications

**Beamforming:** Output $y = w^{T}X$ (weight vector applied to sensor array)
- $E[y] = w^{T}\mu$
- $\operatorname{Var}(y) = w^{T}\Sigma w$

**Whitening:** Find matrix $W$ such that $W\Sigma W^{T}$ = I (decorrelates data)
- $W = \Sigma^{-1/2}$

**Principal Component Analysis:** Rotate to diagonalize $\Sigma$

---

## 10.8 MATLAB Examples

### Example 1: Generating Multivariate Gaussian

```matlab
%% Generate and Visualize 2D Gaussian Random Vector
mu = [1; 2];
Sigma = [4, 2; 2, 3];  % Covariance matrix

N = 5000;
X = mvnrnd(mu', Sigma, N);

figure;
scatter(X(:,1), X(:,2), 5, 'b', '.');
hold on;
plot(mu(1), mu(2), 'r+', 'MarkerSize', 15, 'LineWidth', 3);
xlabel('X_1'); ylabel('X_2');
title('Bivariate Gaussian Samples');
axis equal; grid on;

% Verify statistics
fprintf('Mean: [%.2f, %.2f] (theory: [%.1f, %.1f])\n', mean(X), mu');
fprintf('Cov matrix:\n'); disp(cov(X));
```

### Example 2: Linear Transformation

```matlab
%% Transform: Y = AX + b
mu_x = [0; 0]; Sigma_x = eye(2);   % Standard normal
A = [2, 1; -1, 3];  b = [1; -2];

N = 10000;
X = mvnrnd(mu_x', Sigma_x, N)';
Y = A*X + b;

% Theoretical
mu_y_theory = A*mu_x + b;
Sigma_y_theory = A*Sigma_x*A';

fprintf('E[Y] theory: [%.1f; %.1f], sim: [%.2f; %.2f]\n', ...
    mu_y_theory, mean(Y,2));
fprintf('Σ_Y theory:\n'); disp(Sigma_y_theory);
fprintf('Σ_Y simulated:\n'); disp(cov(Y'));
```

### Example 3: Sensor Array with Correlated Noise

```matlab
%% 4-Element Sensor Array with Spatially Correlated Noise
n_sensors = 4;
sigma2 = 1;      % Noise power per sensor
rho = 0.6;       % Adjacent sensor correlation

% Build covariance matrix (exponential decay model)
Sigma = zeros(n_sensors);
for i = 1:n_sensors
    for j = 1:n_sensors
        Sigma(i,j) = sigma2 * rho^abs(i-j);
    end
end

fprintf('Noise covariance matrix:\n'); disp(Sigma);

% Generate noise samples
N_samp = 10000;
L = chol(Sigma, 'lower');
noise = L * randn(n_sensors, N_samp);

% Signal present in direction: s = [1; 1; 1; 1] (broadside)
signal = 3;
s = ones(n_sensors, 1) / sqrt(n_sensors);
received = signal * s + noise;  % N_sensors x N_samp

% Beamformer output: y = w'*received (w = s for matched filter)
w = s;
y = w' * received;
SNR_out = signal^2 / (w'*Sigma*w);
fprintf('Output SNR = %.2f (%.1f dB)\n', SNR_out, 10*log10(SNR_out));
```

---

## 10.9 Practice Problems

### Problem 1
Random vector $X = [X_{1},\, X_{2},\, X_{3}]^{T}$ with $\mu = [1,\, 0,\, -1]^{T}$, and covariance matrix $\Sigma = \begin{bmatrix} 4 & 1 & 0 \\ 1 & 2 & -1 \\ 0 & -1 & 3 \end{bmatrix}$.
(a) Find $\operatorname{Var}(X_{1} + X_{2} + X_{3})$. (b) Find $\operatorname{Cov}(X_{1},\, X_{2}+X_{3})$. (c) Are $X_{1}$ and $X_{3}$ uncorrelated?

**Solution:**
(a) $\operatorname{Var}(X_{1}+X_{2}+X_{3}) = 1^{T}\Sigma 1 = 4+2+3 + 2(1) + 2(0) + 2(-1) = 9 + 0 = 9$
Wait: $= \sum_{ij}$ all entries $= 4+1+0+1+2+(-1)+0+(-1)+3 = 9$
(b) $\operatorname{Cov}(X_{1},\, X_{2}+X_{3}) = \operatorname{Cov}(X_{1},\,X_{2}) + \operatorname{Cov}(X_{1},\,X_{3}) = 1 + 0 = 1$
(c) $\operatorname{Cov}(X_{1},\,X_{3}) = 0$, so yes, $X_{1}$ and $X_{3}$ are uncorrelated.

### Problem 2
$X \sim N(0,\, \Sigma)$ with $\Sigma = \begin{bmatrix} 1 & \rho \\ \rho & 1 \end{bmatrix}$. Find the conditional distribution of $X_{1} \mid X_{2} = x_{2}$.

**Solution:** For bivariate Gaussian:
$X_{1} \mid X_{2} = x_{2} \sim N\left(\mu_{1} + \rho \left(\frac{\sigma_{1}}{\sigma_{2}}\right)(x_{2}-\mu_{2}),\, \sigma_{1}^{2}(1-\rho^{2})\right)$
$= N(\rho x_{2},\, 1-\rho^{2})$

The conditional mean is a linear function of $x_{2}$, and conditional variance is reduced by factor $(1-\rho^{2})$.

### Problem 3
Write MATLAB code to generate a $3D$ Gaussian vector with given $\mu$ and $\Sigma$, compute the sample covariance, and verify.

**Solution:**
```matlab
mu = [1; 0; -1];
Sigma = [4 1 0; 1 2 -1; 0 -1 3];
N = 50000;
X = mvnrnd(mu', Sigma, N);
fprintf('Sample mean: '); disp(mean(X)');
fprintf('Sample cov:\n'); disp(cov(X));
```

---

*Next Module: [Module 11 — Central Limit Theorem](module11-central-limit-theorem.md)*
