# Module 9 — Covariance and Correlation

## Learning Objectives

After completing this module, students will be able to:
1. Compute and interpret covariance
2. Compute and interpret the correlation coefficient
3. Distinguish between independence and uncorrelatedness
4. Construct and interpret correlation matrices
5. Apply correlation analysis to engineering problems

---

## 9.1 Introduction: Measuring Linear Relationships

When two random variables are not independent, we need to quantify **how they relate**. Covariance and correlation measure the **linear relationship** between two RVs.

Engineering examples:
- Correlated noise in adjacent sensor channels
- Signal measurements at two antennas (MIMO correlation)
- Voltage and current in a circuit (related through impedance)
- Temperature and device performance (thermal dependence)

---

## 9.2 Covariance

### Definition

$$\text{Cov}(X,Y) = E[(X - \mu_X)(Y - \mu_Y)] = E[XY] - E[X]E[Y]$$

### Properties

1. $\operatorname{Cov}(X,\, X) = \operatorname{Var}(X)$
2. $\operatorname{Cov}(X,\, Y) = \operatorname{Cov}(Y,\, X)$ (symmetric)
3. $\operatorname{Cov}(\text{aX} + b,\, \text{cY} + d)$ = ac $\cdot \operatorname{Cov}(X,\, Y)$
4. If $X,\, Y$ independent $\to \operatorname{Cov}(X,\, Y) = 0$
5. $\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,\, Y)$
6. $\operatorname{Var}(X - Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) - 2\operatorname{Cov}(X,\, Y)$

### Sign Interpretation

| $\operatorname{Cov}(X,\,Y)$ | Meaning |
|-----------|---------|
| > 0 | $X$ and $Y$ tend to increase together |
| < 0 | When $X$ increases, $Y$ tends to decrease |
| = 0 | No linear relationship (uncorrelated) |

### Engineering Example: Amplifier Gain and Output

If gain $G$ varies randomly and input is fixed at $V_{\mathrm{in}}$:
- Output: $V_{\mathrm{out}} = G \cdot V_{\mathrm{in}}$
- $\operatorname{Cov}(G,\, V_{\text{out}}) = \operatorname{Cov}(G,\, G \cdot V_{\mathrm{in}}) = V_{\mathrm{in}} \cdot \operatorname{Var}(G) > 0$

Higher gain → higher output (positive covariance).

---

## 9.3 Correlation Coefficient

### Definition

$$\rho_{XY} = \frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}$$

### Properties

1. **Bounded:** $-1 \le \rho \le 1$
2. **$\rho = 1$:** Perfect positive linear relationship ($Y$ = aX $+ b,\, a > 0$)
3. **$\rho = -1$:** Perfect negative linear relationship ($Y$ = aX $+ b,\, a < 0$)
4. **$\rho = 0$:** Uncorrelated (no linear relationship)
5. **Dimensionless:** Unlike covariance, $\rho$ doesn't depend on units

### Interpretation Scale

| |$\rho$| Range | Interpretation |
|-----------|---------------|
| 0.0 – 0.2 | Very weak/negligible |
| 0.2 – 0.4 | Weak |
| 0.4 – 0.6 | Moderate |
| 0.6 – 0.8 | Strong |
| 0.8 – 1.0 | Very strong |

### Engineering Example: Correlated Fading

Two antenna paths with correlation $\rho = 0.3$:
- Low correlation → diversity works well (independent fading)
- High correlation → diversity gain is reduced

MIMO system design requires: $\rho < 0.5$ for effective spatial multiplexing.

---

## 9.4 Cross-Correlation

### Definition

$$R_{XY} = E[XY]$$

### Relationship to Covariance

$$\text{Cov}(X,Y) = R_{XY} - \mu_X \mu_Y$$

If either $X$ or $Y$ has zero mean: $\operatorname{Cov}(X,\,Y) = E[XY] = R_{XY}$

### Engineering Significance

Cross-correlation is fundamental in:
- Matched filters (correlation receivers in communications)
- Radar signal processing (detecting time delay)
- Signal detection (correlating received with known template)

---

## 9.5 Independence vs. Uncorrelatedness

### The Critical Distinction

| Statement | Always True? |
|-----------|-------------|
| Independent → Uncorrelated | ✅ YES |
| Uncorrelated → Independent | ❌ NO (in general) |
| Uncorrelated → Independent (jointly Gaussian) | ✅ YES |

### Why Independent → Uncorrelated

If $X,\, Y$ independent: $E[XY] = E[X]E[Y]$, so $\operatorname{Cov}(X,\,Y) = E[XY] - E[X]E[Y] = 0$.

### Counterexample: Uncorrelated but Dependent

Let $X \sim N(0,\,1)$ and $Y = X^{2}$. Then:
- $\operatorname{Cov}(X,\, Y) = E[XY] - E[X]E[Y] = E[X^{3}] - 0 = 0$ (since $X$ is symmetric)
- But $Y$ is completely determined by $X$ → maximally dependent!

**$\rho = 0$ does NOT mean independent** (except for jointly Gaussian RVs).

### The Gaussian Exception

For **jointly Gaussian** random variables:

$$\text{Uncorrelated} \Leftrightarrow \text{Independent}$$

This is why the Gaussian distribution is so convenient in engineering — checking $\rho = 0$ suffices to establish independence.

### Engineering Implication

In communications with Gaussian noise:
- If noise samples are uncorrelated → they are independent
- This simplifies analysis enormously
- For non-Gaussian interference: uncorrelated does NOT guarantee independence

---

## 9.6 Correlation Matrix

### Definition for Random Vector $X = [X_{1},\, X_{2},\, \ldots,\, X_{n}]^{T}$

The **covariance matrix** $\Sigma$:

$$\Sigma_{ij} = \text{Cov}(X_i, X_j)$$

$$\Sigma = \begin{bmatrix} \text{Var}(X_1) & \text{Cov}(X_1,X_2) & \cdots \\ \text{Cov}(X_2,X_1) & \text{Var}(X_2) & \cdots \\ \vdots & & \ddots \end{bmatrix}$$

The **correlation matrix** $R$ (normalized):

$$R_{ij} = \rho_{X_i X_j} = \frac{\Sigma_{ij}}{\sqrt{\Sigma_{ii}\Sigma_{jj}}}$$

### Properties of Covariance Matrix
1. Symmetric: $\Sigma = \Sigma^{T}$
2. Positive semi-definite: $x^{T}\Sigma x \ge 0$ for all $x$
3. Diagonal entries = variances
4. Off-diagonal entries = covariances

### Engineering Example: Three Sensors

Three temperature sensors with:
- $\sigma_{1} = \sigma_{2} = \sigma_{3} = 1^{\circ}C$
- $\rho_{12} = 0.8$ (sensors 1,2 are nearby)
- $\rho_{13} = 0.3$ (sensor 3 is farther)
- $\rho_{23} = 0.4$

Correlation matrix:
$$R = \begin{bmatrix} 1.0 & 0.8 & 0.3 \\ 0.8 & 1.0 & 0.4 \\ 0.3 & 0.4 & 1.0 \end{bmatrix}$$

---

## 9.7 Variance of Linear Combinations

### General Formula

$$\text{Var}\left(\sum_{i=1}^n a_i X_i\right) = \sum_{i=1}^n a_i^2 \text{Var}(X_i) + 2\sum_{i<j} a_i a_j \text{Cov}(X_i, X_j)$$

In matrix form: $\operatorname{Var}(a^{T}X) = a^{T}\Sigma a$

### Special Case: Sum of Two RVs

$\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,\,Y) = \sigma_{X}^{2} + \sigma_{Y}^{2} + 2\rho \sigma_{X}\sigma_{Y}$

| Correlation | $\operatorname{Var}(X+Y)$ | Effect |
|-------------|----------|--------|
| $\rho = 1$ | $(\sigma_{X} + \sigma_{Y})^{2}$ | Maximum — variances add constructively |
| $\rho = 0$ | $\sigma_{X}^{2} + \sigma_{Y}^{2}$ | Independent addition |
| $\rho = -1$ | $(\sigma_{X} - \sigma_{Y})^{2}$ | Minimum — cancellation |

### Engineering Application: Averaging Correlated Sensors

If $n$ sensors have equal variance $\sigma^{2}$ and pairwise correlation $\rho$:

$$\text{Var}(\bar{X}) = \frac{\sigma^2}{n}[1 + (n-1)\rho]$$

- If $\rho = 0$ (independent): $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n}$ (full averaging benefit)
- If $\rho = 1$ (identical): $\operatorname{Var}(\bar{X}) = \sigma^{2}$ (no benefit from averaging!)
- If $\rho = 0.5,\, n = 4$: $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}(1 + 3 \times 0.5)}{4} = 0.625\sigma^{2}$ (reduced benefit)

---

## 9.8 MATLAB Examples

### Example 1: Computing Correlation

```matlab
%% Sample Correlation: Correlated Sensor Data
N = 10000;
rho = 0.7;  sigma1 = 2;  sigma2 = 3;

% Generate correlated Gaussians
Z1 = randn(1, N);
Z2 = randn(1, N);
X = sigma1 * Z1;
Y = sigma2 * (rho*Z1 + sqrt(1-rho^2)*Z2);

% Compute sample statistics
cov_xy = mean(X.*Y) - mean(X)*mean(Y);
rho_hat = cov_xy / (std(X)*std(Y));

fprintf('True ρ = %.2f, Estimated ρ = %.4f\n', rho, rho_hat);
fprintf('Cov(X,Y): Theory=%.2f, Estimated=%.4f\n', rho*sigma1*sigma2, cov_xy);

% Scatter plot
figure;
scatter(X, Y, 1, 'b', '.');
xlabel('X (Sensor 1)'); ylabel('Y (Sensor 2)');
title(sprintf('Correlated Sensors (ρ = %.2f)', rho));
grid on;
```

### Example 2: Uncorrelated but Dependent

```matlab
%% Demonstration: ρ = 0 does NOT mean independent
N = 100000;
X = randn(1, N);
Y = X.^2;        % Y completely depends on X

rho_hat = corr(X', Y');
fprintf('Correlation ρ(X, X²) = %.4f ≈ 0 (UNCORRELATED)\n', rho_hat);
fprintf('But Y = X² is perfectly determined by X (DEPENDENT!)\n');

figure;
scatter(X, Y, 1, '.'); xlabel('X'); ylabel('Y = X²');
title('Uncorrelated but Dependent!');
```

### Example 3: Correlation Matrix Visualization

```matlab
%% Correlation Matrix for Multiple Sensors
n_sensors = 5;
% Create correlation matrix with decaying correlation
R = zeros(n_sensors);
for i = 1:n_sensors
    for j = 1:n_sensors
        R(i,j) = 0.9^abs(i-j);  % Correlation decays with distance
    end
end

% Generate correlated samples
L = chol(R, 'lower');   % Cholesky decomposition
N = 10000;
Z = randn(n_sensors, N);
X = L * Z;              % Correlated samples

% Estimated correlation matrix
R_hat = corrcoef(X');

figure;
subplot(1,2,1); imagesc(R); colorbar;
title('Theoretical R'); xlabel('Sensor'); ylabel('Sensor');
subplot(1,2,2); imagesc(R_hat); colorbar;
title('Estimated R'); xlabel('Sensor'); ylabel('Sensor');
```

---

## 9.9 Practice Problems

### Problem 1
$X$ and $Y$ have: $E[X] = 2,\, E[Y] = 3,\, \operatorname{Var}(X) = 4,\, \operatorname{Var}(Y) = 9,\, E[XY] = 8$.
(a) Find $\operatorname{Cov}(X,\,Y)$. (b) Find $\rho$. (c) Find $\operatorname{Var}(X+Y)$. (d) Find $\operatorname{Var}(2X-3Y)$.

**Solution:**
(a) $\operatorname{Cov}(X,\,Y) = E[XY] - E[X]E[Y] = 8 - 2(3) = 2$
(b) $\rho = \frac{2}{\sqrt{4} \cdot \sqrt{9}} = \frac{2}{6} = \frac{1}{3}$
(c) $\operatorname{Var}(X+Y) = 4 + 9 + 2(2) = 17$
(d) $\operatorname{Var}(2X-3Y) = 4(4) + 9(9) + 2(2)(-3)(2) = 16 + 81 - 24 = 73$

### Problem 2
Four identical sensors $(\sigma^{2} = 1)$ have pairwise correlation $\rho = 0.5$. What is $\operatorname{Var}(\bar{X})$?

**Solution:** $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n} \cdot [1 + (n-1)\rho] = \frac{1}{4} \cdot [1 + 3(0.5)] = 0.25 \times 2.5 = 0.625$

Compare: if independent $(\rho = 0)$: $\operatorname{Var}(\bar{X}) = 0.25$. Correlation reduces the effectiveness of averaging.

### Problem 3
Given $X \sim N(0,\,1)$, define $Y = X$ when $\lvert X\rvert < 1$ and $Y = -X$ when $\lvert X\rvert \ge 1$. Show that $\operatorname{Cov}(X,\,Y) \ne 0$ in this case but that $E[X \cdot Y]$ can be computed.

**Solution:** $E[XY] = E[X^{2} \cdot 1(\lvert X\rvert < 1)] + E[-X^{2} \cdot 1(\lvert X\rvert \ge 1)] = E[X^{2} \cdot 1(\lvert X\rvert < 1)] - E[X^{2} \cdot 1(\lvert X\rvert \ge 1)]$
$= P(\lvert X\rvert < 1)E[X^{2}\lvert \rvert X \mid < 1] - P(\lvert X\rvert \ge 1)E[X^{2}\lvert \rvert X \mid \ge 1]$
This is nonzero since $E[X^{2}\lvert \rvert X \mid < 1] \ne E[X^{2}\lvert \rvert X \mid \ge 1]$, so $\operatorname{Cov}(X,\,Y) = E[XY] - 0 \ne 0$.
$X$ and $Y$ are clearly dependent ($Y$ is defined from $X$), and in this case also correlated.

---

*Next Module: [Module 10 — Multiple Random Variables](module10-multiple-random-variables.md)*
