<!-- TRANSLATION START -->
# ماژول 5 — توابع مهم متغیر تصادفی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. تبدیل‌های خطی را روی متغیرهای تصادفی اعمال کنند
2. PDF تبدیل‌های غیرخطی را با روش‌های CDF و ژاکوبی استخراج کنند
3. تبدیل‌ها را در مسائل مهندسی (توان، SNR، پوش) به کار ببرند
4. نتایج تحلیلی را با شبیه‌سازی MATLAB بررسی کنند

---

## 5.1 مقدمه: چرا متغیرهای تصادفی را تبدیل کنیم؟

در مهندسی، اغلب آماره‌های یک کمیت را می‌دانیم اما به آماره‌های یک کمیت **مشتق‌شده** نیاز داریم:

| معلوم | موردنیاز | تبدیل |
|-------|--------|---------------|
| ولتاژ $V$ | توان $P$ | $P = \frac{V^{2}}{R}$ |
| SNR خطی | SNR بر حسب dB | $Y = 10 \cdot \log_{10}(X)$ |
| گاوسی $I$، $Q$ | پوش | $R = \sqrt{I^{2} + Q^{2}}$ |
| جریان $I$ | توان | $P = I^{2}R$ |
| فاصله $d$ | تلفات مسیر | $L = d^{\alpha}$ |

---

## 5.2 تبدیل‌های خطی: $Y$ = aX $+ b$

### نتیجه

اگر $X$ دارای PDF $f_{X}(x)$ باشد، آنگاه $Y$ = aX $+ b$ دارای:

$$f_Y(y) = \frac{1}{|a|} f_X\left(\frac{y-b}{a}\right)$$

### میانگین و واریانس

- $E[Y]$ = aE[X] $+ b$
- $\operatorname{Var}(Y) = a^{2}\operatorname{Var}(X)$

### مثال مهندسی: تقویت‌کننده

سیگنال ورودی $X \sim N(0,\, \sigma^{2})$ از تقویت‌کننده‌ای با بهره $G = 10$ و آفست DC یعنی $V_{0} = 1.5\,\mathrm{V}$ می‌گذرد.

خروجی: $Y = 10X + 1.5$

- $Y \sim N(10 \cdot 0 + 1.5,\, 10^{2} \cdot \sigma^{2}) = N(1.5,\, 100\sigma^{2})$
- توان نویز خروجی به اندازه $G^{2} = 100$ افزایش یافته است

### مثال مهندسی: حسگر دما

خروجی حسگر $V = 0.01 \cdot T + 0.5$ (V بر حسب ولت، $T$ بر حسب °C)

اگر $T \sim N(25,\, 4)$: $V \sim N(0.01 \times 25 + 0.5,\, 0.01^{2} \times 4) = N(0.75,\, 0.0004)$

---

## 5.3 تبدیل‌های یکنوای عمومی

### روش CDF (کلی‌ترین روش)

برای $Y = g(X)$، $f_{Y}(y)$ را به این ترتیب بیابید:

1. بنویسید $F_{Y}(y) = P(Y \le y) = P(g(X) \le y)$
2. نامساوی را برای $X$ حل کنید
3. بر حسب $F_{X}$ بیان کنید
4. مشتق بگیرید تا $f_{Y}(y)$ به دست آید

### فرمول برای $g$ یکنوا

اگر $g$ اکیداً صعودی با وارون $g^{-1}$ باشد:
$$f_Y(y) = f_X(g^{-1}(y)) \cdot \frac{d}{dy}[g^{-1}(y)]$$

اگر $g$ اکیداً نزولی باشد:
$$f_Y(y) = f_X(g^{-1}(y)) \cdot \left|\frac{d}{dy}[g^{-1}(y)]\right|$$

ترکیبی (برای هر دو کار می‌کند):
$$f_Y(y) = \frac{f_X(x)}{|g'(x)|}\bigg|_{x=g^{-1}(y)}$$

### مثال مهندسی: SNR بر حسب dB

SNR خطی: $X \sim \mathrm{Exponential}(1)$ (توان محوشدگی رایلی نرمال‌شده).
SNR بر حسب dB: $Y = 10 \cdot \log_{10}(X)$

- $g(x) = 10 \cdot \log_{10}(x)$، $g'(x) = \frac{10}{x \cdot \ln 10}$
- $g^{-1}(y) = 10^{y/10}$

$$f_Y(y) = \frac{f_X(10^{y/10})}{10/(10^{y/10} \cdot \ln 10)} = \frac{\ln 10}{10} \cdot 10^{y/10} \cdot e^{-10^{y/10}}$$

این توزیع گاوسی نیست — دنباله چپ بلندتری دارد (فروافت‌های عمیق).

---

## 5.4 تبدیل‌های غیرخطی: حالت غیریکنوا

### هنگامی که $Y = g(X)$ یک‌به‌یک نیست

اگر چند مقدار $x$ به همان $y$ نگاشت شوند، روی تمام جواب‌ها جمع کنید:

$$f_Y(y) = \sum_{i} \frac{f_X(x_i)}{|g'(x_i)|}$$

که در آن $x_{1},\, x_{2}$, ... تمام ریشه‌های $g(x) = y$ هستند.

### مثال مهندسی: توان از ولتاژ $(Y = X^{2})$

ولتاژ $X \sim N(0,\, \sigma^{2})$. توان: $Y = X^{2}$.

برای $y > 0$، جواب‌ها $x = +\sqrt{y}$ و $x = -\sqrt{y}$ هستند. $g'(x) = 2x$.

$$f_Y(y) = \frac{f_X(\sqrt{y})}{2\sqrt{y}} + \frac{f_X(-\sqrt{y})}{2\sqrt{y}} = \frac{1}{\sqrt{2\pi}\sigma} \cdot \frac{e^{-y/(2\sigma^2)}}{\sqrt{y}}$$

این یک **توزیع کای‌دو با 1 درجه آزادی** است (مقیاس‌شده با $\sigma^{2}$).

### مثال مهندسی: یکسوساز تمام‌موج $(Y = \lvert X\rvert)$

ورودی $X \sim N(0,\, \sigma^{2})$. خروجی $Y = \lvert X\rvert$.

برای $y > 0$: $x = +y$ و $x = -y$ جواب هستند. $\lvert g'(x)\rvert = 1$.

$$f_Y(y) = f_X(y) + f_X(-y) = \frac{2}{\sigma\sqrt{2\pi}} e^{-y^2/(2\sigma^2)}, \quad y \geq 0$$

این توزیع **نرمال تاشده** (نیم‌نرمال) است.

---

## 5.5 روش ژاکوبی

### رویه برای $Y = g(X)$

1. تبدیل $y = g(x)$ را مشخص کنید
2. وارون را بیابید: $x = g^{-1}(y)$
3. ژاکوبی را محاسبه کنید: $J = \lvert \frac{dx}{dy}\rvert = \lvert \frac{d[g^{-1}(y)]}{dy}\rvert$
4. اعمال کنید: $f_{Y}(y) = f_{X}(g^{-1}(y)) \cdot \lvert J\rvert$

### مثال گام‌به‌گام: نمایی گاوسی

$X \sim N(\mu,\, \sigma^{2})$. $Y = e^{x}$ (تبدیل لگ‌نرمال).

1. $g(x) = e^{x}$ (یکنوای صعودی)
2. $x = \ln (y)$، معتبر برای $y > 0$
3. $J = \lvert \frac{dx}{dy}\rvert = \frac{1}{y}$
4. $f_{Y}(y) = f_{X}(\ln y) \cdot \frac{1}{y} = \frac{1}{y\sigma \sqrt{2\pi}} \cdot \exp \left(-\frac{(\ln y - \mu)^{2}}{2\sigma^{2}}\right)$

این همان **توزیع لگ‌نرمال** است — بسیاری از کمیت‌های مهندسی را مدل می‌کند (سایه‌اندازی در بی‌سیم، قیمت سهام، عمر قطعات همراه با فرسودگی).

---

## 5.6 کاربرد مهندسی: رایلی از گاوسی

### استخراج توزیع رایلی

در مخابرات، سیگنال دریافتی مؤلفه‌های هم‌فاز $(I)$ و مربعی $(Q)$ دارد:
- $I \sim N(0,\, \sigma^{2})$، $Q \sim N(0,\, \sigma^{2})$، مستقل

پوش: $R = \sqrt{I^{2} + Q^{2}}$

با استفاده از تبدیل $(I,\, Q) \to (R,\, \theta)$ در مختصات قطبی:
- $I = R \cdot \cos (\theta)$، $Q = R \cdot \sin (\theta)$
- ژاکوبی: $\lvert \frac{\partial (I,\,Q)}{\partial (R,\,\theta)}\rvert = R$

PDF مشترک $(R,\, \theta)$:
$f_{R,\Theta}(r,\,\theta) = f_{I,Q}(r \cdot \cos \theta,\, r \cdot \sin \theta) \cdot r = \frac{r}{2\pi \sigma^{2}} \cdot e^{-r^{2}/(2\sigma^{2})}$

حاشیه‌سازی روی $\theta \in [0,\, 2\pi)$:

$$f_R(r) = \frac{r}{\sigma^2} e^{-r^2/(2\sigma^2)}, \quad r \geq 0$$

این همان **توزیع رایلی** است — بنیادی برای مدل‌سازی کانال محوشدگی بی‌سیم.

---

## 5.7 مثال‌های MATLAB

### مثال 1: بررسی تبدیل خطی

```matlab
%% Linear Transformation: Amplifier Output
sigma_in = 0.1;  G = 5;  offset = 2;
N = 100000;

X = sigma_in * randn(1, N);   % Input noise
Y = G*X + offset;              % Output

figure;
subplot(1,2,1);
histogram(X, 80, 'Normalization', 'pdf'); hold on;
x = linspace(-0.5, 0.5, 200);
plot(x, normpdf(x, 0, sigma_in), 'r', 'LineWidth', 2);
title('Input X ~ N(0, 0.01)'); xlabel('Voltage (V)');

subplot(1,2,2);
histogram(Y, 80, 'Normalization', 'pdf'); hold on;
y = linspace(0, 4, 200);
plot(y, normpdf(y, offset, G*sigma_in), 'r', 'LineWidth', 2);
title('Output Y = 5X + 2'); xlabel('Voltage (V)');
```

### مثال 2: توان از ولتاژ گاوسی

```matlab
%% Nonlinear: Y = X^2 (instantaneous power)
sigma = 1; N = 200000;
X = sigma * randn(1, N);
Y = X.^2;

figure;
histogram(Y, 150, 'Normalization', 'pdf', 'BinLimits', [0, 8]);
hold on;
y = linspace(0.01, 8, 300);
f_theory = chi2pdf(y/sigma^2, 1) / sigma^2;
plot(y, f_theory, 'r', 'LineWidth', 2);
xlabel('Power (V²)'); ylabel('PDF');
title('Distribution of Power Y = X²');
legend('Simulation', 'Theory (scaled χ²₁)');
```

### مثال 3: SNR بر حسب dB از SNR خطی

```matlab
%% SNR transformation: Y = 10*log10(X)
% X ~ Exponential(1) (Rayleigh fading power, normalized)
N = 200000;
X = exprnd(1, 1, N);
Y = 10*log10(X);   % SNR in dB

figure;
histogram(Y, 150, 'Normalization', 'pdf');
hold on;
y = linspace(-30, 15, 300);
% Theoretical PDF of Y = 10*log10(X) where X ~ Exp(1)
f_Y = (log(10)/10) .* 10.^(y/10) .* exp(-10.^(y/10));
plot(y, f_Y, 'r', 'LineWidth', 2);
xlabel('SNR (dB)'); ylabel('PDF');
title('Distribution of SNR in dB (Rayleigh Fading)');
legend('Simulation', 'Theory');
```

### مثال 4: پوش رایلی از مؤلفه‌های گاوسی

```matlab
%% Envelope of Complex Gaussian
sigma = 1; N = 100000;
I = sigma * randn(1, N);
Q = sigma * randn(1, N);
R = sqrt(I.^2 + Q.^2);

figure;
histogram(R, 100, 'Normalization', 'pdf');
hold on;
r = linspace(0, 5, 200);
plot(r, raylpdf(r, sigma), 'r', 'LineWidth', 2);
xlabel('Envelope |h|'); ylabel('PDF');
title('Rayleigh Distribution from I/Q Components');
legend('Simulation', 'Theory');
fprintf('Mean: Sim=%.3f, Theory=%.3f\n', mean(R), sigma*sqrt(pi/2));
```

### مثال 5: لگ‌نرمال از گاوسی

```matlab
%% Log-Normal: Y = exp(X) where X ~ N(mu, sigma^2)
mu = 0; sigma_x = 0.5; N = 100000;
X = mu + sigma_x*randn(1, N);
Y = exp(X);

figure;
histogram(Y, 150, 'Normalization', 'pdf', 'BinLimits', [0, 5]);
hold on;
y = linspace(0.01, 5, 300);
f_lognorm = lognpdf(y, mu, sigma_x);
plot(y, f_lognorm, 'r', 'LineWidth', 2);
xlabel('Y = e^X'); ylabel('PDF');
title('Log-Normal Distribution');
legend('Simulation', 'Theory');
```

---

## 5.8 مسائل تمرینی

### مسئله 1
سیگنال $X \sim \mathrm{Uniform}[0,\, 1]$. خروجی $Y = -2 \cdot \ln (X)$. PDF مربوط به $Y$ را بیابید و توزیع را تشخیص دهید.

**حل:** روش CDF: $F_{Y}(y) = P(-2\ln (X) \le y) = P(X \ge e^{-y/2}) = 1 - e^{-y/2}$ برای $y \ge 0$.
$f_{Y}(y) = \frac{1}{2}e^{-y/2} \to Y \sim \mathrm{Exponential}(\beta = 2)$. (این همان روش CDF وارون برای تولید توزیع نمایی است!)

### مسئله 2
ولتاژ نویز $X \sim N(0,\, \sigma^{2})$ با $\sigma = 2\,\mathrm{V}$. توان تلف‌شده در $R = 50\,\Omega$: $P = \frac{X^{2}}{R}$.
(a) $E[P]$ را بیابید. (ب) PDF مربوط به $P$ را بیابید. (ج) $P(P > 0.2\,\mathrm{W})$ را بیابید.

**حل:**
(a) $E[P] = \frac{E[X^{2}]}{R} = \frac{\sigma^{2}}{R} = \frac{4}{50} = 0.08\,\mathrm{W}$
(b) $P = \frac{X^{2}}{50}$. فرض کنید $W = X^{2} \sim \sigma^{2} \cdot \chi^{2}(1)$. آنگاه $P = \frac{W}{50}$. $f_{P}(p) = 50 \cdot f_{W}(50p) = \frac{50}{2\sigma^{2}} \cdot \frac{50p}{\sigma^{2}}^{-1/2} \cdot e^{-50p/(2\sigma^{2})}$ برای $p > 0$.
(c) از MATLAB استفاده کنید: `1 - chi2cdf(0.2*50/4, 1)` $= 1 - \texttt{chi2cdf}(2.5,\, 1) \approx 0.114$

### مسئله 3
$X \sim \mathrm{Exponential}(\lambda = 1)$. $Y = \sqrt{X}$. مقدار $f_{Y}(y)$ را بیابید.

**حل:** $g(x) = \sqrt{x} \to x = y^{2}$، $\frac{dx}{dy} = 2y$.
$f_{Y}(y) = f_{X}(y^{2}) \cdot \lvert 2y\rvert = e^{-y^{2}} \cdot 2y$ برای $y \ge 0$. این یک توزیع رایلی با $\sigma^{2} = \frac{1}{2}$ است.

### مسئله 4
فاصله $D \sim \mathrm{Uniform}[1,\, 10]$ km. تلفات مسیر: $L = 20 \cdot \log_{10}(D)$ dB. مقدار $E[L]$ و PDF مربوط به $L$ را بیابید.

**حل:** $E[L] = E[20 \cdot \log_{10}(D)] = \int_{1}^{10} 20 \cdot \log_{10}(d) \cdot \frac{1}{9}\,dd = \frac{20}{9} \cdot \int_{1}^{10} \log_{10}(d)\,dd$
$= \frac{20}{9} \cdot \left[d \cdot \log_{10}(d) - \frac{d}{\ln (10)}\right]_{1}^{10} = \frac{20}{9} \cdot \left[10 - \frac{10}{\ln 10} + \frac{1}{\ln 10}\right] = \frac{20}{9} \cdot \left(10 - \frac{9}{\ln 10}\right) \approx 13.55$ dB

PDF: $L$ از 0 تا 20 dB تغییر می‌کند. $g^{-1}(l) = 10^{l/20}$، $\lvert \frac{dg^{-1}}{dl}\rvert = \frac{\ln 10}{20} \cdot 10^{l/20}$.
$f_{L}(l) = \frac{1}{9} \cdot \frac{\ln 10}{20} \cdot 10^{l/20}$ برای $0 \le l \le 20$.

### مسئله 5
کد MATLAB را برای بررسی مسئله 2 با شبیه‌سازی بنویسید.

**حل:**
```matlab
sigma = 2; R = 50; N = 500000;
X = sigma*randn(1,N);
P = X.^2 / R;
fprintf('E[P]: Theory=%.4f W, Sim=%.4f W\n', sigma^2/R, mean(P));
fprintf('P(P>0.2): Theory=%.4f, Sim=%.4f\n', 1-chi2cdf(0.2*R/sigma^2,1), mean(P>0.2));
```

---

*ماژول بعدی: [ماژول 6 — دو متغیر تصادفی](module06-two-random-variables.md)*
<!-- TRANSLATION END -->
