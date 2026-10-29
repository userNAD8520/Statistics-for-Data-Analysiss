# STATS 1060 — Midterm Solutions in R (with full questions)

> Allowed functions: `mean()`, `sd()`, `factorial()`, `exp()`, `pnorm()`, `qnorm()`, `pt()`, `qt()`, `choose()`.
> Binomial/Poisson written by formula (manual), per the command templates.

---

# PART 1 — Practice Midterm (July 2016)

## Q1
**Question:** A manufacturer claims that their stainless steel oxygen lines are manufactured to a mean outside diameter of 10 mm. The standard deviation is known to be 1.1 mm. You take a sample of 37 tubes and measure the outside diameter, finding the mean to be 9.5 mm. At 97% confidence, is the mean outside diameter statistically smaller than the manufacturer's claimed mean?

```r
sigma <- 1.1; n <- 37; xbar <- 9.5; conf <- 0.97
alpha <- 1 - conf
z_crit <- qnorm(1 - alpha/2)        # 2.1701
E <- z_crit * sigma / sqrt(n)       # 0.3924
c(xbar - E, xbar + E)               # (9.108, 9.892)
```
**Answer:** 9.5 ± 0.39 → (9.11, 9.89). Entire interval below 10 → **yes, smaller.**

## Q2
**Question:** In a certain town, the number of emergency calls for an ambulance is 0.8 per hour. What is the probability that in any given hour there will be 3 or more emergency calls?

```r
mu <- 0.8; x <- 3
1 - sum(mu^(0:(x-1)) * exp(-mu) / factorial(0:(x-1)))
# 0.0474
```
**Answer:** 0.0474

## Q3
**Question:** Magnetic fluctuations from a superconducting magnet are normally distributed around a mean field strength of 4.5 A/m with standard deviation 0.25 A/m.
- (a) If the field fluctuates above 5.2 A/m, the field collapses. What is the probability of collapse?
- (b) What range of field strengths constitutes the lowest 17%?

```r
# (a) P(x > 5.2)
pnorm(5.2, mean = 4.5, sd = 0.25, lower.tail = FALSE)   # 0.0026

# (b) lowest 17%
qnorm(0.17, mean = 4.5, sd = 0.25)                      # 4.26
```
**Answer:** (a) 0.0026 = 0.26%, (b) 4.26 A/m

## Q4
**Question:** 10% of men cannot distinguish red from green (colour blindness). If 6 men are randomly selected, find the probability that at least two of them cannot distinguish red from green.

```r
n <- 6; p <- 0.1; x <- 2
1 - sum(choose(n, 0:(x-1)) * p^(0:(x-1)) * (1-p)^(n-(0:(x-1))))
# 0.1143
```
**Answer:** 0.114

## Q5
**Question:** Nicotine in SomeBrand cigarettes has mean 0.941 g and standard deviation 0.313 g. A sample of 80 cigarettes has mean 0.882 g.
- (a) Assuming the mean and standard deviation have not changed, find the probability of getting a sample mean of 0.882 g or less.
- (b) Is it valid to claim the nicotine is lower? Why?

```r
mu <- 0.941; sigma <- 0.313; n <- 80
se <- sigma / sqrt(n)
pnorm(0.882, mean = mu, sd = se)
# 0.0459
```
**Answer:** (a) 0.0459 = 4.6%. (b) Small → plausible nicotine is lower.

## Q6
**Question:** Women's SAT scores: mean 998, sd 202. Women's ACT scores: mean 20.9, sd 4.6. If a woman scores 1220 on the SAT, find her equivalent ACT score.

```r
z <- (1220 - 998) / 202
20.9 + z * 4.6
# 25.96
```
**Answer:** 26.0

---

# PART 2 — Midterm Fall 2019

## Q1
**Question:** Customers can choose from 10 CPUs, 3 memory capacities, 5 keyboards, and 4 monitors. How many possible desktop computer systems can be made?

```r
prod(10, 3, 5, 4)   # 600
```
**Answer:** 600

## Q2
**Question:** You want to take 3 online courses from 1000 available. How many ways can you do it?

```r
choose(1000, 3)     # 166167000
```
**Answer:** 1.6617 × 10⁸

## Q3
**Question:** A random number generator produces integers from 1 to 99 inclusive. What is the probability that a generated number is even?

```r
49 / 99             # 0.4949
```
**Answer:** 0.4949

## Q4
**Question:** 98% of BCIT graduates and 77% of other institutions' graduates find jobs within 3 months. At a conference, 30 BCIT graduates and 152 others attend (everyone graduated 3 months ago). You randomly pick one of the 182 who has a job. What is the probability this person is a BCIT graduate?

```r
(30 * 0.98) / (30 * 0.98 + 152 * 0.77)   # 0.2008
```
**Answer:** 0.2008

## Q5
**Question:** MATH1060 test marks are normally distributed with mean 78% and sd 14%.
- (a) P(score < 60%)
- (b) P(score > 90%)
- (c) P(score within ±1.5 standard deviations)
- (d) 60th and 90th percentiles

```r
pnorm(60, mean = 78, sd = 14)                      # (a) 0.0993
pnorm(90, mean = 78, sd = 14, lower.tail = FALSE)  # (b) 0.1957
pnorm(1.5) - pnorm(-1.5)                           # (c) 0.8664
qnorm(0.60, mean = 78, sd = 14)                    # (d) 81.5
qnorm(0.90, mean = 78, sd = 14)                    # (d) 95.9
```
**Answers:** (a) 0.0993, (b) 0.1957, (c) 0.8664, (d) 81.5 & 95.9

## Q6
**Question:** On average, one asteroid hits Earth every 500,000 years. Estimate the probability that at least one asteroid hits Earth in the next 1000 years.

```r
mu <- 1 * 1000 / 500000    # 1 per 500,000 → 0.002 per 1,000
x <- 1
1 - sum(mu^(0:(x-1)) * exp(-mu) / factorial(0:(x-1)))
# = 1 - exp(-0.002) = 0.0020
```
**Answer:** 0.0020

## Q7
**Question:** Puffer fish: probability of getting poisoned is 0.001.
- (a) three out of ten people get poisoned
- (b) more than or equal to three out of ten get poisoned

```r
n <- 10; p <- 0.001

# (a) exactly 3
choose(n, 3) * p^3 * (1-p)^(n-3)                    # 1.192e-07

# (b) at least 3
x <- 3
1 - sum(choose(n, 0:(x-1)) * p^(0:(x-1)) * (1-p)^(n-(0:(x-1))))   # 1.194e-07
```
**Answers:** (a) 1.192 × 10⁻⁷, (b) 1.194 × 10⁻⁷

## Q8
**Question:** Thickness of 300 pizzas is measured daily. One day the mean was 14 mm and sd was 2 mm. Assuming normal, find the probability that the mean is between 13.9 and 14.1 mm.

```r
mu <- 14; sigma <- 2; n <- 300
se <- sigma / sqrt(n)
pnorm(14.1, mean = mu, sd = se) - pnorm(13.9, mean = mu, sd = se)
# 0.6135
```
**Answer:** 0.6135

## Q9
**Question:** A radar measures distance with sd 0.15 m. Find the minimum number of measurements required for a margin of error of 0.05 m at 99% confidence.

```r
sigma <- 0.15; E <- 0.05; conf <- 0.99
alpha <- 1 - conf
n <- (qnorm(1 - alpha/2) * sigma / E)^2
ceiling(n)
# 60
```
**Answer:** 60 measurements (assume independent, normal measurements).

## Q10
**Question:** Train delay times (min): 2.2, −0.5, 1.3, 3.5, 2.1, 0.3 (negative = ahead of schedule). Assume normal.
- (a) 80% CI for the mean, assuming population sd = 1.5 min.
- (b) 80% CI for the mean, if population sd is unknown.

```r
x <- c(2.2, -0.5, 1.3, 3.5, 2.1, 0.3)
xbar <- mean(x)      # 1.4833
s    <- sd(x)        # 1.4372
n <- 6; conf <- 0.80
alpha <- 1 - conf

# (a) σ KNOWN = 1.5 → z
sigma <- 1.5
z_crit <- qnorm(1 - alpha/2)
E <- z_crit * sigma / sqrt(n)
c(xbar - E, xbar + E)     # (0.6985, 2.2681)

# (b) σ UNKNOWN → t, df = n-1
t_crit <- qt(1 - alpha/2, df = n - 1)
E <- t_crit * s / sqrt(n)
c(xbar - E, xbar + E)     # (0.6174, 2.3493)
```
**Answers:** (a) 0.70 < μ < 2.27 (z), (b) 0.62 < μ < 2.35 (t, wider).

---

# PART 3 — Midterm Solutions Set

## Q1
**Question:** Hours internet users spend online per week are normally distributed with mean 22.4 hours and sd 6.9 hours.
- (a) Find the z-score of 30 hours.
- (b) Find the percentile rank of 30 hours.
- (c) Find the 10th percentile.

```r
# (a) z-score of 30
z <- (30 - 22.4) / 6.9           # 1.1014

# (b) percentile rank of 30
pnorm(30, mean = 22.4, sd = 6.9) # 0.8646

# (c) 10th percentile
qnorm(0.10, mean = 22.4, sd = 6.9) # 13.56
```
**Answers:** (a) 1.10, (b) 86.5th percentile, (c) 13.6 hours

## Q2
**Question:** An online shop has a 2% conversion rate (probability a customer makes a purchase = 2%). If 150 customers visit, find the probability that at least 2 make a purchase.

```r
n <- 150; p <- 0.02; x <- 2
1 - sum(choose(n, 0:(x-1)) * p^(0:(x-1)) * (1-p)^(n-(0:(x-1))))
# 0.8039
```
**Answer:** 0.8039

## Q3
**Question:** Daily website visitors are normally distributed with mean 1250 and sd 240. Find the probability that visitors on a given day are between 900 and 1400.

```r
pnorm(1400, mean = 1250, sd = 240) - pnorm(900, mean = 1250, sd = 240)
# 0.6616
```
**Answer:** 0.6616

## Q4
**Question:** Visitors per day to an amusement park are sampled for 5 days: 711, 649, 602, 634, 671. Find the 90% CI for the population mean, assuming normal.

```r
x <- c(711, 649, 602, 634, 671)
xbar <- mean(x)      # 653.4
s    <- sd(x)        # 40.82
n <- 5; conf <- 0.90
alpha <- 1 - conf
t_crit <- qt(1 - alpha/2, df = n - 1)   # 2.1318
E <- t_crit * s / sqrt(n)               # 38.92
c(xbar - E, xbar + E)                   # (614.5, 692.3)
```
**Answer:** 614.5 < μ < 692.3 (t, df = 4)

## Q5
**Question:** A drugstore sells brand-A cough syrup at an average of 0.65 bottles per week. Find the probability that 3 or more bottles are sold in a given week.

```r
mu <- 0.65; x <- 3
1 - sum(mu^(0:(x-1)) * exp(-mu) / factorial(0:(x-1)))
# 0.0283
```
**Answer:** 0.0283

## Q6
**Question:** 50 beer bottles are sampled; mean alcohol content 11.55 ml, sd 0.37 ml (assume sd ≈ population sd).
- (a) Find the 95% CI for the mean alcohol content.
- (b) To make the margin of error one tenth of the value in (a), what minimum sample size is needed (same sd 0.37, same 95%)?

```r
xbar <- 11.55; s <- 0.37; n <- 50; conf <- 0.95
alpha <- 1 - conf

# (a) CI using z (n large)
z_crit <- qnorm(1 - alpha/2)         # 1.9600
E <- z_crit * s / sqrt(n)            # 0.1026
c(xbar - E, xbar + E)                # (11.447, 11.653)

# (b) reduce E by 10x → n × 100
n_new <- n * 10^2                    # 5000
```
**Answers:** (a) 11.45 < μ < 11.65, (b) minimum n = 5000

---

## The "at least x" templates (used throughout)

```r
# Binomial — at least x successes
1 - sum(choose(n, 0:(x-1)) * p^(0:(x-1)) * (1-p)^(n-(0:(x-1))))

# Poisson — at least x occurrences
1 - sum(mu^(0:(x-1)) * exp(-mu) / factorial(0:(x-1)))
```
