---
layout : page
title: Statistical Inference
date : 2025-09-05
---
# Introduction

In **statistical inference**, a **parameter** is a fixed, unknown numerical value that describes a characteristic of an entire population. You can study the **characteristics/estimators** (eg. mean, SD, proportion, etc.) of the parameter on a **sample** of the population. You can do parameter **estimation**.

# Parameter Estimation

It can be point estimation or interval estimation. A **point estimate** is a single numerical value that serves as the "best guess" or single-point approximation of an unknown population parameter. While **interval estimation** provides a range of values with a certain level of confidence. Confidence interval is a range of values that is likely to contain the true population parameter with a certain level of confidence. You can study the **distribution** of the estimate of that parameter using **statistical methods**.

# Hypothesis Testing

Unlike Estimation, in **Hypothesis testing** on a parameter, we have a pre-idea of the population parameter (which you may arrive at with some **EDA**). Hypothesis testing is a method of testing that claim or hypothesis about a population parameter using sample data. It involves setting up a **null hypothesis (H0)** and an **alternative hypothesis (H1)**, and then using statistical methods to determine whether the null hypothesis can be rejected in favor of the alternative hypothesis. H1 can be omitted if you you're qualitatively sure about H0 and just want to quantitatively test it.

In hypothesis testing, you often look for a single numeric value called **test statistic** and if it lies in the **region of acceptance**, then you accept H0, and if it lies in the **critical region**, you reject H0. If we incorrectly reject H0, it's called a **Type 1 error**, and if we incorrectly accept H0, it's called **Type 2 error**. It's difficult to minimise both errors simultaneously, so we need to see which is practically more critical and then focus on minimizing that type of error.

We have 2 probabilities:
Alpha (α): Probability of Type 1 error; False positive rate
Beta (β): Probability of Type 2 error; False negative rate

Commonly, $\alpha$ (called **significance level**) is set at 0.05 (5%), meaning a 5% risk of Type 1 error is tolerated in the test decision. $\beta$ is often set between 0.10 and 0.20 (10% to 20%), reflecting the risk of Type 2 error. The lower the $\beta$, the higher the power of the test (1−β).

## Types of Hypothesis Tests

##  1.   Z Test

A **z test** tests a **hypothesis of population mean based on a sample mean**. It is a type of hypothesis test which tests if the "actual" sample mean is **significantly** different from a "hypothesized" population mean. It's a fun fact that even if the difference between these 2 means may be substantial, it may not be statistically significant if the region of acceptance is not breached !

In z-test,  $H_0$ = sample mean is statistically similar to population mean
and $H_1$= sample mean is statistically different (+ or -) from the population mean

### Assumptions
The z-test assumes :
-   **Sample size is large (n > 30)** or the population is normally distributed. For large sample size, normality is assured as per the Central Limit Theorem. When in doubt, you can use a normality test (e.g., Shapiro-Wilk) or a histogram to check this.
-   **Population standard deviation (σ) is known** : You can assume a value if not known.
-   **Data is independent**

### Key Features


- The **z-test statistic** ($z$) is calculated as:
  $$
  z = \frac{\overline{x}-\mu}{\sigma/\sqrt{n}}
  $$
  where $\overline{x}$ is the "actual" sample mean, $\mu$ is the "hypothesized" population mean, $\sigma$ is the "known/guessed" population standard deviation, and $n$ is the sample size. The quantity $({\sigma/\sqrt{n}})$ is also called **standard error** of the mean.
- The result (z-score) tells how many standard deviations the sample mean is from the population mean.

### Types of Z Tests

- **One-sample z test**: Compares the mean of a single sample to a known population mean.
- **Two-sample z test**: Compares the means of two independent samples to see if they are significantly different.
- **Proportion z test**: Tests hypotheses about proportions in large samples.

### Practical Steps

1. State the null and alternative hypothesis.
2. Choose the significance level ($\alpha$) and use it to find the critical value from the z-table.
3. Calculate the z statistic using the formula for $z$.
4. Compare the statistic with the critical value to decide to accept or reject the null hypothesis.
5. Alternatively, compare the p-value (tail area after $z)$ with $\alpha$ (tail area after critical value) to decide to accept ($p$>$\alpha$) or reject the null hypothesis

In essence, we are converting our chosen parameter into a $z$ score having a standard-normal $\mathcal{N}$(0,1) distribution. Graphically, the distribution of $z$ would be as follows :

![img](https://i.postimg.cc/4xfrnd25/Screenshot-2025-09-06-at-02-22-31-Python-Playground-Programiz.png)

The midpoint $z$=0 denotes perfect compliance with the null hypothesis $H_0$. The area other than critical region is the region of acceptance. Again note that z-test is mainly limited to **testing a hypothesis of population mean based on a sample mean**.


### R function

In R, you can use the `z.test()` function from the **BSDA** package to perform z test on a single numeric column of a dataframe (i.e. one-sample z-test).
```R
z.test(x, mu, sigma.x)
```
- **x**: Numeric vector of sample data.
- **mu**: Hypothesized population mean.
- **sigma.x**: Known population standard deviation.

Default: `alternative = "two.sided"`, `conf.level = 0.95`.



## 2. T Test

Z-test is great for example in analyzing the mean gene expression across thousands of microarray measurements or say in aggregated read counts in whole-genome sequencing, where population standard deviation has been well-characterized in prior studies. But consider comparing the mean expression of a particular gene in two small groups of patients (say n = 15 per group) for a disease study. In such cases **where population variance is unknown and/or sample size is small, t-test is preferred.** Like z-test, normality of data is needed for t-test as well.

Like the z-test, the t-test allows you to transform your data into a score fitting a known distribution, but here the distribution is **Student’s t**. For smaller sample sizes (n < 30), the t-distribution is wider and has larger critical values than the standard normal (z) distribution. This means that the t-test requires stronger evidence (a larger absolute test statistic) to reject the null hypothesis. However the t-score tends to be smaller because its given by :
  $$
  t = \frac{\overline{x} - \mu}{s / \sqrt{n}}
  $$
  where $s$ is the sample standard deviation and is usually larger than the population standard deviation $\sigma$ used in z-test.

In R, you can use the built-in `t.test()` function to perform a t-test on a numeric column:
```R
t.test(x, mu)
```
