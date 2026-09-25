<div align="center">

<img src="template/ferdowsi-logo-white.png" width="130" alt="Ferdowsi University of Mashhad">

# آمار و احتمال مهندسی
### Engineering Probability & Statistics

**نیمسال پاییز ۱۴۰۵-۱۴۰۶ · Fall 2026–2027**
دانشگاه فردوسی مشهد · Ferdowsi University of Mashhad

استاد: [امین آذری](https://github.com/AminAzari) · Instructor: [Amin Azari](https://github.com/AminAzari)

گردآوری شده توسط [آیدین شکاری](https://shekari.me) · Compiled by [Aidin Shekari](https://shekari.me)

</div>

---

## 📘 جزوه کامل · Complete Notes

| | فارسی | English |
|---|---|---|
| جزوه کامل (همه ماژول‌ها) · Full notes | [ProbAndStats-fa.pdf](00-full-notes/fa/ProbAndStats-fa.pdf) | [ProbAndStats-en.pdf](00-full-notes/en/ProbAndStats-en.pdf) |
| طرح درس · Syllabus | [syllabus.pdf](00-syllabus/fa/syllabus.pdf) | — |

## 📑 ماژول‌ها · Modules

| # | ماژول | Module | PDF (fa) | PDF (en) |
|---|---|---|---|---|
| 1 | احتمال | Probability | [PDF](01-probability/fa/module01-probability.pdf) | [PDF](01-probability/en/module01-probability.pdf) |
| 2 | یک متغیر تصادفی | One Random Variable | [PDF](02-one-random-variable/fa/module02-one-random-variable.pdf) | [PDF](02-one-random-variable/en/module02-one-random-variable.pdf) |
| 3 | توزیع‌های مهم | Important Distributions | [PDF](03-important-distributions/fa/module03-important-distributions.pdf) | [PDF](03-important-distributions/en/module03-important-distributions.pdf) |
| 4 | امید ریاضی و گشتاورها | Expectation & Moments | [PDF](04-expectation-moments/fa/module04-expectation-moments.pdf) | [PDF](04-expectation-moments/en/module04-expectation-moments.pdf) |
| 5 | توابع متغیر تصادفی | Functions of a RV | [PDF](05-rv-functions/fa/module05-rv-functions.pdf) | [PDF](05-rv-functions/en/module05-rv-functions.pdf) |
| 6 | دو متغیر تصادفی | Two Random Variables | [PDF](06-two-random-variables/fa/module06-two-random-variables.pdf) | [PDF](06-two-random-variables/en/module06-two-random-variables.pdf) |
| 7 | توابع دو متغیر تصادفی | Functions of Two RVs | [PDF](07-functions-two-rv/fa/module07-functions-two-rv.pdf) | [PDF](07-functions-two-rv/en/module07-functions-two-rv.pdf) |
| 8 | امید ریاضی شرطی | Conditional Expectation | [PDF](08-conditional-expectation/fa/module08-conditional-expectation.pdf) | [PDF](08-conditional-expectation/en/module08-conditional-expectation.pdf) |
| 9 | کوواریانس و همبستگی | Covariance & Correlation | [PDF](09-covariance-correlation/fa/module09-covariance-correlation.pdf) | [PDF](09-covariance-correlation/en/module09-covariance-correlation.pdf) |
| 10 | چند متغیر تصادفی | Multiple Random Variables | [PDF](10-multiple-random-variables/fa/module10-multiple-random-variables.pdf) | [PDF](10-multiple-random-variables/en/module10-multiple-random-variables.pdf) |
| 11 | قضیه حد مرکزی | Central Limit Theorem | [PDF](11-central-limit-theorem/fa/module11-central-limit-theorem.pdf) | [PDF](11-central-limit-theorem/en/module11-central-limit-theorem.pdf) |
| 12 | مقدمه‌ای بر آمار | Intro to Statistics | [PDF](12-intro-statistics/fa/module12-intro-statistics.pdf) | [PDF](12-intro-statistics/en/module12-intro-statistics.pdf) |
| 13 | برآورد پارامتر | Parameter Estimation | [PDF](13-parameter-estimation/fa/module13-parameter-estimation.pdf) | [PDF](13-parameter-estimation/en/module13-parameter-estimation.pdf) |
| 14 | فاصله اطمینان | Confidence Intervals | [PDF](14-confidence-intervals/fa/module14-confidence-intervals.pdf) | [PDF](14-confidence-intervals/en/module14-confidence-intervals.pdf) |
| 15 | آزمون فرض | Hypothesis Testing | [PDF](15-hypothesis-testing/fa/module15-hypothesis-testing.pdf) | [PDF](15-hypothesis-testing/en/module15-hypothesis-testing.pdf) |
| 16 | کاربردهای مهندسی | Engineering Applications | [PDF](16-engineering-applications/fa/module16-engineering-applications.pdf) | [PDF](16-engineering-applications/en/module16-engineering-applications.pdf) |

## 🗂 ساختار پوشه‌ها · Repository layout

```
ProbAndStats/
├── 00-full-notes/
│   ├── fa/   ProbAndStats-fa.tex + .pdf   جزوه کامل فارسی (با فهرست مطالب)
│   └── en/   ProbAndStats-en.tex + .pdf   complete English notes
├── 00-syllabus/fa/                        طرح درس
├── 01-probability/
│   ├── fa/   module01-probability.md · .tex · content.tex · .pdf
│   └── en/   module01-probability.md · .tex · content.tex · .pdf
├── …                                      (one folder per topic, 01 … 16)
└── template/                              قالب لتک، لوگوی دانشگاه، فونت B Nazanin
```

هر فایل `.tex` داخل پوشه‌ی خودش با XeLaTeX کامپایل می‌شود ·
Every `.tex` compiles with XeLaTeX from inside its own folder
(fonts: *B Nazanin* bundled in `template/fonts/`, *Times New Roman* and *Menlo*).
