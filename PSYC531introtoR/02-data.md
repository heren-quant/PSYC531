# Lesson 2 — Working with data

[← R basics](01-r-basics.md) | [Home](README.md) | [Next: Statistics in R →](03-analysis.md)

In this lesson: open a dataset, look at it properly, build a small one yourself, and pull out the rows
and columns you need.

Start a new script called `lesson02.R`.

---

## 1. A dataset to practise on

R comes with practice datasets already built in, so nothing to download. We will use one called
**`attitude`**: a survey of employees in 30 departments of a large company. Each department has a score
from 0 to 100 on seven questions.

| Variable | What it measures |
|---|---|
| `rating` | Overall rating of the department |
| `complaints` | How well complaints are handled |
| `privileges` | Does not allow special privileges |
| `learning` | Opportunity to learn |
| `raises` | Raises are based on performance |
| `critical` | Too critical |
| `advance` | Chances for advancement |

Type `?attitude` any time to read more about it.

---

## 2. Looking at your data

Never analyse a dataset you have not looked at. These five commands are all you need:

```r
View(attitude)      # opens it like a spreadsheet (capital V!)
str(attitude)       # structure: how many cases, what type each variable is
head(attitude)      # the first 6 rows
dim(attitude)       # how many rows and columns
summary(attitude)   # lowest, highest, and average of every variable
```

<details>
<summary><b>▸ Show what <code>str(attitude)</code> gives you</b></summary>

<br>

```
'data.frame':	30 obs. of  7 variables:
 $ rating    : num  43 63 71 61 81 43 58 71 72 67 ...
 $ complaints: num  51 64 70 63 78 55 67 75 82 61 ...
 $ privileges: num  30 51 68 45 56 49 42 50 72 45 ...
 $ learning  : num  39 54 69 47 66 44 56 55 67 47 ...
 $ raises    : num  61 63 76 54 71 54 66 70 71 62 ...
 $ critical  : num  92 73 86 84 83 49 68 66 83 80 ...
 $ advance   : num  45 47 48 35 47 34 35 41 31 41 ...
```

Read it as: 30 cases, 7 variables, and every variable is a number (`num`).

**`str()` is the most useful command in R.** Run it on every dataset before you do anything else. It tells
you whether R understood your variables as numbers (`num`), words (`chr`), or categories (`Factor`) —
and if R got that wrong, nothing else will work.

</details>

---

## 3. Getting one variable: the `$` sign

A dataset is a table. To grab a single column, put a `$` between the dataset and the variable name:

```r
attitude$rating          # the whole rating column
mean(attitude$rating)    # the average rating
```

Type `attitude$` and pause — RStudio pops up a list of the variable names. Press **Tab** to fill one in.
Use this every time; it saves you from typos.

---

## 4. Opening your own data file

When you have your own data, save it from Excel as a **.csv** file, put it in your `PSYC531` folder,
then tell R where to look:

```r
# 1. tell R which folder to work in (yours will look different)
setwd("C:/Users/yourname/Desktop/PSYC531")     # Windows
setwd("/Users/yourname/Desktop/PSYC531")       # Mac

# 2. check it worked
getwd()
list.files()      # shows the files R can see — is your file listed?

# 3. open the file
mydata <- read.csv("myfile.csv", header = TRUE)

# 4. always look at what arrived
str(mydata)
head(mydata)
```

Two rules that catch everyone:

- Use **forward slashes** `/` in the path, even on Windows.
- Give the path to the **folder**, not to the file.

<details>
<summary><b>▸ How to find your folder path</b></summary>

<br>

**Easiest way:** in RStudio's **Files** panel (bottom right), click through to your folder, then choose
**More → Copy Folder Path to Clipboard**. Paste it between the quote marks.

**Windows:** open File Explorer, click the address bar, copy the path, then change every `\` to `/`.

**Mac:** in Finder, right-click the folder, hold down **Option**, and choose *Copy … as Pathname*.

</details>

<details>
<summary><b>▸ Common problems opening a file</b></summary>

<br>

| Message | What it means | Fix |
|---|---|---|
| `cannot open file … No such file or directory` | R is looking in the wrong folder, or the name is misspelled | Run `getwd()` and `list.files()` |
| Columns are named `X1`, `X2`… | Your first row was data, not variable names | Add a header row in Excel and re-save |
| A number column shows as `chr` | Something non-numeric is in it (a note, "N/A", a stray comma) | Fix it in Excel and re-open |

</details>

**Saving data back out:**

```r
write.csv(mydata, "mydata_clean.csv", row.names = FALSE)
```

It appears in your folder. Keep `row.names = FALSE` or you get an extra junk column.

---

## 5. Typing in a small dataset yourself

For small studies you can type the data straight into R. The key tool is `c()`, which means "combine
these values into a list":

```r
ID <- c(1:10)                                # the numbers 1 through 10
ExerciseHours <- c(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
SBP <- c(150, 148, 145, 142, 140, 137, 135, 132, 130, 128)   # blood pressure

Name <- c(rep("Mike", 2), rep("Tina", 2), rep("Herbert", 2),
          rep("Yale", 2), rep("YOURNAME", 2))     # rep = repeat
```

Text goes in quotes; numbers do not. Now glue the columns into a dataset:

```r
ExerciseData <- data.frame(ID = ID,
                           Name = Name,
                           ExerciseHours = ExerciseHours,
                           SBP = SBP)
ExerciseData
```

<details>
<summary><b>▸ Show what you should see</b></summary>

<br>

```
   ID     Name ExerciseHours SBP
1   1     Mike             0 150
2   2     Mike             1 148
3   3     Tina             2 145
4   4     Tina             3 142
5   5  Herbert             4 140
6   6  Herbert             5 137
7   7     Yale             6 135
8   8     Yale             7 132
9   9 YOURNAME             8 130
10 10 YOURNAME             9 128
```

Every column must have the same number of values. If one has 9 and the rest have 10, R stops with
`arguments imply differing number of rows`.

</details>

---

## 6. Picking out rows and columns

Square brackets take two things separated by a comma: **`[rows, columns]`**. Rows first, columns second.

```r
ExerciseData[1, ]      # row 1, all columns
ExerciseData[, 2]      # all rows, column 2
ExerciseData[1, 2]     # row 1, column 2 — one cell

ExerciseData[, c(2, 4)]     # columns 2 and 4
ExerciseData[c(1, 5), ]     # rows 1 and 5

ExerciseData[, c("Name", "SBP")]              # columns by name — safer
ExerciseData[ExerciseData$ExerciseHours > 5, ]   # only rows meeting a condition
```

Save any of these under a new name if you want to keep it:

```r
short_version <- ExerciseData[, c("Name", "SBP")]
short_version
```

> [!TIP]
> Picking columns by **name** is safer than by number. If someone adds a column to the file, the numbers
> all shift and your code quietly analyses the wrong variable.

---

## 7. Adding a new variable

```r
# a calculated variable
ExerciseData$SBP_change <- ExerciseData$SBP - 150

# a grouping variable: "active" if 5+ hours, otherwise "less active"
ExerciseData$Group <- ifelse(ExerciseData$ExerciseHours >= 5, "active", "less active")

table(ExerciseData$Group)   # how many in each group
```

`ifelse()` reads as: *if this is true, use the first answer, otherwise use the second.*

---

## Check yourself

<details>
<summary><b>▸ Q1. Write the code to see the last 4 rows of <code>attitude</code>.</b></summary>

<br>

```r
tail(attitude, 4)
```

</details>

<details>
<summary><b>▸ Q2. What is the difference between <code>attitude[3, ]</code> and <code>attitude[, 3]</code>?</b></summary>

<br>

`attitude[3, ]` is the third **row** — one department, all seven variables.
`attitude[, 3]` is the third **column** — the `privileges` scores for all 30 departments.

The comma placement is the whole difference. Rows first, columns second.

</details>

<details>
<summary><b>▸ Q3. You open your file and <code>str()</code> shows <code>$ age : chr "23" "31" "twenty"</code>. What happened?</b></summary>

<br>

Someone typed a word into the age column, so R decided the whole column was text (`chr`). Nothing
numeric will work on it — `mean(mydata$age)` will fail — until you fix that value in the original file
and open it again.

This is exactly why you run `str()` first.

</details>

---

[← R basics](01-r-basics.md) | [Home](README.md) | [Next: Lesson 3 — Statistics in R →](03-analysis.md)
