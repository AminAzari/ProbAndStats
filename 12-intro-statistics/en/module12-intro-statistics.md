# Module 12 — Introduction to Statistics

## Learning Objectives

After completing this module, students will be able to:
1. Distinguish between population and sample
2. Distinguish between parameter and statistic
3. Define and compute sample mean and sample variance
4. Explain sampling distributions and why they matter
5. Understand the transition from probability $(\text{model}\ \to \text{predictions})$ to statistics $(\text{data}\ \to \text{model})$

---

## 12.1 Introduction: From Probability to Statistics

### The Two Directions

| Probability | Statistics |
|------------|-----------|
| Model is KNOWN | Model is UNKNOWN |
| Predict outcomes | Infer model from data |
| Forward problem | Inverse problem |
| "Given distribution, what data?" | "Given data, what distribution?" |

**Probability** (Modules 1–11): Given a model → predict observations.
**Statistics** (Modules 12–16): Given observations → infer the model.

### Engineering Context

- **Probability:** "If noise is $N(0,\, 0.01)$, what is $P(\text{error})$?"
- **Statistics:** "I measured 1000 noise samples. What is $\sigma^{2}$?"

---

## 12.2 Population vs. Sample

### Population

The **complete** set of ALL items/measurements of interest.

| Engineering Context | Population |
|-------------------|------------|
| All chips ever produced by a fab | ALL chip delays |
| The noise process forever | ALL noise voltage values |
| All network packets ever sent | ALL packet delays |

Populations are usually **infinite or impractically large** — we cannot measure everything.

### Sample

A **subset** of the population that we actually observe/measure.

| Engineering Context | Sample |
|-------------------|--------|
| 50 chips tested from production line | Measured delays |
| 10000 noise voltage samples recorded | Recorded values |
| 500 packet delays measured | Observed delays |

### Why We Sample

- Cannot test every chip (destructive testing)
- Cannot record noise forever (finite memory)
- Cannot measure every packet (too many)

The goal: **Infer properties of the population from the sample.**

---

## 12.3 Parameter vs. Statistic

### Parameter

A **fixed** (but unknown) number that describes the population:
- $\mu$ = true population mean
- $\sigma^{2}$ = true population variance
- $p$ = true defect rate
- $\lambda$ = true failure rate

Parameters are **constants** — they have ONE true value (we just don't know it).

### Statistic

A **function of sample data** used to estimate or test parameters:
- $\bar{X}$ = sample mean (estimates $\mu$)
- $S^{2}$ = sample variance (estimates $\sigma^{2}$)
- $\hat{p}$ = sample proportion (estimates $p$)

> ⚠️ **Key insight:** Statistics are RANDOM VARIABLES! Different samples give different values.

### Example

True noise power: $\sigma^{2} = 0.04\,\mathrm{V}^{2}$ (parameter — fixed, unknown)

Experiment 1: Collect 100 samples → compute $S^{2} = 0.038$ (one realization)
Experiment 2: Collect 100 samples → compute $S^{2} = 0.042$ (different realization)
Experiment 3: Collect 100 samples → compute $S^{2} = 0.041$ (another realization)

Each $S^{2}$ is different because of randomness in sampling.

---

## 12.4 Random Sample

### Definition

A **random sample** $X_{1},\, X_{2},\, \ldots,\, X_{n}$ is a set of $n$ independent, identically distributed $(i.i.d.)$ random variables from the population distribution.

### Requirements
1. **Independent:** Each observation doesn't influence others
2. **Identically distributed:** All come from the same population
3. **Random selection:** No systematic bias in choosing which items to measure

### When i.i.d. Applies
✅ Random selection of components from a large batch
✅ Independent measurements of a stable process
✅ Noise samples separated by more than the correlation time

### When i.i.d. Fails
❌ Consecutive samples of a correlated signal
❌ Measurements during a drifting process
❌ Non-random selection (e.g., only testing "suspicious" components)

---

## 12.5 Sampling Distributions

### Definition

The **sampling distribution** of a statistic is the probability distribution of that statistic across all possible samples of size $n$.

### Why It Matters

A statistic (like $\bar{X}$) is a random variable. Its distribution tells us:
- How much will it vary from sample to sample?
- How close is it likely to be to the true parameter?
- How should we quantify our uncertainty?

This is the foundation for confidence intervals and hypothesis testing.

---

## 12.6 Sample Mean $\bar{X}$

### Definition

$$\bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$$

### Properties

**Mean of $\bar{X}$:**
$$E[\bar{X}] = \mu$$

$\bar{X}$ is an **unbiased** estimator of $\mu$ (on average, it equals the truth).

**Variance of $\bar{X}$:**
$$\text{Var}(\bar{X}) = \frac{\sigma^2}{n}$$

More measurements → less variability → more precise estimate.

**Standard Error:**
$$\text{SE}(\bar{X}) = \frac{\sigma}{\sqrt{n}}$$

### Sampling Distribution of $\bar{X}$

- If population is Gaussian: $\bar{X} \sim N\left(\mu,\, \frac{\sigma^{2}}{n}\right)$ **exactly** for any $n$
- If population is NOT Gaussian: $\bar{X} \approx N\left(\mu,\, \frac{\sigma^{2}}{n}\right)$ **approximately** for large $n$ (by CLT)

---

## 12.7 Sample Variance $S^{2}$

### Definition

$$S^2 = \frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2$$

### Why $n-1$? (Bessel's Correction)

Using $n$ in the denominator gives a **biased** estimator (systematically underestimates $\sigma^{2}$):

$E\left[\frac{1}{n} \cdot \sum (X_{i} - \bar{X})^{2}\right] = \frac{n-1}{n}\sigma^{2} \ne \sigma^{2}$

Using $n-1$ corrects this:
$$E[S^2] = \sigma^2$$

**Intuition:** $\bar{X}$ is computed from the same data, so deviations from $\bar{X}$ are smaller than deviations from $\mu$. We "lose one degree of freedom" by estimating the mean.

### Degrees of Freedom

With $n$ data points and $\bar{X}$ estimated:
- The $n$ deviations $(X_{i} - \bar{X})$ sum to zero: $\sum (X_{i} - \bar{X}) = 0$
- Only $n-1$ are "free" — the last is determined
- $df = n - 1$

### Sampling Distribution (Normal Population)

If population is $N(\mu,\, \sigma^{2})$:
$$(n-1)S^2/\sigma^2 \sim \chi^2(n-1)$$

---

## 12.8 Sample Standard Deviation

$$S = \sqrt{S^2} = \sqrt{\frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2}$$

Note: $E[S] \ne \sigma$ (S is slightly biased for $\sigma$, though $S^{2}$ is unbiased for $\sigma^{2}$).

---

## 12.9 Connection to CLT

From Module 11: $\bar{X} \to N\left(\mu,\, \frac{\sigma^{2}}{n}\right)$ as $n \to \infty$.

This means the sampling distribution of $\bar{X}$ is approximately Gaussian. This is what allows us to:
1. Build confidence intervals (Module 14)
2. Perform hypothesis tests (Module 15)
3. Quantify uncertainty in estimates

---

## 12.10 MATLAB Examples

### Example 1: Sampling Distribution of $\bar{X}$

```matlab
%% Demonstrate: X̄ is a random variable with its own distribution
% Population: Exponential(λ=1), μ=1, σ²=1
mu_pop = 1; sigma2_pop = 1;
n = 25;  % Sample size
N_experiments = 10000;

% Repeat experiment N_experiments times
X_bar = zeros(1, N_experiments);
for i = 1:N_experiments
    sample = exprnd(1, 1, n);
    X_bar(i) = mean(sample);
end

figure;
histogram(X_bar, 60, 'Normalization', 'pdf');
hold on;
x = linspace(0, 3, 200);
plot(x, normpdf(x, mu_pop, sqrt(sigma2_pop/n)), 'r', 'LineWidth', 2);
xlabel('Sample Mean'); ylabel('PDF');
title(sprintf('Sampling Distribution of X̄ (n=%d)', n));
legend('Empirical', sprintf('N(%.1f, %.4f)', mu_pop, sigma2_pop/n));

fprintf('E[X̄] = %.4f (theory: %.4f)\n', mean(X_bar), mu_pop);
fprintf('Var(X̄) = %.4f (theory: %.4f)\n', var(X_bar), sigma2_pop/n);
```

### Example 2: Sample Variance Distribution

```matlab
%% Sampling Distribution of S²
% Population: N(0, 4), σ² = 4
sigma2 = 4; n = 10;
N_experiments = 10000;

S2 = zeros(1, N_experiments);
for i = 1:N_experiments
    sample = sqrt(sigma2) * randn(1, n);
    S2(i) = var(sample);   % Uses n-1 denominator
end

fprintf('E[S²] = %.4f (theory: %.4f)\n', mean(S2), sigma2);
fprintf('Var(S²) = %.4f (theory: %.4f)\n', var(S2), 2*sigma2^2/(n-1));

figure;
histogram(S2, 80, 'Normalization', 'pdf');
hold on;
x = linspace(0, 15, 200);
% (n-1)S²/σ² ~ χ²(n-1), so S² ~ σ²/(n-1) * χ²(n-1)
plot(x, chi2pdf(x*(n-1)/sigma2, n-1)*(n-1)/sigma2, 'r', 'LineWidth', 2);
xlabel('S²'); ylabel('PDF');
title(sprintf('Sampling Distribution of S² (n=%d, σ²=%d)', n, sigma2));
legend('Empirical', 'Theoretical (scaled χ²)');
```

### Example 3: Effect of Sample Size

```matlab
%% Precision improves with n: Var(X̄) = σ²/n
sigma = 2;
n_values = [5, 10, 25, 50, 100, 500];
N_exp = 10000;

fprintf('n\t| SE(X̄) Theory\t| SE(X̄) Sim\n');
fprintf('--------+---------------+-----------\n');
for n = n_values
    X_bar = mean(sigma*randn(n, N_exp), 1);
    fprintf('%d\t| %.4f\t| %.4f\n', n, sigma/sqrt(n), std(X_bar));
end
```

---

## 12.11 Practice Problems

### Problem 1
A population has $\mu = 50$ and $\sigma = 10$. A random sample of $n = 64$ is taken.
(a) What is $E[\bar{X}]$? (b) What is $\operatorname{Var}(\bar{X})$? (c) What is $P(48 < \bar{X} < 52)$?

**Solution:**
(a) $E[\bar{X}] = 50$
(b) $\operatorname{Var}(\bar{X}) = \frac{100}{64} = 1.5625,\, \operatorname{SE} = 1.25$
(c) $P(48 < \bar{X} < 52) = P(-1.6 < Z < 1.6) = 2\Phi (1.6) - 1 = 0.8904$

### Problem 2
Why $do$ we use $n-1$ in the sample variance formula instead of $n$? Explain both mathematically and intuitively.

**Solution:** Mathematically: $E\left[\frac{1}{n}\sum (X_{i}-\bar{X})^{2}\right] = \frac{n-1}{n} \cdot \sigma^{2}$, so dividing by $n-1$ gives $E[S^{2}] = \sigma^{2}$. Intuitively: we use $\bar{X}$ from the same data (not the true $\mu$), so deviations from $\bar{X}$ are artificially small. One degree of freedom is "used up" estimating the mean.

### Problem 3
An engineer measures noise power from 20 samples and gets $S^{2} = 0.045\,\mathrm{V}^{2}$. The true variance is $\sigma^{2} = 0.04\,\mathrm{V}^{2}$. Is this concerning? Use the sampling distribution to assess.

**Solution:** Under $H_{0}: \sigma^{2} = 0.04,\, \frac{(n-1)S^{2}}{\sigma^{2}} \sim \chi^{2}(19)$. Observed: $\frac{19(0.045)}{0.04} = 21.375$. $P(\chi^{2}(19) > 21.375) \approx 0.31$. This is not unusual — about 31% of samples would give $S^{2} \ge 0.045$ when $\sigma^{2} = 0.04$. Not concerning.

---

*Next Module: [Module 13 — Parameter Estimation](module13-parameter-estimation.md)*
