<!-- TRANSLATION START -->
# ماژول 9 — کوواریانس و همبستگی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. کوواریانس را محاسبه و تفسیر کنند
2. ضریب همبستگی را محاسبه و تفسیر کنند
3. میان استقلال و ناهمبستگی تمایز قائل شوند
4. ماتریس‌های همبستگی را بسازند و تفسیر کنند
5. تحلیل همبستگی را در مسائل مهندسی به کار ببرند

---

## 9.1 مقدمه: اندازه‌گیری روابط خطی

هنگامی که دو متغیر تصادفی مستقل نباشند، باید **نحوه ارتباط آن‌ها** را کمّی کنیم. کوواریانس و همبستگی **رابطه خطی** میان دو متغیر تصادفی را می‌سنجند.

مثال‌های مهندسی:
- نویز همبسته در کانال‌های حسگر مجاور
- اندازه‌گیری سیگنال در دو آنتن (همبستگی MIMO)
- ولتاژ و جریان در یک مدار (مرتبط از طریق امپدانس)
- دما و عملکرد قطعه (وابستگی گرمایی)

---

## 9.2 کوواریانس

### تعریف

$$\text{Cov}(X,Y) = E[(X - \mu_X)(Y - \mu_Y)] = E[XY] - E[X]E[Y]$$

### ویژگی‌ها

1. $\operatorname{Cov}(X,\, X) = \operatorname{Var}(X)$
2. $\operatorname{Cov}(X,\, Y) = \operatorname{Cov}(Y,\, X)$ (متقارن)
3. $\operatorname{Cov}(\text{aX} + b,\, \text{cY} + d)$ = ac $\cdot \operatorname{Cov}(X,\, Y)$
4. اگر $X$، $Y$ مستقل باشند $\to \operatorname{Cov}(X,\, Y) = 0$
5. $\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,\, Y)$
6. $\operatorname{Var}(X - Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) - 2\operatorname{Cov}(X,\, Y)$

### تفسیر علامت

| $\operatorname{Cov}(X,\,Y)$ | معنا |
|-----------|---------|
| > 0 | $X$ و $Y$ تمایل دارند با هم افزایش یابند |
| < 0 | وقتی $X$ افزایش می‌یابد، $Y$ تمایل به کاهش دارد |
| = 0 | رابطه خطی وجود ندارد (ناهمبسته) |

### مثال مهندسی: بهره تقویت‌کننده و خروجی

اگر بهره $G$ به‌طور تصادفی تغییر کند و ورودی در $V_{\mathrm{in}}$ ثابت باشد:
- خروجی: $V_{\mathrm{out}} = G \cdot V_{\mathrm{in}}$
- $\operatorname{Cov}(G,\, V_{\text{out}}) = \operatorname{Cov}(G,\, G \cdot V_{\mathrm{in}}) = V_{\mathrm{in}} \cdot \operatorname{Var}(G) > 0$

بهره بالاتر → خروجی بالاتر (کوواریانس مثبت).

---

## 9.3 ضریب همبستگی

### تعریف

$$\rho_{XY} = \frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}$$

### ویژگی‌ها

1. **کران‌دار:** $-1 \le \rho \le 1$
2. **$\rho = 1$:** رابطه خطی مثبت کامل ($Y$ = aX $+ b$، $a > 0$)
3. **$\rho = -1$:** رابطه خطی منفی کامل ($Y$ = aX $+ b$، $a < 0$)
4. **$\rho = 0$:** ناهمبسته (بدون رابطه خطی)
5. **بی‌بُعد:** برخلاف کوواریانس، $\rho$ به یکاها وابسته نیست

### مقیاس تفسیر

| بازه |$\rho$| | تفسیر |
|-----------|---------------|
| 0.0 – 0.2 | بسیار ضعیف/ناچیز |
| 0.2 – 0.4 | ضعیف |
| 0.4 – 0.6 | متوسط |
| 0.6 – 0.8 | قوی |
| 0.8 – 1.0 | بسیار قوی |

### مثال مهندسی: محوشدگی همبسته

دو مسیر آنتنی با همبستگی $\rho = 0.3$:
- همبستگی کم → تنوع به‌خوبی کار می‌کند (محوشدگی مستقل)
- همبستگی زیاد → بهره تنوع کاهش می‌یابد

طراحی سیستم MIMO نیازمند: $\rho < 0.5$ برای مالتی‌پلکسینگ فضایی مؤثر.

---

## 9.4 همبستگی متقابل

### تعریف

$$R_{XY} = E[XY]$$

### رابطه با کوواریانس

$$\text{Cov}(X,Y) = R_{XY} - \mu_X \mu_Y$$

اگر $X$ یا $Y$ میانگین صفر داشته باشد: $\operatorname{Cov}(X,\,Y) = E[XY] = R_{XY}$

### اهمیت مهندسی

همبستگی متقابل در موارد زیر بنیادی است:
- فیلترهای منطبق (گیرنده‌های همبستگی در مخابرات)
- پردازش سیگنال رادار (تشخیص تأخیر زمانی)
- آشکارسازی سیگنال (همبسته‌سازی دریافتی با الگوی معلوم)

---

## 9.5 استقلال در برابر ناهمبستگی

### تمایز حیاتی

| گزاره | همیشه درست است؟ |
|-----------|-------------|
| مستقل → ناهمبسته | ✅ بله |
| ناهمبسته → مستقل | ❌ خیر (به‌طور کلی) |
| ناهمبسته → مستقل (گاوسی مشترک) | ✅ بله |

### چرا مستقل → ناهمبسته

اگر $X$، $Y$ مستقل باشند: $E[XY] = E[X]E[Y]$، بنابراین $\operatorname{Cov}(X,\,Y) = E[XY] - E[X]E[Y] = 0$.

### مثال نقض: ناهمبسته اما وابسته

فرض کنید $X \sim N(0,\,1)$ و $Y = X^{2}$. آنگاه:
- $\operatorname{Cov}(X,\, Y) = E[XY] - E[X]E[Y] = E[X^{3}] - 0 = 0$ (چون $X$ متقارن است)
- اما $Y$ کاملاً توسط $X$ تعیین می‌شود → وابستگی حداکثری!

**$\rho = 0$ به معنای استقلال نیست** (به‌جز برای متغیرهای تصادفی گاوسی مشترک).

### استثنای گاوسی

برای متغیرهای تصادفی **گاوسی مشترک**:

$$\text{Uncorrelated} \Leftrightarrow \text{Independent}$$

به همین دلیل توزیع گاوسی در مهندسی بسیار راحت است — بررسی $\rho = 0$ برای اثبات استقلال کافی است.

### پیام مهندسی

در مخابرات با نویز گاوسی:
- اگر نمونه‌های نویز ناهمبسته باشند → مستقل هستند
- این موضوع تحلیل را به‌شدت ساده می‌کند
- برای تداخل غیرگاوسی: ناهمبستگی استقلال را تضمین نمی‌کند

---

## 9.6 ماتریس همبستگی

### تعریف برای بردار تصادفی $X = [X_{1},\, X_{2},\, \ldots,\, X_{n}]^{T}$

**ماتریس کوواریانس** $\Sigma$:

$$\Sigma_{ij} = \text{Cov}(X_i, X_j)$$

$$\Sigma = \begin{bmatrix} \text{Var}(X_1) & \text{Cov}(X_1,X_2) & \cdots \\ \text{Cov}(X_2,X_1) & \text{Var}(X_2) & \cdots \\ \vdots & & \ddots \end{bmatrix}$$

**ماتریس همبستگی** $R$ (نرمال‌شده):

$$R_{ij} = \rho_{X_i X_j} = \frac{\Sigma_{ij}}{\sqrt{\Sigma_{ii}\Sigma_{jj}}}$$

### ویژگی‌های ماتریس کوواریانس
1. متقارن: $\Sigma = \Sigma^{T}$
2. نیم‌معین مثبت: $x^{T}\Sigma x \ge 0$ برای تمام $x$
3. درایه‌های قطری = واریانس‌ها
4. درایه‌های غیرقطری = کوواریانس‌ها

### مثال مهندسی: سه حسگر

سه حسگر دما با:
- $\sigma_{1} = \sigma_{2} = \sigma_{3} = 1^{\circ}C$
- $\rho_{12} = 0.8$ (حسگرهای 1 و 2 نزدیک هم هستند)
- $\rho_{13} = 0.3$ (حسگر 3 دورتر است)
- $\rho_{23} = 0.4$

ماتریس همبستگی:
$$R = \begin{bmatrix} 1.0 & 0.8 & 0.3 \\ 0.8 & 1.0 & 0.4 \\ 0.3 & 0.4 & 1.0 \end{bmatrix}$$

---

## 9.7 واریانس ترکیب‌های خطی

### فرمول کلی

$$\text{Var}\left(\sum_{i=1}^n a_i X_i\right) = \sum_{i=1}^n a_i^2 \text{Var}(X_i) + 2\sum_{i<j} a_i a_j \text{Cov}(X_i, X_j)$$

در شکل ماتریسی: $\operatorname{Var}(a^{T}X) = a^{T}\Sigma a$

### حالت خاص: جمع دو متغیر تصادفی

$\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,\,Y) = \sigma_{X}^{2} + \sigma_{Y}^{2} + 2\rho \sigma_{X}\sigma_{Y}$

| همبستگی | $\operatorname{Var}(X+Y)$ | اثر |
|-------------|----------|--------|
| $\rho = 1$ | $(\sigma_{X} + \sigma_{Y})^{2}$ | بیشینه — واریانس‌ها به‌صورت سازنده جمع می‌شوند |
| $\rho = 0$ | $\sigma_{X}^{2} + \sigma_{Y}^{2}$ | جمع مستقل |
| $\rho = -1$ | $(\sigma_{X} - \sigma_{Y})^{2}$ | کمینه — حذف متقابل |

### کاربرد مهندسی: میانگین‌گیری از حسگرهای همبسته

اگر $n$ حسگر واریانس برابر $\sigma^{2}$ و همبستگی زوجی $\rho$ داشته باشند:

$$\text{Var}(\bar{X}) = \frac{\sigma^2}{n}[1 + (n-1)\rho]$$

- اگر $\rho = 0$ (مستقل): $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n}$ (بهره کامل میانگین‌گیری)
- اگر $\rho = 1$ (یکسان): $\operatorname{Var}(\bar{X}) = \sigma^{2}$ (بدون بهره از میانگین‌گیری!)
- اگر $\rho = 0.5$، $n = 4$: $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}(1 + 3 \times 0.5)}{4} = 0.625\sigma^{2}$ (بهره کاهش‌یافته)

---

## 9.8 مثال‌های MATLAB

### مثال 1: محاسبه همبستگی

```matlab
%% Sample Correlation: Correlated Sensor Data
N = 10000;
rho = 0.7;  sigma1 = 2;  sigma2 = 3;

% Generate correlated Gaussians
Z1 = randn(1, N);
Z2 = randn(1, N);
X = sigma1 * Z1;
Y = sigma2 * (rho*Z1 + sqrt(1-rho^2)*Z2);

% Compute sample statistics
cov_xy = mean(X.*Y) - mean(X)*mean(Y);
rho_hat = cov_xy / (std(X)*std(Y));

fprintf('True ρ = %.2f, Estimated ρ = %.4f\n', rho, rho_hat);
fprintf('Cov(X,Y): Theory=%.2f, Estimated=%.4f\n', rho*sigma1*sigma2, cov_xy);

% Scatter plot
figure;
scatter(X, Y, 1, 'b', '.');
xlabel('X (Sensor 1)'); ylabel('Y (Sensor 2)');
title(sprintf('Correlated Sensors (ρ = %.2f)', rho));
grid on;
```

### مثال 2: ناهمبسته اما وابسته

```matlab
%% Demonstration: ρ = 0 does NOT mean independent
N = 100000;
X = randn(1, N);
Y = X.^2;        % Y completely depends on X

rho_hat = corr(X', Y');
fprintf('Correlation ρ(X, X²) = %.4f ≈ 0 (UNCORRELATED)\n', rho_hat);
fprintf('But Y = X² is perfectly determined by X (DEPENDENT!)\n');

figure;
scatter(X, Y, 1, '.'); xlabel('X'); ylabel('Y = X²');
title('Uncorrelated but Dependent!');
```

### مثال 3: مصورسازی ماتریس همبستگی

```matlab
%% Correlation Matrix for Multiple Sensors
n_sensors = 5;
% Create correlation matrix with decaying correlation
R = zeros(n_sensors);
for i = 1:n_sensors
    for j = 1:n_sensors
        R(i,j) = 0.9^abs(i-j);  % Correlation decays with distance
    end
end

% Generate correlated samples
L = chol(R, 'lower');   % Cholesky decomposition
N = 10000;
Z = randn(n_sensors, N);
X = L * Z;              % Correlated samples

% Estimated correlation matrix
R_hat = corrcoef(X');

figure;
subplot(1,2,1); imagesc(R); colorbar;
title('Theoretical R'); xlabel('Sensor'); ylabel('Sensor');
subplot(1,2,2); imagesc(R_hat); colorbar;
title('Estimated R'); xlabel('Sensor'); ylabel('Sensor');
```

---

## 9.9 مسائل تمرینی

### مسئله 1
برای $X$ و $Y$ داریم: $E[X] = 2$، $E[Y] = 3$، $\operatorname{Var}(X) = 4$، $\operatorname{Var}(Y) = 9$، $E[XY] = 8$.
(a) $\operatorname{Cov}(X,\,Y)$ را بیابید. (ب) $\rho$ را بیابید. (ج) $\operatorname{Var}(X+Y)$ را بیابید. (د) $\operatorname{Var}(2X-3Y)$ را بیابید.

**حل:**
(a) $\operatorname{Cov}(X,\,Y) = E[XY] - E[X]E[Y] = 8 - 2(3) = 2$
(b) $\rho = \frac{2}{\sqrt{4} \cdot \sqrt{9}} = \frac{2}{6} = \frac{1}{3}$
(c) $\operatorname{Var}(X+Y) = 4 + 9 + 2(2) = 17$
(d) $\operatorname{Var}(2X-3Y) = 4(4) + 9(9) + 2(2)(-3)(2) = 16 + 81 - 24 = 73$

### مسئله 2
چهار حسگر یکسان $(\sigma^{2} = 1)$ همبستگی زوجی $\rho = 0.5$ دارند. $\operatorname{Var}(\bar{X})$ چقدر است؟

**حل:** $\operatorname{Var}(\bar{X}) = \frac{\sigma^{2}}{n} \cdot [1 + (n-1)\rho] = \frac{1}{4} \cdot [1 + 3(0.5)] = 0.25 \times 2.5 = 0.625$

مقایسه کنید: اگر مستقل باشند $(\rho = 0)$: $\operatorname{Var}(\bar{X}) = 0.25$. همبستگی اثربخشی میانگین‌گیری را کاهش می‌دهد.

### مسئله 3
با داده $X \sim N(0,\,1)$، تعریف کنید $Y = X$ هنگامی که $\lvert X\rvert < 1$ و $Y = -X$ هنگامی که $\lvert X\rvert \ge 1$. نشان دهید در این حالت $\operatorname{Cov}(X,\,Y) \ne 0$ اما $E[X \cdot Y]$ قابل محاسبه است.

**حل:** $E[XY] = E[X^{2} \cdot 1(\lvert X\rvert < 1)] + E[-X^{2} \cdot 1(\lvert X\rvert \ge 1)] = E[X^{2} \cdot 1(\lvert X\rvert < 1)] - E[X^{2} \cdot 1(\lvert X\rvert \ge 1)]$
$= P(\lvert X\rvert < 1)E[X^{2}\lvert \rvert X \mid < 1] - P(\lvert X\rvert \ge 1)E[X^{2}\lvert \rvert X \mid \ge 1]$
این مقدار ناصفر است زیرا $E[X^{2}\lvert \rvert X \mid < 1] \ne E[X^{2}\lvert \rvert X \mid \ge 1]$، بنابراین $\operatorname{Cov}(X,\,Y) = E[XY] - 0 \ne 0$.
$X$ و $Y$ به‌روشنی وابسته هستند ($Y$ از $X$ تعریف شده است) و در این حالت همبسته نیز هستند.

---

*ماژول بعدی: [ماژول 10 — چند متغیر تصادفی](module10-multiple-random-variables.md)*
<!-- TRANSLATION END -->
