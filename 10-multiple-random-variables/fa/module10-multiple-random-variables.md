<!-- TRANSLATION START -->
# ماژول 10 — چند متغیر تصادفی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. توزیع‌های مشترک را به $N$ متغیر تصادفی تعمیم دهند
2. بردارهای تصادفی را تعریف کنند و با آن‌ها کار کنند
3. بردار میانگین و ماتریس‌های کوواریانس را محاسبه کنند
4. توزیع گاوسی چندمتغیره را به کار ببرند
5. تبدیل‌های خطی بردارهای تصادفی را انجام دهند

---

## 10.1 مقدمه: از 2 به $N$ متغیر

سیستم‌های واقعی اغلب شامل چندین کمیت نامعین به‌طور همزمان هستند:
- **آرایه حسگر:** $N$ حسگر که هر یک یک اندازه‌گیری نویزی تولید می‌کند
- **سیستم MIMO:** ضرایب کانال $M \times N$
- **برآورد پارامتر:** $p$ پارامتر نامعلوم
- **نمونه‌های سیگنال:** $N$ نمونه نویز متوالی

به چارچوبی برای بردارهای تصادفی **N-بعدی** نیاز داریم.

---

## 10.2 توزیع‌های مشترک برای $N$ متغیر تصادفی

### PDF مشترک

برای $X_{1},\, X_{2},\, \ldots,\, X_{n}$:
$$f_{X_1,...,X_n}(x_1,...,x_n) \geq 0, \quad \int\cdots\int f_{X_1,...,X_n} \, dx_1 \cdots dx_n = 1$$

### توزیع‌های حاشیه‌ای

متغیرهای ناخواسته را انتگرال بگیرید (یا جمع کنید):
$$f_{X_1}(x_1) = \int\cdots\int f_{X_1,...,X_n}(x_1,...,x_n) \, dx_2 \cdots dx_n$$

### توزیع‌های شرطی

$$f_{X_1|X_2,...,X_n}(x_1|x_2,...,x_n) = \frac{f_{X_1,...,X_n}(x_1,...,x_n)}{f_{X_2,...,X_n}(x_2,...,x_n)}$$

### استقلال متقابل

$X_{1},\, \ldots,\, X_{n}$ به‌طور متقابل مستقل هستند اگر:
$$f_{X_1,...,X_n}(x_1,...,x_n) = \prod_{i=1}^n f_{X_i}(x_i)$$

---

## 10.3 بردارهای تصادفی

### تعریف

یک **بردار تصادفی** مجموعه‌ای مرتب از متغیرهای تصادفی است:

$$\mathbf{X} = \begin{bmatrix} X_1 \\ X_2 \\ \vdots \\ X_n \end{bmatrix}$$

### مثال‌های مهندسی

| بردار تصادفی | مؤلفه‌ها | زمینه |
|--------------|-----------|---------|
| خروجی آرایه حسگر | $X_{1},\,\ldots,\,X_{n}$ = قرائت‌های حسگر | پردازش آرایه |
| کانال MIMO | $h_{1},\,\ldots,\,h_{n}$ = بهره‌های کانال | مخابرات بی‌سیم |
| بردار نویز | $N_{1},\,\ldots,\,N_{n}$ = نمونه‌های نویز | پردازش سیگنال |
| بردار حالت | مکان، سرعت، شتاب | فیلتر کالمن |
| بردار ویژگی | اندازه‌گیری‌های استخراج‌شده | بازشناسی الگو |

---

## 10.4 بردار میانگین

### تعریف

$$\boldsymbol{\mu} = E[\mathbf{X}] = \begin{bmatrix} E[X_1] \\ E[X_2] \\ \vdots \\ E[X_n] \end{bmatrix} = \begin{bmatrix} \mu_1 \\ \mu_2 \\ \vdots \\ \mu_n \end{bmatrix}$$

### ویژگی‌ها
- $E[AX + b]$ = AE[X] $+ b = A\mu + b$
- خطی‌بودن به بردارها و ماتریس‌ها تعمیم می‌یابد

---

## 10.5 ماتریس کوواریانس

### تعریف

$$\boldsymbol{\Sigma} = E[(\mathbf{X} - \boldsymbol{\mu})(\mathbf{X} - \boldsymbol{\mu})^T]$$

درایه $(i,\,j)$: $\Sigma_{ij} = \operatorname{Cov}(X_{i},\, X_{j})$

$$\boldsymbol{\Sigma} = \begin{bmatrix} \sigma_1^2 & \text{Cov}(X_1,X_2) & \cdots & \text{Cov}(X_1,X_n) \\ \text{Cov}(X_2,X_1) & \sigma_2^2 & \cdots & \text{Cov}(X_2,X_n) \\ \vdots & & \ddots & \vdots \\ \text{Cov}(X_n,X_1) & \cdots & \cdots & \sigma_n^2 \end{bmatrix}$$

### ویژگی‌ها

1. **متقارن:** $\Sigma = \Sigma^{T}$
2. **نیم‌معین مثبت:** $v^{T}\Sigma v \ge 0$ برای تمام $v$
3. **قطر = واریانس‌ها:** $\Sigma_{ii} = \operatorname{Var}(X_{i})$
4. **غیرقطری = کوواریانس‌ها:** $\Sigma_{ij} = \operatorname{Cov}(X_{i},\, X_{j})$
5. **مؤلفه‌های مستقل $\to \Sigma$ قطری**

### فرمول جایگزین

$$\boldsymbol{\Sigma} = E[\mathbf{X}\mathbf{X}^T] - \boldsymbol{\mu}\boldsymbol{\mu}^T$$

---

## 10.6 توزیع گاوسی چندمتغیره

### تعریف

$X \sim N(\mu,\, \Sigma)$ دارای PDF زیر است:

$$f_\mathbf{X}(\mathbf{x}) = \frac{1}{(2\pi)^{n/2}|\boldsymbol{\Sigma}|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)$$

که در آن $\lvert \Sigma\rvert$ = دترمینان $\Sigma$.

### ویژگی‌های کلیدی

1. **حاشیه‌ای‌ها گاوسی هستند:** هر زیرمجموعه‌ای از مؤلفه‌ها به‌طور مشترک گاوسی است
2. **شرطی‌ها گاوسی هستند:** $X_{1} \mid X_{2} = x_{2}$ گاوسی است
3. **تبدیل‌های خطی:** $Y$ = AX $+ b \sim N(A\mu +b,\, A\Sigma A^{T})$
4. **ناهمبسته = مستقل** (ویژه گاوسی!)
5. **کاملاً مشخص‌شده** با $\mu$ و $\Sigma$ (فقط 2 پارامتر برای هر متغیر به‌علاوه همبستگی‌ها لازم است)

### چرا این‌قدر مهم است؟

- قضیه حد مرکزی: جمع‌های بسیاری از متغیرهای تصادفی → گاوسی چندمتغیره
- بردارهای نویز حرارتی گاوسی چندمتغیره هستند
- از نظر تحلیلی خوش‌رفتار است (حاشیه‌ای‌ها و شرطی‌های فرم بسته)
- فیلترهای بهینه (کالمن، وینر) گاوسی‌بودن را فرض می‌کنند

### حالت دوبعدی (گاوسی دوبعدی)

برای $n = 2$ با $\mu = [0,\,0]^{T}$، $\sigma_{1} = \sigma_{2} = 1$:

$$f(x,y) = \frac{1}{2\pi\sqrt{1-\rho^2}} \exp\left(-\frac{x^2 - 2\rho xy + y^2}{2(1-\rho^2)}\right)$$

خطوط تراز **بیضی** هستند (در حالت $\rho \ne 0$ کج شده‌اند).

---

## 10.7 تبدیل‌های خطی بردارهای تصادفی

### تبدیل $Y$ = AX $+ b$

اگر $X$ یک بردار تصادفی با میانگین $\mu_{X}$ و کوواریانس $\Sigma_{X}$ باشد:

$$E[\mathbf{Y}] = A\boldsymbol{\mu}_X + \mathbf{b}$$
$$\boldsymbol{\Sigma}_Y = A\boldsymbol{\Sigma}_X A^T$$

### کاربردهای مهندسی

**شکل‌دهی پرتو (Beamforming):** خروجی $y = w^{T}X$ (بردار وزن اعمال‌شده به آرایه حسگر)
- $E[y] = w^{T}\mu$
- $\operatorname{Var}(y) = w^{T}\Sigma w$

**سفیدسازی:** ماتریس $W$ را بیابید که در آن $W\Sigma W^{T} = I$ (داده را ناهمبسته می‌کند)
- $W = \Sigma^{-1/2}$

**تحلیل مؤلفه‌های اصلی:** چرخش برای قطری‌کردن $\Sigma$

---

## 10.8 مثال‌های MATLAB

### مثال 1: تولید گاوسی چندمتغیره

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

### مثال 2: تبدیل خطی

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

### مثال 3: آرایه حسگر با نویز همبسته

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

## 10.9 مسائل تمرینی

### مسئله 1
بردار تصادفی $X = [X_{1},\, X_{2},\, X_{3}]^{T}$ با $\mu = [1,\, 0,\, -1]^{T}$ و ماتریس کوواریانس $\Sigma = \begin{bmatrix} 4 & 1 & 0 \\ 1 & 2 & -1 \\ 0 & -1 & 3 \end{bmatrix}$.
(a) $\operatorname{Var}(X_{1} + X_{2} + X_{3})$ را بیابید. (ب) $\operatorname{Cov}(X_{1},\, X_{2}+X_{3})$ را بیابید. (ج) آیا $X_{1}$ و $X_{3}$ ناهمبسته هستند؟

**حل:**
(a) $\operatorname{Var}(X_{1}+X_{2}+X_{3}) = 1^{T}\Sigma 1 = 4+2+3 + 2(1) + 2(0) + 2(-1) = 9 + 0 = 9$
توجه: $= \sum_{ij}$ تمام درایه‌ها $= 4+1+0+1+2+(-1)+0+(-1)+3 = 9$
(b) $\operatorname{Cov}(X_{1},\, X_{2}+X_{3}) = \operatorname{Cov}(X_{1},\,X_{2}) + \operatorname{Cov}(X_{1},\,X_{3}) = 1 + 0 = 1$
(c) $\operatorname{Cov}(X_{1},\,X_{3}) = 0$، بنابراین بله، $X_{1}$ و $X_{3}$ ناهمبسته هستند.

### مسئله 2
$X \sim N(0,\, \Sigma)$ با $\Sigma = \begin{bmatrix} 1 & \rho \\ \rho & 1 \end{bmatrix}$. توزیع شرطی $X_{1} \mid X_{2} = x_{2}$ را بیابید.

**حل:** برای گاوسی دوبعدی:
$X_{1} \mid X_{2} = x_{2} \sim N\left(\mu_{1} + \rho \left(\frac{\sigma_{1}}{\sigma_{2}}\right)(x_{2}-\mu_{2}),\, \sigma_{1}^{2}(1-\rho^{2})\right)$
$= N(\rho x_{2},\, 1-\rho^{2})$

میانگین شرطی تابعی خطی از $x_{2}$ است و واریانس شرطی با ضریب $(1-\rho^{2})$ کاهش می‌یابد.

### مسئله 3
کد MATLAB را برای تولید یک بردار گاوسی سه‌بعدی با $\mu$ و $\Sigma$ معلوم، محاسبه کوواریانس نمونه و بررسی آن بنویسید.

**حل:**
```matlab
mu = [1; 0; -1];
Sigma = [4 1 0; 1 2 -1; 0 -1 3];
N = 50000;
X = mvnrnd(mu', Sigma, N);
fprintf('Sample mean: '); disp(mean(X)');
fprintf('Sample cov:\n'); disp(cov(X));
```

---

*ماژول بعدی: [ماژول 11 — قضیه حد مرکزی](module11-central-limit-theorem.md)*
<!-- TRANSLATION END -->
