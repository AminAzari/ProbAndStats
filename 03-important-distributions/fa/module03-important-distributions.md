<!-- TRANSLATION START -->
# ماژول 3 — توزیع‌های مهم احتمال

## اهداف یادگیری

پس از تکمیل این ماژول، دانشجویان قادر خواهند بود:

1. تشخیص دهند که کدام توزیع احتمال یک سناریوی مهندسی معین را مدل می‌کند
2. $\frac{\mathrm{PMF}}{\mathrm{PDF}}$، CDF، میانگین و واریانس را برای تمام توزیع‌های اصلی بیان کنند
3. تفسیر فیزیکی و زمینه مهندسی هر توزیع را توضیح دهند
4. تعیین کنند چه زمانی از هر توزیع استفاده شود (و چه زمانی استفاده نشود)
5. توزیع‌ها را برای شبیه‌سازی در MATLAB پیاده‌سازی کنند
6. الگوهای توزیع را از مشخصه‌های داده تشخیص دهند

---

## 3.1 مقدمه: انتخاب توزیع مناسب

### چالش مهندسی

هنگام مدل‌سازی یک پدیده تصادفی، پرسش حیاتی این است:

> «کدام توزیع احتمال این کمیت نامعین را بهتر توصیف می‌کند؟»

انتخاب توزیع نادرست به پیش‌بینی‌های نادرست، طراحی‌های نامناسب و خرابی سیستم‌ها منجر می‌شود.

### چگونه انتخاب کنیم

توزیع باید با **سازوکار فیزیکی** تولید تصادفی‌بودن همخوانی داشته باشد:
- شمارش موفقیت‌ها → دوجمله‌ای
- انتظار برای رویدادها → نمایی/هندسی
- جمع بسیاری از اثرهای کوچک → گاوسی
- پوش سیگنال در محوشدگی → رایلی

این ماژول ابزارهای لازم برای انجام درست این انتخاب را فراهم می‌کند.

---

## 3.2 توزیع برنولی

### تفسیر فیزیکی

یک **آزمایش منفرد** با دو خروجی ممکن را مدل می‌کند: «موفقیت» (1) یا «شکست» (0).

### زمینه مهندسی

هر خروجی تصادفی دودویی:
- یک بیت ارسال‌شده: خطا یا درست
- یک قطعه آزمایش‌شده: قبول یا رد
- یک بسته ارسال‌شده: تحویل‌شده یا مفقود
- یک تلاش آشکارسازی: هدف حاضر یا غایب

### تعریف ریاضی

$X \sim \mathrm{Bernoulli}(p)$، که در آن $p$ = احتمال موفقیت.

### PMF

$$p_X(x) = \begin{cases} p & x = 1 \\ 1-p = q & x = 0 \end{cases}$$

شکل فشرده: $p_{X}(x) = p^{x}(1-p)^{1-x}$ برای $x \in \{0,\, 1\}$

### CDF

$$F_X(x) = \begin{cases} 0 & x < 0 \\ 1-p & 0 \leq x < 1 \\ 1 & x \geq 1 \end{cases}$$

### میانگین

$$E[X] = p$$

### واریانس

$$\text{Var}(X) = p(1-p)$$

بیشترین واریانس در $p = 0.5$ است (بیشترین عدم‌قطعیت).

### زمان استفاده

✅ آزمایش دودویی منفرد با احتمال ثابت
✅ بلوک سازنده برای توزیع‌های پیچیده‌تر
✅ مدل‌سازی حالت‌های روشن/خاموش، خروجی‌های قبول/رد

### زمان عدم استفاده

❌ آزمایش‌های متعدد (از دوجمله‌ای استفاده کنید)
❌ خروجی دودویی نیست
❌ احتمال میان آزمایش‌ها تغییر می‌کند

### پیاده‌سازی در MATLAB

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

## 3.3 توزیع دوجمله‌ای

### تفسیر فیزیکی

**تعداد موفقیت‌ها** را در $n$ آزمایش **مستقل** که هر یک همان احتمال موفقیت $p$ را دارند مدل می‌کند.

### زمینه مهندسی

- تعداد خطاهای بیت در $n$ بیت ارسال‌شده
- تعداد قطعات معیوب در یک دسته n‌تایی
- تعداد انتقال‌های موفق بسته از میان $n$ تلاش
- تعداد حسگرهایی که یک سیگنال را آشکار می‌کنند (از میان $n$ حسگر)

### تعریف ریاضی

$X \sim \mathrm{Binomial}(n,\, p)$

### PMF

$$p_X(k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k = 0, 1, 2, \ldots, n$$

که در آن $C(n,\,k) = \frac{n!}{k!(n-k)!}$

### CDF

$$F_X(k) = \sum_{i=0}^{\lfloor k \rfloor} \binom{n}{i} p^i (1-p)^{n-i}$$

فرم بسته ندارد — از جدول‌ها یا MATLAB استفاده کنید.

### میانگین

$$E[X] = np$$

### واریانس

$$\text{Var}(X) = np(1-p)$$

### زمان استفاده

✅ تعداد ثابت آزمایش‌ها $n$
✅ هر آزمایش مستقل است
✅ هر آزمایش همان احتمال $p$ را دارد
✅ شمارش تعداد «موفقیت‌ها»

### زمان عدم استفاده

❌ آزمایش‌ها مستقل نیستند (خرابی‌های همبسته)
❌ احتمال میان آزمایش‌ها تغییر می‌کند
❌ $n$ ثابت نیست (از دوجمله‌ای منفی یا پواسون استفاده کنید)
❌ $n$ بسیار بزرگ با $p$ کوچک (از تقریب پواسون استفاده کنید)

### پیاده‌سازی در MATLAB

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

### مثال شبیه‌سازی: نرخ خطای بسته

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

## 3.4 توزیع هندسی

### تفسیر فیزیکی

**تعداد آزمایش‌ها تا نخستین موفقیت** را مدل می‌کند (یا به‌طور معادل، تعداد شکست‌ها پیش از نخستین موفقیت).

### زمینه مهندسی

- تعداد انتقال‌ها تا تحویل موفق یک بسته
- تعداد قطعات آزمایش‌شده تا یافتن یک قطعه معیوب
- تعداد تلاش‌های ورود تا احراز هویت موفق
- تعداد شکاف‌های زمانی تا در دسترس قرار گرفتن کانال

### تعریف ریاضی

$X \sim \mathrm{Geometric}(p)$

قرارداد: $X$ = شماره آزمایشی که نخستین موفقیت در آن رخ می‌دهد $(X \in \{1,\, 2,\, 3,\, \ldots \})$.

### PMF

$$p_X(k) = (1-p)^{k-1} p, \quad k = 1, 2, 3, \ldots$$

($k-1$ شکست، سپس یک موفقیت)

### CDF

$$F_X(k) = 1 - (1-p)^k, \quad k = 1, 2, 3, \ldots$$

### میانگین

$$E[X] = \frac{1}{p}$$

### واریانس

$$\text{Var}(X) = \frac{1-p}{p^2}$$

### خاصیت بی‌حافظه

توزیع هندسی تنها توزیع گسسته با خاصیت بی‌حافظه است:

$$P(X > m + n \mid X > m) = P(X > n)$$

**معنای مهندسی:** اگر یک سیستم در $m$ تلاش موفق نشده باشد، احتمال نیاز به $n$ تلاش بیشتر همانند شروع از صفر است. «شکست‌های گذشته بر احتمال آینده اثر نمی‌گذارند.»

### زمان استفاده

✅ آزمایش‌های مستقل مکرر تا نخستین موفقیت
✅ هر آزمایش همان احتمال را دارد
✅ فرض بی‌حافظه مناسب است (مثلاً ارسال‌های مجدد مستقل)

### زمان عدم استفاده

❌ احتمال موفقیت در طول زمان تغییر می‌کند (پیری، یادگیری)
❌ آزمایش‌ها مستقل نیستند
❌ به‌دنبال r-امین موفقیت هستید (از دوجمله‌ای منفی استفاده کنید)

### پیاده‌سازی در MATLAB

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

## 3.5 توزیع دوجمله‌ای منفی

### تفسیر فیزیکی

**تعداد آزمایش‌ها تا r-امین موفقیت** را مدل می‌کند (تعمیم هندسی).

### زمینه مهندسی

- تعداد انتقال‌ها تا تحویل موفق $r$ بسته
- تعداد قطعات بازرسی‌شده تا یافتن $r$ قطعه معیوب
- تعداد شکاف‌های زمانی تا در دسترس قرار گرفتن $r$ کانال

### تعریف ریاضی

$X \sim \mathrm{NegBin}(r,\, p)$ = تعداد آزمایش‌ها تا r-امین موفقیت.

### PMF

$$p_X(k) = \binom{k-1}{r-1} p^r (1-p)^{k-r}, \quad k = r, r+1, r+2, \ldots$$

(انتخاب این‌که کدام $r-1$ آزمایش از میان $k-1$ آزمایش نخست موفق بوده‌اند؛ آزمایش k-ام همان r-امین موفقیت است.)

### CDF

فرم بسته ساده‌ای ندارد. با جمع‌بندی یا MATLAB محاسبه می‌شود.

### میانگین

$$E[X] = \frac{r}{p}$$

### واریانس

$$\text{Var}(X) = \frac{r(1-p)}{p^2}$$

### رابطه با هندسی

وقتی $r = 1$، توزیع دوجمله‌ای منفی به توزیع هندسی کاهش می‌یابد.

### زمان استفاده

✅ آزمایش‌های مستقل مکرر تا r-امین موفقیت
✅ احتمال ثابت در هر آزمایش
✅ تعمیم هندسی به چندین موفقیت موردنیاز

### زمان عدم استفاده

❌ فقط نخستین موفقیت لازم است (از هندسی استفاده کنید — ساده‌تر)
❌ آزمایش‌ها مستقل نیستند
❌ تعداد آزمایش‌ها ثابت است (از دوجمله‌ای استفاده کنید)

### پیاده‌سازی در MATLAB

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

## 3.6 توزیع پواسون

### تفسیر فیزیکی

**تعداد رویدادهای رخ‌داده در یک بازه ثابت** (زمان، فضا، مساحت) را مدل می‌کند، هنگامی که رویدادها به‌طور مستقل و با نرخ میانگین ثابت رخ می‌دهند.

### زمینه مهندسی

- تعداد فوتون‌های برخوردکننده با یک آشکارساز در $1 \mu s$
- تعداد بسته‌های واردشده به یک مسیریاب در 1 ثانیه
- تعداد نقص‌ها در هر $cm^{2}$ روی یک ویفر سیلیکونی
- تعداد رویدادهای پرتو کیهانی در یک حسگر در هر ساعت
- تعداد جهش‌های تداخل در یک پنجره اندازه‌گیری

### تعریف ریاضی

$X \sim \mathrm{Poisson}(\lambda)$، که در آن $\lambda$ = میانگین تعداد رویدادها در هر بازه.

### PMF

$$p_X(k) = \frac{\lambda^k e^{-\lambda}}{k!}, \quad k = 0, 1, 2, \ldots$$

### CDF

$$F_X(k) = e^{-\lambda} \sum_{i=0}^{\lfloor k \rfloor} \frac{\lambda^i}{i!}$$

### میانگین

$$E[X] = \lambda$$

### واریانس

$$\text{Var}(X) = \lambda$$

**ویژگی منحصربه‌فرد:** میانگین برابر واریانس است! این یک بررسی سریع است — اگر میانگین نمونه ≈ واریانس نمونه باشد، داده ممکن است پواسون باشد.

### پواسون به‌عنوان تقریب دوجمله‌ای

وقتی $n$ بزرگ و $p$ کوچک باشد، با $\lambda$ = np:

$$\text{Binomial}(n, p) \approx \text{Poisson}(\lambda = np)$$

قاعده سرانگشتی: $n \ge 20$ و $p \le 0.05$.

### زمان استفاده

✅ شمارش رویدادها در یک بازه ثابت
✅ رویدادها مستقل رخ می‌دهند
✅ رویدادها با نرخ میانگین ثابت رخ می‌دهند
✅ رویدادهای نادر با فرصت‌های فراوان ($n$ بزرگ، $p$ کوچک)
✅ میانگین ≈ واریانس در داده

### زمان عدم استفاده

❌ رویدادها مستقل نیستند (ورودهای خوشه‌ای)
❌ نرخ با زمان تغییر می‌کند (فرایند ناهمگن)
❌ رویدادها نادر نیستند ($p$ کوچک نیست) — از دوجمله‌ای استفاده کنید
❌ میانگین ≠ واریانس (به بیش‌پراکندگی/کم‌پراکندگی توجه کنید)

### پیاده‌سازی در MATLAB

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

### مثال شبیه‌سازی: شمارش فوتون

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

## 3.7 توزیع یکنواخت (پیوسته)

### تفسیر فیزیکی

کمیتی را مدل می‌کند که **احتمال برابر** برای اختیار کردن هر مقدار در بازه $[a,\, b]$ دارد. نماینده **بیشترین عدم‌قطعیت** در یک محدوده معلوم است.

### زمینه مهندسی

- فاز یک سیگنال دریافتی (نامعلوم، با احتمال برابر در $[0,\, 2\pi)$)
- خطای گِردکردن در یک مبدل آنالوگ به دیجیتال (یکنواخت در $\left[-\frac{\Delta}{2},\, \frac{\Delta}{2}\right]$ که $\Delta$ = گام کوانتیزاسیون)
- زمان ورود در یک شکاف زمانی (موقعیت نامعلوم)
- پروتکل دسترسی تصادفی: انتخاب یک زمان عقب‌نشینی تصادفی

### تعریف ریاضی

$X \sim \mathrm{Uniform}(a,\, b)$ یا $X \sim U(a,\, b)$

### PDF

$$f_X(x) = \begin{cases} \frac{1}{b-a} & a \leq x \leq b \\ 0 & \text{otherwise} \end{cases}$$

### CDF

$$F_X(x) = \begin{cases} 0 & x < a \\ \frac{x-a}{b-a} & a \leq x \leq b \\ 1 & x > b \end{cases}$$

### میانگین

$$E[X] = \frac{a+b}{2}$$

### واریانس

$$\text{Var}(X) = \frac{(b-a)^2}{12}$$

### زمان استفاده

✅ تمام مقادیر در یک محدوده احتمال برابر دارند
✅ هیچ اطلاعاتی به نفع مقدار خاصی نیست
✅ مدل‌سازی خطای کوانتیزاسیون
✅ فاز حامل بدون مدولاسیون

### زمان عدم استفاده

❌ مقادیر نزدیک به مرکز محتمل‌تر هستند (از گاوسی استفاده کنید)
❌ مقادیر نامنفی با کاهش نمایی هستند (از نمایی استفاده کنید)
❌ محدوده کران‌دار نیست

### پیاده‌سازی در MATLAB

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

## 3.8 توزیع نمایی

### تفسیر فیزیکی

**زمان میان رویدادها** در یک فرایند پواسون، یا **عمر** یک قطعه بی‌حافظه را مدل می‌کند.

### زمینه مهندسی

- زمان میان ورود بسته‌ها
- زمان تا خرابی قطعه (نرخ خرابی ثابت)
- زمان میان رویدادهای پرتو کیهانی در یک آشکارساز
- زمان بین‌ورودی تماس‌ها در یک مرکز سوئیچ
- مدت یک فروافت محوشدگی در یک کانال بی‌سیم

### تعریف ریاضی

$X \sim \mathrm{Exponential}(\lambda)$ که در آن $\lambda$ = پارامتر نرخ (رویداد در واحد زمان).

پارامتربندی جایگزین: $X \sim \mathrm{Exp}(\beta)$ که در آن $\beta = \frac{1}{\lambda}$ = میانگین.

### PDF

$$f_X(x) = \lambda e^{-\lambda x}, \quad x \geq 0$$

یا با میانگین $\beta$: $f_{X}(x) = \frac{1}{\beta}e^{-x/\beta}$

### CDF

$$F_X(x) = 1 - e^{-\lambda x}, \quad x \geq 0$$

### میانگین

$$E[X] = \frac{1}{\lambda} = \beta$$

### واریانس

$$\text{Var}(X) = \frac{1}{\lambda^2} = \beta^2$$

**توجه:** برای توزیع نمایی، انحراف معیار برابر میانگین است.

### خاصیت بی‌حافظه

$$P(X > s + t \mid X > s) = P(X > t)$$

توزیع نمایی تنها توزیع پیوسته با این خاصیت است.

**معنای مهندسی:** قطعه‌ای که $s$ ساعت کار کرده است، از نظر آماری «به‌خوبی نو» است. عمر باقی‌مانده آن همان توزیع یک قطعه کاملاً نو را دارد.

### رابطه با پواسون

اگر رویدادها به‌صورت یک فرایند پواسون با نرخ $\lambda$ وارد شوند:
- تعداد رویدادها در زمان $T \sim \mathrm{Poisson}(\lambda T)$
- زمان میان رویدادها $\sim \mathrm{Exponential}(\lambda)$

### زمان استفاده

✅ مدل‌سازی زمان میان رویدادها در یک فرایند پواسون
✅ عمر قطعه با نرخ خرابی ثابت
✅ فرض بی‌حافظه معقول است

### زمان عدم استفاده

❌ نرخ خرابی با سن افزایش می‌یابد (فرسودگی) — از وایبول یا گاما استفاده کنید
❌ نرخ خرابی با سن کاهش می‌یابد (سوختگی اولیه) — از وایبول استفاده کنید
❌ داده افزایش خطر را نشان می‌دهد — بی‌حافظه نیست

### پیاده‌سازی در MATLAB

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

### مثال شبیه‌سازی: بررسی خاصیت بی‌حافظه

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

## 3.9 توزیع گاوسی (نرمال)

### تفسیر فیزیکی

کمیت‌هایی را مدل می‌کند که از **جمع بسیاری از اثرهای تصادفی کوچک و مستقل** پدید می‌آیند. به‌دلیل قضیه حد مرکزی، مهم‌ترین توزیع در مهندسی است.

### زمینه مهندسی

- ولتاژ نویز حرارتی (جمع سهم‌های بسیاری از الکترون‌ها)
- خطاهای اندازه‌گیری (جمع بسیاری از منابع خطای کوچک)
- تغییرات ساخت (بسیاری از تغییرات مستقل فرایند)
- تداخل تجمعی در مخابرات
- هر کمیتی که به‌خوبی با CLT مدل می‌شود

### تعریف ریاضی

$X \sim N(\mu,\, \sigma^{2})$ که در آن $\mu$ = میانگین و $\sigma^{2}$ = واریانس.

### PDF

$$f_X(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right), \quad -\infty < x < \infty$$

### CDF

$$F_X(x) = \Phi\left(\frac{x-\mu}{\sigma}\right) = \frac{1}{2}\left[1 + \text{erf}\left(\frac{x-\mu}{\sigma\sqrt{2}}\right)\right]$$

فرم بسته ندارد — به‌صورت عددی یا با جدول‌ها محاسبه می‌شود.

### میانگین

$$E[X] = \mu$$

### واریانس

$$\text{Var}(X) = \sigma^2$$

### نرمال استاندارد

$Z \sim N(0,\, 1)$: شکل استانداردشده.

هر توزیع نرمال را می‌توان استانداردسازی کرد: $Z = \frac{X - \mu}{\sigma}$

### قواعد کلیدی احتمال (قواعد $\sigma$)

| محدوده | احتمال |
|-------|-------------|
| $\mu \pm 1\sigma$ | 68.27% |
| $\mu \pm 2\sigma$ | 95.45% |
| $\mu \pm 3\sigma$ | 99.73% |
| $\mu \pm 4\sigma$ | 99.9937% |

### ویژگی‌ها

1. **متقارن** حول $\mu$
2. **تبدیل خطی:** اگر $X \sim N(\mu,\, \sigma^{2})$، آنگاه aX $+ b \sim N(a\mu + b,\, a^{2}\sigma^{2})$
3. **جمع گاوسی‌ها:** اگر $X \sim N(\mu_{1},\, \sigma_{1}^{2})$ و $Y \sim N(\mu_{2},\, \sigma_{2}^{2})$ مستقل باشند، آنگاه $X + Y \sim N(\mu_{1}+\mu_{2},\, \sigma_{1}^{2}+\sigma_{2}^{2})$
4. **کاملاً مشخص‌شده** با میانگین و واریانس (تمام گشتاورهای مراتب بالاتر تعیین می‌شوند)

### زمان استفاده

✅ نویز حرارتی در مدارها
✅ خطاهای اندازه‌گیری
✅ هر جمعی از بسیاری از اثرهای تصادفی مستقل (CLT)
✅ تغییرات ساخت
✅ هنگامی که هیستوگرام داده زنگوله‌ای و متقارن است

### زمان عدم استفاده

❌ کمیت‌های اکیداً مثبت (گاوسی مقادیر منفی را مجاز می‌داند)
❌ داده‌های به‌شدت چوله
❌ کمیت‌های کران‌دار (گاوسی تکیه‌گاه نامتناهی دارد)
❌ شمارش‌های گسسته
❌ پدیده‌های سنگین‌دنباله (از توزیع $t$ یا کوشی استفاده کنید)

### پیاده‌سازی در MATLAB

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

### مثال شبیه‌سازی: تابع $Q$ و BER

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

## 3.10 توزیع گاما

### تفسیر فیزیکی

**کل زمان انتظار تا k-امین رویداد** در یک فرایند پواسون، یا هر جمعی از $k$ متغیر تصادفی نمایی مستقل را مدل می‌کند.

### زمینه مهندسی

- کل زمان تعمیر برای $k$ قطعه (هر یک با زمان تعمیر نمایی)
- زمان تا k-امین ورود بسته
- زمان سرویس تجمعی برای $k$ کار صف‌بندی‌شده
- مدل‌سازی مقدار بارش

### تعریف ریاضی

$X \sim \mathrm{Gamma}(\alpha,\, \beta)$ که در آن $\alpha$ = پارامتر شکل و $\beta$ = پارامتر مقیاس (یا نرخ $\lambda = \frac{1}{\beta}$).

### PDF

$$f_X(x) = \frac{x^{\alpha-1} e^{-x/\beta}}{\beta^\alpha \Gamma(\alpha)}, \quad x \geq 0$$

که در آن برای $\alpha$ صحیح $\Gamma (\alpha) = (\alpha -1)$!، و در حالت کلی $\Gamma (\alpha) = \int_{0}^{\infty} t^{\alpha -1}e^{-t}dt$.

### CDF

برای $\alpha$ کلی فرم بسته ندارد. از طریق تابع گامای ناقص در دسترس است.

### میانگین

$$E[X] = \alpha\beta$$

### واریانس

$$\text{Var}(X) = \alpha\beta^2$$

### حالت‌های خاص

| پارامترها | توزیع |
|-----------|-------------|
| $\alpha = 1$ | $\mathrm{Exponential}(\beta)$ |
| $\alpha = \frac{n}{2},\, \beta = 2$ | Chi-square(n) |
| $\alpha$ = integer $k$ | $\mathrm{Erlang}(k,\, \beta)$ |

### زمان استفاده

✅ جمع عمرهای نمایی مستقل
✅ زمان انتظار برای k-امین رویداد پواسون
✅ داده‌های چوله و نامنفی
✅ شکل انعطاف‌پذیر (چولگی قابل تنظیم)

### زمان عدم استفاده

❌ داده می‌تواند منفی باشد
❌ داده متقارن است (از گاوسی استفاده کنید)
❌ توزیع ساده‌تری برازش می‌شود (نمایی برای $k = 1$)

### پیاده‌سازی در MATLAB

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

## 3.11 توزیع رایلی

### تفسیر فیزیکی

**اندازه (پوش)** یک بردار تصادفی گاوسی دوبعدی را مدل می‌کند. هنگامی که $X$ و $Y$ مستقل و $N(0,\, \sigma^{2})$ باشند، آنگاه $R = \sqrt{X^{2} + Y^{2}}$ از توزیع رایلی پیروی می‌کند.

### زمینه مهندسی

- پوش نویز باریک‌باند (هم‌فاز + مربعی)
- اندازه ضریب کانال محوشدگی بی‌سیم (محوشدگی رایلی)
- مدل‌سازی سرعت باد
- خطای فاصله در مکان‌یابی دوبعدی (هنگامی که خطاها در $x$ و $y$ گاوسی هستند)

### تعریف ریاضی

$R \sim \mathrm{Rayleigh}(\sigma)$، که در آن $\sigma$ پارامتر است (نه انحراف معیار $R$).

### PDF

$$f_R(r) = \frac{r}{\sigma^2} e^{-r^2/(2\sigma^2)}, \quad r \geq 0$$

### CDF

$$F_R(r) = 1 - e^{-r^2/(2\sigma^2)}, \quad r \geq 0$$

### میانگین

$$E[R] = \sigma\sqrt{\frac{\pi}{2}} \approx 1.2533\sigma$$

### واریانس

$$\text{Var}(R) = \frac{4-\pi}{2}\sigma^2 \approx 0.4292\sigma^2$$

### رابطه با سایر توزیع‌ها

- $R^{2} \sim \mathrm{Exponential}(2\sigma^{2})$ — توان به‌صورت نمایی توزیع شده است
- اگر $X,\, Y \sim N(0,\, \sigma^{2})$ مستقل باشند، آنگاه $\sqrt{X^{2}+Y^{2}} \sim \mathrm{Rayleigh}(\sigma)$

### زمان استفاده

✅ پوش/اندازه یک سیگنال گاوسی مختلط
✅ مدل‌سازی کانال محوشدگی رایلی (بدون خط دید)
✅ فاصله دوبعدی با خطاهای گاوسی در هر بُعد
✅ RSS (جذر مجموع مربعات) دو مؤلفه گاوسی مستقل

### زمان عدم استفاده

❌ مؤلفه خط دید قوی موجود است (از رایسی استفاده کنید)
❌ اندازه/پوش را مدل نمی‌کنید
❌ مؤلفه‌ها گاوسی نیستند یا واریانس برابر ندارند

### پیاده‌سازی در MATLAB

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

### مثال شبیه‌سازی: محوشدگی رایلی

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

## 3.12 توزیع کای‌دو

### تفسیر فیزیکی

**جمع مربعات** متغیرهای تصادفی نرمال استاندارد مستقل را مدل می‌کند. به‌طور طبیعی در برآورد واریانس و آزمون نیکویی برازش ظاهر می‌شود.

### زمینه مهندسی

- توزیع واریانس نمونه (برای داده‌های گاوسی)
- توان نویز گاوسی (جمع مؤلفه‌های مربع‌شده)
- آماره آزمون نیکویی برازش
- بازه‌های اطمینان برای واریانس
- آشکارسازی انرژی در سنجش طیف

### تعریف ریاضی

اگر $Z_{1},\, Z_{2},\, \ldots,\, Z_{n}$ مستقل و $N(0,\,1)$ باشند، آنگاه:
$$\chi^2 = Z_1^2 + Z_2^2 + \cdots + Z_n^2 \sim \chi^2(n)$$

$n$ = درجات آزادی.

### PDF

$$f_X(x) = \frac{x^{n/2-1} e^{-x/2}}{2^{n/2}\Gamma(n/2)}, \quad x \geq 0$$

### CDF

برای $n$ کلی فرم بسته ندارد. از طریق تابع گامای ناقص محاسبه می‌شود.

### میانگین

$$E[X] = n$$

### واریانس

$$\text{Var}(X) = 2n$$

### رابطه با سایر توزیع‌ها

- Chi-square(n) $= \mathrm{Gamma}\left(\frac{n}{2},\, 2\right)$
- Chi-square(1) = مربع $N(0,\,1)$
- Chi-square(2) $= \mathrm{Exponential}\left(\frac{1}{2}\right)$
- برای $n$ بزرگ: $\chi^{2}(n) \approx N(n,\, 2n)$ بر اساس CLT

### زمان استفاده

✅ جمع مربعات متغیرهای تصادفی گاوسی
✅ آزمون واریانس و بازه‌های اطمینان
✅ آزمون‌های نیکویی برازش
✅ آشکارسازی انرژی در $N$ بُعد

### زمان عدم استفاده

❌ متغیرها گاوسی نیستند
❌ متغیرها مستقل نیستند
❌ متغیرها نرمال استاندارد نیستند (نیاز به مقیاس‌بندی دارند)

### پیاده‌سازی در MATLAB

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

## 3.13 راهنمای انتخاب توزیع

### درخت تصمیم: توزیع‌های گسسته

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

### درخت تصمیم: توزیع‌های پیوسته

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

### الگوهای تشخیص سریع

| اگر ببینید... | فکر کنید... |
|--------------|----------|
| خروجی دودویی | برنولی |
| «$n$ آزمایش، $k$ موفقیت» | دوجمله‌ای |
| «چند تا تا نخستین...» | هندسی |
| «رویداد در هر بازه» | پواسون |
| «شانس برابر در هر جای $[a,\,b]$» | یکنواخت |
| «زمان تا رویداد بعدی» | نمایی |
| «جمع بسیاری از اثرها» یا «نویز» | گاوسی |
| «کل زمان انتظار برای $k$ رویداد» | گاما |
| «پوش سیگنال» یا «محوشدگی» | رایلی |
| «جمع مربعات نرمال‌ها» | کای‌دو |

---

## 3.14 جدول خلاصه

| توزیع | نوع | پارامترها | میانگین | واریانس | MATLAB |
|-------------|------|-----------|------|----------|--------|
| برنولی | گسسته | $p$ | $p$ | $p(1-p)$ | `binornd(1,p)` |
| دوجمله‌ای | گسسته | $n,\, p$ | $np$ | $np(1-p)$ | `binornd(n,p)` |
| هندسی | گسسته | $p$ | $\frac{1}{p}$ | $\frac{1-p}{p^{2}}$ | `geornd(p)+1` |
| دوجمله‌ای منفی | گسسته | $r,\, p$ | $\frac{r}{p}$ | $\frac{r(1-p)}{p^{2}}$ | `nbinrnd(r,p)+r` |
| پواسون | گسسته | $\lambda$ | $\lambda$ | $\lambda$ | `poissrnd(λ)` |
| یکنواخت | پیوسته | $a,\, b$ | $\frac{a+b}{2}$ | $\frac{(b-a)^{2}}{12}$ | `unifrnd(a,b)` |
| نمایی | پیوسته | $\lambda$ (یا $\beta = \frac{1}{\lambda}$) | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^{2}}$ | `exprnd(β)` |
| گاوسی | پیوسته | $\mu,\, \sigma^{2}$ | $\mu$ | $\sigma^{2}$ | `normrnd(μ,σ)` |
| گاما | پیوسته | $\alpha,\, \beta$ | $\alpha \beta$ | $\alpha \beta^{2}$ | `gamrnd(α,β)` |
| رایلی | پیوسته | $\sigma$ | $\sigma \sqrt{\frac{\pi}{2}}$ | $\frac{(4-\pi)\sigma^{2}}{2}$ | `raylrnd(σ)` |
| کای‌دو | پیوسته | $n$ | $n$ | $2n$ | `chi2rnd(n)` |

---

## 3.15 مسائل تمرینی

### مسئله 1: تشخیص توزیع
توزیع مناسب را برای هر سناریو تشخیص دهید:

(a) تعداد تماس‌های قطع‌شده در یک پنجره 10 دقیقه‌ای (میانگین: 2 در هر 10 دقیقه)
(b) ولتاژ نویز حرارتی در دو سر یک مقاومت
(c) زمان تا از کار افتادن یک سرور (نرخ خرابی ثابت)
(d) تعداد مدارهای مجتمع معیوب از میان یک دسته 100 تایی (نرخ نقص 3%)
(e) پوش سیگنال دریافتی از مسیر چندگانه بدون LOS

**حل:** (الف) $\mathrm{Poisson}(\lambda = 2)$، (ب) $\mathrm{Gaussian}(0,\, \sigma^{2})$، (ج) $\mathrm{Exponential}(\lambda)$، (د) $\mathrm{Binomial}(100,\, 0.03)$، (ه) $\mathrm{Rayleigh}(\sigma)$

### مسئله 2: نمایی در برابر پواسون
بسته‌ها به‌صورت یک فرایند پواسون با نرخ $\lambda = 5$ بسته بر ثانیه به یک مسیریاب می‌رسند.

(a) $P(\text{بیش از 8 بسته در 1 ثانیه})$ چقدر است؟
(b) $P(\text{هیچ بسته‌ای در 0.5 ثانیه بعدی نرسد})$ چقدر است؟
(c) چه توزیعی زمان بین‌ورودی را توصیف می‌کند؟
(d) میانگین زمان بین‌ورودی چقدر است؟

**حل:**
(a) $X \sim \mathrm{Poisson}(5)$: $P(X > 8) = 1 - \texttt{poisscdf}(8,\, 5) = 0.0681$
(b) $T \sim \mathrm{Exp}(5)$: $P(T > 0.5) = e^{-5 \times 0.5} = e^{-2.5} = 0.0821$
(c) نمایی با نرخ $\lambda = 5$
(d) میانگین $= \frac{1}{\lambda} = 0.2$ ثانیه

### مسئله 3: احتمال گاوسی
یک مقاومت مقدار نامی 1000Ω با تغییرات ساخت مدل‌شده به‌صورت $N(1000,\, 25)$ دارد $(\sigma = 5\,\Omega)$.

(a) چه کسری از مقاومت‌ها در محدوده $\pm 10\,\Omega$ حول مقدار نامی هستند (مشخصه: $990-1010\,\Omega$)؟
(b) چه کسری در آزمون رواداری $\pm 1\%$ رد می‌شوند؟
(c) کدام بازه رواداری شامل 99.7% مقاومت‌ها است؟

**حل:**
(a) $P(990 \le X \le 1010) = P(-2 \le Z \le 2) = 0.9545 \to 95.45\%$
(b) $\pm 1\% = \pm 10\,\Omega$ → همانند (a)، نرخ رد $= 1 - 0.9545 = 4.55\%$
(c) $\pm 3\sigma = \pm 15\,\Omega$ → محدوده $[985,\, 1015]\Omega$

---

*ماژول بعدی: [ماژول 4 — امید ریاضی و گشتاورها](module04-expectation-moments.md)*
<!-- TRANSLATION END -->
