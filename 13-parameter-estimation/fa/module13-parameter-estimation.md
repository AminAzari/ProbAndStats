<!-- TRANSLATION START -->
# ماژول 13 — برآورد پارامتر

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. میان برآوردگر (قاعده) و برآورد (عدد) تمایز قائل شوند
2. ویژگی‌های برآوردگر را ارزیابی کنند: اریبی، سازگاری، کارایی، MSE
3. روش گشتاورها را به کار ببرند
4. برآورد بیشینه درست‌نمایی (MLE) را به کار ببرند
5. برآوردگرها را با استفاده از MSE و توازن اریبی-واریانس مقایسه کنند

---

## 13.1 مقدمه

داده $X_{1},\, \ldots,\, X_{n}$ را از توزیعی با پارامتر نامعلوم $\theta$ مشاهده می‌کنیم. **برآورد پارامتر** روشی برای استنباط $\theta$ از داده به ما می‌دهد.

مثال‌های مهندسی:
- برآورد واریانس نویز $\sigma^{2}$ از نمونه‌های ثبت‌شده
- برآورد نرخ خرابی $\lambda$ از عمر قطعات
- برآورد بهره کانال از قدرت سیگنال دریافتی
- برآورد سطح میانگین سیگنال از اندازه‌گیری‌ها

---

## 13.2 برآوردگر در برابر برآورد

### تعاریف

| اصطلاح | نوع | تعریف | مثال |
|------|------|-----------|---------|
| **برآوردگر** $\hat{\theta}$ | متغیر تصادفی (تابع داده) | قاعده اعمال‌شده به داده | $\hat{\theta} = \bar{X} = \frac{1}{n}\sum X_{i}$ |
| **برآورد** | عدد | مقدار حاصل از یک نمونه مشخص | $\hat{\theta} = 3.47$ (از یک مجموعه داده) |

### نکته کلیدی

**برآوردگر** یک متغیر تصادفی است — توزیع نمونه‌گیری دارد.
**برآورد** یک تحقق از آن متغیر تصادفی است.

**قیاس:** «میانگین بگیر» برآوردگر است (قاعده). «میانگین برابر 3.47 بود» برآورد است (نتیجه).

---

## 13.3 ویژگی‌های برآوردگرها

### اریبی

$$B(\hat{\theta}) = E[\hat{\theta}] - \theta$$

- **نااریب:** $B(\hat{\theta}) = 0$، یعنی $E[\hat{\theta}] = \theta$ (به‌طور میانگین درست)
- **اریب:** $E[\hat{\theta}] \ne \theta$ (خطای نظام‌مند)

مثال‌ها:
- $\bar{X}$ برای $\mu$ نااریب است: $E[\bar{X}] = \mu$ ✓
- $S^{2}$ (با $n-1$) برای $\sigma^{2}$ نااریب است: $E[S^{2}] = \sigma^{2}$ ✓
- $S$ (انحراف معیار نمونه) برای $\sigma$ اریب است: $E[S] \ne \sigma$ (اریبی اندک به سمت پایین)

### سازگاری

$\hat{\theta}_{n}$ **سازگار** است اگر $\hat{\theta}_{n} \to \theta$ در احتمال وقتی $n \to \infty$.

معنای عملی: با داده کافی، برآوردگر به‌طور دلخواه به مقدار واقعی نزدیک می‌شود.

شرط کافی: اگر اریبی → 0 و واریانس → 0 وقتی $n \to \infty$، آنگاه سازگار است.

### کارایی

در میان تمام برآوردگرهای نااریب، برآوردگر **کارا** کمترین واریانس را دارد.

**کران پایین کرامر-رائو** (CRLB) کمترین واریانس ممکن را می‌دهد:
$$\text{Var}(\hat{\theta}) \geq \frac{1}{nI(\theta)}$$

که در آن $I(\theta)$ = اطلاعات فیشر $= -E\left[\frac{\partial^{2}\ln f(X; \theta)}{\partial \theta^{2}}\right]$.

برآوردگری که به CRLB برسد **کارا** یا **کم‌واریانس نااریب (MVU)** نامیده می‌شود.

### میانگین مربعات خطا (MSE)

$$\text{MSE}(\hat{\theta}) = E[(\hat{\theta} - \theta)^2] = \text{Var}(\hat{\theta}) + [B(\hat{\theta})]^2$$

**$\operatorname{MSE} = \text{واریانس} + \text{اریبی}^{2}$**

این همان **توازن اریبی-واریانس** است: گاهی یک برآوردگر کمی اریب با واریانس بسیار کمتر، MSE کوچک‌تری از یک برآوردگر نااریب دارد.

---

## 13.4 روش گشتاورها (MoM)

### اصل

گشتاورهای جامعه را برابر گشتاورهای نمونه قرار دهید، سپس برای پارامترها حل کنید.

- گشتاور مرتبه $k$ جامعه: $\mu 'k = E[X^{k}]$
- گشتاور مرتبه $k$ نمونه: $m'k = \frac{1}{n}\sum X_{i}^{k}$

### رویه

1. پارامترها را بر حسب گشتاورهای جامعه بیان کنید
2. گشتاورهای جامعه را با گشتاورهای نمونه جایگزین کنید
3. برای برآوردهای پارامتر حل کنید

### مثال: توزیع نمایی

داده از $\mathrm{Exp}(\lambda)$. گشتاور جامعه: $E[X] = \frac{1}{\lambda}$.
برابر قرار دهید: $\frac{1}{n}\sum X_{i} = \frac{1}{\hat{\lambda}} \to \hat{\lambda}_{\mathrm{MoM}} = \frac{1}{\bar{X}}$

### مثال: توزیع گاوسی

داده از $N(\mu,\, \sigma^{2})$:
- گشتاور اول: $\hat{\mu} = \bar{X}$
- گشتاور مرکزی دوم: $\hat{\sigma}^{2} = \frac{1}{n}\sum (X_{i} - \bar{X})^{2}$ (توجه: اریب، از $n$ استفاده می‌کند نه $n-1$)

### مزایا/معایب

✅ ساده، همیشه قابل اعمال، جواب‌های فرم بسته اغلب در دسترس
❌ همیشه کارا نیست، می‌تواند برآوردهای اریب تولید کند، ممکن است مقادیر نامعتبر پارامتر بدهد

---

## 13.5 برآورد بیشینه درست‌نمایی (MLE)

### اصل

مقدار پارامتری را انتخاب کنید که داده مشاهده‌شده را **محتمل‌ترین** می‌کند.

### تابع درست‌نمایی

با داده $x_{1},\, \ldots,\, x_{n}$ از $f(x; \theta)$:

$$L(\theta) = \prod_{i=1}^n f(x_i; \theta)$$

### لگاریتم درست‌نمایی (معمولاً آسان‌تر)

$$\ell(\theta) = \ln L(\theta) = \sum_{i=1}^n \ln f(x_i; \theta)$$

### رویه MLE

1. لگاریتم درست‌نمایی $\ell (\theta)$ را بنویسید
2. مشتق بگیرید: $\frac{d\ell}{d\theta} = 0$
3. برای $\hat{\theta}_{\mathrm{MLE}}$ حل کنید
4. بررسی کنید که بیشینه است $\left(\frac{d^{2}\ell}{d\theta^{2}} < 0\right)$

### ویژگی‌های MLE

1. **سازگار** (به مقدار واقعی همگرا می‌شود)
2. **به‌طور مجانبی نااریب** (اریبی → 0 وقتی $n \to \infty$)
3. **به‌طور مجانبی کارا** (برای $n$ بزرگ به CRLB می‌رسد)
4. **ناوردا:** اگر $\hat{\theta}$ برآوردگر MLE برای $\theta$ باشد، آنگاه $g(\hat{\theta})$ برآوردگر MLE برای $g(\theta)$ است

### مثال: MLE برای میانگین گاوسی

داده $X_{1},\, \ldots,\, X_{n} \sim N(\mu,\, \sigma^{2})$ با $\sigma^{2}$ معلوم.

$\ell (\mu) = -\frac{n}{2}\ln (2\pi \sigma^{2}) - \frac{1}{2\sigma^{2}}\sum (x_{i} - \mu)^{2}$

$\frac{d\ell}{d\mu} = \frac{1}{\sigma^{2}}\sum (x_{i} - \mu) = 0 \to \hat{\mu}_{\mathrm{MLE}} = \bar{X}$

### مثال: MLE برای نرخ نمایی

داده $X_{1},\, \ldots,\, X_{n} \sim \mathrm{Exp}(\lambda)$.

$\ell (\lambda) = n \cdot \ln (\lambda) - \lambda \cdot \sum x_{i}$

$\frac{d\ell}{d\lambda} = \frac{n}{\lambda} - \sum x_{i} = 0 \to \hat{\lambda}_{\mathrm{MLE}} = \frac{n}{\sum x_{i}} = \frac{1}{\bar{X}}$

### مثال: MLE برای واریانس گاوسی

داده $X_{1},\, \ldots,\, X_{n} \sim N(\mu,\, \sigma^{2})$، $\mu$ معلوم.

$\ell (\sigma^{2}) = -\frac{n}{2}\ln (2\pi \sigma^{2}) - \frac{1}{2\sigma^{2}}\sum (x_{i} - \mu)^{2}$

$\frac{d\ell}{d(\sigma^{2})} = -\frac{n}{2\sigma^{2}} + \frac{1}{2\sigma^{4}}\sum (x_{i} - \mu)^{2} = 0$

$\hat{\sigma}^{2}_{\mathrm{MLE}} = \frac{1}{n}\sum (x_{i} - \mu)^{2}$

توجه: با $\mu$ نامعلوم، $\hat{\sigma}^{2}_{\mathrm{MLE}} = \frac{1}{n}\sum (x_{i} - \bar{x})^{2}$ — اریب! (از $n$ استفاده می‌کند، نه $n-1$)

---

## 13.6 مثال‌های مهندسی

### برآورد واریانس نویز

**مسئله:** $n$ نمونه نویز اندازه‌گیری کنید. توان نویز $\sigma^{2}$ را برآورد کنید.

**داده:** $x_{1},\, \ldots,\, x_{n}$ (نمونه‌های ولتاژ نویز، با فرض میانگین صفر)

**MLE:** $\hat{\sigma}^{2} = \frac{1}{n}\sum x_{i}^{2}$ (اریب با ضریب $\frac{n-1}{n}$)
**نااریب:** $S^{2} = \frac{1}{n-1}\sum (x_{i} - \bar{x})^{2}$ یا اگر $\mu = 0$ معلوم باشد: $\frac{1}{n}\sum x_{i}^{2}$ در واقع برای $E[X^{2}] = \sigma^{2}$ نااریب است

### برآورد نرخ خرابی

**مسئله:** 20 قطعه تا خرابی آزمایش می‌شوند. عمرها: $t_{1},\, \ldots,\, t_{20}$.

**مدل:** $T \sim \mathrm{Exp}(\lambda)$، پارامتر $\lambda$ (نرخ خرابی).

**MLE:** $\hat{\lambda} = \frac{20}{\sum t_{i}} = \frac{1}{\bar{T}}$

اگر $\bar{T} = 500$ ساعت باشد: $\hat{\lambda} = \frac{1}{500} = 0.002$ خرابی در ساعت.

### برآورد پارامتر کانال

**مسئله:** کانال محوشدگی رایلی. نمونه‌های توان مشاهده‌شده: $p_{1},\, \ldots,\, p_{n}$.

**مدل:** توان $P \sim \mathrm{Exp}\left(\frac{1}{\Omega}\right)$، که در آن $\Omega = E[P]$ = توان میانگین.

**MLE:** $\hat{\Omega} = \bar{P} = \frac{1}{n}\sum p_{i}$

---

## 13.7 مقایسه برآوردگرها

### توازن اریبی-واریانس

دو برآوردگر برای $\sigma^{2}$:
- $\hat{\theta}_{1} = S^{2}$ (نااریب، واریانس $= \frac{2\sigma^{4}}{n-1}$)
- $\hat{\theta}_{2} = \frac{1}{n}\sum (X_{i}-\bar{X})^{2}$ (اریب با $-\frac{\sigma^{2}}{n}$، واریانس $= \frac{2\sigma^{4}(n-1)}{n^{2}}$)

$\operatorname{MSE}(\hat{\theta}_{1}) = \frac{2\sigma^{4}}{n-1}$
$\operatorname{MSE}(\hat{\theta}_{2}) = \frac{2\sigma^{4}(n-1)}{n^{2}} + \frac{\sigma^{4}}{n^{2}} = \frac{\sigma^{4}(2n-1)}{n^{2}}$

برای هر $n \ge 2$: $\operatorname{MSE}(\hat{\theta}_{2}) < \operatorname{MSE}(\hat{\theta}_{1})$! برآوردگر اریب MLE در واقع خطای کل کوچک‌تری دارد.

---

## 13.8 مثال‌های MATLAB

### مثال 1: MLE برای توزیع نمایی

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

### مثال 2: MLE در برابر روش گشتاورها

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

### مثال 3: MLE برای پارامترهای گاوسی

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

## 13.9 مسائل تمرینی

### مسئله 1
داده از $N(\mu,\, 9)$: $\bar{x} = 4.2$، $n = 25$.
(a) MLE مربوط به $\mu$ را بنویسید. (ب) $\operatorname{Var}(\hat{\mu})$ چقدر است؟ (ج) آیا $\bar{X}$ برای $\mu$ کارا است؟

**حل:**
(a) $\hat{\mu}_{\mathrm{MLE}} = \bar{X} = 4.2$
(b) $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n} = \frac{9}{25} = 0.36$
(c) CRLB برای $N(\mu,\,\sigma^{2})$: $\frac{1}{n \cdot I(\mu)} = \frac{\sigma^{2}}{n} = 0.36$. $\bar{X}$ به CRLB می‌رسد → بله، کارا است.

### مسئله 2
عمر قطعات (ساعت): 120، 350، 200، 480، 90، 560، 310، 175، 420، 280.
(a) با فرض $\mathrm{Exp}(\lambda)$ مقدار $\lambda$ (نرخ خرابی) را با MLE برآورد کنید. (ب) MTTF را برآورد کنید.

**حل:**
(a) $\bar{X} = \frac{120+350+200+480+90+560+310+175+420+280}{10} = 298.5$.
$\hat{\lambda} = \frac{1}{298.5} = 0.00335$ خرابی در ساعت.
(b) $\mathrm{MTTF} = \frac{1}{\hat{\lambda}} = \bar{X} = 298.5$ ساعت.

### مسئله 3
برای داده $\mathrm{Poisson}(\lambda)$: $x_{1},\,\ldots,\,x_{n}$. MLE مربوط به $\lambda$ را استخراج کنید.

**حل:** $\ell (\lambda) = \sum [x_{i} \ln (\lambda) - \lambda - \ln (x_{i}!)] = \left(\sum x_{i}\right)\ln (\lambda) - n\lambda - \sum \ln (x_{i}!)$
$\frac{d\ell}{d\lambda} = \frac{\sum x_{i}}{\lambda} - n = 0 \to \hat{\lambda}_{\mathrm{MLE}} = \frac{1}{n}\sum x_{i} = \bar{X}$.
MLE برای نرخ پواسون همان میانگین نمونه است.

---

*ماژول بعدی: [ماژول 14 — بازه‌های اطمینان](module14-confidence-intervals.md)*
<!-- TRANSLATION END -->
