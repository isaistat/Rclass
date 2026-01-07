# **Introduction to R Programming**

R is a **free and open-source programming language** used for **data analysis, statistics, and data visualization**. You do **not** need any prior programming knowledge to start learning R. This book is written for **absolute beginners**.

R is widely used in many fields such as **public health, science, business, social sciences, and education**.

---

## Installing R and RStudio

To work with R comfortably, you need **two things**:

1. **R** – the programming language
2. **RStudio** – a user-friendly interface to work with R

### Step 1: Install R

Download R from the official website:
- https://cran.rstudio.com/

Choose your operating system (Windows / Mac / Linux) and follow the instructions.

### Step 2: Install RStudio

Download RStudio from:
- https://rstudio.com/products/rstudio/download/#download

> ⚠️ Always install **R first**, and then install **RStudio**.

---

## Understanding the RStudio Screen

When you open RStudio, you will see **four main sections (panes)**. Do not worry if this looks confusing at first — you will get used to it quickly.

### 1. Source Pane

- This is where you **write your R code**
- Files here are called **scripts**
- You can save your code and run it again later

### 2. Console Pane

- This is where R **runs your commands**
- You can type simple commands and press **Enter** to see results
- Errors and messages are also shown here

### 3. Environment Pane

- Shows the **data and variables** you have created
- Displays how many rows and columns your data has

### 4. Files, Plots, Packages, Help, Viewer Pane

- **Files:** Shows files on your computer
- **Plots:** Displays graphs you create
- **Packages:** Shows installed R packages
- **Help:** Explains how R functions work
- **Viewer:** Shows web pages or reports created in R

---

## Getting Help in R

R has built-in help for every function.

```r
??mean
help(mean)
```

These commands explain what the `mean()` function does and how to use it.

---

## Making RStudio Comfortable to Use

You can change how RStudio looks to make it easier on your eyes.

### Change Pane Layout

- Click **Tools → Global Options → Pane Layout**
- Rearrange panes if needed

### Change Theme and Font

- Click **Tools → Global Options → Appearance**
- Choose a theme and font size you like

---

## Basic Data Structures in R

R stores information in different ways. Below are the most important ones for beginners.

### 1. Vector

- A vector stores **one type of data only**
- Think of it as a **single column** of numbers or text

Example:
```r
ages <- c(20, 25, 30)
```

---

### 2. Data Frame

- A data frame looks like an **Excel sheet**
- It has **rows and columns**
- Each column can store different types of data

This is the **most commonly used** data structure in R.


---

### 3. List

- A list can store **many different objects together**
- For beginners, lists can be confusing and are used less often

---

## Important Terms You Should Know

| Term | Simple Meaning |
|-----|---------------|
| function | A command that does action (always has `()`) |
| argument | The options given in the function |
| base R | The default package in R installation |
| class | The type of data (number, text, logical, date, etc) |
| data frame | A table with rows and columns |
| environment | Where R stores your data |
| factor | A categorical variable (e.g. Male/Female) |
| library | A function used to load packages |
| package | A collection of helpful functions |
| script | A file containing R code |
| vector | A single column of same-type values |

---

## Common Symbols in R

### Assignment and Access

| Symbol | Meaning |
|------|---------|
| `<-` | Store a value in a variable |
| `=` | Used inside functions |
| `%>%` | Pipe: pass result to next step |
| `::` | Use a function from a package |
| `[]` | Select rows or columns |
| `[[]]` | Select items from a list |

---

### Logical Values and Notes

| Symbol | Meaning |
|------|---------|
| TRUE / T | Yes / correct |
| FALSE / F | No / incorrect |
| NA | Missing value |
| NaN | Invalid number |
| NULL | Nothing |
| `#` | Comment (not run by R) |
| `c()` | Combine values |

---

## Logical Operators (Conditions)

| Symbol | Meaning |
|------|---------|
| `|` | OR |
| `&` | AND |
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |

---

## Installing and Loading Packages

A package is a collection of ready-made functions that help R perform specific tasks easily.
R comes with some basic packages already installed, but many advanced features require additional packages.


Installing a package downloads it from the internet and stores it on your computer.
```r
install.packages("package_name")
```
Loading a package makes its functions available for use in the current R session.

```r
library(package_name)
```
Install the package used for data manipulation `tidyverse`
```r
install.packages("tidyverse")
library(tidyverse)
```

You install a package **once**, but load it **every time** you start R.

---

## Working Directory

The working directory is the folder where R reads and saves files.

```r
getwd()      # Show current folder
setwd("D:/New folder")  # Change folder
```

---

## Useful RStudio Shortcuts

### Common Shortcuts

| Action | Windows/Linux | Mac |
|------|---------------|-----|
| Run code | Ctrl + Enter | Cmd + Return |
| Save file | Ctrl + S | Cmd + S |
| Clear console | Ctrl + L | Ctrl + L |
| Insert `<-` | Alt + - | Option + - |
| Insert pipe `%>%` | Ctrl + Shift + M | Cmd + Shift + M |

Full shortcut list:
https://support.posit.co/hc/en-us/articles/200711853-Keyboard-Shortcuts-in-the-RStudio-IDE

---

**End of Chapter 1**  
In the next chapter, we will start writing our **first R commands** step by step.

