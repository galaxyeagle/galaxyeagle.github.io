---
layout : page
title: Statistical Inference
date : 2025-09-05
---
<style>
  html {
    background: #ffffff no-repeat center center fixed;
    background-size: cover;
  }
  #bg-image {
    display: none; /* Hide video on this page */
  }
  .body-css {
    color: black; /* Set default text color for body for this page only */
  }
  a { color: blue; }
  a:hover { color: red; }

  pre, code {
    background-color: #585858;  /* dark theme */
    color: #f0f0f0;
    border-radius: 6px;
  }

</style>



# Table of Contents

1. [Introduction](#introduction)
2. [Parameter Estimation](#parameter-estimation)
3. [Hypothesis Testing](#hypothesis-testing)
4. [Normality Tests](#normality-tests)
   4.1 [Boxplot](#1-boxplot)
   4.2 [Q-Q Plot (Quantile-Quantile Plot)](#2-q-q-plot-quantile-quantile-plot)
   4.3 [Shapiro-Wilk Test](#3-shapiro-wilk-test)
   4.4 [Kolmogorov-Smirnov Test](#4-kolmogorov--smirnov-test)
   4.5 [Lilliefors (Kolmogorov-Smirnov) Test](#5-lilliefors-kolmogorov-smirnov-test)
5. [Types of Hypothesis Tests](#types-of-hypothesis-tests)
   5.1 [Z Test](#1-z-test)
       5.1.1 [Assumptions](#assumptions)
       5.1.2 [Key Features](#key-features)
       5.1.3 [Practical Steps](#practical-steps)
       5.1.4 [R function](#r-function)
   5.2 [T Test](#2-t-test)
       5.2.1 [Types of T Tests](#types-of-t-tests)
             5.2.1.1 [One-sample t test](#one-sample-t-test)
             5.2.1.2 [Two-sample t test](#two-sample-t-test)
                      5.2.1.2.1 [Independent samples t test](#independent-samples-t-test)
                      5.2.1.2.2 [Paired t test](#paired-t-test)




# Introduction

In **statistical inference**, a **parameter** is a fixed, unknown numerical value that describes a characteristic of an entire population. You can study the **characteristics/estimators** (eg. mean, SD, proportion, etc.) of the parameter on a **sample** of the population. You can do parameter **estimation**.

# Parameter Estimation

It can be point estimation or interval estimation. A **point estimate** is a single numerical value that serves as the "best guess" or single-point approximation of an unknown population parameter. While **interval estimation** provides a range of values with a certain level of confidence. Confidence interval is a range of values that is likely to contain the true population parameter with a certain level of confidence. You can study the **distribution** of the estimate of that parameter using **statistical methods**.

# Hypothesis Testing

Unlike Estimation, in **Hypothesis testing** on a parameter, we have a pre-idea of the population parameter (which you may arrive at with some **EDA**). Hypothesis testing is a method of testing that claim or hypothesis about a population parameter using sample data. It involves setting up a **null hypothesis (H0)** and an **alternative hypothesis (H1)**, and then using statistical methods to determine whether the null hypothesis can be rejected in favor of the alternative hypothesis. H1 can be omitted if you you're qualitatively sure about H0 and just want to quantitatively test it.

In hypothesis testing, you often look for a single numeric value called **test statistic** and if it lies in the **region of acceptance**, then you accept H0, and if it lies in the **critical region**, you reject H0. If we incorrectly reject H0, it's called a **Type 1 error**, and if we incorrectly accept H0, it's called **Type 2 error**. It's difficult to minimise both errors simultaneously, so we need to see which is practically more critical and then focus on minimizing that type of error.

We have 2 probabilities:
Alpha (α):  Probability of Type 1 error; False positive rate
Beta (β):  Probability of Type 2 error; False negative rate

Commonly, $$\alpha$$ (called **significance level**) is set at 0.05 (5%), meaning a 5% risk of Type 1 error is tolerated in the test decision. $$\beta$$ is often set between 0.10 and 0.20 (10% to 20%), reflecting the risk of Type 2 error. The lower the $$\beta$$, the higher the power of the test (1−β).


# Normality Tests

We'll study this before moving to hypothesis tests like z-test and t-test. Normality tests check if the data follows a **normal (Gaussian) distribution**. This is important because many parametric tests, including z-test and t-test, assume normality.

A normality test’s null hypothesis $$H_0$$ states that the data is normally distributed; rejecting it means data significantly deviates from normal.


## 1. Boxplot

-   A **boxplot** is a simple graphical tool that displays the distribution of data through quartiles and highlights outliers.
-   It helps visually assess symmetry and detect skewness or extreme values—signs the data may not be normal.
-  `boxplot(x)`


## 2. Q-Q Plot (Quantile-Quantile Plot)

-   A **Q-Q plot** graphs the quantiles of the data against the quantiles of a normal distribution.
-   You first use `qqnorm()` to generate the basic Q-Q plot of your data.
-   Then, you use `qqline()` to add the reference line to this plot.
-   You then look at the resulting plot: if the data points from `qqnorm()` fall closely along the `qqline()`, it indicates that the data is approximately normally distributed. Deviations from the line suggest that the data does not follow a normal distribution

## 3. Shapiro-Wilk Test

-   A **statistical test specifically designed to test normality**, especially effective for **small to medium-sized samples (<2000)**.
-  Null hypothesis: sample comes from a normal distribution.
-   A **p-value > 0.05** suggests the data is likely normal; **p-value < 0.05** suggests non-normality.
-   The **W statistic** measures how well your sample data fits a normal distribution, with values closer to 1 indicating better fit.
- `shapiro.test(your_data_vector)` function in R is used for this purpose.


## 4. Kolmogorov- Smirnov Test

`ks.test(x, "pnorm", mean=mean(x), sd=sd(x))`

Here also the null hypothesis is that the sample is normally distributed. A **p-value** > 0.05 indicates acceptance of the null hypothesis. A **D-value** is also generated which represents the maximum distance between your sample's cumulative distribution function (CDF) and the CDF of a perfect normal distribution. A larger D-value indicates a greater deviation from normality.
This test is used for larger datasets cf. Shapiro-Wilks.

## 5. Lilliefors (Kolmogorov-Smirnov) Test

This is a variant of the earlier K-S test and may give a slightly different p-value.
```r
install.packages(nortest)
library(nortest)
lillie.test(your_data_vector)
```
----------
***To conclude :***

1.  Use **boxplots or Q-Q plots** for a quick visual check of normality.
2.  Confirm with **Shapiro-Wilk test** for small/medium datasets, or **K-S test** for larger datasets.
3.  Interpret **p-values**:
    -   p>0.05 → Fail to reject normality (data may be normal)
    -   p<0.05 → Reject normality (non-normal data)

---

# Types of Hypothesis Tests

##  1.   Z Test

A **z test** tests a **hypothesis of population mean based on a sample mean**. It is a type of hypothesis test which tests if the "actual" sample mean is **significantly** different from a "hypothesized" population mean. It's a fun fact that even if the difference between these 2 means may be substantial, it may not be statistically significant if the region of acceptance is not breached !

In z-test,  $$H_0$$ = sample mean is statistically similar to population mean
and $$H_1$$= sample mean is statistically different (+ or -) from the population mean

### Assumptions
The z-test assumes :
-   **Sample size is large (n > 30)** or the population is normally distributed. For large sample size, normality is assured as per the Central Limit Theorem. When in doubt, you can use a normality test (e.g., Shapiro-Wilk) or a histogram to check this.
-   **Population standard deviation (σ) is known** : You can assume a value if not known.
-   **Data is independent**

### Key Features


- The **z-test statistic** ($$z$$) is calculated as:
  $$
  z = \frac{\overline{x}-\mu}{\sigma/\sqrt{n}}
  $$
  where $$\overline{x}$$ is the "actual" sample mean, $$\mu$$ is the "hypothesized" population mean, $$\sigma$$ is the "known/guessed" population standard deviation, and $$n$$ is the sample size. The quantity $$({\sigma/\sqrt{n}})$$ is also called **standard error** of the mean.
- The result (z-score) tells how many standard deviations the sample mean is from the population mean.

### Practical Steps

1. State the null and alternative hypothesis.
2. Choose the significance level ($$\alpha$$) and use it to find the critical value from the z-table.
3. Calculate the z statistic using the formula for $$z$$.
4. Compare the statistic with the critical value to decide to accept or reject the null hypothesis.
5. Alternatively, compare the p-value (tail area after $$z$$) with $$\alpha$$ (tail area after critical value) to decide to accept ($$p$$>$$\alpha$$) or reject the null hypothesis

In essence, we are converting our chosen parameter into a $$z$$ score having a standard-normal $$\mathcal{N}$$(0,1) distribution. Graphically, the distribution of $$z$$ would be as follows :

<img class="exim" src="https://i.postimg.cc/4xfrnd25/Screenshot-2025-09-06-at-02-22-31-Python-Playground-Programiz.png">
<style> .exim{float:left; width:300px; height:200px; padding-right:20px} </style>

The midpoint $$z$$=0 denotes perfect compliance with the null hypothesis $$H_0$$. The area other than critical region is the region of acceptance. Again note that z-test is mainly limited to **testing a hypothesis of population mean based on a sample mean**.


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
  where $$s$$ is the sample standard deviation and is usually larger than the population standard deviation $$\sigma$$ used in z-test.

In R, you can use the built-in `t.test()` function to perform a t-test on a numeric column:
```R
t.test(x, mu)
```
Apart from x and mu, some other parameters can be passed to the function, such as `alternative` to specify the alternative hypothesis, `var.equal` to specify whether the variances of the two groups are equal, `paired` to specify whether the data is paired or not and `conf.level` to specify the confidence level (eg. 0.95) of the confidence interval.

The output of the `t.test()` function is a dataframe with columns like `output$statistic`, `output$conf.int`,`output$p.value`, `output$parameter`(for degrees of freedom), etc.

### Types of T Tests

- #### One-sample t test :
  Compares the mean of a single sample to a known population mean.
- #### Two-sample t test:
  Compares the means of two independent samples to see if they are significantly different.
    - ##### Independent samples t test:
    Compares the means of two independent samples to see if they are significantly different.
  In the default pooled test version, we need to check the statistical similarity in the variances of the 2 samples using:
  ```r
  var.test(data$sample1, data$sample2)
  ```
  But even if the variances are not equal, the test can still be used using the `var.equal=False` argument (Welch's t-test).
  Eg.
  ```r
  t.test(data$sample1, data$sample2, alternative="greater", paired = TRUE)
  ```

  - ##### Paired t test:
  Similar to independent samples t test, but here both groups are the same sample under different conditions. Here you'll use the `paired=True` argument in `t.test()`.
  Here it's mandatory to first test the normality of the difference between the two samples.
  ```r
  data$diff <- data$before_treat - data$after_treat
  shapiro.test(data$diff)
  ```
  Here `alternative = "greater"` specifies that the alternative hypothesis is that the mean of `before_treat` is greater than the mean of `after_treat`.


  **Note on two-sample t-tests** : In these tests, by default we assume that the experimenter has no indication of a directional hypothesis (e.g., no prior evidence suggesting the values of sample-1 are greater than sample-2). Thus `alternative = "two.sided"` argument is assumed by default. However if we have a directional research question like "Is the new drug better than the old one?", then we can specify `alternative = "greater"` or `alternative = "less"` depending on the direction of the hypothesis, and its called a ***one-tailed/one-sided*** test.
