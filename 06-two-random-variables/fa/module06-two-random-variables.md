<!-- TRANSLATION START -->
# ماژول 6 — دو متغیر تصادفی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. PMF، PDF و CDF مشترک را تعریف و محاسبه کنند
2. توزیع‌های حاشیه‌ای را از توزیع‌های مشترک استخراج کنند
3. توزیع‌های شرطی را محاسبه کنند
4. استقلال دو متغیر تصادفی را آزمون کنند
5. امیدهای ریاضی شرطی را محاسبه کنند
6. توزیع‌های مشترک را در MATLAB مصورسازی کنند

---

## 6.1 مقدمه: چرا توزیع‌های مشترک را مطالعه کنیم؟

سیستم‌های مهندسی واقعی شامل **چند کمیت نامعین برهم‌کنش‌کننده** هستند:
- ولتاژ و جریان در یک مدار نویزی
- سیگنال و نویز در گیرنده
- دما و مقاومت یک حسگر
- کانال $I$ و کانال $Q$ یک سیگنال مخابراتی

درک **رفتار مشترک** چیزهایی را به ما می‌گوید که توزیع‌های حاشیه‌ای نمی‌توانند — مانند همبستگی، وابستگی و رفتار شرطی.

---

## 6.2 PMF مشترک (حالت گسسته)

### تعریف

برای متغیرهای تصادفی گسسته $X$، $Y$:
$$p_{X,Y}(x,y) = P(X = x, Y = y)$$

### ویژگی‌ها
1. $p_{X,Y}(x,\,y) \ge 0$
2. $\sum_{x} \Sigma_{y} p_{X,Y}(x,\,y) = 1$

### مصورسازی: جدول احتمال

**مثال:** $X$ = تعداد ارسال‌های مجدد (0,1,2)، $Y$ = دسته تأخیر تأیید ($1 = \text{سریع}$، $2 = \text{کند}$)

| | $Y = 1$ | $Y = 2$ | $p_{X}(x)$ |
|---|---|---|---|
| $X = 0$ | 0.30 | 0.10 | 0.40 |
| $X = 1$ | 0.15 | 0.20 | 0.35 |
| $X = 2$ | 0.05 | 0.20 | 0.25 |
| $p_{Y}(y)$ | 0.50 | 0.50 | 1.00 |

---

## 6.3 PDF مشترک (حالت پیوسته)

### تعریف

برای متغیرهای تصادفی پیوسته $X$، $Y$، PDF مشترک $f_{X,Y}(x,\,y)$ در رابطه زیر صدق می‌کند:
$$P((X,Y) \in A) = \iint_A f_{X,Y}(x,y) \, dx \, dy$$

### ویژگی‌ها
1. $f_{X,Y}(x,\,y) \ge 0$
2. $\int \int f_{X,Y}(x,\,y)\,dx\,dy = 1$

### مصورسازی

PDF مشترک یک **رویه** روی صفحه $(x,\,y)$ است. احتمال = حجم زیر رویه روی یک ناحیه.

### مثال مهندسی: گاوسی دوبعدی

دو ولتاژ نویز همبسته $(X,\, Y)$ با همبستگی $\rho$:

$$f_{X,Y}(x,y) = \frac{1}{2\pi\sigma_X\sigma_Y\sqrt{1-\rho^2}} \exp\left(-\frac{1}{2(1-\rho^2)}\left[\frac{x^2}{\sigma_X^2} - \frac{2\rho xy}{\sigma_X\sigma_Y} + \frac{y^2}{\sigma_Y^2}\right]\right)$$

---

## 6.4 CDF مشترک

### تعریف

$$F_{X,Y}(x,y) = P(X \leq x, Y \leq y)$$

### ویژگی‌ها
1. $F_{X,Y}(-\infty,\, y) = 0$، $F_{X,Y}(x,\, -\infty) = 0$
2. $F_{X,Y}(\infty,\, \infty) = 1$
3. نسبت به هر دو آرگومان نامنزولی است
4. $f_{X,Y}(x,\,y) = \frac{\partial^{2}F_{X,Y}}{\partial x \partial y}$

---

## 6.5 توزیع‌های حاشیه‌ای

### به‌دست‌آوردن حاشیه‌ای‌ها از توزیع مشترک

**گسسته:**
$$p_X(x) = \sum_y p_{X,Y}(x,y), \quad p_Y(y) = \sum_x p_{X,Y}(x,y)$$

**پیوسته:**
$$f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y) \, dy, \quad f_Y(y) = \int_{-\infty}^{\infty} f_{X,Y}(x,y) \, dx$$

### تفسیر بصری

PDF حاشیه‌ای $f_{X}(x)$ همان **تصویر** (انتگرال) رویه مشترک روی محور $x$ است. به‌همین‌ترتیب $f_{Y}(y)$ روی محور $y$ تصویر می‌شود.

### مثال مهندسی

PDF مشترک: $f_{X,Y}(x,\,y) = 2e^{-x}e^{-2y}$، $x \ge 0$، $y \ge 0$

حاشیه‌ای‌ها:
- $f_{X}(x) = \int_{0}^{\infty} 2e^{-x}e^{-2y}\,dy = 2e^{-x} \cdot \left[\frac{1}{2}\right] = e^{-x} \to X \sim \mathrm{Exp}(1)$
- $f_{Y}(y) = \int_{0}^{\infty} 2e^{-x}e^{-2y}\,dx = 2e^{-2y} \cdot [1] = 2e^{-2y} \to Y \sim \mathrm{Exp}(2)$

---

## 6.6 توزیع‌های شرطی

### گسسته

$$p_{X|Y}(x|y) = \frac{p_{X,Y}(x,y)}{p_Y(y)}$$

### پیوسته

$$f_{X|Y}(x|y) = \frac{f_{X,Y}(x,y)}{f_Y(y)}$$

### تفسیر

توزیع شرطی، $X$ را هنگامی توصیف می‌کند که $Y = y$ را **می‌دانیم**. این یک «برش» از توزیع مشترک در مقدار ثابت $y$ است که به‌گونه‌ای نرمال شده که انتگرال آن برابر 1 شود.

### مثال مهندسی: سیگنال به‌شرط وضعیت کانال

توان دریافتی $Y$ به‌شرط وضعیت کانال $X$:
- اگر کانال خوب باشد $(X = 1)$: $Y \sim N(10,\, 1)$
- اگر کانال ضعیف باشد $(X = 0)$: $Y \sim N(2,\, 4)$
- $P(X = 1) = 0.7$، $P(X = 0) = 0.3$

PDF‌های شرطی رفتار گیرنده در هر وضعیت را توصیف می‌کنند.

---

## 6.7 استقلال دو متغیر تصادفی

### تعریف

$X$ و $Y$ مستقل هستند اگر و تنها اگر:

$$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y) \quad \text{for all } x, y$$

(یا به‌طور معادل برای PMF‌ها: $p_{X,Y}(x,\,y) = p_{X}(x) \cdot p_{Y}(y)$)

### پیامدهای استقلال
- $f_{X \mid Y}(x \mid y) = f_{X}(x)$ (دانستن $Y$ مقدار $X$ را تغییر نمی‌دهد)
- $E[XY] = E[X]E[Y]$
- $\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y)$

### آزمون استقلال
بررسی کنید: آیا توزیع مشترک به حاصل‌ضرب یک تابع فقط از $x$ و یک تابع فقط از $y$ تجزیه می‌شود؟
- $f_{X,Y}(x,\,y) = 2e^{-x} \cdot e^{-2y} = [e^{-x}] \cdot [2e^{-2y}]$ ✓ مستقل!
- $f_{X,Y}(x,\,y) = x + y$ برای $0 \le x,\,y \le 1$ → تجزیه نمی‌شود → مستقل نیست

### مثال مهندسی: سیگنال و نویز مستقل

سیگنال ارسالی $S$ و نویز کانال $N$ بر اساس فرض فیزیکی مستقل هستند:
$f_{S,N}(s,\,n) = f_{S}(s) \cdot f_{N}(n)$

این فرض استقلال برای طراحی سیستم مخابراتی بنیادی است.

---

## 6.8 امید ریاضی شرطی

### تعریف

$$E[X|Y=y] = \begin{cases} \sum_x x \cdot p_{X|Y}(x|y) & \text{discrete} \\ \int_{-\infty}^{\infty} x \cdot f_{X|Y}(x|y) \, dx & \text{continuous} \end{cases}$$

### تفسیر

$E[X \mid Y = y]$ **بهترین پیش‌بینی** $X$ است به‌شرط این‌که $Y = y$ را مشاهده کرده‌ایم (به معنای میانگین مربعات).

### مثال مهندسی: مقاومت وابسته به دما

مقدار یک مقاومت $R$ به دما $T$ وابسته است. اگر $R \mid T \sim N(1000 + 0.5T,\, 4)$:

$E[R \mid T = 50^{\circ}C] = 1000 + 0.5(50) = 1025\,\Omega$

با دانستن دما، می‌توانیم مقاومت را پیش‌بینی کنیم.

---

## 6.9 مثال‌های MATLAB

### مثال 1: مصورسازی PMF مشترک

```matlab
%% Joint PMF: Retransmissions vs Delay Category
pXY = [0.30 0.10; 0.15 0.20; 0.05 0.20];
x = 0:2; y = 1:2;

figure;
bar3(pXY);
xlabel('Delay Category Y'); ylabel('Retransmissions X');
zlabel('P(X=x, Y=y)');
title('Joint PMF');

% Marginals
pX = sum(pXY, 2)';   % Sum over Y
pY = sum(pXY, 1);    % Sum over X
fprintf('Marginal of X: [%.2f %.2f %.2f]\n', pX);
fprintf('Marginal of Y: [%.2f %.2f]\n', pY);
```

### مثال 2: مصورسازی گاوسی دوبعدی

```matlab
%% Joint Gaussian PDF with Correlation
mu = [0 0]; sigma_x = 1; sigma_y = 1.5; rho = 0.7;
Sigma = [sigma_x^2, rho*sigma_x*sigma_y; rho*sigma_x*sigma_y, sigma_y^2];

[X, Y] = meshgrid(linspace(-4, 4, 100), linspace(-5, 5, 100));
XY = [X(:) Y(:)];
Z = mvnpdf(XY, mu, Sigma);
Z = reshape(Z, size(X));

figure;
subplot(1,2,1);
surf(X, Y, Z, 'EdgeColor', 'none'); view(30, 40);
xlabel('X'); ylabel('Y'); zlabel('f_{X,Y}(x,y)');
title(sprintf('Joint Gaussian (ρ = %.1f)', rho));

subplot(1,2,2);
contour(X, Y, Z, 20);
xlabel('X'); ylabel('Y');
title('Contour Plot');
axis equal;
```

### مثال 3: آزمون استقلال

```matlab
%% Independence Check via Simulation
N = 100000;

% Case 1: Independent (signal and noise)
S = randn(1, N);               % Signal
Noise = 0.5*randn(1, N);       % Independent noise
R = S + Noise;                  % Received

% Case 2: Dependent (voltage and current through same resistor)
V = randn(1, N);               % Voltage
I = V / 50 + 0.01*randn(1,N); % Current (dependent on V!)

fprintf('Independent (S, Noise): corr = %.4f\n', corr(S', Noise'));
fprintf('Dependent (V, I): corr = %.4f\n', corr(V', I));
```

### مثال 4: توزیع شرطی

```matlab
%% Conditional PDF: Slice of Bivariate Gaussian
rho = 0.8; N = 100000;
X = randn(1, N);
Y = rho*X + sqrt(1-rho^2)*randn(1, N);  % Y|X ~ N(rho*x, 1-rho^2)

% Conditional: Y given X ≈ 1 (take samples where 0.9 < X < 1.1)
mask = (X > 0.9) & (X < 1.1);
Y_cond = Y(mask);

figure;
histogram(Y_cond, 50, 'Normalization', 'pdf');
hold on;
y = linspace(-3, 4, 200);
plot(y, normpdf(y, rho*1, sqrt(1-rho^2)), 'r', 'LineWidth', 2);
xlabel('Y'); ylabel('PDF');
title('f_{Y|X}(y|X≈1) for Bivariate Gaussian');
legend('Empirical', sprintf('N(%.1f, %.2f)', rho, 1-rho^2));
```

---

## 6.10 مسائل تمرینی

### مسئله 1
PDF مشترک: $f_{X,Y}(x,\,y) = 6(1-y)$ برای $0 \le x \le y \le 1$ و در جای دیگر صفر.
(a) نرمال‌سازی را بررسی کنید. (ب) حاشیه‌ای‌ها را بیابید. (ج) آیا $X$ و $Y$ مستقل هستند؟ (د) $P(X < 0.5,\, Y < 0.5)$ را بیابید.

**حل:**
(a) $\int_{0}^{1} \int_{0}^{y} 6(1-y)\,dx\,dy = \int_{0}^{1} 6y(1-y)\,dy = 6\left[\frac{y^{2}}{2} - \frac{y^{3}}{3}\right]_{0}^{1} = 6\left(\frac{1}{2} - \frac{1}{3}\right) = 1$ ✓
(b) $f_{X}(x) = \int_{x}^{1} 6(1-y)\,dy = 6\left[\left(1-\frac{y^{2}}{2}\right) - (y)\right]_{x}^{1} = 3(1-x)^{2}$؛ $f_{Y}(y) = 6y(1-y)$
(c) $f_{X,Y} \ne f_{X} \cdot f_{Y}$ → مستقل نیستند (همچنین تکیه‌گاه مثلثی است، نه مستطیلی)
(d) $P(X < 0.5,\, Y < 0.5) = \int_{0}^{0.5} \int_{0}^{y} 6(1-y)\,dx\,dy = \int_{0}^{0.5} 6y(1-y)\,dy = 6\left[\frac{y^{2}}{2} - \frac{y^{3}}{3}\right]_{0}^{0.5} = 0.500$

### مسئله 2
دو حسگر یک سیگنال را با نویز مستقل اندازه‌گیری می‌کنند. $X = S + N_{1}$، $Y = S + N_{2}$ که در آن $S = 5\,\mathrm{V}$، $N_{1} \sim N(0,\,1)$، $N_{2} \sim N(0,\,4)$. آیا $X$ و $Y$ مستقل هستند؟ $E[X]$، $E[Y]$ را بیابید و توضیح دهید چرا $\operatorname{Cov}(X,\,Y) \ne 0$.

**حل:** $X$ و $Y$ مستقل نیستند زیرا در $S$ مشترک‌اند (هرچند نویز مستقل است). $E[X] = E[Y] = 5$. $\operatorname{Cov}(X,\,Y) = \operatorname{Cov}(S+N_{1},\, S+N_{2}) = \operatorname{Var}(S) + \operatorname{Cov}(S,\,N_{1}) + \operatorname{Cov}(N_{2},\,S) + \operatorname{Cov}(N_{1},\,N_{2})$. چون $S$ ثابت است، $\operatorname{Var}(S) = 0$، بنابراین اگر $S$ تصادفی بود، آن‌ها همبسته می‌شدند. با $S$ ثابت، $X$ و $Y$ در این حالت در واقع مستقل هستند. اگر $S$ تصادفی با $\operatorname{Var}(S) = \sigma_{S}^{2}$ باشد، آنگاه $\operatorname{Cov}(X,\,Y) = \sigma_{S}^{2}$.

### مسئله 3
با توجه به PMF مشترک در جدول بخش 6.2، موارد زیر را بیابید: (الف) $P(X \le 1,\, Y = 2)$، (ب) $f_{X \mid Y}(x \mid Y = 1)$، (ج) $E[X \mid Y = 1]$.

**حل:**
(a) $P(X \le 1,\, Y = 2) = p(0,\,2) + p(1,\,2) = 0.10 + 0.20 = 0.30$
(b) $p_{X \mid Y}(x \mid 1) = \frac{p(x,\,1)}{p_{Y}(1)}$: $p(0 \mid 1) = \frac{0.30}{0.50} = 0.60$، $p(1 \mid 1) = \frac{0.15}{0.50} = 0.30$، $p(2 \mid 1) = \frac{0.05}{0.50} = 0.10$
(c) $E[X \mid Y = 1] = 0(0.60) + 1(0.30) + 2(0.10) = 0.50$

---

*ماژول بعدی: [ماژول 7 — توابع دو متغیر تصادفی](module07-functions-two-rv.md)*
<!-- TRANSLATION END -->
