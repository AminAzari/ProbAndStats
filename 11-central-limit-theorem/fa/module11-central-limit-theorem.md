<!-- TRANSLATION START -->
# ماژول 11 — قضیه حد مرکزی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. قانون اعداد بزرگ را بیان و توضیح دهند
2. قضیه حد مرکزی را بیان و به کار ببرند
3. توضیح دهند چرا توزیع‌های گاوسی این‌قدر در مهندسی فراوان‌اند
4. از CLT برای تقریب‌زنی استفاده کنند
5. CLT را به‌صورت بصری با شبیه‌سازی‌های مونت‌کارلوی MATLAB نشان دهند

---

## 11.1 مقدمه: چرا گاوسی همه‌جا هست؟

توزیع گاوسی در موارد زیر ظاهر می‌شود:
- نویز حرارتی، خطاهای اندازه‌گیری، رواداری‌های ساخت، تداخل تجمعی، نوسان‌های سهام

**دلیل:** همه این کمیت‌ها جمع‌ها (یا میانگین‌ها)ی بسیاری از اثرهای تصادفی کوچک و مستقل هستند. CLT همگرایی به گاوسی را صرف‌نظر از توزیع زیربنایی تضمین می‌کند.

---

## 11.2 قانون اعداد بزرگ (LLN)

### قانون ضعیف اعداد بزرگ

برای $X_{1},\, X_{2},\, \ldots,\, X_{n}$ مستقل و هم‌توزیع $(i.i.d.)$ با میانگین $\mu$ و واریانس متناهی $\sigma^{2}$:

$$\bar{X}_n = \frac{1}{n}\sum_{i=1}^n X_i \xrightarrow{P} \mu \quad \text{as } n \to \infty$$

(«همگرایی در احتمال»: $P(\lvert \bar{X}_{n} - \mu\rvert > \varepsilon) \to 0$ برای هر $\varepsilon > 0$)

### قانون قوی اعداد بزرگ

$$\bar{X}_n \xrightarrow{a.s.} \mu \quad \text{as } n \to \infty$$

(«همگرایی تقریباً مطمئن»: $P(\bar{X}_{n} \to \mu) = 1$)

### اهمیت مهندسی

- **میانگین نمونه با رشد اندازه نمونه به میانگین واقعی همگرا می‌شود**
- برآورد میانگین‌ها از داده را توجیه می‌کند
- بنیاد شبیه‌سازی مونت‌کارلو (شبیه‌سازی بارها → میانگین همگرا می‌شود)
- این گزاره را توجیه می‌کند: «اگر به‌قدر کافی اندازه‌گیری کنم، میانگین من به مقدار واقعی نزدیک خواهد بود»

### با چه سرعتی؟

بر اساس چبیشف: $P(\lvert \bar{X}_{n} - \mu\rvert > \varepsilon) \le \frac{\sigma^{2}}{n\varepsilon^{2}}$

برای اطمینان 95% از این‌که خطا $< \varepsilon$ باشد: به $n \ge \frac{\sigma^{2}}{0.05 \cdot \varepsilon^{2}}$ نیاز است

---

## 11.3 قضیه حد مرکزی (CLT)

### بیان صوری

برای $X_{1},\, X_{2},\, \ldots,\, X_{n}$ مستقل و هم‌توزیع $(i.i.d.)$ با میانگین $\mu$ و واریانس متناهی $\sigma^{2}$:

$$Z_n = \frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} = \frac{\sum_{i=1}^n X_i - n\mu}{\sigma\sqrt{n}} \xrightarrow{d} N(0,1)$$

### شکل عملی

برای $n$ بزرگ:
$$\bar{X}_n \approx N\left(\mu, \frac{\sigma^2}{n}\right)$$

$$S_n = \sum_{i=1}^n X_i \approx N(n\mu, n\sigma^2)$$

### شرایط
1. $X_{1},\, \ldots,\, X_{n}$ مستقل هستند
2. هم‌توزیع هستند (با CLT لیپانوف/لینده‌برگ می‌توان این شرط را سست کرد)
3. میانگین متناهی $\mu$ و واریانس متناهی $\sigma^{2}$
4. $n$ «به‌قدر کافی بزرگ» است (قاعده سرانگشتی: $n \ge 30$، اما به شکل توزیع بستگی دارد)

### آنچه CLT نیاز ندارد
- نیاز ندارد $X_{i}$ گاوسی باشد
- نیاز ندارد $X_{i}$ پیوسته باشد
- نیاز ندارد هیچ توزیع خاصی داشته باشد

---

## 11.4 نرخ همگرایی

CLT با چه سرعتی «اثر می‌گذارد»؟

| توزیع اصلی | $n$ لازم برای تقریب خوب |
|----------------------|--------------------------------|
| متقارن (یکنواخت) | $n \approx 5-10$ |
| کمی چوله | $n \approx 20-30$ |
| به‌شدت چوله (نمایی) | $n \approx 30-50$ |
| به‌غایت چوله | $n \approx 50-100+$ |

**قاعده:** توزیع‌های متقارن‌تر → همگرایی سریع‌تر.

---

## 11.5 نمایش بصری

### جمع متغیرهای تصادفی یکنواخت → گاوسی

- $n = 1$: یکنواخت (مسطح)
- $n = 2$: مثلثی
- $n = 3$: شروع به زنگوله‌ای‌شدن می‌کند
- $n = 5$: تقریباً گاوسی
- $n = 12$: عملاً از گاوسی قابل تفکیک نیست

### جمع متغیرهای تصادفی نمایی → گاوسی

- $n = 1$: نمایی (به‌شدت چوله به راست)
- $n = 3$: همچنان چوله اما کمتر $(\mathrm{Gamma}(3))$
- $n = 10$: تقریباً متقارن، زنگوله‌ای
- $n = 30$: اساساً گاوسی

### دوجمله‌ای → تقریب نرمال

$\mathrm{Binomial}(n,\, p) \approx N(np,\, np(1-p))$ برای $n$ بزرگ

این همان CLT اعمال‌شده به جمع آزمایش‌های برنولی است!
- تصحیح پیوستگی: $P(X \le k) \approx \Phi \left(\frac{k + 0.5 - np}{\sqrt{np(1-p)}}\right)$

---

## 11.6 اهمیت مهندسی

### چرا نویز حرارتی گاوسی است

نویز حرارتی = اثر تجمعی میلیاردها الکترون که به‌طور تصادفی حرکت می‌کنند. سهم هر الکترون ناچیز و مستقل است → CLT اعمال می‌شود → نویز گاوسی است.

### چرا تداخل تجمعی گاوسی است

در یک شبکه سلولی، تداخل کل = جمع سیگنال‌های بسیاری از کاربران مستقل. $\mathrm{CLT}$ → تداخل ≈ گاوسی (برای کاربران زیاد).

### چرا خطاهای اندازه‌گیری گاوسی هستند

خطای کل = جمع بسیاری از منابع خطای کوچک و مستقل (کالیبراسیون، کوانتیزاسیون، گرمایی، لرزش و ...) $\to \mathrm{CLT}$ → خطا ≈ گاوسی.

### چرا تغییرات ساخت گاوسی هستند

مقدار مقاومت = مقدار نامی + جمع بسیاری از تغییرات کوچک فرایند (دوپینگ، حکاکی، دما و ...) $\to \mathrm{CLT}$ → مقدار ≈ گاوسی حول مقدار نامی.

---

## 11.7 نمایش‌های مونت‌کارلوی MATLAB

### مثال 1: CLT با توزیع یکنواخت

```matlab
%% CLT: Sum of Uniform RVs approaches Gaussian
N_experiments = 50000;
n_values = [1, 2, 5, 12, 30];

figure;
for idx = 1:length(n_values)
    n = n_values(idx);
    
    % Generate sum of n Uniform(0,1) RVs
    X = sum(rand(n, N_experiments), 1);
    
    % Standardize: Z = (X - nμ)/(σ√n) where μ=0.5, σ²=1/12
    mu_sum = n * 0.5;
    sigma_sum = sqrt(n / 12);
    Z = (X - mu_sum) / sigma_sum;
    
    subplot(2, 3, idx);
    histogram(Z, 60, 'Normalization', 'pdf');
    hold on;
    z = linspace(-4, 4, 200);
    plot(z, normpdf(z), 'r', 'LineWidth', 2);
    title(sprintf('n = %d', n));
    xlabel('Standardized Sum'); xlim([-4 4]);
    if idx == 1, ylabel('PDF'); end
end
sgtitle('CLT: Sum of Uniform RVs → Gaussian');
```

### مثال 2: CLT با توزیع نمایی

```matlab
%% CLT: Sample Mean of Exponential(1) converges to Gaussian
lambda = 1;  mu = 1;  sigma = 1;
N_experiments = 100000;
n_values = [1, 3, 10, 30, 100];

figure;
for idx = 1:length(n_values)
    n = n_values(idx);
    
    % Generate sample means
    X = exprnd(1, n, N_experiments);
    X_bar = mean(X, 1);
    
    subplot(2, 3, idx);
    histogram(X_bar, 80, 'Normalization', 'pdf');
    hold on;
    x = linspace(0, max(X_bar), 200);
    plot(x, normpdf(x, mu, sigma/sqrt(n)), 'r', 'LineWidth', 2);
    title(sprintf('n = %d', n));
    xlabel('Sample Mean');
end
sgtitle('CLT: Mean of Exponential → Gaussian');
```

### مثال 3: تقریب نرمال دوجمله‌ای

```matlab
%% Binomial(n,p) approximated by Gaussian
n = 50; p = 0.3;
k = 0:n;
pmf_binom = binopdf(k, n, p);

% Normal approximation
mu_approx = n*p;
sigma_approx = sqrt(n*p*(1-p));
pdf_normal = normpdf(k, mu_approx, sigma_approx);

figure;
bar(k, pmf_binom, 'FaceAlpha', 0.5);
hold on;
plot(k, pdf_normal, 'r-', 'LineWidth', 2);
xlabel('k'); ylabel('Probability');
title(sprintf('Binomial(%d, %.1f) vs Normal(%.1f, %.2f)', n, p, mu_approx, sigma_approx^2));
legend('Binomial (exact)', 'Normal approximation');
```

### مثال 4: نمایش کامل CLT با مونت‌کارلو

```matlab
%% Full CLT demonstration: non-Gaussian → Gaussian via averaging
% Generate N samples from a VERY non-Gaussian distribution
% (mixture: 80% from Exp(1), 20% from Exp(0.1))
N_experiments = 100000;
n_samples = 50;

% Generate raw data
raw = zeros(n_samples, N_experiments);
for i = 1:N_experiments
    mask = rand(n_samples, 1) < 0.8;
    raw(mask, i) = exprnd(1, sum(mask), 1);
    raw(~mask, i) = exprnd(10, sum(~mask), 1);
end

% True mean and variance of mixture
mu_true = 0.8*1 + 0.2*10;  % = 2.8
var_true = 0.8*(1+1^2) + 0.2*(100+10^2) - mu_true^2;  % Second moment - mean^2

% Compute sample means
X_bar = mean(raw, 1);

figure;
subplot(2,1,1);
histogram(raw(:,1), 100, 'Normalization', 'pdf');
title('Original Distribution (highly skewed mixture)');
xlabel('X'); ylabel('PDF');

subplot(2,1,2);
histogram(X_bar, 100, 'Normalization', 'pdf');
hold on;
x = linspace(min(X_bar), max(X_bar), 200);
plot(x, normpdf(x, mu_true, sqrt(var_true/n_samples)), 'r', 'LineWidth', 2);
title(sprintf('Sample Mean (n=%d) — Gaussian by CLT!', n_samples));
xlabel('Sample Mean'); ylabel('PDF');
legend('Empirical', 'Gaussian approximation');
```

---

## 11.8 تقریب‌زنی با استفاده از CLT

### رویه کلی

برای تقریب $P(S_{n} \le x)$ که در آن $S_{n} = X_{1} + \ldots + X_{n}$:
1. $\mu = E[X_{i}]$، $\sigma^{2} = \operatorname{Var}(X_{i})$ را محاسبه کنید
2. $S_{n} \approx N(n\mu,\, n\sigma^{2})$
3. $P(S_{n} \le x) \approx \Phi \left(\frac{x - n\mu}{\sigma \sqrt{n}}\right)$

### تصحیح پیوستگی (برای متغیرهای تصادفی گسسته)

هنگام تقریب‌زدن یک جمع گسسته با گاوسی:
- $P(X \le k) \approx \Phi \left(\frac{k + 0.5 - n\mu}{\sigma \sqrt{n}}\right)$
- $P(X = k) \approx \Phi \left(\frac{k + 0.5 - n\mu}{\sigma \sqrt{n}}\right) - \Phi \left(\frac{k - 0.5 - n\mu}{\sigma \sqrt{n}}\right)$

### مثال: خطاهای بسته

1000 بسته، احتمال خطای هر یک 0.02. $X$ = مجموع خطاها.
دقیق: $X \sim \mathrm{Binomial}(1000,\, 0.02)$
تقریب CLT: $X \approx N(20,\, 19.6)$

$P(X > 30) \approx 1 - \Phi \left(\frac{30.5 - 20}{\sqrt{19.6}}\right) = 1 - \Phi (2.37) = 0.0089$

---

## 11.9 مسائل تمرینی

### مسئله 1
یک کارخانه پیچ تولید می‌کند. طول $X$ ~ توزیعی با $\mu = 10\,\mathrm{cm}$ و $\sigma = 0.2\,\mathrm{cm}$. یک دسته 100 پیچی اندازه‌گیری می‌شود.
(a) توزیع تقریبی $\bar{X}$ چیست؟ (ب) $P(\bar{X} > 10.05)$؟ (ج) $P(9.96 < \bar{X} < 10.04)$؟

**حل:**
(a) $\bar{X} \sim N\left(10,\, \frac{0.04}{100}\right) = N(10,\, 0.0004)$، $\sigma_{\bar{X}} = 0.02$
(b) $P(\bar{X} > 10.05) = P(Z > 2.5) = 0.0062$
(c) $P(9.96 < \bar{X} < 10.04) = \Phi (2) - \Phi (-2) = 0.9545$

### مسئله 2
یک لینک مخابراتی 10000 بیت با $\mathrm{BER} = 0.001$ دارد. با استفاده از CLT مقدار $P(\text{بیش از 15 خطا})$ را تقریب بزنید.

**حل:** $X \sim \mathrm{Binomial}(10000,\, 0.001)$. $\mu = 10$، $\sigma^{2} = 9.99$.
$P(X > 15) \approx 1 - \Phi \left(\frac{15.5-10}{\sqrt{9.99}}\right) = 1 - \Phi (1.74) = 0.0409$

### مسئله 3
کد MATLAB را برای بررسی CLT برای توزیع کای‌دو(1) (بسیار چوله) بنویسید و همگرایی را برای $n = 2,\,5,\,10,\,50$ نشان دهید.

**حل:**
```matlab
N = 100000; ns = [2 5 10 50];
figure;
for i = 1:4
    X_bar = mean(chi2rnd(1, ns(i), N), 1);
    subplot(2,2,i);
    histogram(X_bar, 80, 'Normalization', 'pdf'); hold on;
    x = linspace(0, 3, 200);
    plot(x, normpdf(x, 1, sqrt(2/ns(i))), 'r', 'LineWidth', 2);
    title(sprintf('n=%d', ns(i)));
end
sgtitle('CLT for χ²(1): Mean→Gaussian');
```

---

*ماژول بعدی: [ماژول 12 — مقدمه‌ای بر آمار](module12-intro-statistics.md)*
<!-- TRANSLATION END -->
