# Module 14 — Confidence Intervals

## Learning Objectives

After completing this module, students will be able to:
1. Correctly interpret confidence intervals (frequentist interpretation)
2. Construct CIs for means (known and unknown variance)
3. Apply the t-distribution for small samples
4. Construct CIs for variance
5. Determine required sample sizes
6. Avoid common misconceptions about CIs

---

## 14.1 Introduction: Why Point Estimates Aren't Enough

A point estimate (e.g., $\bar{X} = 3.47$) gives no information about **precision**. We need to quantify uncertainty:

"The true mean is estimated to be 3.47, **give or take** some amount."

A **confidence interval** provides a range that, with a specified confidence level, captures the true parameter.

---

## 14.2 Correct Interpretation of Confidence Intervals

### ✅ CORRECT (Frequentist) Interpretation

> "If we repeat this experiment many times, each time computing a 95% CI, then approximately 95% of those intervals will contain the true parameter $\mu$."

The **procedure** has a 95% success rate. Each specific interval either contains $\mu$ or it doesn't.

### ❌ WRONG Interpretation

> $\sim \sim "\mathrm{There}$ is a 95% probability that $\mu$ is inside this particular interval $[2.1,\, 4.8]." \sim \sim$

**Why this is wrong:** $\mu$ is a fixed (non-random) number. It's either in [2.1, 4.8] or not — there is no probability about it. The **interval** is random (varies from sample to sample), not the parameter.

### Analogy

Think of CIs like a fishing net:
- Before you cast: 95% chance your net will catch the fish (the procedure works 95% of the time)
- After you cast: the fish is either caught or not — no probability anymore
- The fish $(\mu)$ doesn't move; your net $(\mathrm{CI})$ does

---

## 14.3 CI for Mean — Known Variance (Z-interval)

### Setup

Data: $X_{1},\, \ldots,\, X_{n}$ i.i.d. from $N(\mu,\, \sigma^{2})$ with $\sigma^{2}$ **known**.

### Derivation

$\bar{X} \sim N\left(\mu,\, \frac{\sigma^{2}}{n}\right)$, so $Z = \frac{\bar{X} - \mu}{\frac{\sigma}{\sqrt{n}}} \sim N(0,\,1)$

$P(-z_{\alpha /2} \le Z \le z_{\alpha /2}) = 1 - \alpha$

Solving for $\mu$:
$$P\left(\bar{X} - z_{\alpha/2}\frac{\sigma}{\sqrt{n}} \leq \mu \leq \bar{X} + z_{\alpha/2}\frac{\sigma}{\sqrt{n}}\right) = 1 - \alpha$$

### Formula

$$\text{CI: } \bar{X} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$$

### Common z-values

| Confidence Level | $\alpha$ | $z_{\alpha /2}$ |
|-----------------|---|---------|
| 90% | 0.10 | 1.645 |
| 95% | 0.05 | 1.960 |
| 99% | 0.01 | 2.576 |

### Example

Noise measurements: $n = 36,\, \bar{X} = 0.52\,\mathrm{V},\, \sigma = 0.12\,\mathrm{V}$ (known from equipment spec).

95% CI: $0.52 \pm 1.96 \times \frac{0.12}{\sqrt{36}} = 0.52 \pm 0.0392 = \ast \ast [0.481,\, 0.559] V$**

---

## 14.4 CI for Mean — Unknown Variance (t-interval)

### The Problem

Usually $\sigma$ is unknown. We estimate it with $S$. But $\frac{\bar{X}}{\frac{S}{\sqrt{n}}}$ does NOT follow $N(0,\,1)$.

### Student's t-Distribution

$$T = \frac{\bar{X} - \mu}{S/\sqrt{n}} \sim t(n-1)$$

The t-distribution:
- Symmetric about 0
- Heavier tails than $N(0,\,1)$ (more uncertainty because $S$ is estimated)
- Degrees of freedom: $\nu = n - 1$
- As $\nu \to \infty$: $t(\nu) \to N(0,\,1)$
- For $\nu \ge 30$: $t \approx z$ (practically)

### Formula

$$\text{CI: } \bar{X} \pm t_{\alpha/2, n-1} \cdot \frac{S}{\sqrt{n}}$$

### Example

Component delay: $n = 15,\, \bar{X} = 2.34$ ns, $S = 0.45$ ns.

95% CI: $t_{0.025, 14} = 2.145$

CI: $2.34 \pm 2.145 \times \frac{0.45}{\sqrt{15}} = 2.34 \pm 0.249 = \ast \ast [2.09,\, 2.59]$ ns**

(Wider than z-interval because of additional uncertainty in S.)

---

## 14.5 CI for Variance

### Setup

Data from $N(\mu,\, \sigma^{2})$. Want CI for $\sigma^{2}$.

### Key Distribution

$$(n-1)S^2/\sigma^2 \sim \chi^2(n-1)$$

### Formula

$$\left[\frac{(n-1)S^2}{\chi^2_{\alpha/2, n-1}}, \quad \frac{(n-1)S^2}{\chi^2_{1-\alpha/2, n-1}}\right]$$

### Example

Noise power: $n = 20,\, S^{2} = 0.045\,\mathrm{V}^{2}$.

95% CI for $\sigma^{2}$: $\chi^{2}_{0.025, 19} = 32.852,\, \chi^{2}_{0.975, 19} = 8.907$

CI: $\left[\frac{(19)(0.045)}{32.852},\, \frac{(19)(0.045)}{8.907}\right] = [0.026,\, 0.096] V^{2}$

95% CI for $\sigma$: $\left[\sqrt{0.026},\, \sqrt{0.096}\right] = [0.161,\, 0.310] V$

---

## 14.6 CI for Proportions

### Setup

$n$ trials, $\hat{x} = \frac{k}{n}$ successes. Estimate $p$ = true proportion.

### Large-Sample Formula (Wald Interval)

$$\hat{p} \pm z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

### Example: Bit Error Rate

Transmitted 100,000 bits, observed 53 errors. $\hat{p} = \frac{53}{100000} = 5.3 \times 10^{-4}$.

95% CI: $5.3 \times 10^{-4} \pm 1.96\sqrt{5.3 \times 10^{-4} \times \frac{0.9995}{100000}} = 5.3 \times 10^{-4} \pm 1.43 \times 10^{-4}$

CI: **$[3.9 \times 10^{-4},\, 6.7 \times 10^{-4}]$**

---

## 14.7 Factors Affecting CI Width

| Factor | Effect on Width | Explanation |
|--------|----------------|-------------|
| ↑ Sample size $n$ | Narrower | More data → more precision |
| ↑ Confidence level | Wider | More confidence → wider net |
| ↑ Variability $\sigma$ | Wider | More noise → harder to pin down $\mu$ |

### The Tradeoff

- Want narrow CI (precise) → need more data or accept lower confidence
- Want high confidence → accept wider CI or collect more data
- Want both narrow AND high confidence → need LOTS of data

---

## 14.8 Sample Size Determination

### Question: How many measurements $do$ I need?

For CI width $\le 2E$ (margin of error $E$):

$$n \geq \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2$$

### Example

Want to estimate mean delay within $\pm 0.5\,\mathrm{ms}$ with 95% confidence. Expect $\sigma \approx 3\,\mathrm{ms}$.

$n \ge \left(1.96 \times \frac{3}{0.5}\right)^{2} = (11.76)^{2} = 138.3$ → need **$n = 139$ measurements**

---

## 14.9 MATLAB Demonstrations

### Example 1: CI Coverage Demonstration

```matlab
%% Demonstrate: ~95% of 95% CIs contain the true mean
mu_true = 5;  sigma = 2;  n = 25;
N_experiments = 1000;
alpha = 0.05;
z_crit = norminv(1-alpha/2);

covers = 0;
figure; hold on;
for i = 1:N_experiments
    data = mu_true + sigma*randn(1, n);
    x_bar = mean(data);
    margin = z_crit * sigma / sqrt(n);
    ci_low = x_bar - margin;
    ci_high = x_bar + margin;
    
    if ci_low <= mu_true && mu_true <= ci_high
        covers = covers + 1;
        color = 'b';
    else
        color = 'r';
    end
    
    if i <= 100  % Plot first 100
        plot([ci_low ci_high], [i i], color, 'LineWidth', 0.5);
    end
end
plot([mu_true mu_true], [0 101], 'k--', 'LineWidth', 2);
xlabel('μ'); ylabel('Experiment #');
title(sprintf('95%% CI Coverage: %.1f%% of %d intervals contain μ', ...
    100*covers/N_experiments, N_experiments));
fprintf('Coverage: %.1f%% (theory: 95%%)\n', 100*covers/N_experiments);
```

### Example 2: t-interval in Practice

```matlab
%% t-Confidence Interval: Unknown Variance
data = [2.3, 2.7, 2.1, 2.8, 2.5, 2.9, 2.4, 2.6, 2.2, 2.7];
n = length(data);
x_bar = mean(data);
s = std(data);
alpha = 0.05;

t_crit = tinv(1-alpha/2, n-1);
margin = t_crit * s / sqrt(n);

fprintf('Sample mean: %.3f\n', x_bar);
fprintf('Sample std:  %.3f\n', s);
fprintf('t_crit (df=%d): %.3f\n', n-1, t_crit);
fprintf('95%% CI: [%.3f, %.3f]\n', x_bar-margin, x_bar+margin);
```

### Example 3: Sample Size Calculation

```matlab
%% How many samples do we need?
sigma = 0.1;           % Expected std dev (from pilot study)
E_values = [0.05, 0.02, 0.01, 0.005];  % Desired margins
confidence = 0.95;
z = norminv(1 - (1-confidence)/2);

fprintf('Desired Margin | Required n\n');
fprintf('--------------+-----------\n');
for E = E_values
    n_req = ceil((z*sigma/E)^2);
    fprintf('  ±%.3f      |    %d\n', E, n_req);
end
```

---

## 14.10 Practice Problems

### Problem 1
A sensor measures voltage. From 50 measurements: $\bar{X} = 3.32\,\mathrm{V},\, S = 0.15\,\mathrm{V}$.
(a) Construct 95% CI for $\mu$. (b) Construct 99% CI for $\mu$. (c) Which is wider and why?

**Solution:**
(a) $t_{0.025,49} \approx 2.010$. CI: $3.32 \pm 2.010\left(\frac{0.15}{\sqrt{50}}\right) = 3.32 \pm 0.043 = [3.277,\, 3.363]$
(b) $t_{0.005,49} \approx 2.680$. CI: $3.32 \pm 2.680\left(\frac{0.15}{\sqrt{50}}\right) = 3.32 \pm 0.057 = [3.263,\, 3.377]$
(c) 99% CI is wider — higher confidence requires a wider interval.

### Problem 2
From 25 noise power measurements: $S^{2} = 0.048\,\mathrm{V}^{2}$. Construct 95% CI for $\sigma^{2}$.

**Solution:** $\chi^{2}_{0.025,24} = 39.364,\, \chi^{2}_{0.975,24} = 12.401$
CI: $\left[\frac{24(0.048)}{39.364},\, \frac{24(0.048)}{12.401}\right] = [0.029,\, 0.093] V^{2}$

### Problem 3
How many BER measurements needed to estimate BER within $\pm 20\%$ of its true value with 95% confidence, if expected $\mathrm{BER} \approx 10^{-3}$?

**Solution:** For proportion: $n \ge \frac{z^{2}p(1-p)}{E^{2}}$ where $E = 0.2p = 2 \times 10^{-4}$.
$n \ge \frac{(1.96)^{2}(10^{-3})(0.999)}{(2 \times 10^{-4})^{2}} = 3.84 \times \frac{10^{-3}}{4 \times 10^{-8}} \approx 96{,}000$ bits.
Need to transmit approximately 96,000 bits (or observe $\sim 96$ errors).

---

*Next Module: [Module 15 — Hypothesis Testing](module15-hypothesis-testing.md)*
