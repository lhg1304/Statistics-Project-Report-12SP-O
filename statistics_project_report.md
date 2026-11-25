---
title: "Statistics Practical Project"
subtitle: "Introduction to Bioinformatics"
format:
  html:
    toc: false
    number-sections: false
    code-fold: false
execute:
  echo: true
  warning: false
  message: false
---

# Introduction

You have been assigned a sample of patients who have autoimmune diseases, which are enrolled to a study on respiratory infections.

You will need to use this file during the four practical sessions for Statistics: this will be your statistics practical deliverable.

**DO NOT modify variable names: they are used to grade your work.**

Complete the exercises using R. Do not delete or rename the data frames that are placed to collect your exercise answers. Replace their `NA` values with your calculated results.

Only use the R functions listed for each exercise, together with basic arithmetic, comparisons, indexing, assignment, and previously introduced functions. You may use `c()`, `data.frame()`, and `as.numeric()` to organize results.

Do not use additional packages or functions.

## Group parameters

Replace the five placeholders using your group's assignment table.

```{r}
group_id <- "[12SP-O]" 

distribution_variable <- "elevated_cortisol_measurements" 
alternative_distribution <- "Poisson"

correlation_variable_x <- "bmi_kg_m2"
correlation_variable_y <- "crp_mg_l"
```

## Dataset

```{r}
# PROVIDED CODE — DO NOT MODIFY

sample_data <- read.csv("sample.csv")

stopifnot(
  distribution_variable %in% names(sample_data),
  correlation_variable_x %in% names(sample_data),
  correlation_variable_y %in% names(sample_data)
)
```

| Column | Meaning and coding |
|------------------------------------|------------------------------------|
| `patient_id` | Unique patient identifier; never use as a measurement |
| `recruitment_year` | Recruitment year, 2018–2023 |
| `age_years` | Age in years |
| `bmi_kg_m2` | Body mass index in kg/m² |
| `autoimmune_disease_type` | articular, systemic or intestinal |
| `immunomodulatory_treatment` | Treatment indicator, 1 yes / 0 no |
| `pcr_test_result` | PCR reference result, 1 positive / 0 negative |
| `antigen_test_result` | Antigen test result, 1 positive / 0 negative |
| `cough`, `fever`, `diarrhea`, `vomiting` | Symptom indicators, 1 present / 0 absent |
| `flare_episodes_12m` | Number of flare episodes in a 12-month window |
| `crp_mg_l` | CRP concentration in mg/L |
| `elevated_cortisol_measurements` | Number elevated among six standardized measurements |
| `days_since_last_flare` | Days since last documented flare, 1–730 |

# Practical 1 — Probability and Diagnostic Testing

## Exercise 1 — Frequencies

Calculate the **absolute and relative frequencies** of `pcr_test_result` in your sample.

**Allowed functions:** `table()`, `prop.table()`, `data.frame()`, `as.numeric()`

```{r}
# STUDENT CODE
# Write your calculations here.
abs_freq <- table(sample_data$pcr_test_result)
abs_freq
rel_freq <- prop.table(abs_freq)
rel_freq
absolute <- as.numeric(abs_freq)
relative <- as.numeric(rel_freq)

```

**Enter your answer variables into the structured data frame below**

```{r}
ex1_frequencies <- data.frame(
  pcr_result = c(0, 1),
  absolute = c(256, 44),
  relative = c(0.8533333, 0.1466667)
)
```

## Exercise 2 — Conditional Probability

Calculate the probability that a patient has fever, given that their PCR result is positive:

$P(\text{fever}=1\mid\text{PCR}=1)$

**Allowed functions:** `sum()`, `mean()`, `nrow()`, `subset()`

```{r}
# STUDENT CODE
# Write your calculations here.

pcr_positive <- subset(sample_data, pcr_test_result == 1)
n_positive <- nrow(pcr_positive)
n_fever_and_positive <- sum(pcr_positive$fever == 1)
prob <- n_fever_and_positive / n_positive
prob
```

**Enter your answer variables into the structured data frame below**

```{r}
ex2_probability <- prob
```

## Exercise 3 — Diagnostic Performance

Using `pcr_test_result` as the reference and `antigen_test_result` as the diagnostic test:

1.  Construct a confusion matrix.
2.  Calculate the **positive predictive value (PPV)** of the antigen test.

Use PCR status for the rows and antigen test status for the columns. In both cases, use the order **0, 1**.

**Allowed functions:** `table()`, `sum()`, `mean()`

```{r}
# STUDENT CODE
# Write your calculations here.
cm <- table(sample_data$pcr_test_result, sample_data$antigen_test_result)
cm

TN <- cm[1, 1]   #PCR 0 antigen 0
FP <- cm[1, 2]   #PCR 0 antigen 1
FN <- cm[2, 1]   #PCR 1 antigen 0
TP <- cm[2, 2]   #PCR 1 antigen 1

ppv <- TP / (TP + FP)
ppv
```

**Enter your answer variables into the structured data frame below**

```{r}
ex3_confusion_matrix <- matrix(
  c(TN, FN, FP, TP),
  nrow = 2,
  ncol = 2,
  dimnames = list(
    PCR = c("0", "1"),
    Antigen = c("0", "1")
  )
)

ex3_ppv <- ppv
```

## R1 — Diagnostic Interpretation

Interpret the PPV obtained. How useful is the antigen test for identifying patients with a respiratory infection? Discuss its strengths and limitations using the confusion matrix, and explain how disease prevalence affects the interpretation of a positive result.

**Maximum: 150 words.**

The PPV is 0.386, which means that only about 39% of patients who test positive on the antigen test actually have a positive PCR. The other 43 are false alarms, so a positive antigen result isn't reliable for confirming an infection.

The test does have some strengths. Its specificity is fairly good (213/256, about 83%), so most people without the infection test negative. But it misses a lot of real cases: its sensitivity is only 61% (27/44), and 17 infected patients got a negative result.

# Practical 2 — Descriptive Statistics and Distributions

## Exercise 4 — Descriptive Statistics

For your assigned variable, `distribution_variable`, calculate:

- Mean and median.
- Variance and standard deviation.
- Standard error of the mean.
- 25th, 50th and 75th percentiles.

**Allowed functions:** `mean()`, `median()`, `var()`, `sd()`, `sqrt()`, `length()`, `quantile()`

```{r}
# STUDENT CODE
# Write your calculations here.

```

**Enter your answer variables into the structured data frame below**

```{r}
ex4_statistics <- c(
  mean = NA_real_,
  median = NA_real_,
  variance = NA_real_,
  sd = NA_real_,
  se = NA_real_,
  q25 = NA_real_,
  q50 = NA_real_,
  q75 = NA_real_
)
```

## Exercise 5 — Distribution

Plot a histogram of your assigned variable.

Based on the histogram and the properties of the variable, identify the most plausible distribution among:

**Binomial, Poisson, Negative Binomial, Normal, Gamma.**

**Allowed functions:** `hist()`, `barplot()`, `table()`, `mean()`, `var()`, `sd()`

```{r}
# STUDENT CODE
# Create your plot here.

```

**Enter your answer variables into the structured data frame below**

Use exactly one of these names: `"Binomial"`, `"Poisson"`, `"Negative Binomial"`, `"Normal"`, `"Gamma"`.

```{r}
ex5_distribution <- NA_character_
```

## R2 — Distribution Interpretation

Explain why you selected your distribution. Would the alternative distribution specified by `alternative_distribution` also be appropriate?

Justify your answer using the variable's characteristics, histogram and descriptive statistics.

**Maximum: 120 words.**

\[Write your answer here.\]

# Practical 3 — Sampling and the Central Limit Theorem

## Population Data

The full population dataset is now available. Use `population.csv` for the sampling simulations.

```{r}
# PROVIDED CODE — DO NOT MODIFY

population_data <- read.csv("population.csv")
```

## Provided Sampling Function

The following function draws repeated random samples **with replacement** and calculates their means.

Do not modify this function.

```{r}
# PROVIDED CODE — DO NOT MODIFY

sample_means <- function(x, n, repetitions = 1000) {
  replicate(
    repetitions,
    mean(sample(x, size = n, replace = TRUE))
  )
}
```

## Exercise 6 — Sampling Distribution of the Mean

Using `crp_mg_l` from the full population:

1.  Generate **1,000 sample means**, each obtained from a random sample of **30 patients**, using the provided function.
2.  Plot a histogram of the sample means.
3.  Calculate their mean and standard deviation.
4.  Calculate the population mean and theoretical standard error for a sample size of 30.

For the theoretical standard error, use the population standard deviation calculated with divisor (N), rather than (N-1).

**Allowed functions:** `sample_means()`, `set.seed()`, `mean()`, `sd()`, `sqrt()`, `length()`, `sum()`, `hist()`

```{r}
# PROVIDED CODE — DO NOT MODIFY
# Use this seed immediately before your simulation.

set.seed(20261115)
```

```{r}
# STUDENT CODE
# Write your calculations and create your plot here.

```

**Enter your answer variables into the structured data frame belows**

```{r}
ex6_sample_means <- rep(NA_real_, 1000)

ex6_summary <- c(
  empirical_mean = NA_real_,
  empirical_sd = NA_real_,
  population_mean = NA_real_,
  theoretical_se = NA_real_
)
```

## Exercise 7 — Effect of Sample Size

Repeat the sampling simulation using sample sizes of **5 and 100 patients**, with 1,000 repetitions each.

Compare the results for sample sizes **5, 30 and 100**.

For each sample size, report:

- Mean of the simulated sample means.
- Standard deviation of the simulated sample means.
- Theoretical standard error.

**Allowed functions:** `sample_means()`, `set.seed()`, `mean()`, `sd()`, `sqrt()`, `length()`, `sum()`, `data.frame()`

```{r}
# PROVIDED CODE — DO NOT MODIFY
# Use this seed immediately before the simulations.

set.seed(20261116)
```

```{r}
# STUDENT CODE
# Write your calculations here.

```

**Enter your answer variables into the structured data frame below**

```{r}
ex7_summary <- data.frame(
  n = c(5, 30, 100),
  empirical_mean = c(NA_real_, NA_real_, NA_real_),
  empirical_sd = c(NA_real_, NA_real_, NA_real_),
  theoretical_se = c(NA_real_, NA_real_, NA_real_)
)
```

## R3 — Sampling Interpretation

Explain how sample size affects the sampling distribution of the mean.

Compare your assigned sample (`sample.csv`) with the full population (`population.csv`) using:

- Mean `age_years`.
- Proportion of PCR-positive patients.
- Distribution of `autoimmune_disease_type`.

Is your sample representative of the population? Could the differences be explained by random sampling variability, or do they suggest a biased sampling process?

**Maximum: 180 words.**

**Allowed functions for your comparison:** `mean()`, `table()`, `prop.table()`, `sum()`, `nrow()`

```{r}
# STUDENT CODE
# Calculate the sample and population comparisons here.

```

\[Write your answer here.\]

# Practical 4 — Confidence Intervals and Associations

Use your assigned sample (`sample_data`) for all exercises in this practical.

## Exercise 8 — Confidence Intervals

Calculate separate **95% confidence intervals for the mean CRP concentration** (`crp_mg_l`) for:

- PCR-negative patients (`pcr_test_result == 0`).
- PCR-positive patients (`pcr_test_result == 1`).

For each subgroup, calculate the sample size, mean, standard error, critical t-value and lower and upper confidence limits.

Use a two-sided Student's t confidence interval with (n-1) degrees of freedom.

**Allowed functions:** `subset()`, `mean()`, `sd()`, `sqrt()`, `length()`, `nrow()`, `qt()`

```{r}
# STUDENT CODE
# Write your calculations here.

```

**Enter your answer variables into the structured data frame below**

```{r}
ex8_confidence_intervals <- data.frame(
  pcr_result = c(0, 1),
  n = c(NA_real_, NA_real_),
  mean = c(NA_real_, NA_real_),
  se = c(NA_real_, NA_real_),
  t_critical = c(NA_real_, NA_real_),
  ci_lower = c(NA_real_, NA_real_),
  ci_upper = c(NA_real_, NA_real_)
)
```

## Exercise 9 — Correlation

Using your assigned variables `correlation_variable_x` and `correlation_variable_y`:

1.  Create a scatterplot.
2.  Calculate Pearson's correlation coefficient.
3.  Calculate Spearman's correlation coefficient.

**Allowed functions:** `plot()`, `cor()`

```{r}
# STUDENT CODE
# Create the scatterplot and calculate the correlations here.

```

**Enter your answer variables into the structured data frame below**

```{r}
ex9_correlations <- c(
  pearson = NA_real_,
  spearman = NA_real_
)
```

## Exercise 10 — Jaccard Similarity

Calculate the **Jaccard similarity** between `fever` and `diarrhea` separately for:

- PCR-negative patients.
- PCR-positive patients.

Treat a value of 1 as symptom presence and a value of 0 as absence.

**Allowed functions:** `subset()`, `sum()`, `nrow()`

```{r}
# STUDENT CODE
# Write your calculations here.

```

**Enter your answer variables into the structured data frame below**

```{r}
ex10_jaccard <- c(
  pcr_negative = NA_real_,
  pcr_positive = NA_real_
)
```

## R4 — Integrated Interpretation

Based on your results:

1.  What differences do you observe between PCR-positive and PCR-negative patients?
2.  Which correlation coefficient better describes the relationship between your assigned variables, and why?
3.  What does Jaccard similarity tell you about the co-occurrence of fever and diarrhea in each group?
4.  What conclusions can you draw, and what can you **not** conclude from these analyses?

**Maximum: 200 words.**

\[Write your answer here.\]
