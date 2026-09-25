# Module 3 — Important Probability Distributions

## Learning Objectives

After completing this module, students will be able to:

1. Identify which probability distribution models a given engineering scenario
2. State the $\frac{\mathrm{PMF}}{\mathrm{PDF}}$, CDF, mean, and variance for all major distributions
3. Explain the physical interpretation and engineering context for each distribution
4. Determine when to use (and when NOT to use) each distribution
5. Implement distributions in MATLAB for simulation
6. Recognize distribution patterns from data characteristics

---

## 3.1 Introduction: Choosing the Right Distribution

### The Engineering Challenge

When modeling a random phenomenon, the critical question is:

> "Which probability distribution best describes this uncertain quantity?"

Choosing the wrong distribution leads to incorrect predictions, improper designs, and system failures.

### How to Choose

The distribution should match the **physical mechanism** generating randomness:
- Counting successes → Binomial
- Waiting for events → Exponential/Geometric
- Sum of many small effects → Gaussian
- Signal envelope in fading → Rayleigh

This module provides the tools to make this choice correctly.

---

## 3.2 Bernoulli Distribution

### Physical Interpretation

Models a **single trial** with two possible outcomes: "success" (1) or "failure" (0).

### Engineering Context

Any binary random outcome:
- One bit transmitted: error or correct
- One component tested: pass or fail
- One packet sent: delivered or lost
- One detection attempt: target present or absent

### Mathematical Definition

$X \sim \mathrm{Bernoulli}(p)$, where $p$ = probability of success.

### PMF

$$p_X(x) = \begin{cases} p & x = 1 \\ 1-p = q & x = 0 \end{cases}$$

Compact form: $p_{X}(x) = p^{x}(1-p)^{1-x}$ for $x \in \{0,\, 1\}$

### CDF

$$F_X(x) = \begin{cases} 0 & x < 0 \\ 1-p & 0 \leq x < 1 \\ 1 & x \geq 1 \end{cases}$$

### Mean

$$E[X] = p$$

### Variance

$$\text{Var}(X) = p(1-p)$$

Maximum variance at $p = 0.5$ (maximum uncertainty).

### When to Use

✅ Single binary trial with fixed probability
✅ Building block for more complex distributions
✅ Modeling $\frac{ON}{\mathrm{OFF}}$ states, pass/fail outcomes

### When NOT to Use

❌ Multiple trials (use Binomial)
❌ Outcome is not binary
❌ Probability changes between trials

### MATLAB Implementation

```matlab
% Generate Bernoulli random variables
p = 0.1;          % Bit error probability
N = 10000;
X = binornd(1, p, 1, N);  % or: X = rand(1,N) < p;

% Verify
fprintf('Mean: %.4f (theory: %.4f)\n', mean(X), p);
fprintf('Var:  %.4f (theory: %.4f)\n', var(X), p*(1-p));
```

---

## 3.3 Binomial Distribution

### Physical Interpretation

Models the **number of successes** in $n$ **independent** trials, each with the same success probability $p$.

### Engineering Context

- Number of bit errors in $n$ transmitted bits
- Number of defective components in a batch of $n$
- Number of successful packet transmissions out of $n$ attempts
- Number of sensors that detect a signal (out of $n$ sensors)

### Mathematical Definition

$X \sim \mathrm{Binomial}(n,\, p)$

### PMF

$$p_X(k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k = 0, 1, 2, \ldots, n$$

where $C(n,\,k) = \frac{n!}{k!(n-k)!}$

### CDF

$$F_X(k) = \sum_{i=0}^{\lfloor k \rfloor} \binom{n}{i} p^i (1-p)^{n-i}$$

No closed-form expression — use tables or MATLAB.

### Mean

$$E[X] = np$$

### Variance

$$\text{Var}(X) = np(1-p)$$

### When to Use

✅ Fixed number of trials $n$
✅ Each trial is independent
✅ Each trial has the same probability $p$
✅ Counting the number of "successes"

### When NOT to Use

❌ Trials are not independent (correlated failures)
❌ Probability changes between trials
❌ $n$ is not fixed (use Negative Binomial or Poisson)
❌ Very large $n$ with small $p$ (use Poisson approximation)

### MATLAB Implementation

```matlab
%% Binomial Distribution: Bit Errors in a Frame
n = 100;         % bits per frame
p = 0.05;        % bit error probability
k = 0:20;        % range of interest

% PMF and CDF
pmf = binopdf(k, n, p);
cdf_vals = binocdf(k, n, p);

% Plot PMF
figure;
stem(k, pmf, 'filled', 'LineWidth', 1.5);
xlabel('Number of Errors (k)');
ylabel('P(X = k)');
title(sprintf('Binomial PMF: n=%d, p=%.2f', n, p));
grid on;

% Simulation verification
N_sim = 100000;
X_sim = binornd(n, p, 1, N_sim);
fprintf('E[X]: Theory = %.2f, Simulation = %.2f\n', n*p, mean(X_sim));
fprintf('Var(X): Theory = %.2f, Simulation = %.2f\n', n*p*(1-p), var(X_sim));
```

### Simulation Example: Packet Error Rate

```matlab
%% Engineering Scenario: How many packets out of 50 have errors?
% Each packet has 1000 bits, BER = 1e-4
% A packet has an error if ANY bit is in error.

BER = 1e-4;
bits_per_packet = 1000;
n_packets = 50;

% P(packet error) = 1 - P(all bits correct)
p_packet_error = 1 - (1 - BER)^bits_per_packet;
fprintf('P(packet error) = %.4f\n', p_packet_error);

% Number of errored packets ~ Binomial(50, p_packet_error)
k = 0:10;
pmf_packets = binopdf(k, n_packets, p_packet_error);
fprintf('P(0 packet errors) = %.4f\n', pmf_packets(1));
fprintf('P(≤ 2 packet errors) = %.4f\n', sum(pmf_packets(1:3)));
```

---

## 3.4 Geometric Distribution

### Physical Interpretation

Models the **number of trials until the first success** (or equivalently, the number of failures before the first success).

### Engineering Context

- Number of transmissions until a packet is successfully delivered
- Number of components tested until finding a defective one
- Number of login attempts until successful authentication
- Number of time slots until channel becomes available

### Mathematical Definition

$X \sim \mathrm{Geometric}(p)$

Convention: $X$ = number of the trial on which first success occurs $(X \in \{1,\, 2,\, 3,\, \ldots \})$.

### PMF

$$p_X(k) = (1-p)^{k-1} p, \quad k = 1, 2, 3, \ldots$$

($k-1$ failures, then one success)

### CDF

$$F_X(k) = 1 - (1-p)^k, \quad k = 1, 2, 3, \ldots$$

### Mean

$$E[X] = \frac{1}{p}$$

### Variance

$$\text{Var}(X) = \frac{1-p}{p^2}$$

### Memoryless Property

The geometric distribution is the ONLY discrete distribution with the memoryless property:

$$P(X > m + n \mid X > m) = P(X > n)$$

**Engineering meaning:** If a system hasn't succeeded in $m$ attempts, the probability of needing $n$ more attempts is the same as starting fresh. "Past failures don't affect future probability."

### When to Use

✅ Repeated independent trials until first success
✅ Each trial has the same probability
✅ Memoryless assumption is appropriate (e.g., independent retransmissions)

### When NOT to Use

❌ Success probability changes over time (aging, learning)
❌ Trials are not independent
❌ Looking for the $r-\mathrm{th}$ success (use Negative Binomial)

### MATLAB Implementation

```matlab
%% Geometric Distribution: Retransmissions Until Success
p_success = 0.7;   % P(successful transmission)
k = 1:15;

% PMF
pmf_geom = geopdf(k-1, p_success);  % MATLAB counts failures (k-1)

% Simulation
N = 100000;
X_sim = geornd(p_success, 1, N) + 1;  % +1 to count trial number

figure;
subplot(1,2,1);
stem(k, pmf_geom, 'filled');
xlabel('Trial Number k'); ylabel('P(X = k)');
title(sprintf('Geometric PMF (p = %.1f)', p_success));
grid on;

subplot(1,2,2);
histogram(X_sim, 'Normalization', 'probability', 'BinMethod', 'integers');
xlabel('Trial Number k'); ylabel('Relative Frequency');
title('Simulation Histogram');
grid on;

fprintf('E[X]: Theory = %.2f, Simulation = %.2f\n', 1/p_success, mean(X_sim));
```



---

## 3.5 Negative Binomial Distribution

### Physical Interpretation

Models the **number of trials until the $r-\mathrm{th}$ success** (generalization of Geometric).

### Engineering Context

- Number of transmissions until $r$ packets are successfully delivered
- Number of components inspected until finding $r$ defectives
- Number of time slots until $r$ channels become available

### Mathematical Definition

$X \sim \mathrm{NegBin}(r,\, p)$ = number of trials until $r-\mathrm{th}$ success.

### PMF

$$p_X(k) = \binom{k-1}{r-1} p^r (1-p)^{k-r}, \quad k = r, r+1, r+2, \ldots$$

(Choose which $r-1$ of the first $k-1$ trials were successes, the $k-\mathrm{th}$ is the $r-\mathrm{th}$ success.)

### CDF

No simple closed form. Computed by summation or MATLAB.

### Mean

$$E[X] = \frac{r}{p}$$

### Variance

$$\text{Var}(X) = \frac{r(1-p)}{p^2}$$

### Relationship to Geometric

When $r = 1$, the Negative Binomial reduces to the Geometric distribution.

### When to Use

✅ Repeated independent trials until $r-\mathrm{th}$ success
✅ Constant probability per trial
✅ Generalizing geometric to multiple required successes

### When NOT to Use

❌ Only need first success (use Geometric — simpler)
❌ Trials not independent
❌ Fixed number of trials (use Binomial)

### MATLAB Implementation

```matlab
%% Negative Binomial: Transmissions Until r Successes
r = 5;             % Need 5 successful deliveries
p_success = 0.8;   % P(success per trial)

% MATLAB's nbinpdf counts FAILURES before r-th success
% X_matlab = number of failures, so total trials = X_matlab + r
k_failures = 0:20;

pmf_nb = nbinpdf(k_failures, r, p_success);

figure;
stem(k_failures + r, pmf_nb, 'filled');
xlabel('Total Trials Until 5th Success');
ylabel('Probability');
title(sprintf('Negative Binomial: r=%d, p=%.1f', r, p_success));
grid on;

% Simulation
N = 100000;
X_sim = nbinrnd(r, p_success, 1, N) + r;  % total trials
fprintf('E[trials]: Theory = %.2f, Simulation = %.2f\n', r/p_success, mean(X_sim));
```

---

## 3.6 Poisson Distribution

### Physical Interpretation

Models the **number of events occurring in a fixed interval** (time, space, area) when events occur independently at a constant average rate.

### Engineering Context

- Number of photons hitting a detector in $1 \mu s$
- Number of packets arriving at a router in 1 second
- Number of defects per $cm^{2}$ on a silicon wafer
- Number of cosmic ray events in a sensor per hour
- Number of interference spikes in a measurement window

### Mathematical Definition

$X \sim \mathrm{Poisson}(\lambda)$, where $\lambda$ = average number of events per interval.

### PMF

$$p_X(k) = \frac{\lambda^k e^{-\lambda}}{k!}, \quad k = 0, 1, 2, \ldots$$

### CDF

$$F_X(k) = e^{-\lambda} \sum_{i=0}^{\lfloor k \rfloor} \frac{\lambda^i}{i!}$$

### Mean

$$E[X] = \lambda$$

### Variance

$$\text{Var}(X) = \lambda$$

**Unique property:** Mean equals variance! This is a quick check — if sample mean ≈ sample variance, data may be Poisson.

### Poisson as Binomial Approximation

When $n$ is large and $p$ is small, with $\lambda$ = np:

$$\text{Binomial}(n, p) \approx \text{Poisson}(\lambda = np)$$

Rule of thumb: $n \ge 20$ and $p \le 0.05$.

### When to Use

✅ Counting events in a fixed interval
✅ Events occur independently
✅ Events occur at a constant average rate
✅ Rare events with many opportunities (large $n$, small $p$)
✅ Mean ≈ Variance in data

### When NOT to Use

❌ Events are not independent (clustered arrivals)
❌ Rate varies with time (non-homogeneous process)
❌ Events are not rare ($p$ not small) — use Binomial
❌ Mean ≠ Variance (look at overdispersion/underdispersion)

### MATLAB Implementation

```matlab
%% Poisson Distribution: Network Packet Arrivals
lambda = 4.5;     % Average packets per millisecond
k = 0:15;

pmf_pois = poisspdf(k, lambda);
cdf_pois = poisscdf(k, lambda);

figure;
subplot(1,2,1);
stem(k, pmf_pois, 'filled', 'LineWidth', 1.5);
xlabel('Number of Arrivals (k)');
ylabel('P(X = k)');
title(sprintf('Poisson PMF (λ = %.1f)', lambda));
grid on;

subplot(1,2,2);
stairs(k, cdf_pois, 'LineWidth', 2);
xlabel('k'); ylabel('P(X ≤ k)');
title('Poisson CDF');
grid on;
```

### Simulation Example: Photon Counting

```matlab
%% Engineering Scenario: Photon Detection
% An optical receiver detects photons. Average rate: 10 photons/μs.
% What is P(detecting fewer than 5 photons in one μs)?

lambda = 10;   % average photons per μs
N = 100000;

% Analytical
p_fewer_5 = poisscdf(4, lambda);
fprintf('P(X < 5): Analytical = %.6f\n', p_fewer_5);

% Simulation
X_sim = poissrnd(lambda, 1, N);
p_fewer_5_sim = mean(X_sim < 5);
fprintf('P(X < 5): Simulation = %.6f\n', p_fewer_5_sim);

% Verify mean = variance (Poisson signature)
fprintf('Sample Mean = %.3f, Sample Variance = %.3f\n', mean(X_sim), var(X_sim));
```



---

## 3.7 Uniform Distribution (Continuous)

### Physical Interpretation

Models a quantity that is **equally likely** to take any value in an interval $[a,\, b]$. Represents **maximum uncertainty** within a known range.

### Engineering Context

- Phase of a received signal (unknown, equally likely in $[0,\, 2\pi)$)
- Round-off error in an ADC (uniform in $\left[-\frac{\Delta}{2},\, \frac{\Delta}{2}\right]$ where $\Delta$ = quantization step)
- Arrival time within a time slot (unknown position)
- Random access protocol: choose a random backoff time

### Mathematical Definition

$X \sim \mathrm{Uniform}(a,\, b)$ or $X \sim U(a,\, b)$

### PDF

$$f_X(x) = \begin{cases} \frac{1}{b-a} & a \leq x \leq b \\ 0 & \text{otherwise} \end{cases}$$

### CDF

$$F_X(x) = \begin{cases} 0 & x < a \\ \frac{x-a}{b-a} & a \leq x \leq b \\ 1 & x > b \end{cases}$$

### Mean

$$E[X] = \frac{a+b}{2}$$

### Variance

$$\text{Var}(X) = \frac{(b-a)^2}{12}$$

### When to Use

✅ All values in a range are equally likely
✅ No information favors any particular value
✅ Quantization error modeling
✅ Phase of unmodulated carrier

### When NOT to Use

❌ Values near the center are more likely (use Gaussian)
❌ Values are non-negative with exponential decay (use Exponential)
❌ Range is unbounded

### MATLAB Implementation

```matlab
%% Uniform Distribution: Quantization Error
a = -0.5;  b = 0.5;  % Quantization step = 1 LSB
N = 100000;

X = a + (b-a)*rand(1, N);  % or: unifrnd(a, b, 1, N)

figure;
histogram(X, 50, 'Normalization', 'pdf');
hold on;
plot([a a b b], [0 1/(b-a) 1/(b-a) 0], 'r-', 'LineWidth', 2);
xlabel('Quantization Error'); ylabel('f_X(x)');
title('Uniform PDF: Quantization Error');
legend('Simulated', 'Theoretical');

fprintf('Mean: Theory=%.3f, Sim=%.3f\n', (a+b)/2, mean(X));
fprintf('Var:  Theory=%.4f, Sim=%.4f\n', (b-a)^2/12, var(X));
```

---

## 3.8 Exponential Distribution

### Physical Interpretation

Models the **time between events** in a Poisson process, or the **lifetime** of a memoryless component.

### Engineering Context

- Time between packet arrivals
- Time until component failure (constant failure rate)
- Time between cosmic ray events in a detector
- Inter-arrival time of calls at a switch
- Duration of a fading dip in a wireless channel

### Mathematical Definition

$X \sim \mathrm{Exponential}(\lambda)$ where $\lambda$ = rate parameter (events per unit time).

Alternative parameterization: $X \sim \mathrm{Exp}(\beta)$ where $\beta = \frac{1}{\lambda}$ = mean.

### PDF

$$f_X(x) = \lambda e^{-\lambda x}, \quad x \geq 0$$

Or with mean $\beta$: $f_{X}(x) = \frac{1}{\beta}e^{-x/\beta}$

### CDF

$$F_X(x) = 1 - e^{-\lambda x}, \quad x \geq 0$$

### Mean

$$E[X] = \frac{1}{\lambda} = \beta$$

### Variance

$$\text{Var}(X) = \frac{1}{\lambda^2} = \beta^2$$

**Note:** Standard deviation = mean for exponential distribution.

### Memoryless Property

$$P(X > s + t \mid X > s) = P(X > t)$$

The exponential is the ONLY continuous distribution with this property.

**Engineering meaning:** A component that has been working for $s$ hours is statistically "as good as new." Its remaining lifetime has the same distribution as a brand-new component.

### Relationship to Poisson

If events arrive as a Poisson process with rate $\lambda$:
- Number of events in time $T \sim \mathrm{Poisson}(\lambda T)$
- Time between events $\sim \mathrm{Exponential}(\lambda)$

### When to Use

✅ Modeling time between events in a Poisson process
✅ Component lifetime with constant failure rate
✅ Memoryless assumption is reasonable

### When NOT to Use

❌ Failure rate increases with age (wear-out) — use Weibull or Gamma
❌ Failure rate decreases with age (burn-in) — use Weibull
❌ Data shows increasing hazard — NOT memoryless

### MATLAB Implementation

```matlab
%% Exponential Distribution: Component Lifetime
lambda = 0.001;          % Failure rate: 0.001 per hour
beta = 1/lambda;         % Mean lifetime: 1000 hours

x = linspace(0, 5000, 1000);
pdf_exp = exppdf(x, beta);
cdf_exp = expcdf(x, beta);

figure;
subplot(2,1,1);
plot(x, pdf_exp, 'b-', 'LineWidth', 2);
xlabel('Time (hours)'); ylabel('f_X(x)');
title(sprintf('Exponential PDF: Mean Lifetime = %d hours', beta));
grid on;

subplot(2,1,2);
plot(x, cdf_exp, 'r-', 'LineWidth', 2);
xlabel('Time (hours)'); ylabel('F_X(x) = P(failure by time x)');
title('Exponential CDF');
grid on;

% P(component lasts more than 2000 hours)
p_survive_2000 = 1 - expcdf(2000, beta);
fprintf('P(T > 2000 hours) = %.4f\n', p_survive_2000);
```

### Simulation Example: Memoryless Property Verification

```matlab
%% Verify Memoryless Property
beta = 100;   % Mean lifetime = 100 hours
N = 100000;

X = exprnd(beta, 1, N);

% Conditional: given X > 50, what is distribution of (X - 50)?
survivors = X(X > 50);
remaining_life = survivors - 50;

fprintf('Mean of X: %.1f (should be %.1f)\n', mean(X), beta);
fprintf('Mean of remaining life given survival past 50: %.1f\n', mean(remaining_life));
fprintf('Should also be %.1f (memoryless!)\n', beta);
```

---

## 3.9 Gaussian (Normal) Distribution

### Physical Interpretation

Models quantities that result from the **sum of many small, independent random effects**. The most important distribution in engineering due to the Central Limit Theorem.

### Engineering Context

- Thermal noise voltage (sum of many electron contributions)
- Measurement errors (sum of many small error sources)
- Manufacturing variations (many independent process variations)
- Aggregate interference in communications
- Any quantity well-modeled by CLT

### Mathematical Definition

$X \sim N(\mu,\, \sigma^{2})$ where $\mu$ = mean, $\sigma^{2}$ = variance.

### PDF

$$f_X(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right), \quad -\infty < x < \infty$$

### CDF

$$F_X(x) = \Phi\left(\frac{x-\mu}{\sigma}\right) = \frac{1}{2}\left[1 + \text{erf}\left(\frac{x-\mu}{\sigma\sqrt{2}}\right)\right]$$

No closed-form — computed numerically or via tables.

### Mean

$$E[X] = \mu$$

### Variance

$$\text{Var}(X) = \sigma^2$$

### Standard Normal

$Z \sim N(0,\, 1)$: standardized form.

Any Normal can be standardized: $Z = \frac{X - \mu}{\sigma}$

### Key Probability Rules $(\sigma -\text{rules})$

| Range | Probability |
|-------|-------------|
| $\mu \pm 1\sigma$ | 68.27% |
| $\mu \pm 2\sigma$ | 95.45% |
| $\mu \pm 3\sigma$ | 99.73% |
| $\mu \pm 4\sigma$ | 99.9937% |

### Properties

1. **Symmetric** about $\mu$
2. **Linear transformation:** If $X \sim N(\mu,\, \sigma^{2})$, then aX $+ b \sim N(a\mu + b,\, a^{2}\sigma^{2})$
3. **Sum of Gaussians:** If $X \sim N(\mu_{1},\, \sigma_{1}^{2})$ and $Y \sim N(\mu_{2},\, \sigma_{2}^{2})$ are independent, then $X + Y \sim N(\mu_{1}+\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$
4. **Fully characterized** by mean and variance (all higher moments determined)

### When to Use

✅ Thermal noise in circuits
✅ Measurement errors
✅ Any sum of many independent random effects (CLT)
✅ Manufacturing variations
✅ When data histogram is bell-shaped and symmetric

### When NOT to Use

❌ Strictly positive quantities (Gaussian allows negative values)
❌ Heavily skewed data
❌ Bounded quantities (Gaussian has infinite support)
❌ Discrete counts
❌ Heavy-tailed phenomena (use t-distribution or Cauchy)

### MATLAB Implementation

```matlab
%% Gaussian Distribution: Thermal Noise
mu = 0;           % Zero-mean noise
sigma = 0.1;      % RMS noise = 100 mV

x = linspace(mu - 4*sigma, mu + 4*sigma, 1000);
pdf_norm = normpdf(x, mu, sigma);

figure;
plot(x, pdf_norm, 'b-', 'LineWidth', 2);
hold on;
% Shade the ±2σ region
x_fill = linspace(mu-2*sigma, mu+2*sigma, 200);
fill([x_fill, fliplr(x_fill)], [normpdf(x_fill,mu,sigma), zeros(size(x_fill))], ...
     'b', 'FaceAlpha', 0.3);
xlabel('Noise Voltage (V)');
ylabel('f_X(x)');
title(sprintf('Gaussian PDF: μ=%.1f, σ=%.3f V', mu, sigma));
legend('PDF', '±2σ region (95.45%)');
grid on;

% Probability computations
fprintf('P(|noise| > 0.2V) = %.4f\n', 2*(1-normcdf(0.2, 0, sigma)));
fprintf('P(|noise| > 0.3V) = %.4f\n', 2*(1-normcdf(0.3, 0, sigma)));
```

### Simulation Example: Q-Function and BER

```matlab
%% Engineering Application: Binary Communication BER
% In BPSK with AWGN: BER = Q(sqrt(2*Eb/N0))
% Q(x) = 1 - Phi(x) = probability that N(0,1) > x

EbN0_dB = 0:0.5:12;
EbN0 = 10.^(EbN0_dB/10);

% Theoretical BER
BER_theory = qfunc(sqrt(2*EbN0));

% Simulated BER
N_bits = 1e6;
BER_sim = zeros(size(EbN0_dB));
for idx = 1:length(EbN0)
    % BPSK: transmit ±1
    bits = 2*randi([0,1], 1, N_bits) - 1;
    noise_std = 1/sqrt(2*EbN0(idx));
    received = bits + noise_std * randn(1, N_bits);
    decisions = sign(received);
    BER_sim(idx) = mean(decisions ~= bits);
end

figure;
semilogy(EbN0_dB, BER_theory, 'b-', 'LineWidth', 2);
hold on;
semilogy(EbN0_dB, BER_sim, 'ro', 'MarkerSize', 6);
xlabel('E_b/N_0 (dB)'); ylabel('Bit Error Rate');
title('BPSK BER: Theory vs Simulation');
legend('Theory: Q(√(2E_b/N_0))', 'Simulation');
grid on;
```



---

## 3.10 Gamma Distribution

### Physical Interpretation

Models the **total waiting time until the $k-\mathrm{th}$ event** in a Poisson process, or any sum of $k$ independent exponential random variables.

### Engineering Context

- Total repair time for $k$ components (each with exponential repair time)
- Time until $k-\mathrm{th}$ packet arrival
- Aggregate service time for $k$ queued jobs
- Rainfall amount modeling

### Mathematical Definition

$X \sim \mathrm{Gamma}(\alpha,\, \beta)$ where $\alpha$ = shape parameter, $\beta$ = scale parameter (or rate $\lambda = \frac{1}{\beta}$).

### PDF

$$f_X(x) = \frac{x^{\alpha-1} e^{-x/\beta}}{\beta^\alpha \Gamma(\alpha)}, \quad x \geq 0$$

where $\Gamma (\alpha) = (\alpha -1)$! for integer $\alpha$, and $\Gamma (\alpha) = \int_{0}^{\infty} t^{\alpha -1}e^{-t}dt$ in general.

### CDF

No closed form for general $\alpha$. Available via incomplete gamma function.

### Mean

$$E[X] = \alpha\beta$$

### Variance

$$\text{Var}(X) = \alpha\beta^2$$

### Special Cases

| Parameters | Distribution |
|-----------|-------------|
| $\alpha = 1$ | $\mathrm{Exponential}(\beta)$ |
| $\alpha = \frac{n}{2},\, \beta = 2$ | Chi-square(n) |
| $\alpha$ = integer $k$ | $\mathrm{Erlang}(k,\, \beta)$ |

### When to Use

✅ Sum of independent exponential lifetimes
✅ Waiting time for $k-\mathrm{th}$ Poisson event
✅ Skewed, non-negative data
✅ Flexible shape (adjustable skewness)

### When NOT to Use

❌ Data can be negative
❌ Data is symmetric (use Gaussian)
❌ Simpler distribution fits (exponential for $k = 1$)

### MATLAB Implementation

```matlab
%% Gamma Distribution: Total Repair Time
% 3 components must be repaired sequentially
% Each repair time ~ Exp(mean = 2 hours)
alpha = 3;         % shape (number of stages)
beta = 2;          % scale (mean per stage)

x = linspace(0, 20, 500);
pdf_gamma = gampdf(x, alpha, beta);

figure;
plot(x, pdf_gamma, 'b-', 'LineWidth', 2);
xlabel('Total Repair Time (hours)');
ylabel('f_X(x)');
title(sprintf('Gamma PDF: α=%d, β=%d', alpha, beta));
grid on;

% Verify by summing exponentials
N = 100000;
X_sim = sum(exprnd(beta, alpha, N), 1);  % Sum of 3 exponentials
fprintf('Mean: Theory=%.2f, Sim=%.2f\n', alpha*beta, mean(X_sim));
fprintf('Var:  Theory=%.2f, Sim=%.2f\n', alpha*beta^2, var(X_sim));
```

---

## 3.11 Rayleigh Distribution

### Physical Interpretation

Models the **magnitude (envelope)** of a $2D$ Gaussian random vector. When $X$ and $Y$ are independent $N(0,\, \sigma^{2})$, then $R = \sqrt{X^{2} + Y^{2}}$ follows a Rayleigh distribution.

### Engineering Context

- Envelope of narrowband noise (in-phase + quadrature)
- Magnitude of wireless fading channel coefficient (Rayleigh fading)
- Wind speed modeling
- Distance error in $2D$ positioning (when errors are Gaussian in $x$ and $y$)

### Mathematical Definition

$R \sim \mathrm{Rayleigh}(\sigma)$, where $\sigma$ is the parameter (NOT the standard deviation of $R$).

### PDF

$$f_R(r) = \frac{r}{\sigma^2} e^{-r^2/(2\sigma^2)}, \quad r \geq 0$$

### CDF

$$F_R(r) = 1 - e^{-r^2/(2\sigma^2)}, \quad r \geq 0$$

### Mean

$$E[R] = \sigma\sqrt{\frac{\pi}{2}} \approx 1.2533\sigma$$

### Variance

$$\text{Var}(R) = \frac{4-\pi}{2}\sigma^2 \approx 0.4292\sigma^2$$

### Relationship to Other Distributions

- $R^{2} \sim \mathrm{Exponential}(2\sigma^{2})$ — the power is exponentially distributed
- If $X,\, Y \sim N(0,\, \sigma^{2})$ independent, then $\sqrt{X^{2}+Y^{2}} \sim \mathrm{Rayleigh}(\sigma)$

### When to Use

✅ Envelope/magnitude of complex Gaussian signal
✅ Rayleigh fading channel modeling (no line-of-sight)
✅ $2D$ distance with Gaussian errors in each dimension
✅ RSS (root-sum-square) of two independent Gaussian components

### When NOT to Use

❌ Strong line-of-sight component present (use Rician)
❌ Not modeling a magnitude/envelope
❌ Components are not Gaussian or not equal-variance

### MATLAB Implementation

```matlab
%% Rayleigh Distribution: Wireless Fading Channel
sigma = 1;   % Rayleigh parameter
r = linspace(0, 5, 500);

pdf_ray = raylpdf(r, sigma);
cdf_ray = raylcdf(r, sigma);

figure;
subplot(2,1,1);
plot(r, pdf_ray, 'b-', 'LineWidth', 2);
xlabel('Channel Gain |h|'); ylabel('f_R(r)');
title(sprintf('Rayleigh PDF (σ = %d)', sigma));
grid on;

subplot(2,1,2);
plot(r, cdf_ray, 'r-', 'LineWidth', 2);
xlabel('r'); ylabel('P(R ≤ r)');
title('Rayleigh CDF = Outage Probability');
grid on;

% Outage probability: P(|h| < threshold)
threshold = 0.5;
p_outage = raylcdf(threshold, sigma);
fprintf('P(outage, |h| < %.1f) = %.4f\n', threshold, p_outage);
```

### Simulation Example: Rayleigh Fading

```matlab
%% Generate Rayleigh Fading from Complex Gaussian
sigma = 1;
N = 100000;

% Complex Gaussian channel coefficient
h = sigma/sqrt(2) * (randn(1,N) + 1j*randn(1,N));

% Magnitude is Rayleigh distributed
R = abs(h);

figure;
histogram(R, 100, 'Normalization', 'pdf');
hold on;
r = linspace(0, 5, 300);
plot(r, raylpdf(r, sigma/sqrt(2)), 'r-', 'LineWidth', 2);
xlabel('|h|'); ylabel('PDF');
title('Rayleigh Fading: Simulated vs Theory');
legend('Simulation', 'Theory');
```

---

## 3.12 Chi-Square Distribution

### Physical Interpretation

Models the **sum of squares of independent standard normal** random variables. Arises naturally in variance estimation and goodness-of-fit testing.

### Engineering Context

- Sample variance distribution (for Gaussian data)
- Power of Gaussian noise (sum of squared components)
- Goodness-of-fit test statistic
- Confidence intervals for variance
- Energy detection in spectrum sensing

### Mathematical Definition

If $Z_{1},\, Z_{2},\, \ldots,\, Z_{n}$ are independent $N(0,\,1)$, then:
$$\chi^2 = Z_1^2 + Z_2^2 + \cdots + Z_n^2 \sim \chi^2(n)$$

$n$ = degrees of freedom.

### PDF

$$f_X(x) = \frac{x^{n/2-1} e^{-x/2}}{2^{n/2}\Gamma(n/2)}, \quad x \geq 0$$

### CDF

No closed form for general $n$. Computed via incomplete gamma function.

### Mean

$$E[X] = n$$

### Variance

$$\text{Var}(X) = 2n$$

### Relationship to Other Distributions

- Chi-square(n) $= \mathrm{Gamma}\left(\frac{n}{2},\, 2\right)$
- Chi-square(1) = square of $N(0,\,1)$
- Chi-square(2) $= \mathrm{Exponential}\left(\frac{1}{2}\right)$
- For large $n$: $\chi^{2}(n) \approx N(n,\, 2n)$ by CLT

### When to Use

✅ Sum of squared Gaussian random variables
✅ Variance testing and confidence intervals
✅ Goodness-of-fit tests
✅ Energy detection in $N$ dimensions

### When NOT to Use

❌ Variables are not Gaussian
❌ Variables are not independent
❌ Variables are not standard normal (need scaling)

### MATLAB Implementation

```matlab
%% Chi-Square Distribution: Noise Power
% Noise power from N complex dimensions = sum of 2N squared Gaussians

n_dof = 10;        % degrees of freedom
x = linspace(0, 30, 500);

pdf_chi2 = chi2pdf(x, n_dof);
cdf_chi2 = chi2cdf(x, n_dof);

figure;
plot(x, pdf_chi2, 'b-', 'LineWidth', 2);
xlabel('χ² Value (Noise Power)');
ylabel('f_X(x)');
title(sprintf('Chi-Square PDF: %d degrees of freedom', n_dof));
grid on;

% Simulation: sum of squared standard normals
N = 100000;
Z = randn(n_dof, N);
X_sim = sum(Z.^2, 1);

fprintf('Mean: Theory=%d, Sim=%.2f\n', n_dof, mean(X_sim));
fprintf('Var:  Theory=%d, Sim=%.2f\n', 2*n_dof, var(X_sim));
```

---

## 3.13 Distribution Selection Guide

### Decision Tree: Discrete Distributions

```{.figure #m03-tree-discrete}
Is the random variable discrete (countable outcomes)?
│
├── Only two outcomes (success/failure)?
│   └── YES → Bernoulli(p)
│
├── Fixed n trials, counting successes?
│   └── YES → Binomial(n, p)
│
├── Counting trials until FIRST success?
│   └── YES → Geometric(p)
│
├── Counting trials until r-th success?
│   └── YES → Negative Binomial(r, p)
│
└── Counting events in a fixed interval?
    └── Events independent and rare? → Poisson(λ)
```

### Decision Tree: Continuous Distributions

```{.figure #m03-tree-continuous}
Is the random variable continuous?
│
├── Equally likely over a bounded range?
│   └── YES → Uniform(a, b)
│
├── Time between events (memoryless)?
│   └── YES → Exponential(λ)
│
├── Sum of many small independent effects?
│   └── YES → Gaussian(μ, σ²)
│
├── Total time for k events (sum of exponentials)?
│   └── YES → Gamma(k, β)
│
├── Magnitude of 2D Gaussian vector?
│   └── YES → Rayleigh(σ)
│
└── Sum of squared Gaussians?
    └── YES → Chi-square(n)
```

### Quick Recognition Patterns

| If you see... | Think... |
|--------------|----------|
| Binary outcome | Bernoulli |
| "$n$ trials, $k$ successes" | Binomial |
| "How many until first..." | Geometric |
| "Events per interval" | Poisson |
| "Equal chance anywhere in $[a,\,b]$" | Uniform |
| "Time until next event" | Exponential |
| "Sum of many effects" or "noise" | Gaussian |
| "Total waiting time for $k$ events" | Gamma |
| "Signal envelope" or "fading" | Rayleigh |
| "Sum of squared normals" | Chi-square |

---

## 3.14 Summary Table

| Distribution | Type | Parameters | Mean | Variance | MATLAB |
|-------------|------|-----------|------|----------|--------|
| Bernoulli | Discrete | $p$ | $p$ | $p(1-p)$ | `binornd(1,p)` |
| Binomial | Discrete | $n,\, p$ | $np$ | $np(1-p)$ | `binornd(n,p)` |
| Geometric | Discrete | $p$ | $\frac{1}{p}$ | $\frac{1-p}{p^{2}}$ | `geornd(p)+1` |
| Neg. Binomial | Discrete | $r,\, p$ | $\frac{r}{p}$ | $\frac{r(1-p)}{p^{2}}$ | `nbinrnd(r,p)+r` |
| Poisson | Discrete | $\lambda$ | $\lambda$ | $\lambda$ | `poissrnd(λ)` |
| Uniform | Continuous | a, $b$ | $\frac{a+b}{2}$ | $\frac{(b-a)^{2}}{12}$ | `unifrnd(a,b)` |
| Exponential | Continuous | $\lambda$ (or $\beta = \frac{1}{\lambda}$) | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^{2}}$ | `exprnd(β)` |
| Gaussian | Continuous | $\mu,\, \sigma^{2}$ | $\mu$ | $\sigma^{2}$ | `normrnd(μ,σ)` |
| Gamma | Continuous | $\alpha,\, \beta$ | $\alpha \beta$ | $\alpha \beta^{2}$ | `gamrnd(α,β)` |
| Rayleigh | Continuous | $\sigma$ | $\sigma \sqrt{\frac{\pi}{2}}$ | $\frac{(4-\pi)\sigma^{2}}{2}$ | `raylrnd(σ)` |
| Chi-square | Continuous | $n$ | $n$ | $2n$ | `chi2rnd(n)` |

---

## 3.15 Practice Problems

### Problem 1: Distribution Identification
Identify the appropriate distribution for each scenario:

(a) Number of dropped calls in a 10-minute window (average: 2 per 10 min)
(b) Voltage of thermal noise across a resistor
(c) Time until a server crashes (constant failure rate)
(d) Number of defective ICs out of a batch of 100 (defect rate 3%)
(e) Envelope of received signal through multipath with no LOS

**Solution:** (a) $\mathrm{Poisson}(\lambda = 2)$, (b) $\mathrm{Gaussian}(0,\, \sigma^{2})$, (c) $\mathrm{Exponential}(\lambda)$, (d) $\mathrm{Binomial}(100,\, 0.03)$, (e) $\mathrm{Rayleigh}(\sigma)$

### Problem 2: Exponential vs Poisson
Packets arrive at a router as a Poisson process with rate $\lambda = 5$ packets/second.

(a) What is $P(\text{more than 8 packets in 1 second})$?
(b) What is $P(\text{no packet arrives in the next 0.5 seconds})$?
(c) What distribution describes the inter-arrival time?
(d) What is the mean inter-arrival time?

**Solution:**
(a) $X \sim \mathrm{Poisson}(5)$: $P(X > 8) = 1 - \texttt{poisscdf}(8,\, 5) = 0.0681$
(b) $T \sim \mathrm{Exp}(5)$: $P(T > 0.5) = e^{-5 \times 0.5} = e^{-2.5} = 0.0821$
(c) Exponential with rate $\lambda = 5$
(d) Mean $= \frac{1}{\lambda} = 0.2$ seconds

### Problem 3: Gaussian Probability
A resistor has nominal value 1000Ω with manufacturing variation modeled as $N(1000,\, 25) (\sigma = 5\,\Omega)$.

(a) What fraction of resistors are within $\pm 10\,\Omega$ of nominal (spec: $990-1010\,\Omega$)?
(b) What fraction fail the $\pm 1\%$ tolerance test?
(c) What tolerance range contains 99.7% of resistors?

**Solution:**
(a) $P(990 \le X \le 1010) = P(-2 \le Z \le 2) = 0.9545 \to 95.45\%$
(b) $\pm 1\% = \pm 10\,\Omega$ → same as (a), fail rate $= 1 - 0.9545 = 4.55\%$
(c) $\pm 3\sigma = \pm 15\,\Omega$ → range $[985,\, 1015]\Omega$

---

*Next Module: [Module 4 — Expectation and Moments](module04-expectation-moments.md)*
