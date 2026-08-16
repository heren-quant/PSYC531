# Lesson 1 — R basics

[← Install](00-install.md) | [Home](README.md) | [Next: Working with data →](02-data.md)

In this lesson: run code, do arithmetic, save results under a name, and add notes to yourself.

Open RStudio, click **File → New File → R Script**, and save it in your `PSYC531` folder as
`lesson01.R`. Type along as you read.

---

## 1. Running code

Code you type in the script panel does nothing until you **run** it. Two ways:

- Highlight the code and click the **Run** button at the top of the panel, or
- Highlight it and press **Ctrl + Enter** (Windows) / **Cmd + Return** (Mac)

The code and its answer appear in the console below.

> [!TIP]
> If the console shows a `+` instead of a `>`, R thinks you left a sentence unfinished — usually a
> missing `)` or `"`. Click in the console and press **Esc**, then fix the line.

---

## 2. R as a calculator

```r
3 + 2        # add
3 - 2        # subtract
3 * 2        # multiply
3 / 2        # divide
3^2          # 3 squared
sqrt(9)      # square root
```

<details>
<summary><b>▸ Show what you should see</b></summary>

<br>

```
[1] 5
[1] 1
[1] 6
[1] 1.5
[1] 9
[1] 3
```

Ignore the `[1]` — it is R numbering the answers, not part of the result.

</details>

Words like `sqrt` are **functions**. A function is an instruction with its ingredients in brackets:
`sqrt(9)` means "take the square root of 9". You will use dozens of them, and they all follow that shape.

---

## 3. Saving a result: the `<-` arrow

If you just type `3 + 2`, R answers and forgets. To keep something, give it a name using the arrow `<-`:

```r
mynum <- 2931    # store 2931 under the name mynum
mynum            # show me what mynum is
mynum + 1        # use it in a calculation
```

Look at the **Environment** panel (top right) — `mynum` appeared there. That panel is a list of
everything R currently knows about.

**Shortcut for the arrow:** **Alt + −** (Windows) or **Option + −** (Mac).

Try it with your own name:

```r
allen <- 7
allen
```

### Naming rules

| Rule | Good | Bad |
|---|---|---|
| Start with a letter | `score1` | `1score` |
| No spaces — use `_` | `reaction_time` | `reaction time` |
| Capitals matter | `myData` is not `mydata` | |

> [!NOTE]
> You will also see `=` used instead of `<-`. Both work. This course uses `<-` because it makes it
> obvious you are storing something.

---

## 4. Notes to yourself: the `#` symbol

Anything after a `#` is ignored by R. Use it constantly to explain what your code does — for your
instructor, your group, and yourself next month.

```r
# This whole line is a note
3 + 2   # this part is a note too
```

**Shortcut:** highlight some lines and press **Ctrl + Shift + C** (Windows) / **Cmd + Shift + C** (Mac)
to turn them into notes, or turn them back.

**Handy trick:** end a note with four dashes and RStudio turns it into a heading you can collapse and
jump to:

```r
# 1. Load the data ----
# 2. Descriptive statistics ----
# 3. Analyses ----
```

Once your homework script is 100 lines long, you will be glad you did this.

---

## 5. Add-on packages

Base R covers the basics. Packages add more. Install once, load every session.

```r
# Run this ONCE (needs internet):
install.packages(c("car", "ggplot2", "psych"))

# Run this EVERY TIME you open R:
library(car)
library(ggplot2)
library(psych)
```

Notice the quotes: **in quotes to install, no quotes to load.**

After the first time, put a `#` in front of the `install.packages` line so you do not download them again.

<details>
<summary><b>▸ What are these three packages for?</b></summary>

<br>

| Package | What it gives you |
|---|---|
| `car` | Tools for checking regression assumptions |
| `ggplot2` | Better-looking graphs |
| `psych` | Quick descriptive statistics tables, built for psychologists |

Your instructor may add others as the term goes on.

</details>

---

## 6. Two housekeeping habits

**Start clean.** Leftovers from yesterday cause confusing results:

```r
rm(list = ls())   # clears everything out of the Environment panel
```

This only clears R's memory. Your files and your script are untouched. (The little broom icon in the
Environment panel does the same thing.)

**Show full numbers.** By default R prints tiny numbers as `1.98e-08`, which is hard to read in a
results section:

```r
options(scipen = 999)   # show full numbers instead
```

Put both lines at the top of every script you write.

---

## Check yourself

<details>
<summary><b>▸ Q1. What is the difference between <code>=</code> and <code>==</code>?</b></summary>

<br>

One equals sign stores a value. Two equals signs ask a question and answer TRUE or FALSE:

```r
x <- 5
x == 5   # TRUE  — "is x equal to 5?"
x == 6   # FALSE
```

Mixing these up is one of the most common beginner mistakes.

</details>

<details>
<summary><b>▸ Q2. You run <code>library(psych)</code> and see "there is no package called 'psych'". Why?</b></summary>

<br>

You tried to open an app you never downloaded. Run `install.packages("psych")` first (with quotes),
then `library(psych)` (without quotes).

</details>

<details>
<summary><b>▸ Q3. Predict the answer, then test it.</b></summary>

<br>

```r
a <- 10
b <- a
a <- 20
b
```

`b` is still **10**. Storing copies the value at that moment; `b` does not follow `a` around afterwards.

</details>

---

[← Install](00-install.md) | [Home](README.md) | [Next: Lesson 2 — Working with data →](02-data.md)
