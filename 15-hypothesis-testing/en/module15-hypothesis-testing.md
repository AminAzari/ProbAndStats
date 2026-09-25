# Module 15 — Hypothesis Testing

## Learning Objectives

After completing this module, students will be able to:
1. Formulate null and alternative hypotheses for engineering problems
2. Identify Type I and Type II errors and their engineering consequences
3. Compute test statistics and p-values
4. Perform Z-tests and t-tests for means
5. Correctly interpret p-values (and avoid common misconceptions)
6. Choose between one-sided and two-sided tests

---

## 15.1 Introduction: Making Decisions from Data

**Engineering question:** "Is this sensor biased?" "Did the process change?" "Does the new algorithm perform better?"

We need a **systematic framework** to answer such questions from data, while controlling the probability of making errors.

---

## 15.2 Framework

### Null Hypothesis $H_{0}$

The **default assumption** — typically "nothing changed" or "no effect":
- $H_{0}: \mu = \mu_{0}$ (sensor is not biased)
- $H_{0}: \mu_{1} = \mu_{2}$ (two methods perform equally)
- $H_{0}: \sigma^{2} = \sigma_{0}^{2}$ (variance hasn't changed)

### Alternative Hypothesis $H_{1}$

What we want to **detect** or prove:
- $H_{1}: \mu \ne \mu_{0}$ (sensor IS biased — two-sided)
- $H_{1}: \mu > \mu_{0}$ (performance improved — one-sided)
- $H_{1}: \sigma^{2} > \sigma_{0}^{2}$ (variance increased)

### Decision

Based on data, either:
- **Reject $H_{0}$** → evidence supports $H_{1}$
- **Fail to reject $H_{0}$** → insufficient evidence against $H_{0}$

> ⚠️ "Fail to reject $H_{0}$" ≠ "$H_{0}$ is true". Absence of evidence ≠ evidence of absence.

---

## 15.3 Types of Errors

| | $H_{0}$ True (reality) | $H_{1}$ True (reality) |
|---|---|---|
| **Reject $H_{0}$** (decision) | Type I Error $(\alpha)$ | Correct! (Power $= 1-\beta$) |
| **Fail to reject $H_{0}$** (decision) | Correct! | Type II Error $(\beta)$ |

### Type I Error (False Alarm) — probability $\alpha$

Rejecting $H_{0}$ when it's actually true. "Crying wolf."

Engineering analogy: Declaring a process has changed when it hasn't → unnecessary recalibration.

### Type II Error (Missed Detection) — probability $\beta$

Failing to reject $H_{0}$ when $H_{1}$ is actually true. "Missing the signal."

Engineering analogy: Failing to detect a sensor bias → biased measurements continue.

### Engineering Analogy: Radar Detection

| Statistical Term | Radar Equivalent |
|-----------------|------------------|
| $H_{0}$: no target | No aircraft present |
| $H_{1}$: target present | Aircraft present |
| Type I error $(\alpha)$ | False alarm |
| Type II error $(\beta)$ | Missed detection |
| Power $(1-\beta)$ | Probability of detection |

### The Tradeoff

Reducing $\alpha$ (fewer false alarms) → increases $\beta$ (more missed detections), and vice versa.

Fix $\alpha$ (significance level), then maximize power $= 1 - \beta$.

---

## 15.4 Significance Level $\alpha$

The **maximum acceptable Type I error rate**, chosen BEFORE looking at data.

Common choices: $\alpha = 0.05,\, 0.01,\, 0.10$

**Choosing $\alpha$:** Consider the cost of a false alarm:
- $\alpha = 0.01$: Very conservative (e.g., medical device safety)
- $\alpha = 0.05$: Standard in most engineering/science
- $\alpha = 0.10$: More tolerant (exploratory analysis)

---

## 15.5 Power of a Test

$$\text{Power} = 1 - \beta = P(\text{reject } H_0 | H_1 \text{ is true})$$

Power depends on:
1. **Effect size:** Larger true difference → easier to detect → more power
2. **Sample size $n$:** More data → more power
3. **Significance level $\alpha$:** Larger $\alpha$ → more power (but more false alarms)
4. **Variability $\sigma$:** Less noise → more power

---

## 15.6 The p-value

### Definition

The **p-value** is the probability of obtaining a test statistic **as extreme or more extreme** than the observed value, **assuming $H_{0}$ is true**.

$$p\text{-value} = P(\text{data this extreme or more} | H_0 \text{ true})$$

### Decision Rule

- If p-value $< \alpha$ → Reject $H_{0}$
- If p-value $\ge \alpha$ → Fail to reject $H_{0}$

### ✅ CORRECT Interpretation

> "The p-value is the probability of seeing data this extreme (or more) if $H_{0}$ were true. A small p-value means the data is unlikely under $H_{0}$."

### ❌ WRONG Interpretation

> $\sim \sim "\text{The}$ p-value is the probability that $H_{0}$ is $\mathrm{true}." \sim \sim$

**Why wrong:** $H_{0}$ is either true or false (fixed). The p-value says nothing about $P(H_{0}\ \text{true})$. It measures how surprising the data is under $H_{0}$.

### Calibration

| p-value | Evidence against $H_{0}$ |
|---------|---------------------|
| > 0.10 | Weak or none |
| 0.05 – 0.10 | Marginal |
| 0.01 – 0.05 | Moderate |
| 0.001 – 0.01 | Strong |
| < 0.001 | Very strong |

---

## 15.7 One-Sided vs. Two-Sided Tests

### Two-Sided ($H_{1}$: $\mu \ne \mu_{0}$)

Used when deviation in **either** direction is important.
- Reject if $\lvert T\rvert > t_{\alpha /2, n-1}$
- p-value $= 2 \cdot P(T > \lvert t_{\text{obs}}\rvert)$

### One-Sided ($H_{1}$: $\mu > \mu_{0}$ or $H_{1}: \mu < \mu_{0}$)

Used when only one direction of deviation matters.
- $H_{1}: \mu > \mu_{0}$: Reject if $T > t_{\alpha, n-1}$
- $H_{1}: \mu < \mu_{0}$: Reject if $T < -t_{\alpha, n-1}$

### When to Use Which

| Scenario | Test Type |
|----------|-----------|
| Is the sensor biased (either direction)? | Two-sided |
| Does the new algorithm IMPROVE BER? | One-sided ($H_{1}$: $\mathrm{BER} < \mathrm{BER}_{\mathrm{old}}$) |
| Has noise power INCREASED? | One-sided ($H_{1}$: $\sigma^{2} > \sigma_{0}^{2}$) |
| Did the process mean CHANGE? | Two-sided |

---

## 15.8 Tests for Means

### One-Sample Z-Test ($\sigma$ known)

Test: $H_{0}: \mu = \mu_{0}$

Test statistic: $Z = \frac{\bar{X} - \mu_{0}}{\frac{\sigma}{\sqrt{n}}}$

Under $H_{0}: Z \sim N(0,\,1)$

### One-Sample t-Test ($\sigma$ unknown)

Test: $H_{0}: \mu = \mu_{0}$

Test statistic: $T = \frac{\bar{X} - \mu_{0}}{\frac{S}{\sqrt{n}}}$

Under $H_{0}: T \sim t(n-1)$

### Two-Sample t-Test (comparing two means)

Test: $H_{0}: \mu_{1} = \mu_{2}$

Test statistic: $T = \frac{\bar{X}_{1} - \bar{X}_{2}}{\sqrt{\frac{S_{1}^{2}}{n_{1}} + \frac{S_{2}^{2}}{n_{2}}}}$

Approximate $df$ by Welch-Satterthwaite formula.

---

## 15.9 Test for Variance (Chi-Square Test)

### One-Sample Test

$H_{0}: \sigma^{2} = \sigma_{0}^{2}$

Test statistic: $\chi^{2} = \frac{(n-1)S^{2}}{\sigma_{0}^{2}}$

Under $H_{0}: \chi^{2} \sim \chi^{2}(n-1)$

### Decision

- $H_{1}: \sigma^{2} > \sigma_{0}^{2}$: Reject if $\chi^{2} > \chi^{2}_{\alpha, n-1}$
- $H_{1}: \sigma^{2} \ne \sigma_{0}^{2}$: Reject if $\chi^{2} < \chi^{2}_{1-\alpha /2, n-1}$ or $\chi^{2} > \chi^{2}_{\alpha /2, n-1}$

---

## 15.10 Step-by-Step Hypothesis Testing Procedure

1. **State hypotheses:** $H_{0}$ and $H_{1}$
2. **Choose significance level:** $\alpha$ (e.g., 0.05)
3. **Select test statistic:** $Z,\, t$, or $\chi^{2}$ depending on scenario
4. **Determine critical value(s)** or compute p-value
5. **Compute test statistic** from data
6. **Make decision:** Reject $H_{0}$ if test statistic in critical region (or $p < \alpha$)
7. **State conclusion** in engineering terms

---

## 15.11 Engineering Examples

### Example 1: Is a Sensor Biased?

A sensor should read 0V with no input. 20 measurements give $\bar{X} = 0.012\,\mathrm{V},\, S = 0.03\,\mathrm{V}$.

$H_{0}: \mu = 0$ (no bias), $H_{1}: \mu \ne 0$ (biased), $\alpha = 0.05$

$T = \frac{0.012 - 0}{\frac{0.03}{\sqrt{20}}} = \frac{0.012}{0.00671} = 1.789$

$t_{0.025, 19} = 2.093$. Since $\lvert T\rvert = 1.789 < 2.093$ → **Fail to reject $H_{0}$.**

p-value $= 2 \cdot P(t(19) > 1.789) \approx 0.090$. Not significant at 5% level.

Conclusion: Insufficient evidence to conclude the sensor is biased.

### Example 2: Did Manufacturing Process Change?

Before: $\mu_{0} = 100\,\Omega$ (established). After modification: $n = 30,\, \bar{X} = 101.2\,\Omega,\, S = 3.5\,\Omega$.

$H_{0}: \mu = 100,\, H_{1}: \mu \ne 100,\, \alpha = 0.05$

$T = \frac{101.2 - 100}{\frac{3.5}{\sqrt{30}}} = \frac{1.2}{0.639} = 1.878$

$t_{0.025, 29} = 2.045$. $\lvert T\rvert = 1.878 < 2.045$ → **Fail to reject $H_{0}$.**

### Example 3: Does New Algorithm Improve BER?

Old algorithm: $\mathrm{BER}_{0} = 10^{-3}$. New algorithm tested: $n = 50000$ bits, 38 errors.

$\mathrm{BER}_{\mathrm{new}} = \frac{38}{50000} = 7.6 \times 10^{-4}$

$H_{0}: p = 0.001,\, H_{1}: p < 0.001$ (improvement), $\alpha = 0.05$

$Z = \frac{\hat{p} - p_{0}}{\sqrt{\frac{p_{0}(1-p_{0})}{n}}} = \frac{0.00076 - 0.001}{\sqrt{0.001 \times \frac{0.999}{50000}}} = -\frac{0.00024}{0.000141} = -1.70$

p-value $= P(Z < -1.70) = 0.0446 < 0.05$ → **Reject $H_{0}$.**

Conclusion: Evidence supports that the new algorithm has lower BER.

### Example 4: Has Noise Power Increased?

Historical: $\sigma_{0}^{2} = 0.04\,\mathrm{V}^{2}$. New measurements: $n = 25,\, S^{2} = 0.058\,\mathrm{V}^{2}$.

$H_{0}: \sigma^{2} = 0.04,\, H_{1}: \sigma^{2} > 0.04,\, \alpha = 0.05$

$\chi^{2} = \frac{(24)(0.058)}{0.04} = 34.8$

$\chi^{2}_{0.05, 24} = 36.415$. Since $34.8 < 36.415$ → **Fail to reject $H_{0}$.**

---

## 15.12 MATLAB Examples

### Example 1: One-Sample t-test

```matlab
%% Is the sensor biased? One-sample t-test
data = [0.012, -0.005, 0.031, 0.008, -0.012, 0.022, 0.015, ...
        0.003, 0.028, -0.001, 0.018, 0.009, 0.025, -0.008, ...
        0.014, 0.007, 0.020, 0.011, -0.003, 0.019];

mu_0 = 0;  % Hypothesized mean (no bias)
[h, p_value, ci, stats] = ttest(data, mu_0);

fprintf('Test result: H = %d (1=reject H₀)\n', h);
fprintf('p-value = %.4f\n', p_value);
fprintf('t-statistic = %.3f, df = %d\n', stats.tstat, stats.df);
fprintf('95%% CI for μ: [%.4f, %.4f]\n', ci);
```

### Example 2: Two-Sample t-test

```matlab
%% Compare two algorithms: is there a significant difference?
BER_algo1 = [0.0012, 0.0008, 0.0015, 0.0010, 0.0013, ...
             0.0011, 0.0009, 0.0014, 0.0012, 0.0010];
BER_algo2 = [0.0007, 0.0009, 0.0006, 0.0008, 0.0005, ...
             0.0008, 0.0007, 0.0006, 0.0009, 0.0007];

[h, p_value, ci, stats] = ttest2(BER_algo1, BER_algo2);
fprintf('Two-sample t-test:\n');
fprintf('  H = %d (1=reject H₀: means are equal)\n', h);
fprintf('  p-value = %.4e\n', p_value);
fprintf('  t-stat = %.3f, df = %.1f\n', stats.tstat, stats.df);
fprintf('  Mean diff: %.4f\n', mean(BER_algo1) - mean(BER_algo2));
```

### Example 3: Power Analysis

```matlab
%% Power: How likely are we to detect a true difference?
mu_0 = 0;          % Null hypothesis mean
mu_true = 0.015;   % True mean (sensor has bias of 15mV)
sigma = 0.03;      % Known std dev
alpha = 0.05;

n_values = 5:5:100;
power = zeros(size(n_values));

for i = 1:length(n_values)
    n = n_values(i);
    % Critical value for two-sided test
    z_crit = norminv(1-alpha/2);
    % Power = P(reject | mu_true)
    % Reject when |Z| > z_crit, where Z ~ N((mu_true-mu_0)/(sigma/sqrt(n)), 1)
    ncp = (mu_true - mu_0) / (sigma/sqrt(n));  % Non-centrality parameter
    power(i) = 1 - normcdf(z_crit - ncp) + normcdf(-z_crit - ncp);
end

figure;
plot(n_values, power, 'b-o', 'LineWidth', 2);
xlabel('Sample Size n'); ylabel('Power (1 - β)');
title(sprintf('Power vs Sample Size (true bias = %.0f mV)', mu_true*1000));
grid on;
yline(0.8, 'r--', 'Desired Power = 0.8');
fprintf('Need n ≈ %d for 80%% power\n', n_values(find(power >= 0.8, 1)));
```

---

## 15.13 Practice Problems

### Problem 1
A manufacturing spec requires mean resistance = 100Ω. A sample of 16 resistors gives $\bar{X} = 101.5\,\Omega,\, S = 3\,\Omega$. At $\alpha = 0.05$, is there evidence the process has drifted?

**Solution:** $H_{0}: \mu = 100,\, H_{1}: \mu \ne 100$. $T = \frac{101.5-100}{\frac{3}{4}} = 2.0$. $t_{0.025,15} = 2.131$. $\lvert T\rvert = 2.0 < 2.131$ → Fail to reject. p-value $\approx 0.064 > 0.05$.

### Problem 2
Average packet delay was 5ms. After network upgrade, 40 measurements give $\bar{X} = 4.6\,\mathrm{ms},\, S = 1.2\,\mathrm{ms}$. Test if delay decreased at $\alpha = 0.01$.

**Solution:** $H_{0}: \mu = 5,\, H_{1}: \mu < 5$. $T = \frac{4.6-5}{\frac{1.2}{\sqrt{40}}} = -\frac{0.4}{0.190} = -2.11$. $t_{0.01,39} \approx -2.426$. $T = -2.11 > -2.426$ → Fail to reject at 1% level. (Would reject at 5% since $t_{0.05,39} \approx -1.685.$)

### Problem 3
Explain why "$p = 0.04$ means there is only a 4% chance $H_{0}$ is true" is WRONG.

**Solution:** The p-value is $P(\text{data this extreme}\ \mid H_{0}\ \text{true})$, NOT $P(H_{0}\ \text{true}\ \mid \text{data})$. It conditions on $H_{0}$ being true and asks about data extremity. To get $P(H_{0}\ \text{true}\ \mid \text{data})$ would require Bayesian analysis with a prior. The p-value is a property of the data-generating process under $H_{0}$, not a posterior probability about $H_{0}$.

---

*Next Module: [Module 16 — Engineering Statistical Applications](module16-engineering-applications.md)*
