# Module 13 — Parameter Estimation

## Learning Objectives

After completing this module, students will be able to:
1. Distinguish between estimator (rule) and estimate (number)
2. Evaluate estimator properties: bias, consistency, efficiency, MSE
3. Apply the Method of Moments
4. Apply Maximum Likelihood Estimation (MLE)
5. Compare estimators using MSE and bias-variance tradeoff

---

## 13.1 Introduction

We observe data $X_{1},\, \ldots,\, X_{n}$ from a distribution with unknown parameter $\theta$. **Parameter estimation** gives us a method to infer $\theta$ from data.

Engineering examples:
- Estimate noise variance $\sigma^{2}$ from recorded samples
- Estimate failure rate $\lambda$ from component lifetimes
- Estimate channel gain from received signal strength
- Estimate mean signal level from measurements

---

## 13.2 Estimator vs. Estimate

### Definitions

| Term | Type | Definition | Example |
|------|------|-----------|---------|
| **Estimator** $\hat{\theta}$ | Random variable (function of data) | The RULE applied to data | $\hat{\theta} = \bar{X} = \frac{1}{n}\sum X_{i}$ |
| **Estimate** | Number | The VALUE from a specific sample | $\hat{\theta} = 3.47$ (from one dataset) |

### Key Insight

The **estimator** is a random variable — it has a sampling distribution.
The **estimate** is one realization of that random variable.

**Analogy:** "Take the average" is the estimator (rule). "The average was 3.47" is the estimate (result).

---

## 13.3 Properties of Estimators

### Bias

$$B(\hat{\theta}) = E[\hat{\theta}] - \theta$$

- **Unbiased:** $B(\hat{\theta}) = 0$, i.e., $E[\hat{\theta}] = \theta$ (on average, correct)
- **Biased:** $E[\hat{\theta}] \ne \theta$ (systematic error)

Examples:
- $\bar{X}$ is unbiased for $\mu$: $E[\bar{X}] = \mu$ ✓
- $S^{2}$ (with $n-1$) is unbiased for $\sigma^{2}$: $E[S^{2}] = \sigma^{2}$ ✓
- $S$ (sample std dev) is biased for $\sigma$: $E[S] \ne \sigma$ (slight downward bias)

### Consistency

$\hat{\theta}_{n}$ is **consistent** if $\hat{\theta}_{n} \to \theta$ in probability as $n \to \infty$.

Practical meaning: with enough data, the estimator gets arbitrarily close to the truth.

Sufficient condition: If bias → 0 and variance → 0 as $n \to \infty$, then consistent.

### Efficiency

Among all unbiased estimators, the **efficient** one has the smallest variance.

The **Cramér-Rao Lower Bound** (CRLB) gives the minimum possible variance:
$$\text{Var}(\hat{\theta}) \geq \frac{1}{nI(\theta)}$$

where $I(\theta)$ = Fisher information $= -E\left[\frac{\partial^{2}\ln f(X; \theta)}{\partial \theta^{2}}\right]$.

An estimator achieving the CRLB is called **efficient** or **minimum variance unbiased (MVU)**.

### Mean Squared Error (MSE)

$$\text{MSE}(\hat{\theta}) = E[(\hat{\theta} - \theta)^2] = \text{Var}(\hat{\theta}) + [B(\hat{\theta})]^2$$

**$\operatorname{MSE} = \operatorname{Variance} + \operatorname{Bias}^{2}$**

This is the **bias-variance tradeoff**: sometimes a slightly biased estimator with much lower variance has smaller MSE than an unbiased one.

---

## 13.4 Method of Moments (MoM)

### Principle

Set population moments equal to sample moments, then solve for parameters.

- $k-\mathrm{th}$ population moment: $\mu 'k = E[X^{k}]$
- $k-\mathrm{th}$ sample moment: $m'k = \frac{1}{n}\sum X_{i}^{k}$

### Procedure

1. Express parameters in terms of population moments
2. Replace population moments with sample moments
3. Solve for parameter estimates

### Example: Exponential Distribution

Data from $\mathrm{Exp}(\lambda)$. Population moment: $E[X] = \frac{1}{\lambda}$.
Set equal: $\frac{1}{n}\sum X_{i} = \frac{1}{\hat{\lambda}} \to \hat{\lambda}_{\mathrm{MoM}} = \frac{1}{\bar{X}}$

### Example: Gaussian Distribution

Data from $N(\mu,\, \sigma^{2})$:
- 1st moment: $\hat{\mu} = \bar{X}$
- 2nd central moment: $\hat{\sigma}^{2} = \frac{1}{n}\sum (X_{i} - \bar{X})^{2}$ (note: biased, uses $n$ not $n-1$)

### Advantages/Disadvantages

✅ Simple, always applicable, closed-form solutions often available
❌ Not always efficient, can produce biased estimates, may give invalid parameter values

---

## 13.5 Maximum Likelihood Estimation (MLE)

### Principle

Choose the parameter value that makes the observed data **most probable**.

### Likelihood Function

Given data $x_{1},\, \ldots,\, x_{n}$ from $f(x; \theta)$:

$$L(\theta) = \prod_{i=1}^n f(x_i; \theta)$$

### Log-Likelihood (usually easier)

$$\ell(\theta) = \ln L(\theta) = \sum_{i=1}^n \ln f(x_i; \theta)$$

### MLE Procedure

1. Write the log-likelihood $\ell (\theta)$
2. Differentiate: $\frac{d\ell}{d\theta} = 0$
3. Solve for $\hat{\theta}_{\mathrm{MLE}}$
4. Verify it's a maximum $\left(\frac{d^{2}\ell}{d\theta^{2}} < 0\right)$

### Properties of MLE

1. **Consistent** (converges to true value)
2. **Asymptotically unbiased** (bias → 0 as $n \to \infty$)
3. **Asymptotically efficient** (achieves CRLB for large $n$)
4. **Invariant:** If $\hat{\theta}$ is MLE of $\theta$, then $g(\hat{\theta})$ is MLE of $g(\theta)$

### Example: MLE for Gaussian Mean

Data $X_{1},\, \ldots,\, X_{n} \sim N(\mu,\, \sigma^{2})$ with $\sigma^{2}$ known.

$\ell (\mu) = -\frac{n}{2}\ln (2\pi \sigma^{2}) - \frac{1}{2\sigma^{2}}\sum (x_{i} - \mu)^{2}$

$\frac{d\ell}{d\mu} = \frac{1}{\sigma^{2}}\sum (x_{i} - \mu) = 0 \to \hat{\mu}_{\mathrm{MLE}} = \bar{X}$

### Example: MLE for Exponential Rate

Data $X_{1},\, \ldots,\, X_{n} \sim \mathrm{Exp}(\lambda)$.

$\ell (\lambda) = n \cdot \ln (\lambda) - \lambda \cdot \sum x_{i}$

$\frac{d\ell}{d\lambda} = \frac{n}{\lambda} - \sum x_{i} = 0 \to \hat{\lambda}_{\mathrm{MLE}} = \frac{n}{\sum x_{i}} = \frac{1}{\bar{X}}$

### Example: MLE for Gaussian Variance

Data $X_{1},\, \ldots,\, X_{n} \sim N(\mu,\, \sigma^{2}),\, \mu$ known.

$\ell (\sigma^{2}) = -\frac{n}{2}\ln (2\pi \sigma^{2}) - \frac{1}{2\sigma^{2}}\sum (x_{i} - \mu)^{2}$

$\frac{d\ell}{d(\sigma^{2})} = -\frac{n}{2\sigma^{2}} + \frac{1}{2\sigma^{4}}\sum (x_{i} - \mu)^{2} = 0$

$\hat{\sigma}^{2}_{\mathrm{MLE}} = \frac{1}{n}\sum (x_{i} - \mu)^{2}$

Note: With $\mu$ unknown, $\hat{\sigma}^{2}_{\mathrm{MLE}} = \frac{1}{n}\sum (x_{i} - \bar{x})^{2}$ — biased! (Uses $n$, not $n-1$)

---

## 13.6 Engineering Examples

### Estimating Noise Variance

**Problem:** Measure $n$ samples of noise. Estimate noise power $\sigma^{2}$.

**Data:** $x_{1},\, \ldots,\, x_{n}$ (noise voltage samples, assumed zero-mean)

**MLE:** $\hat{\sigma}^{2} = \frac{1}{n}\sum x_{i}^{2}$ (biased by factor $\frac{n-1}{n}$)
**Unbiased:** $S^{2} = \frac{1}{n-1}\sum (x_{i} - \bar{x})^{2}$ or if $\mu = 0$ known: $\frac{1}{n}\sum x_{i}^{2}$ is actually unbiased for $E[X^{2}] = \sigma^{2}$

### Estimating Failure Rate

**Problem:** 20 components tested until failure. Lifetimes: $t_{1},\, \ldots,\, t_{20}$.

**Model:** $T \sim \mathrm{Exp}(\lambda)$, parameter $\lambda$ (failure rate).

**MLE:** $\hat{\lambda} = \frac{20}{\sum t_{i}} = \frac{1}{\bar{T}}$

If $\bar{T} = 500$ hours: $\hat{\lambda} = \frac{1}{500} = 0.002$ failures/hour.

### Estimating Channel Parameter

**Problem:** Rayleigh fading channel. Observed power samples: $p_{1},\, \ldots,\, p_{n}$.

**Model:** Power $P \sim \mathrm{Exp}\left(\frac{1}{\Omega}\right)$, where $\Omega = E[P]$ = average power.

**MLE:** $\hat{\Omega} = \bar{P} = \frac{1}{n}\sum p_{i}$

---

## 13.7 Comparing Estimators

### Bias-Variance Tradeoff

Two estimators for $\sigma^{2}$:
- $\hat{\theta}_{1} = S^{2}$ (unbiased, variance $= \frac{2\sigma^{4}}{n-1}$)
- $\hat{\theta}_{2} = \frac{1}{n}\sum (X_{i}-\bar{X})^{2}$ (biased by $-\frac{\sigma^{2}}{n}$, variance $= \frac{2\sigma^{4}(n-1)}{n^{2}}$)

$\operatorname{MSE}(\hat{\theta}_{1}) = \frac{2\sigma^{4}}{n-1}$
$\operatorname{MSE}(\hat{\theta}_{2}) = \frac{2\sigma^{4}(n-1)}{n^{2}} + \frac{\sigma^{4}}{n^{2}} = \frac{\sigma^{4}(2n-1)}{n^{2}}$

For any $n \ge 2$: $\operatorname{MSE}(\hat{\theta}_{2}) < \operatorname{MSE}(\hat{\theta}_{1})$! The biased MLE actually has smaller total error.

---

## 13.8 MATLAB Examples

### Example 1: MLE for Exponential Distribution

```matlab
%% MLE for Exponential: Estimate failure rate
lambda_true = 0.005;  % True failure rate
n = 30;

% Simulate data
data = exprnd(1/lambda_true, 1, n);

% MLE
lambda_hat = 1 / mean(data);
fprintf('True λ = %.4f, MLE λ̂ = %.4f\n', lambda_true, lambda_hat);

% Repeated experiments to see variability
N_exp = 10000;
lambda_estimates = zeros(1, N_exp);
for i = 1:N_exp
    d = exprnd(1/lambda_true, 1, n);
    lambda_estimates(i) = 1/mean(d);
end

fprintf('E[λ̂] = %.5f (biased: true is %.5f)\n', mean(lambda_estimates), lambda_true);
fprintf('Std(λ̂) = %.5f\n', std(lambda_estimates));

figure;
histogram(lambda_estimates, 80, 'Normalization', 'pdf');
xlabel('λ̂'); ylabel('PDF'); title('Sampling Distribution of MLE λ̂');
```

### Example 2: MLE vs Method of Moments

```matlab
%% Compare MLE and MoM for Uniform(0, θ)
% MLE: θ̂ = max(X₁,...,Xₙ)
% MoM: θ̂ = 2*X̄
theta_true = 10;  n = 20;
N_exp = 50000;

MLE_est = zeros(1, N_exp);
MoM_est = zeros(1, N_exp);

for i = 1:N_exp
    data = theta_true * rand(1, n);
    MLE_est(i) = max(data);
    MoM_est(i) = 2 * mean(data);
end

fprintf('Method      | E[θ̂]  | Bias   | Var    | MSE\n');
fprintf('MLE (max)   | %.3f | %.3f | %.3f | %.3f\n', ...
    mean(MLE_est), mean(MLE_est)-theta_true, var(MLE_est), ...
    var(MLE_est)+(mean(MLE_est)-theta_true)^2);
fprintf('MoM (2*mean)| %.3f | %.3f | %.3f | %.3f\n', ...
    mean(MoM_est), mean(MoM_est)-theta_true, var(MoM_est), ...
    var(MoM_est)+(mean(MoM_est)-theta_true)^2);
```

### Example 3: MLE for Gaussian Parameters

```matlab
%% MLE for Gaussian: Estimate both μ and σ²
mu_true = 5;  sigma2_true = 4;  n = 50;
N_exp = 10000;

mu_hat = zeros(1, N_exp);
sigma2_MLE = zeros(1, N_exp);
sigma2_unbiased = zeros(1, N_exp);

for i = 1:N_exp
    data = mu_true + sqrt(sigma2_true)*randn(1, n);
    mu_hat(i) = mean(data);
    sigma2_MLE(i) = mean((data - mean(data)).^2);       % MLE: divides by n
    sigma2_unbiased(i) = var(data);                      % Unbiased: divides by n-1
end

fprintf('μ̂: E=%.3f (true=%.1f), Var=%.4f\n', mean(mu_hat), mu_true, var(mu_hat));
fprintf('σ²_MLE: E=%.3f (true=%.1f, biased!)\n', mean(sigma2_MLE), sigma2_true);
fprintf('σ²_unbiased: E=%.3f (true=%.1f)\n', mean(sigma2_unbiased), sigma2_true);
```

---

## 13.9 Practice Problems

### Problem 1
Data from $N(\mu,\, 9)$: $\bar{x} = 4.2,\, n = 25$.
(a) Give the MLE of $\mu$. (b) What is $\operatorname{Var}(\hat{\mu})$? (c) Is $\bar{X}$ efficient for $\mu$?

**Solution:**
(a) $\hat{\mu}_{\mathrm{MLE}} = \bar{X} = 4.2$
(b) $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n} = \frac{9}{25} = 0.36$
(c) CRLB for $N(\mu,\,\sigma^{2})$: $\frac{1}{n \cdot I(\mu)} = \frac{\sigma^{2}}{n} = 0.36$. $\bar{X}$ achieves $\mathrm{CRLB}$ → yes, efficient.

### Problem 2
Component lifetimes (hours): 120, 350, 200, 480, 90, 560, 310, 175, 420, 280.
(a) Estimate $\lambda$ (failure rate) by MLE assuming $\mathrm{Exp}(\lambda)$. (b) Estimate MTTF.

**Solution:**
(a) $\bar{X} = \frac{120+350+200+480+90+560+310+175+420+280}{10} = 298.5$.
$\hat{\lambda} = \frac{1}{298.5} = 0.00335$ failures/hour.
(b) $\mathrm{MTTF} = \frac{1}{\hat{\lambda}} = \bar{X} = 298.5$ hours.

### Problem 3
For $\mathrm{Poisson}(\lambda)$ data: $x_{1},\,\ldots,\,x_{n}$. Derive the MLE of $\lambda$.

**Solution:** $\ell (\lambda) = \sum [x_{i} \ln (\lambda) - \lambda - \ln (x_{i}!)] = \left(\sum x_{i}\right)\ln (\lambda) - n\lambda - \sum \ln (x_{i}!)$
$\frac{d\ell}{d\lambda} = \frac{\sum x_{i}}{\lambda} - n = 0 \to \hat{\lambda}_{\mathrm{MLE}} = \frac{1}{n}\sum x_{i} = \bar{X}$.
The MLE for Poisson rate is the sample mean.

---

*Next Module: [Module 14 — Confidence Intervals](module14-confidence-intervals.md)*
