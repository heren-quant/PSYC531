# Lesson 4 — Practice

[← Statistics in R](03-analysis.md) | [Home](README.md)

Twelve exercises. Open a new script called `lesson04.R` and write your answer before you open the
solution. Getting an error and fixing it is where the learning actually happens.

> [!TIP]
> Stuck for more than five minutes? Look at the [error table](#when-something-goes-wrong) at the bottom,
> type `?functionname` in R, or search the exact error message online. All three are normal.

---

## Part A — Warm-up

**A1.** Use R to work out 17 × 23, then the square root of 144.

<details>
<summary>▸ Show answer</summary>

<br>

```r
17 * 23      # 391
sqrt(144)    # 12
```

</details>

**A2.** Store your age under the name `my_age`, then create `age_in_months` from it — without typing your
age a second time.

<details>
<summary>▸ Show answer</summary>

<br>

```r
my_age <- 20
age_in_months <- my_age * 12
age_in_months
```

The point is that the second line is built from the first. If you retype the number, you now have two
places to update when it changes — which is how mistakes get into real analyses.

</details>

**A3.** Each of these lines has one mistake. Find and fix them.

```r
my num <- 5
Mean(c(2, 4, 6))
```

<details>
<summary>▸ Show answer</summary>

<br>

```r
my_num <- 5          # names cannot contain a space
mean(c(2, 4, 6))     # R is case-sensitive: mean, not Mean
```

</details>

---

## Part B — Making data

**B1.** Create a list called `scores` containing 12, 15, 9, 22, 18, 7, 14. Then find how many values it
has, its mean, and its standard deviation.

<details>
<summary>▸ Show answer</summary>

<br>

```r
scores <- c(12, 15, 9, 22, 18, 7, 14)
length(scores)   # 7
mean(scores)     # 13.85714
sd(scores)       # 5.145513
```

</details>

**B2.** In one line, make a list that says `"control"` five times and then `"treatment"` five times.

<details>
<summary>▸ Show answer</summary>

<br>

```r
condition <- c(rep("control", 5), rep("treatment", 5))
condition
```

</details>

**B3.** Build a dataset called `study1` with 10 participants: an `ID` numbered 1 to 10, the `condition`
list from B2, and a `score` for each person. Then check it with `str()`.

<details>
<summary>▸ Show answer</summary>

<br>

```r
study1 <- data.frame(ID = c(1:10),
                     condition = c(rep("control", 5), rep("treatment", 5)),
                     score = c(11, 14, 9, 12, 13, 18, 21, 17, 22, 19))
str(study1)
head(study1, 3)
```

All three lists must have exactly 10 values or R will refuse to build it.

</details>

**B4.** From `study1`, show only the treatment participants.

<details>
<summary>▸ Show answer</summary>

<br>

```r
study1[study1$condition == "treatment", ]
```

Note the **two** equals signs — you are asking a question, not storing a value.

</details>

---

## Part C — Describing a real dataset

Use the built-in `attitude` dataset.

**C1.** How many rows and columns does it have? What are the variables called?

<details>
<summary>▸ Show answer</summary>

<br>

```r
str(attitude)     # answers both questions at once
dim(attitude)     # 30 rows, 7 columns
names(attitude)
```

</details>

**C2.** Report the mean and standard deviation of `learning`.

<details>
<summary>▸ Show answer</summary>

<br>

```r
mean(attitude$learning)   # 56.36667
sd(attitude$learning)     # 11.73696
```

Written up: *M* = 56.37, *SD* = 11.74.

</details>

**C3.** Make a histogram of `learning` with a title and an x-axis label.

<details>
<summary>▸ Show answer</summary>

<br>

```r
hist(attitude$learning,
     main = "Opportunity to learn",
     xlab = "Score (0-100)",
     col = "lightblue")
```

</details>

**C4.** How many departments scored above 70 on `rating`?

<details>
<summary>▸ Show answer</summary>

<br>

```r
sum(attitude$rating > 70)   # 10
```

This works because R counts every TRUE as 1.

</details>

---

## Part D — Relationships

**D1.** Is `learning` related to `rating`? Run the correlation with its significance test, then write one
sentence reporting it.

<details>
<summary>▸ Show answer</summary>

<br>

```r
cor.test(attitude$learning, attitude$rating)
```

*r* = .62, *t*(28) = 4.22, *p* < .001.

**Write-up:** Departments offering more opportunity to learn were rated more highly overall,
*r* = .62, *p* < .001.

</details>

**D2.** Draw a scatterplot of `learning` (x) against `rating` (y), with the line of best fit on top.

<details>
<summary>▸ Show answer</summary>

<br>

```r
plot(attitude$learning, attitude$rating,
     xlab = "Opportunity to learn",
     ylab = "Overall rating",
     pch = 19)

abline(lm(rating ~ learning, data = attitude), col = "red")
```

</details>

**D3.** Run a regression predicting `rating` from `learning`. What does the slope mean in plain English?

<details>
<summary>▸ Show answer</summary>

<br>

```r
m1 <- lm(rating ~ learning, data = attitude)
summary(m1)
```

Slope = 0.65, *t*(28) = 4.22, *p* < .001, *R*² = .39.

**In plain English:** for every 1 point higher a department scored on "opportunity to learn", its overall
rating was about 0.65 points higher. Learning opportunity explains roughly 39% of the differences
between departments.

</details>

**D4.** Now add `complaints` as a second predictor. Did *R*² go up?

<details>
<summary>▸ Show answer</summary>

<br>

```r
m2 <- lm(rating ~ learning + complaints, data = attitude)
summary(m2)
```

*R*² rises from about .39 to about .71 — but notice that `learning` is now much weaker. Once you account
for how complaints are handled, learning opportunity adds relatively little. This is a good example of
why adding predictors changes what the earlier ones mean.

</details>

---

## A script template

Copy this into a file called `template.R` and start every assignment from it.

```r
# PSYC531 Assignment X ----
# Name:
# Date:

# 0. Setup ----
rm(list = ls())
options(scipen = 999)
library(psych)

# 1. Load the data ----
mydata <- read.csv("mydata.csv", header = TRUE)
str(mydata)

# 2. Descriptive statistics ----
psych::describe(mydata)

# 3. Graphs ----

# 4. Analyses ----

# 5. What I found ----
# (write your interpretation here as notes)
```

Every heading ends in four dashes, so you can collapse sections and jump between them.

---

## When something goes wrong

| Message | Usual cause | Fix |
|---|---|---|
| `could not find function "X"` | Typo, wrong capitals, or package not loaded | Check spelling; run `library(thepackage)` |
| `object 'X' not found` | You never created it, or spelled it differently | Check the Environment panel for the real name |
| `there is no package called 'X'` | Never installed | `install.packages("X")` with quotes |
| `cannot open file … No such file` | Wrong folder or filename | `getwd()` then `list.files()` |
| `unexpected symbol` or `unexpected ')'` | Missing comma, quote, or bracket | Check the line **above** the one R names |
| Console shows `+` and nothing happens | Unfinished command | Press **Esc**, then fix the missing bracket or quote |
| Results changed and you don't know why | Leftovers from an earlier run | **Session → Restart R**, then run your script from the top |

---

## Want more practice?

- [swirl](https://swirlstats.com/) — an interactive tutorial that runs inside R itself:
  `install.packages("swirl")`, then `library(swirl)` and `swirl()`
- [Posit cheat sheets](https://posit.co/resources/cheatsheets/) — print the base R one
- [UCLA statistics help](https://stats.oarc.ucla.edu/r/) — worked examples for psychology analyses

---

[← Statistics in R](03-analysis.md) | [Home](README.md)
