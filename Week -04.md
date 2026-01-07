# Exploratory Data Analysis (EDA) in R – Descriptives, Tables, and Plots
In this chapter, we perform **Exploratory Data Analysis (EDA)** to understand the data before any cleaning or modeling.

---

## Descriptive Statistics: Continuous Variables

### Quick Summary Check (Base R)

No additional packages required.

Use this for a **first look only**.It mixes statistics for different variable types.

⚠️ Do not report `summary()` directly in results sections.

```r
summary(data$age)
```

---

### Manual Descriptive Measures with `base`

These functions are always available and should be understood before using packages.

```r
mean(data$age, na.rm = TRUE)
median(data$age, na.rm = TRUE)
range(data$age, na.rm = TRUE)
IQR(data$age, na.rm = TRUE)
sd(data$age, na.rm = TRUE)

```
Descriptive measure with packages
```r
library(Desctools)
Desc(data$age)
```

```r
library(summarytools)
descr(data) #only for numeric variable
descr(data$bmi)
```

---

## **Descriptive Statistics: Categorical Variables**

Use frequency tables for **categorical variables only**.

### Frequency and Percentages with `base`

```r
tab_sex <- table(data$sex)
tab_sex
prop.table(tab_sex) * 100
```
Always careful Large number of categories reduces interpretability

### requency and Percentages with other packages

```r
library(janitor)

tabyl(data$sex)
```

when you want:- Counts and percentages together- Clean, readable output


---

### Using package `tableone` for Clinical Descriptives

This package is used for making baseline chracteristics table in manuscript in stuides like Clinical trials, Cohort descriptions and so on.


```r
vars <- c("age", "bmi", "sex") #list the variables u want to make table
CreateTableOne(vars = "age", data = data)

CreateTableOne(vars = names(data), data = data) #Table of All variables
```
If the column should be by treatment group enable the argument `strata`

```r

CreateTableOne(vars = names(data),
              strata="group",#the column split
              data = data) #Table of All variables

table1 <- CreateTableOne(vars = names(data),
              strata="group",#the column split
               addOverall = TRUE #Include overall column in table
              data = data) #Table of All variables
print(table1,showAllLevels=TRUE)
```
If the column should be by treatment group and also required the overall column enable the argument `addOverall`

```r

table1 <- CreateTableOne(vars = names(data),
              strata="group",#the column split
               addOverall = TRUE #Include overall column in table
               test=TRUE,
              data = data) #Table of All variables
print(table1,showAllLevels=TRUE)
```
If the table requires `p value` in the table,
```r

table1 <- CreateTableOne(vars = names(data),
              strata="group",#the column split
               addOverall = TRUE #Include overall column in table
               test=TRUE, #To include the p value
               nonnormal="bmi", #To mention if any variable are not normal
               digits=1,
              data = data) #Table of All variables
print(table1,showAllLevels=TRUE)

```
To export the table output
```r
tab_print<-print(table1,showAllLevels=TRUE,printToggle = FALSE)
write.csv(tab_print,"table.csv",na="")

```
---

## Frequency Tables and Crosstabs

### Two-Way Table

```r
tab2 <- table(data$sex, data$disease)
tab2
```

### 8.2 Row Percentages

```r
prop.table(tab2, 1) * 100
```

---

### 8.3 Crosstab with `gt`

```r
as.data.frame(tab2) %>% gt()
```

---

## Correlation Analysis

Correlation measures **strength of association**, not causation.

Use correlation only when:
- Both variables are continuous
- Relationship is linear (Pearson) or monotonic (Spearman)

⚠️ Correlation will fail or mislead if variables are categorical or heavily skewed.

```r
cor(data$age, data$bmi, use = "complete.obs",method="pearson")
cor(data$age, data$bmi, use = "complete.obs",method="spearman")

```

```r
cor.test(data$age, data$bmi)
```

---
## Graphical Exploration (Base & ggplot)

### Histogram (Distribution)

Histograms are used for **continuous variables**.

Be careful:
- Too few bins hide structure
- Too many bins add noise

```r
hist(data$age, col = "grey", main = "Age Distribution")
```

---

### Boxplot (Outliers & Spread)

```r
boxplot(data$age)
```
Helps to the spread of the data and skewness

```r
ggplot(data, aes(y = age)) + geom_boxplot()
```

---

### Scatter Plot (Continuous vs Continuous)

```r
plot(data$age, data$bmi)
```
Helps to understand the direction of relation between two variables


## 10.Chapter Checklist (Learning Outcomes)

✔ Descriptive measures (mean, median, SD, IQR)
✔ Frequency & percentage tables
✔ Reproducible tables (`gt`, `DT`, `flextable`, `tableone`)
✔ Basic plots (histogram, boxplot, scatter)
✔ Crosstabs with percentages
✔ Correlation analysis

**Not covered**:
✘ Data cleaning
✘ Variable recoding
✘ Statistical modeling

---

## 11.What Comes Next?

➡ Data Cleaning & Transformation
➡ Handling Missing Data
➡ Creating Analysis-ready Variables

---

**End of EDA Chapter**


