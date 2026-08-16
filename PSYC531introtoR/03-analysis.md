# Lesson 3 — Statistics in R

[← Working with data](02-data.md) | [Home](README.md) | [Next: Practice →](04-exercises.md)

In this lesson: describe a variable, draw a graph, and run a regression — and, more importantly, say in
plain English what the output means.

Start a new script called `lesson03.R` and begin with:

```r
rm(list = ls())         # clean start
options(scipen = 999)   # full numbers, not 1.98e-08
library(psych)          # for quick descriptive tables
```

We are still using `attitude` (30 departments, workplace survey scores) from
[Lesson 2](02-data.md).

---

## 1. Describing one variable

```r
mean(attitude$rating)      # average
median(attitude$rating)    # middle value
sd(attitude$rating)        # standard deviation
var(attitude$rating)       # variance
min(attitude$rating)       # lowest
max(attitude$rating)       # highest
```

<details>
<summary><b>▸ Show what you should see</b></summary>

<br>

```
[1] 64.63333
[1] 65.5
[1] 12.17256
[1] 148.1713
[1] 40
[1] 85
```

Written up in APA style, that first pair is: *M* = 64.63, *SD* = 12.17.

</details>

### All variables at once

```r
psych::describe(attitude)
```

This gives you a table with the number of cases, mean, SD, median, minimum, maximum, skew, and kurtosis
for every variable — usually everything you need for a "descriptive statistics" table in a report.

Writing `psych::describe()` rather than `describe()` just tells R which package the command came from.
It is a good habit and prevents a common source of confusing errors.

### Counting categories

For a grouping variable, counts are what you want, not a mean:

```r
table(ExerciseData$Group)               # how many in each group
prop.table(table(ExerciseData$Group))   # as proportions
```

---

## 2. Missing values

If any value is missing, R refuses to guess:

```r
mean(attitude$rating)                  # a number
mean(c(4, 8, NA))                      # NA — R will not just skip it
mean(c(4, 8, NA), na.rm = TRUE)        # 6 — now you have told it to skip
sum(is.na(attitude$rating))            # how many are missing? (0 here)
```

`na.rm = TRUE` means "remove the missing values first". Add it when you need it, and always report how
many cases you dropped.

---

## 3. Graphs

```r
hist(attitude$rating)      # histogram — the shape of one variable
boxplot(attitude$rating)   # boxplot — median, spread, outliers
```

Add labels so the graph makes sense to someone else:

```r
hist(attitude$rating,
     main = "Overall department rating",   # title
     xlab = "Rating (0-100)",              # x-axis label
     col = "lightblue")
```

Graphs appear in the bottom-right panel. Use **Export → Save as Image** to get a file you can paste into
a paper.

<details>
<summary><b>▸ Nicer graphs with ggplot2 (optional)</b></summary>

<br>

`ggplot2` makes publication-quality figures. It builds a graph in layers joined by `+`:

```r
library(ggplot2)

ggplot(attitude, aes(x = complaints, y = rating)) +
  geom_point() +                              # add the dots
  geom_smooth(method = "lm") +                # add a straight line of best fit
  labs(x = "Complaint handling", y = "Overall rating") +
  theme_minimal()
```

You do not need this for the course, but it is worth knowing it exists.

</details>

---

## 4. Correlation

Does handling complaints well go with a higher overall rating?

```r
cor(attitude$rating, attitude$complaints)        # just the correlation
cor.test(attitude$rating, attitude$complaints)   # with a significance test
```

<details>
<summary><b>▸ Show what you should see, and how to report it</b></summary>

<br>

```
	Pearson's product-moment correlation

t = 7.737, df = 28, p-value = 0.00000001988
95 percent confidence interval:
 0.6620128 0.9139139
sample estimates:
      cor
0.8254176
```

The three numbers you need are the correlation (0.83), the degrees of freedom (28), and the p-value.

**Write-up:** Departments that handled complaints better were rated more highly overall,
*r* = .83, 95% CI [.66, .91], *p* < .001.

</details>

To see all the correlations at once:

```r
round(cor(attitude), 2)   # correlation matrix, rounded to 2 decimals
```

---

## 5. Scatterplots

A correlation should always be looked at, not just calculated:

```r
plot(attitude$complaints, attitude$rating,     # x first, then y
     xlab = "Complaint handling",
     ylab = "Overall rating",
     pch = 19)                                  # solid dots

abline(lm(rating ~ complaints, data = attitude), col = "red")   # add the line of best fit
```

---

## 6. Linear regression

**Question:** does complaint handling predict a department's overall rating?

```r
lm_model <- lm(rating ~ complaints, data = attitude)
summary(lm_model)
```

The `~` means "predicted by". Read `rating ~ complaints` as **"rating predicted by complaints"** — the
outcome always goes on the left.

<details>
<summary><b>▸ Show what you should see</b></summary>

<br>

```
Coefficients:
            Estimate Std. Error t value     Pr(>|t|)
(Intercept) 14.37632    6.61999   2.172       0.0385 *
complaints   0.75461    0.09753   7.737 0.0000000199 ***
---
Multiple R-squared:  0.6813,	Adjusted R-squared:  0.6699
F-statistic: 59.86 on 1 and 28 DF,  p-value: 0.00000001988
```

</details>

### Reading the output

| What you see | Here | What it means |
|---|---|---|
| `(Intercept)` estimate | 14.38 | The predicted rating for a department scoring 0 on complaint handling. Not realistic, but the maths needs a starting point |
| `complaints` estimate | 0.75 | **The important one.** For every 1 point higher on complaint handling, the overall rating goes up 0.75 points |
| `Pr(>|t|)` | < .001 | The p-value for that slope |
| `Multiple R-squared` | 0.68 | Complaint handling explains about 68% of the differences in overall rating |
| `*`, `**`, `***` | | Shorthand for p < .05, .01, .001. Report the actual number, not the stars |

**Write-up:** Complaint handling significantly predicted overall department rating,
*b* = 0.75, *SE* = 0.10, *t*(28) = 7.74, *p* < .001, *R*² = .68.

### Adding a second predictor

Just use a `+`:

```r
lm_model2 <- lm(rating ~ complaints + learning, data = attitude)
summary(lm_model2)
```

Each slope now means "the effect of this variable, holding the other one constant".

---

## 7. Logistic regression

Use this when your outcome has only **two** possible answers — yes/no, passed/failed, group A/group B —
instead of being a score.

We switch to another built-in dataset, `infert`: 248 people, where `case` is 1 for the group with
infertility and 0 for the comparison group, and `spontaneous` counts earlier spontaneous events.

```r
infert$case <- as.factor(infert$case)   # tell R this is a category, not a number

logistic_model <- glm(case ~ spontaneous,
                      data = infert,
                      family = binomial)   # "binomial" is what makes it logistic
summary(logistic_model)
```

<details>
<summary><b>▸ Show what you should see</b></summary>

<br>

```
Coefficients:
            Estimate Std. Error z value         Pr(>|z|)
(Intercept)  -1.3739     0.1980  -6.940 0.00000000000391 ***
spontaneous   1.0639     0.1964   5.417 0.00000006053616 ***
```

</details>

### Making the numbers readable

Logistic regression reports its results in "log-odds", which nobody thinks in. One extra line converts
them into **odds ratios**, which you can actually explain:

```r
exp(coef(logistic_model))
```

```
(Intercept) spontaneous
     0.2531      2.8975
```

**What 2.90 means:** each additional prior spontaneous event makes a person about **2.9 times more
likely** (in odds) to be in the case group, *p* < .001.

Rule of thumb: an odds ratio above 1 means the outcome becomes more likely, below 1 means less likely,
and exactly 1 means no relationship.

| | Linear regression | Logistic regression |
|---|---|---|
| Use when the outcome is | a score | one of two categories |
| Command | `lm()` | `glm(..., family = binomial)` |
| Extra step | none | `exp()` to get odds ratios |

---

## 8. Three other tests you will need

```r
# t-test: compare two groups
t.test(len ~ supp, data = ToothGrowth)

# ANOVA: compare three or more groups
aov_model <- aov(weight ~ group, data = PlantGrowth)
summary(aov_model)

# chi-square: two categorical variables
chisq.test(table(infert$case, infert$education))
```

The pattern is always the same: **outcome `~` predictor**, then `data =` your dataset.

---

## Check yourself

<details>
<summary><b>▸ Q1. In <code>lm(rating ~ learning, data = attitude)</code>, which variable is being predicted?</b></summary>

<br>

`rating`. Whatever sits on the **left** of the `~` is the outcome. `learning` is the predictor.

</details>

<details>
<summary><b>▸ Q2. A slope of 0.75, p < .001. Explain it to someone who has never taken statistics.</b></summary>

<br>

"Departments that handled complaints one point better tended to be rated about three-quarters of a point
higher overall, and a pattern this strong is very unlikely to have happened by chance."

</details>

<details>
<summary><b>▸ Q3. <code>mean(mydata$score)</code> gives you <code>NA</code>. Why, and what do you do?</b></summary>

<br>

At least one score is missing. Check how many with `sum(is.na(mydata$score))`, then use
`mean(mydata$score, na.rm = TRUE)` — and say in your write-up how many cases you left out.

</details>

<details>
<summary><b>▸ Q4. Your logistic regression gives a coefficient of 0.41. What is the odds ratio?</b></summary>

<br>

```r
exp(0.41)   # 1.51
```

About 1.5 — the outcome becomes roughly 50% more likely (in odds) for each 1-unit increase.

</details>

---

[← Working with data](02-data.md) | [Home](README.md) | [Next: Lesson 4 — Practice →](04-exercises.md)
