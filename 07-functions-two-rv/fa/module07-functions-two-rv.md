<!-- TRANSLATION START -->
# ماژول 7 — توابع دو متغیر تصادفی

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:
1. توزیع $Z = X + Y$ را با استفاده از کانولوشن استخراج کنند
2. توزیع تفاضل، حاصل‌ضرب، نسبت، بیشینه و کمینه را بیابند
3. روش تبدیل ژاکوبی دوبعدی را به کار ببرند
4. این نتایج را به کاربردهای مهندسی (سیگنال+نویز، قابلیت اطمینان) مرتبط کنند

---

## 7.1 مقدمه

سیستم‌های مهندسی متغیرهای تصادفی را ترکیب می‌کنند:
- **سیگنال دریافتی** = ارسالی + نویز: $R = S + N$
- **تداخل کل** = جمع چند منبع: $I = I_{1} + I_{2} + \ldots + I_{n}$
- **عمر سیستم** = کمینه عمر قطعات: $T = \min (T_{1},\, T_{2})$
- **SNR** $= \dfrac{\text{توان سیگنال}}{\text{توان نویز}}$: نسبت متغیرهای تصادفی

به روش‌هایی برای یافتن توزیع **توابع دو (یا چند) متغیر تصادفی** نیاز داریم.

---

## 7.2 جمع دو متغیر تصادفی: $Z = X + Y$

### روش CDF

$F_{Z}(z) = P(X + Y \le z) = \int \int_{x+y \le z} f_{X,Y}(x,\,y)\,dx\,dy$

### فرمول کانولوشن (حالت مستقل)

اگر $X$ و $Y$ **مستقل** باشند:

$$f_Z(z) = \int_{-\infty}^{\infty} f_X(x) \cdot f_Y(z-x) \, dx = (f_X * f_Y)(z)$$

این همان **کانولوشن** $f_{X}$ و $f_{Y}$ است.

### استخراج

$F_{Z}(z) = P(X + Y \le z) = \int_{-\infty}^{\infty} \int_{-\infty}^{z-x} f_{X}(x)f_{Y}(y)\,dy\,dx$
       $= \int_{-\infty}^{\infty} f_{X}(x) F_{Y}(z-x)\,dx$

با مشتق‌گیری: $f_{Z}(z) = \int_{-\infty}^{\infty} f_{X}(x) f_{Y}(z-x)\,dx$ ✓

### حالت‌های خاص مهم

**جمع دو گاوسی (مستقل):**
$X \sim N(\mu_{1},\, \sigma_{1}^{2})$، $Y \sim N(\mu_{2},\, \sigma_{2}^{2}) \to Z = X+Y \sim N(\mu_{1}+\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$

**جمع دو نمایی (با نرخ یکسان):**
$X$، $Y \sim \mathrm{Exp}(\lambda)$ مستقل $\to Z \sim \mathrm{Gamma}\left(2,\, \frac{1}{\lambda}\right) = \mathrm{Erlang}(2,\, \lambda)$

**جمع دو پواسون:**
$X \sim \mathrm{Poisson}(\lambda_{1})$، $Y \sim \mathrm{Poisson}(\lambda_{2})$ مستقل $\to Z \sim \mathrm{Poisson}(\lambda_{1}+\lambda_{2})$

**جمع دو یکنواخت:**
$X$، $Y \sim \mathrm{Uniform}(0,\,1)$ مستقل $\to Z$ توزیع مثلثی روی [0,2] دارد

### مثال مهندسی: سیگنال + نویز

سیگنال ارسالی $S \sim N(A,\, \sigma_{s}^{2})$، نویز $N \sim N(0,\, \sigma_{n}^{2})$، مستقل.
دریافتی: $R = S + N \sim N(A,\, \sigma_{s}^{2} + \sigma_{n}^{2})$

سیگنال دریافتی همچنان گاوسی است — با واریانس بیشتر (SNR کاهش‌یافته).

---

## 7.3 تفاضل: $Z = X - Y$

برای $X$، $Y$ مستقل:
$$f_Z(z) = \int_{-\infty}^{\infty} f_X(x) \cdot f_Y(x-z) \, dx$$

**حالت گاوسی:** $X \sim N(\mu_{1},\, \sigma_{1}^{2})$، $Y \sim N(\mu_{2},\, \sigma_{2}^{2}) \to Z = X-Y \sim N(\mu_{1}-\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$

توجه: واریانس‌ها همچنان **جمع** می‌شوند (نه تفریق) زیرا $\operatorname{Var}(-Y) = \operatorname{Var}(Y)$.

### مثال مهندسی: اندازه‌گیری تفاضلی

دو حسگر یک کمیت را اندازه‌گیری می‌کنند: $X_{1} = \theta + N_{1}$، $X_{2} = \theta + N_{2}$.
تفاضل: $X_{1} - X_{2} = N_{1} - N_{2} \sim N(0,\, \sigma_{1}^{2} + \sigma_{2}^{2})$

تفاضل، سیگنال مشترک را حذف می‌کند اما واریانس نویز را دو برابر می‌کند.

---

## 7.4 حاصل‌ضرب: $Z$ = XY

برای $X$، $Y$ مستقل:
$$f_Z(z) = \int_{-\infty}^{\infty} \frac{1}{|x|} f_X(x) \cdot f_Y(z/x) \, dx$$

### مثال مهندسی: توان

ولتاژ $V$ و جریان $I$ در یک مدار خطی: $P = V \cdot I$.
اگر $V$ و $I$ توزیع مشترک معلومی داشته باشند، می‌توانیم توزیع توان را بیابیم.

---

## 7.5 نسبت: $Z = \frac{X}{Y}$

برای $X$، $Y$ مستقل:
$$f_Z(z) = \int_{-\infty}^{\infty} |y| \cdot f_X(zy) \cdot f_Y(y) \, dy$$

### مثال مهندسی: SNR

$\mathrm{SNR} = \dfrac{\text{توان سیگنال}}{\text{توان نویز}} = \dfrac{P_s}{P_n}$

اگر هر دو دارای توزیع کای‌دو باشند (جمع مربعات گاوسی‌ها)، نسبت از یک **توزیع $F$** پیروی می‌کند.

---

## 7.6 بیشینه: $Z = \max (X,\, Y)$

### رویکرد CDF

$F_{Z}(z) = P(\max (X,\,Y) \le z) = P(X \le z\ \text{AND}\ Y \le z)$

اگر مستقل باشند: $F_{Z}(z) = F_{X}(z) \cdot F_{Y}(z)$

PDF: $f_{Z}(z) = f_{X}(z)F_{Y}(z) + F_{X}(z)f_{Y}(z)$

### مثال مهندسی: افزونگی موازی

سیستم کار می‌کند اگر **حداقل یکی** از قطعات کار کند. سیستم تنها زمانی خراب می‌شود که **همه** خراب شوند.
عمر سیستم $= \max (T_{1},\, T_{2})$ برای قطعات افزونه موازی.

برای $T_{1}$، $T_{2} \sim \mathrm{Exp}(\lambda)$ مستقل:
$F_{Z}(z) = (1 - e^{-\lambda z})^{2}$ برای $z \ge 0$

$f_{Z}(z) = 2\lambda e^{-\lambda z}(1 - e^{-\lambda z})$

عمر میانگین: $E[\max] = \frac{3}{2\lambda} > \frac{1}{\lambda} = E[\text{تک قطعه}]$ (قابلیت اطمینان بهبود یافته!)

---

## 7.7 کمینه: $Z = \min (X,\, Y)$

### رویکرد تابع بقا

$P(Z > z) = P(\min (X,\,Y) > z) = P(X > z\ \text{AND}\ Y > z)$

اگر مستقل باشند: $P(Z > z) = P(X > z) \cdot P(Y > z) = [1-F_{X}(z)][1-F_{Y}(z)]$

CDF: $F_{Z}(z) = 1 - [1-F_{X}(z)][1-F_{Y}(z)]$

### مثال مهندسی: سیستم سری

سیستم زمانی خراب می‌شود که **نخستین** قطعه خراب شود. عمر سیستم $= \min (T_{1},\, T_{2})$.

برای $T_{1} \sim \mathrm{Exp}(\lambda_{1})$، $T_{2} \sim \mathrm{Exp}(\lambda_{2})$ مستقل:
$P(Z > z) = e^{-\lambda_{1}z} \cdot e^{-\lambda_{2}z} = e^{-(\lambda_{1}+\lambda_{2})z}$

بنابراین: $\min (T_{1},\, T_{2}) \sim \mathrm{Exp}(\lambda_{1} + \lambda_{2})$

**نتیجه کلیدی:** نرخ‌های خرابی در سیستم‌های سری **جمع** می‌شوند. عمر میانگین $= \frac{1}{\lambda_{1}+\lambda_{2}} < \min \left(\frac{1}{\lambda_{1}},\, \frac{1}{\lambda_{2}}\right)$.

---

## 7.8 تبدیل دوبعدی عمومی (روش ژاکوبی)

### صورت‌بندی

با داشتن $(X,\, Y)$ با PDF مشترک معلوم، PDF مشترک $(U,\, V)$ را بیابید که در آن:
- $U = g_{1}(X,\, Y)$
- $V = g_{2}(X,\, Y)$

### رویه

1. برای وارون حل کنید: $X = h_{1}(U,\, V)$، $Y = h_{2}(U,\, V)$
2. دترمینان ژاکوبی را محاسبه کنید:

$$J = \begin{vmatrix} \frac{\partial x}{\partial u} & \frac{\partial x}{\partial v} \\ \frac{\partial y}{\partial u} & \frac{\partial y}{\partial v} \end{vmatrix}$$

3. اعمال کنید:
$$f_{U,V}(u,v) = f_{X,Y}(h_1(u,v), h_2(u,v)) \cdot |J|$$

4. اگر فقط $U$ لازم باشد، $V$ را حاشیه‌سازی کنید.

### مثال: جمع و تفاضل

$U = X + Y$، $V = X - Y \to X = \frac{U+V}{2}$، $Y = \frac{U-V}{2}$

$J = \lvert \frac{\partial x}{\partial u} \cdot \frac{\partial y}{\partial v} - \frac{\partial x}{\partial v} \cdot \frac{\partial y}{\partial u}\rvert = \lvert \frac{1}{2}\left(-\frac{1}{2}\right) - \frac{1}{2}\left(\frac{1}{2}\right)\rvert = \frac{1}{2}$

$f_{U,V}(u,\,v) = \frac{1}{2} \cdot f_{X,Y}\left(\frac{u+v}{2},\, \frac{u-v}{2}\right)$

---

## 7.9 مثال‌های MATLAB

### مثال 1: کانولوشن — جمع دو یکنواخت

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

### مثال 2: جمع گاوسی‌ها (سیگنال + نویز)

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

### مثال 3: قابلیت اطمینان سیستم سری (کمینه)

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

### مثال 4: سیستم موازی (بیشینه)

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

## 7.10 مسائل تمرینی

### مسئله 1
$X \sim \mathrm{Exp}(1)$، $Y \sim \mathrm{Exp}(1)$، مستقل. PDF مربوط به $Z = X + Y$ را بیابید.

**حل:** کانولوشن: $f_{Z}(z) = \int_{0}^{z} e^{-x} \cdot e^{-(z-x)}\,dx = \int_{0}^{z} e^{-z}\,dx = ze^{-z}$، $z \ge 0$.
این $\mathrm{Gamma}(2,\,1) = \mathrm{Erlang}(2,\,1)$ است. $E[Z] = 2$، $\operatorname{Var}(Z) = 2$.

### مسئله 2
سه قطعه در آرایش سری با نرخ‌های خرابی $\lambda_{1} = 0.001$، $\lambda_{2} = 0.002$، $\lambda_{3} = 0.003$ در هر ساعت.
(a) MTTF سیستم را بیابید. (ب) $P(\text{سیستم 100 ساعت دوام می‌آورد})$ را بیابید.

**حل:**
(a) $\mathrm{MTTF} = \frac{1}{0.001+0.002+0.003} = \frac{1}{0.006} = 166.7$ ساعت
(b) $P(T_{\text{sys}} > 100) = e^{-0.006 \times 100} = e^{-0.6} = 0.549$

### مسئله 3
$X \sim N(5,\, 4)$، $Y \sim N(3,\, 9)$، مستقل. توزیع و $P(X + Y > 12)$ را بیابید.

**حل:** $Z = X+Y \sim N(8,\, 13)$. $P(Z > 12) = P\left(Z' > \frac{12-8}{\sqrt{13}}\right) = Q(1.109) = 0.1337$.

---

*ماژول بعدی: [ماژول 8 — امید ریاضی و واریانس شرطی](module08-conditional-expectation.md)*
<!-- TRANSLATION END -->
