# Module 7 — Functions of Two Random Variables

## Learning Objectives

After completing this module, students will be able to:
1. Derive the distribution of $Z = X + Y$ using convolution
2. Find distributions of differences, products, ratios, max, and min
3. Apply the bivariate Jacobian transformation method
4. Connect these results to engineering applications (signal+noise, reliability)

---

## 7.1 Introduction

Engineering systems combine random variables:
- **Received signal** = transmitted + noise: $R = S + N$
- **Total interference** = sum of multiple sources: $I = I_{1} + I_{2} + \ldots + I_{n}$
- **System lifetime** = minimum of component lifetimes: $T = \min (T_{1},\, T_{2})$
- **SNR** $= \dfrac{\text{signal power}}{\text{noise power}}$: ratio of RVs

We need methods to find distributions of **functions of two (or more) RVs**.

---

## 7.2 Sum of Two Random Variables: $Z = X + Y$

### CDF Method

$F_{Z}(z) = P(X + Y \le z) = \int \int_{x+y \le z} f_{X,Y}(x,\,y)\,dx\,dy$

### Convolution Formula (Independent Case)

If $X$ and $Y$ are **independent**:

$$f_Z(z) = \int_{-\infty}^{\infty} f_X(x) \cdot f_Y(z-x) \, dx = (f_X * f_Y)(z)$$

This is the **convolution** of $f_{X}$ and $f_{Y}$.

### Derivation

$F_{Z}(z) = P(X + Y \le z) = \int_{-\infty}^{\infty} \int_{-\infty}^{z-x} f_{X}(x)f_{Y}(y)\,dy\,dx$
       $= \int_{-\infty}^{\infty} f_{X}(x) F_{Y}(z-x)\,dx$

Differentiating: $f_{Z}(z) = \int_{-\infty}^{\infty} f_{X}(x) f_{Y}(z-x)\,dx$ ✓

### Important Special Cases

**Sum of two Gaussians (independent):**
$X \sim N(\mu_{1},\, \sigma_{1}^{2}),\, Y \sim N(\mu_{2},\, \sigma_{2}^{2}) \to Z = X+Y \sim N(\mu_{1}+\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$

**Sum of two exponentials (same rate):**
$X,\, Y \sim \mathrm{Exp}(\lambda)$ independent $\to Z \sim \mathrm{Gamma}\left(2,\, \frac{1}{\lambda}\right) = \mathrm{Erlang}(2,\, \lambda)$

**Sum of two Poissons:**
$X \sim \mathrm{Poisson}(\lambda_{1}),\, Y \sim \mathrm{Poisson}(\lambda_{2})$ independent $\to Z \sim \mathrm{Poisson}(\lambda_{1}+\lambda_{2})$

**Sum of two uniforms:**
$X,\, Y \sim \mathrm{Uniform}(0,\,1)$ independent $\to Z$ has triangular distribution on [0,2]

### Engineering Example: Signal + Noise

Transmitted signal $S \sim N(A,\, \sigma_{s}^{2})$, noise $N \sim N(0,\, \sigma_{n}^{2})$, independent.
Received: $R = S + N \sim N(A,\, \sigma_{s}^{2} + \sigma_{n}^{2})$

The received signal is still Gaussian — with increased variance (reduced SNR).

---

## 7.3 Difference: $Z = X - Y$

For independent $X,\, Y$:
$$f_Z(z) = \int_{-\infty}^{\infty} f_X(x) \cdot f_Y(x-z) \, dx$$

**Gaussian case:** $X \sim N(\mu_{1},\, \sigma_{1}^{2}),\, Y \sim N(\mu_{2},\, \sigma_{2}^{2}) \to Z = X-Y \sim N(\mu_{1}-\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$

Note: variances still ADD (not subtract) because $\operatorname{Var}(-Y) = \operatorname{Var}(Y)$.

### Engineering Example: Differential Measurement

Two sensors measure same quantity: $X_{1} = \theta + N_{1},\, X_{2} = \theta + N_{2}$.
Difference: $X_{1} - X_{2} = N_{1} - N_{2} \sim N(0,\, \sigma_{1}^{2} + \sigma_{2}^{2})$

The difference eliminates the common signal but doubles the noise variance.

---

## 7.4 Product: $Z$ = XY

For independent $X,\, Y$:
$$f_Z(z) = \int_{-\infty}^{\infty} \frac{1}{|x|} f_X(x) \cdot f_Y(z/x) \, dx$$

### Engineering Example: Power

Voltage $V$ and current I in a linear circuit: $P = V \cdot I$.
If $V$ and I have known joint distribution, we can find the power distribution.

---

## 7.5 Ratio: $Z = \frac{X}{Y}$

For independent $X,\, Y$:
$$f_Z(z) = \int_{-\infty}^{\infty} |y| \cdot f_X(zy) \cdot f_Y(y) \, dy$$

### Engineering Example: SNR

$\mathrm{SNR} = \dfrac{\text{Signal Power}}{\text{Noise Power}} = \dfrac{P_s}{P_n}$

If both are chi-squared distributed (sum of squared Gaussians), the ratio follows an **F-distribution**.

---

## 7.6 Maximum: $Z = \max (X,\, Y)$

### CDF Approach

$F_{Z}(z) = P(\max (X,\,Y) \le z) = P(X \le z\ \text{AND}\ Y \le z)$

If independent: $F_{Z}(z) = F_{X}(z) \cdot F_{Y}(z)$

PDF: $f_{Z}(z) = f_{X}(z)F_{Y}(z) + F_{X}(z)f_{Y}(z)$

### Engineering Example: Parallel Redundancy

System works if AT LEAST ONE component works. System fails only when ALL fail.
System lifetime $= \max (T_{1},\, T_{2})$ for parallel redundant components.

For $T_{1},\, T_{2} \sim \mathrm{Exp}(\lambda)$ independent:
$F_{Z}(z) = (1 - e^{-\lambda z})^{2}$ for $z \ge 0$

$f_{Z}(z) = 2\lambda e^{-\lambda z}(1 - e^{-\lambda z})$

Mean lifetime: $E[\max] = \frac{3}{2\lambda} > \frac{1}{\lambda} = E[\text{single component}]$ (reliability improved!)

---

## 7.7 Minimum: $Z = \min (X,\, Y)$

### Survival Function Approach

$P(Z > z) = P(\min (X,\,Y) > z) = P(X > z\ \text{AND}\ Y > z)$

If independent: $P(Z > z) = P(X > z) \cdot P(Y > z) = [1-F_{X}(z)][1-F_{Y}(z)]$

CDF: $F_{Z}(z) = 1 - [1-F_{X}(z)][1-F_{Y}(z)]$

### Engineering Example: Series System

System fails when FIRST component fails. System lifetime $= \min (T_{1},\, T_{2})$.

For $T_{1} \sim \mathrm{Exp}(\lambda_{1}),\, T_{2} \sim \mathrm{Exp}(\lambda_{2})$ independent:
$P(Z > z) = e^{-\lambda_{1}z} \cdot e^{-\lambda_{2}z} = e^{-(\lambda_{1}+\lambda_{2})z}$

Therefore: $\min (T_{1},\, T_{2}) \sim \mathrm{Exp}(\lambda_{1} + \lambda_{2})$

**Key result:** Failure rates ADD in series systems. Mean lifetime $= \frac{1}{\lambda_{1}+\lambda_{2}} < \min \left(\frac{1}{\lambda_{1}},\, \frac{1}{\lambda_{2}}\right)$.

---

## 7.8 General Bivariate Transformation (Jacobian Method)

### Setup

Given $(X,\, Y)$ with known joint PDF, find the joint PDF of $(U,\, V)$ where:
- $U = g_{1}(X,\, Y)$
- $V = g_{2}(X,\, Y)$

### Procedure

1. Solve for the inverse: $X = h_{1}(U,\, V),\, Y = h_{2}(U,\, V)$
2. Compute the Jacobian determinant:

$$J = \begin{vmatrix} \frac{\partial x}{\partial u} & \frac{\partial x}{\partial v} \\ \frac{\partial y}{\partial u} & \frac{\partial y}{\partial v} \end{vmatrix}$$

3. Apply:
$$f_{U,V}(u,v) = f_{X,Y}(h_1(u,v), h_2(u,v)) \cdot |J|$$

4. If only $U$ is needed, marginalize out $V$.

### Example: Sum and Difference

$U = X + Y,\, V = X - Y \to X = \frac{U+V}{2},\, Y = \frac{U-V}{2}$

$J = \lvert \frac{\partial x}{\partial u} \cdot \frac{\partial y}{\partial v} - \frac{\partial x}{\partial v} \cdot \frac{\partial y}{\partial u}\rvert = \lvert \frac{1}{2}\left(-\frac{1}{2}\right) - \frac{1}{2}\left(\frac{1}{2}\right)\rvert = \frac{1}{2}$

$f_{U,V}(u,\,v) = \frac{1}{2} \cdot f_{X,Y}\left(\frac{u+v}{2},\, \frac{u-v}{2}\right)$

---

## 7.9 MATLAB Examples

### Example 1: Convolution — Sum of Two Uniforms

```matlab
%% Z = X + Y, both Uniform(0,1)
N = 200000;
X = rand(1, N); Y = rand(1, N);
Z = X + Y;

figure;
histogram(Z, 100, 'Normalization', 'pdf');
hold on;
z = linspace(0, 2, 200);
f_theory = max(0, 1 - abs(z - 1));  % Triangular PDF
plot(z, f_theory, 'r', 'LineWidth', 2);
xlabel('Z = X + Y'); ylabel('PDF');
title('Sum of Two Uniforms → Triangular');
legend('Simulation', 'Theory');
```

### Example 2: Sum of Gaussians (Signal + Noise)

```matlab
%% Received = Signal + Noise
mu_s = 3; sigma_s = 0.5;  % Signal
sigma_n = 1;               % Noise (zero-mean)
N = 100000;

S = mu_s + sigma_s*randn(1,N);
Noise = sigma_n*randn(1,N);
R = S + Noise;

fprintf('E[R]: Theory=%.2f, Sim=%.2f\n', mu_s, mean(R));
fprintf('Var(R): Theory=%.2f, Sim=%.2f\n', sigma_s^2+sigma_n^2, var(R));
fprintf('SNR = %.2f dB\n', 10*log10(mu_s^2/(sigma_s^2+sigma_n^2)));
```

### Example 3: Series System Reliability (Minimum)

```matlab
%% Series System: T_sys = min(T1, T2)
lambda1 = 0.001; lambda2 = 0.002;  % Failure rates per hour
N = 100000;

T1 = exprnd(1/lambda1, 1, N);
T2 = exprnd(1/lambda2, 1, N);
T_sys = min(T1, T2);

fprintf('MTTF component 1: %.0f hours\n', 1/lambda1);
fprintf('MTTF component 2: %.0f hours\n', 1/lambda2);
fprintf('MTTF system (theory): %.0f hours\n', 1/(lambda1+lambda2));
fprintf('MTTF system (sim): %.0f hours\n', mean(T_sys));
```

### Example 4: Parallel System (Maximum)

```matlab
%% Parallel System: T_sys = max(T1, T2)
lambda = 0.01; N = 100000;
T1 = exprnd(1/lambda, 1, N);
T2 = exprnd(1/lambda, 1, N);
T_parallel = max(T1, T2);

fprintf('MTTF single: %.0f hours\n', 1/lambda);
fprintf('MTTF parallel (theory): %.0f hours\n', 3/(2*lambda));
fprintf('MTTF parallel (sim): %.0f hours\n', mean(T_parallel));
```

---

## 7.10 Practice Problems

### Problem 1
$X \sim \mathrm{Exp}(1),\, Y \sim \mathrm{Exp}(1)$, independent. Find the PDF of $Z = X + Y$.

**Solution:** Convolution: $f_{Z}(z) = \int_{0}^{z} e^{-x} \cdot e^{-(z-x)}\,dx = \int_{0}^{z} e^{-z}\,dx = ze^{-z},\, z \ge 0$.
This is $\mathrm{Gamma}(2,\,1) = \mathrm{Erlang}(2,\,1)$. $E[Z] = 2,\, \operatorname{Var}(Z) = 2$.

### Problem 2
Three components in series with failure rates $\lambda_{1} = 0.001,\, \lambda_{2} = 0.002,\, \lambda_{3} = 0.003$ per hour.
(a) Find system MTTF. (b) Find $P(\text{system survives 100}\,\mathrm{hours})$.

**Solution:**
(a) $\mathrm{MTTF} = \frac{1}{0.001+0.002+0.003} = \frac{1}{0.006} = 166.7$ hours
(b) $P(T_{\text{sys}} > 100) = e^{-0.006 \times 100} = e^{-0.6} = 0.549$

### Problem 3
$X \sim N(5,\, 4),\, Y \sim N(3,\, 9)$, independent. Find distribution and $P(X + Y > 12)$.

**Solution:** $Z = X+Y \sim N(8,\, 13)$. $P(Z > 12) = P\left(Z' > \frac{12-8}{\sqrt{13}}\right) = Q(1.109) = 0.1337$.

---

*Next Module: [Module 8 — Conditional Expectation and Variance](module08-conditional-expectation.md)*
