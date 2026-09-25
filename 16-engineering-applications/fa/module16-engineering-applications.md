<!-- TRANSLATION START -->
# ماژول 16 — کاربردهای آماری مهندسی

## اهداف یادگیری

این ماژول پایانی، تمام ماژول‌های پیشین را در کاربردهای واقعی مهندسی برق یکپارچه می‌کند. دانشجویان قادر خواهند بود:
1. مدل‌سازی احتمالاتی را در سیستم‌های مخابراتی به کار ببرند
2. مسائل پردازش سیگنال را به‌صورت آماری تحلیل کنند
3. تغییرات قطعات الکترونیکی را مدل کنند
4. قابلیت اطمینان سیستم‌های قدرت را ارزیابی کنند
5. عدم‌قطعیت اندازه‌گیری در سیستم‌های کنترل را مدیریت کنند
6. احتمال/آمار را به مبانی یادگیری ماشین مرتبط کنند

---

## 16.1 مخابرات

### 16.1.1 برآورد BER و بازه‌های اطمینان

**مسئله:** برآورد نرخ خطای بیت یک لینک مخابراتی.

**مدل:** هر بیت یک آزمایش برنولی با احتمال خطای $p$ است (همان BER).
- $n$ بیت ارسال‌شده، $k$ خطا مشاهده‌شده
- $\hat{p} = \frac{k}{n}$ (برآوردگر MLE برای BER)
- بازه اطمینان 95%: $\hat{p} \pm 1.96\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$

**MATLAB: شبیه‌سازی BER و بازه اطمینان**

```matlab
%% BER Estimation with Confidence Interval
EbN0_dB = 8; EbN0 = 10^(EbN0_dB/10);
N_bits = 1e6;

% BPSK transmission
bits = 2*randi([0,1], 1, N_bits) - 1;
noise = (1/sqrt(2*EbN0)) * randn(1, N_bits);
received = bits + noise;
decisions = sign(received);
errors = sum(decisions ~= bits);

BER_hat = errors / N_bits;
BER_theory = qfunc(sqrt(2*EbN0));

% 95% Confidence Interval
margin = 1.96 * sqrt(BER_hat*(1-BER_hat)/N_bits);
fprintf('BER estimate: %.4e\n', BER_hat);
fprintf('BER theory:   %.4e\n', BER_theory);
fprintf('95%% CI: [%.4e, %.4e]\n', BER_hat-margin, BER_hat+margin);
```

### 16.1.2 مدل‌سازی نویز AWGN

نویز گاوسی سفید جمع‌شونده (AWGN) مدل نویز بنیادی است:
- **توزیع:** $N\left(0,\, \frac{N_{0}}{2}\right)$ در هر بُعد
- **چرا گاوسی؟** CLT — نویز حرارتی جمع سهم‌های بسیاری از الکترون‌ها است
- **مشخصه‌سازی:** کاملاً با چگالی طیفی توان $N_{0}$ یا واریانس $\sigma^{2} = N_{0}B$ توصیف می‌شود ($B$ = پهنای‌باند)

### 16.1.3 کانال محوشدگی رایلی

**مدل:** ضریب کانال $h = h_{I} + j \cdot h_{Q}$ که در آن $h_{I},\, h_{Q} \sim N(0,\, \sigma^{2})$ و مستقل هستند.

- پوش $\lvert h\rvert \sim \mathrm{Rayleigh}(\sigma)$
- توان $\lvert h\rvert^{2} \sim \mathrm{Exponential}\left(\frac{1}{2\sigma^{2}}\right)$
- **احتمال قطع:** $P(\lvert h\rvert^{2} < \gamma_{\mathrm{th}}) = 1 - \exp \left(-\frac{\gamma_{\mathrm{th}}}{2\sigma^{2}}\right)$

```matlab
%% Rayleigh Fading: Outage Probability
sigma = 1;
SNR_avg_dB = 10;    % Average SNR in dB
SNR_avg = 10^(SNR_avg_dB/10);
SNR_threshold_dB = 5;  % Minimum required SNR
gamma_th = 10^(SNR_threshold_dB/10);

% Analytical outage probability
P_outage = 1 - exp(-gamma_th / SNR_avg);
fprintf('Outage Probability (analytical): %.4f\n', P_outage);

% Simulation
N = 100000;
h = (randn(1,N) + 1j*randn(1,N)) / sqrt(2);  % Rayleigh fading
SNR_inst = SNR_avg * abs(h).^2;
P_outage_sim = mean(SNR_inst < gamma_th);
fprintf('Outage Probability (simulated): %.4f\n', P_outage_sim);
```

### 16.1.4 محوشدگی رایسی

هنگامی که مؤلفه خط دید (LOS) وجود دارد:
- $h = h_{\mathrm{LOS}} + h_{\mathrm{scatter}}$
- $\lvert h\rvert \sim \mathrm{Rician}(\nu,\, \sigma)$ که در آن $K = \frac{\nu^{2}}{2\sigma^{2}}$ ضریب $K$ است

ضریب $K$: $K = 0$ (رایلی) $\to K = \infty$ (بدون محوشدگی، AWGN)

### 16.1.5 آماره‌های SNR

**SNR لحظه‌ای:** $\gamma = \lvert h\rvert^{2} \times \frac{E_{s}}{N_{0}}$

برای رایلی: $\gamma \sim \mathrm{Exponential}(\bar{\gamma})$ که در آن $\bar{\gamma}$ = SNR میانگین

**میانگین BER در محوشدگی:**
$$\bar{P}_e = \int_0^\infty P_e(\gamma) \cdot f_\gamma(\gamma) \, d\gamma$$

برای BPSK در کانال رایلی: $\bar{P}_{e} = \frac{1}{2}\left(1 - \sqrt{\frac{\bar{\gamma}}{1+\bar{\gamma}}}\right)$

---

## 16.2 پردازش سیگنال

### 16.2.1 مشخصه‌سازی نویز اندازه‌گیری

**مسئله:** مشخصه‌سازی نویز در سیگنال اندازه‌گیری‌شده $x(t) = s(t) + n(t)$.

**رویکرد:**
1. نمونه‌های فقط‌نویز (در غیاب سیگنال) را جمع کنید
2. آماره‌های نویز را برآورد کنید: $\hat{\mu}_{n}$، $\hat{\sigma}^{2}_{n}$
3. گاوسی‌بودن را آزمون کنید (هیستوگرام، نمودار QQ)
4. SNR را محاسبه کنید: $\mathrm{SNR} = \frac{P_{\mathrm{signal}}}{\hat{\sigma}^{2}_{n}}$

```matlab
%% Noise Characterization
fs = 1000;  T = 10;  % 1kHz sample rate, 10 seconds
t = 0:1/fs:T-1/fs;
N_samples = length(t);

% Simulate: known signal + noise
signal = 2*sin(2*pi*5*t);  % 5 Hz sine wave
noise = 0.5*randn(1, N_samples);
measured = signal + noise;

% Estimate noise from signal-free segments (or residuals)
% Here: subtract known signal
noise_estimate = measured - signal;

fprintf('Noise Statistics:\n');
fprintf('  Mean: %.4f (should be ~0)\n', mean(noise_estimate));
fprintf('  Std:  %.4f (true: 0.5)\n', std(noise_estimate));
fprintf('  SNR:  %.1f dB\n', 10*log10(var(signal)/var(noise_estimate)));

% Gaussianity test
figure;
subplot(1,2,1); histogram(noise_estimate, 50, 'Normalization', 'pdf');
hold on; x = linspace(-2,2,200); plot(x, normpdf(x,0,0.5),'r','LineWidth',2);
title('Noise Histogram vs Gaussian');
subplot(1,2,2); qqplot(noise_estimate); title('QQ Plot');
```

### 16.2.2 آشکارسازی مبتنی بر همبستگی

**مسئله:** آشکارسازی یک سیگنال معلوم در نویز.

**روش:** سیگنال دریافتی را با الگوی معلوم همبسته کنید. تحت $H_{0}$ (فقط نویز)، خروجی همبسته‌ساز توزیع معلومی دارد → آستانه را تعیین کنید.

```matlab
%% Correlation Detector
N = 100;  % Signal length
s = ones(1,N)/sqrt(N);  % Known signal template (normalized)
sigma_n = 1;

% H0: noise only; H1: signal + noise
N_trials = 10000;
y_H0 = zeros(1, N_trials);  y_H1 = zeros(1, N_trials);
A = 2;  % Signal amplitude

for i = 1:N_trials
    n = sigma_n * randn(1, N);
    y_H0(i) = s * n';              % Correlator output under H0
    y_H1(i) = s * (A*s + n)';     % Correlator output under H1
end

% Set threshold for P_FA = 0.01
threshold = sigma_n/sqrt(N) * norminv(0.99);
P_FA = mean(y_H0 > threshold);
P_D = mean(y_H1 > threshold);
fprintf('P_FA = %.4f (target: 0.01)\n', P_FA);
fprintf('P_D = %.4f\n', P_D);
```

---

## 16.3 الکترونیک

### 16.3.1 مدل‌سازی رواداری قطعه

**مسئله:** مقاومت‌ها مقدار نامی $R_{0}$ با رواداری ساخت دارند.

**مدل:** $R \sim N(R_{0},\, \sigma_{R}^{2})$ که در آن $\sigma_{R}$ به رده رواداری بستگی دارد.

برای رواداری $\pm 5\%$ (99.7% در محدوده): $3\sigma = 0.05R_{0} \to \sigma = 0.0167R_{0}$

### 16.3.2 تحلیل مدار با مونت‌کارلو

**مسئله:** یک تقسیم‌کننده ولتاژ از $R_{1}$ و $R_{2}$ استفاده می‌کند. توزیع ولتاژ خروجی چیست؟

$V_{\mathrm{out}} = V_{\mathrm{in}} \times \frac{R_{2}}{R_{1} + R_{2}}$

با تصادفی‌بودن $R_{1}$ و $R_{2}$: $V_{\mathrm{out}}$ یک متغیر تصادفی است!

```matlab
%% Monte Carlo: Voltage Divider with Tolerant Components
V_in = 5;         % Input voltage
R1_nom = 10e3;    R2_nom = 10e3;   % 10kΩ each
tol = 0.05;       % 5% tolerance
sigma_R = tol * R1_nom / 3;  % 3-sigma = 5%

N = 100000;
R1 = R1_nom + sigma_R*randn(1, N);
R2 = R2_nom + sigma_R*randn(1, N);
V_out = V_in * R2 ./ (R1 + R2);

fprintf('Nominal V_out = %.3f V\n', V_in * R2_nom/(R1_nom+R2_nom));
fprintf('Mean V_out = %.4f V\n', mean(V_out));
fprintf('Std V_out = %.4f V\n', std(V_out));
fprintf('99.7%% range: [%.3f, %.3f] V\n', ...
    mean(V_out)-3*std(V_out), mean(V_out)+3*std(V_out));

% Yield: fraction within ±2% of nominal
V_nom = 2.5;
yield = mean(abs(V_out - V_nom) < 0.02*V_nom);
fprintf('Yield (±2%%): %.2f%%\n', 100*yield);
```

### 16.3.3 انتشار عدم‌قطعیت اندازه‌گیری

اگر $Z = f(X,\, Y)$ و $X$، $Y$ عدم‌قطعیت‌های کوچک داشته باشند:

$$\sigma_Z^2 \approx \left(\frac{\partial f}{\partial X}\right)^2 \sigma_X^2 + \left(\frac{\partial f}{\partial Y}\right)^2 \sigma_Y^2$$

**مثال:** توان $P = \frac{V^{2}}{R}$. $\sigma_{P}^{2} \approx \frac{2\,\mathrm{V}}{R}^{2}\sigma_{V}^{2} + \frac{V^{2}}{R^{2}}^{2}\sigma_{R}^{2}$

---

## 16.4 سیستم‌های قدرت

### 16.4.1 عدم‌قطعیت بار

**مسئله:** بار اوج نامعین است. مدل: $L \sim N(\mu_{L},\, \sigma_{L}^{2})$

پرسش طراحی: چه ظرفیت $C$ تضمین می‌کند $P(\text{بار} > \text{ظرفیت}) < 0.01$؟

$C = \mu_{L} + 2.326\sigma_{L}$ (برای احتمال تخطی 1%)

### 16.4.2 قابلیت اطمینان سیستم

**سیستم سری:** همه قطعات باید کار کنند.

$R_{\mathrm{sys}} = R_{1} \times R_{2} \times \ldots \times R_{n}$ (برای قطعات مستقل)

**سیستم موازی:** حداقل یکی باید کار کند.

$R_{\mathrm{sys}} = 1 - (1-R_{1})(1-R_{2})\ldots (1-R_{n})$

```matlab
%% Reliability Analysis: Series-Parallel System
% System: Two subsystems in series, each with 3 parallel redundant units
R_unit = 0.95;  % Individual component reliability
n_parallel = 3;
n_series = 2;

% Each parallel subsystem
R_subsystem = 1 - (1 - R_unit)^n_parallel;
% Series combination
R_system = R_subsystem^n_series;

fprintf('Unit reliability: %.4f\n', R_unit);
fprintf('Subsystem (3 parallel): %.6f\n', R_subsystem);
fprintf('System (2 in series): %.6f\n', R_system);

% Monte Carlo verification
N = 100000;
sys_works = true(1, N);
for s = 1:n_series
    sub_works = false(1, N);
    for p = 1:n_parallel
        unit_works = rand(1, N) < R_unit;
        sub_works = sub_works | unit_works;
    end
    sys_works = sys_works & sub_works;
end
fprintf('System reliability (sim): %.6f\n', mean(sys_works));
```

---

## 16.5 سیستم‌های کنترل

### 16.5.1 اثرهای نویز حسگر

**مسئله:** یک سیستم کنترل از قرائت‌های نویزی حسگر استفاده می‌کند.

اندازه‌گیری: $y(k) = x(k) + n(k)$ که در آن $n \sim N(0,\, \sigma^{2}_{n})$

کنترل‌کننده روی $y(k)$ عمل می‌کند → خطای ردیابی شامل سهم نویز می‌شود.

**رویکردهای حل:**
- میانگین‌گیری (فیلتر پایین‌گذر): نویز را به اندازه $\sqrt{n}$ کاهش می‌دهد
- فیلتر کالمن: تلفیق بهینه پیش‌بینی مدل و اندازه‌گیری

```matlab
%% Noisy Sensor in Control Loop: Effect of Averaging
true_value = 10;    % Constant being measured
sigma_n = 0.5;      % Sensor noise std dev
N_measurements = 1000;

% Raw measurements
measurements = true_value + sigma_n*randn(1, N_measurements);

% Moving average filter (window size M)
M_values = [1, 5, 10, 20, 50];
figure; hold on;
for M = M_values
    filtered = movmean(measurements, M);
    plot(filtered(100:500));
end
yline(true_value, 'k--', 'LineWidth', 2);
legend(arrayfun(@(m) sprintf('M=%d, σ=%.3f', m, sigma_n/sqrt(m)), ...
    M_values, 'UniformOutput', false));
xlabel('Sample'); ylabel('Estimate');
title('Noise Reduction by Averaging');
```

### 16.5.2 عدم‌قطعیت پارامتر

مدل سیستم: $G(s) = \frac{K}{\tau s + 1}$ که در آن $K$ و $\tau$ نامعین هستند.

رویکرد مونت‌کارلو: $K$ و $\tau$ را از توزیع‌هایشان نمونه‌گیری کنید، پاسخ سیستم را بارها شبیه‌سازی کنید و آماره‌های خروجی را محاسبه کنید.

---

## 16.6 ارتباط با یادگیری ماشین / هوش مصنوعی

### 16.6.1 نمونه‌گیری و جمع‌آوری داده

- داده آموزش = نمونه‌ای از جامعه
- داده آزمون = نمونه مستقل برای اعتبارسنجی
- فرض i.i.d. زیربنای بیشتر نظریه یادگیری ماشین است

### 16.6.2 برآورد به‌عنوان بنیاد یادگیری ماشین

- رگرسیون خطی = MLE تحت فرض نویز گاوسی
- آموزش شبکه عصبی = MLE (یا MAP) برای پارامترهای مدل
- کمینه‌سازی تابع هزینه = اصل بیشینه درست‌نمایی

### 16.6.3 توزیع‌های احتمال در یادگیری ماشین

- مدل‌های مولد: یادگیری مستقیم $P(X)$
- مدل‌های مخلوط گاوسی: جمع وزنی گاوسی‌ها
- خودرمزگذارهای تغییرپذیر: متغیرهای نهفته با پیشین‌های گاوسی

### 16.6.4 توازن اریبی-واریانس

$$\text{Test Error} = \text{Bias}^2 + \text{Variance} + \text{Irreducible Noise}$$

این دقیقاً همان تجزیه MSE از نظریه برآورد است (ماژول 13)!

- اریبی زیاد: کم‌برازش (مدل بسیار ساده)
- واریانس زیاد: بیش‌برازش (مدل بسیار پیچیده)
- بهینه: توازن میان این دو

### 16.6.5 اطمینان و عدم‌قطعیت در پیش‌بینی‌ها

- بازه‌های اطمینان برای پیش‌بینی‌های مدل
- شبکه‌های عصبی بیزی: توزیع پسین روی وزن‌ها → عدم‌قطعیت پیش‌بینی
- کالیبراسیون: احتمال‌های پیش‌بینی‌شده باید با فراوانی‌های مشاهده‌شده همخوانی داشته باشند

---

## 16.7 پروژه جامع MATLAB: تحلیل سیستم مخابراتی

```matlab
%% Complete Communication System Statistical Analysis
% Simulate BPSK over Rayleigh fading with AWGN
% Apply estimation, CI, and hypothesis testing

% System parameters
N_bits = 1e5;
EbN0_dB = 10;
EbN0 = 10^(EbN0_dB/10);

% Generate channel and signal
bits = randi([0,1], 1, N_bits);
symbols = 2*bits - 1;  % BPSK: ±1

% Rayleigh fading channel
h = (randn(1,N_bits) + 1j*randn(1,N_bits)) / sqrt(2);
h_mag = abs(h);

% AWGN noise
noise_var = 1/(2*EbN0);
noise = sqrt(noise_var) * randn(1, N_bits);

% Received signal (with fading)
received = h_mag .* symbols + noise;

% Detection (coherent, known channel)
detected = sign(real(received ./ h_mag));  % Zero-forcing
errors = sum(detected ~= symbols);
BER_measured = errors / N_bits;

% --- Statistical Analysis ---
fprintf('=== Communication System Statistical Analysis ===\n');
fprintf('Eb/N0 = %d dB, N = %d bits\n\n', EbN0_dB, N_bits);

% 1. BER Estimation
fprintf('1. BER ESTIMATION\n');
fprintf('   Measured BER: %.4e (%d errors)\n', BER_measured, errors);
BER_theory_rayleigh = 0.5*(1 - sqrt(EbN0/(1+EbN0)));
fprintf('   Theory (Rayleigh): %.4e\n', BER_theory_rayleigh);

% 2. Confidence Interval
fprintf('\n2. CONFIDENCE INTERVAL (95%%)\n');
margin = 1.96*sqrt(BER_measured*(1-BER_measured)/N_bits);
fprintf('   CI: [%.4e, %.4e]\n', BER_measured-margin, BER_measured+margin);
fprintf('   Theory in CI? %s\n', ...
    string(BER_theory_rayleigh >= BER_measured-margin & ...
           BER_theory_rayleigh <= BER_measured+margin));

% 3. Channel Statistics
fprintf('\n3. CHANNEL STATISTICS\n');
fprintf('   Mean |h|: %.3f (theory: %.3f)\n', mean(h_mag), sqrt(pi/4));
fprintf('   Var |h|²: %.3f (theory: %.3f)\n', var(h_mag.^2), 1-(pi/4));

% 4. Outage Probability
SNR_threshold = 3;  % dB
gamma_th = 10^(SNR_threshold/10);
P_outage_sim = mean(EbN0*h_mag.^2 < gamma_th);
P_outage_theory = 1 - exp(-gamma_th/EbN0);
fprintf('\n4. OUTAGE PROBABILITY (threshold = %d dB)\n', SNR_threshold);
fprintf('   Simulated: %.4f\n', P_outage_sim);
fprintf('   Theory:    %.4f\n', P_outage_theory);
```

---

## 16.8 نقاط یکپارچه‌سازی کلیدی

| ماژول | کاربرد در مهندسی برق |
|--------|------------------|
| 1. احتمال | احتمال‌های رویداد، تشخیص عیب |
| 2. متغیرهای تصادفی | مدل‌سازی کمیت‌های نامعین |
| 3. توزیع‌ها | انتخاب مدل‌های نویز/محوشدگی/خرابی |
| 4. گشتاورها | توان سیگنال، توان نویز، SNR |
| 5. تبدیل‌ها | تبدیل به dB، توان از ولتاژ |
| $6-7$. مشترک/توابع | سیگنال+نویز، ترکیب‌های سیستم |
| 8. شرطی | عملکرد در وضعیت‌های مختلف |
| 9. همبستگی | کانال‌های MIMO، آرایه‌های حسگر |
| 10. چند متغیر تصادفی | پردازش برداری، شکل‌دهی پرتو |
| 11. CLT | چرا نویز گاوسی است، میانگین‌گیری |
| 12. آمار | برآورد پارامترهای سیستم |
| 13. برآورد | MLE برای پارامترهای کانال/نویز |
| 14. اطمینان | کمّی‌سازی عدم‌قطعیت اندازه‌گیری |
| 15. فرض | کنترل کیفیت، مقایسه عملکرد |

---

*پایان ماژول‌های درس. برای راهنماهای تصمیم، مرجع MATLAB و برگه فرمول به [مطالب تکمیلی](../supplementary/) مراجعه کنید.*
<!-- TRANSLATION END -->
