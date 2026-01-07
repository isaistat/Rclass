# Data Preparation and Cleaning in R

In this chapter, you will learn how to **prepare and clean data** using R.  
These steps are essential before performing **descriptive statistics, hypothesis testing, or modeling**.

> **Assumptions**
> - Data has already been imported into R  
> - Column names are clean and readable  
> - You understand basic data types (numeric, character, factor, Date)

---

### Why Data Cleaning Is Important

Real-world data is often:
- Incomplete
- Inconsistent
- Poorly formatted

Data cleaning ensures that:
- Analyses are accurate
- Results are reproducible
- Errors are minimized

---

### Loading Required Package

The **tidyverse** package provides powerful tools for data manipulation.

```r
library(tidyverse)
```

---

### **Selecting Variables**

```r
data %>% select(age, sex, bmi)
data %>% select(age:cholesterol)
data %>% select(-cholesterol)
```
To select specific columns 

---


### **Sorting the Data**

```r
data %>% arrange(age)
```
Default sort is Ascending order the dataset by specific variable mentioned

```r
data %>% arrange(desc(age))
```
Descending the dataset by specific variable

---

### **Renaming Variables**

```r
data <- data %>% rename(sbp = systolic_bp)
data <- data %>% rename(dbp = diastolic_bp, chol = cholesterol)
```
rename shorter name which makes easier to use


### **Filtering Rows**

```r

data %>% filter(age > 50)
data %>% filter(age > 50 & diabetes == "Yes")

```
Helps in Handling Missing Values
```r
data %>% filter(!is.na(age)) #keep only rows without age missing
data %>% filter(is.na(age)) #keep only rows with age missing

```

```r
any(is.na(data))
```


To keep rows that meets certain conditions. logical operators will be used

Recalling the logical operators from chapter-1

| Symbol | Meaning |
|------|---------|
| `|` | OR |
| `&` | AND |
| `==` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |

---

---

### **Creating and Modifying Variables**
#### **1.Using `ifelse()`**
An ifelse command has 3 essential parts. 1. What should it to see if it is true, 2. what should it do if that is true, 3. What should it do if that isn’t true. We need to supply the command with something to check, and two things to do depending on whether that is true or not.

```r
data <- data %>%
  mutate(high_risk = ifelse(age >= 60 | diabetes == "Yes", "High Risk", "Low Risk"))
```
⚠️ Create new varibles based on only with 2 conditions.

---

#### **2.Using `case_when()`**

```r
data <- data %>%
  mutate(age_group = case_when(
    age < 40 ~ "Young",
    age < 60 ~ "Middle-aged",
    TRUE ~ "Elderly"
  ))
```

---

#### **3.Using `cut()` for Categorizing Continuous Variables**

```r
data <- data %>%
  mutate(bmi_category = cut(bmi,
    breaks = c(0, 18.5, 25, 30, 100),
    labels = c("Underweight", "Normal", "Overweight", "Obese")
  ))
```
This converts the numerical into categories

---

### **Recoding Values**

```r
data <- data %>%
  mutate(sex = recode(sex, "Male" = "M", "Female" = "F"))
```
To change existing values into new lables


---

### **Grouped Summaries**
To do the summaries by groups/categories

```r
data %>%
  group_by(sex) %>% #group the data by sex
  summarise(mean_age = mean(age, na.rm = TRUE), count = n())
```

### **Imputing Missing Values with estimates (using ifelse())**

```r
data <- data %>%
  mutate(bmi = ifelse(is.na(bmi), mean(bmi, na.rm = TRUE), bmi))
```
---

### Key takeaways

You learned how to:

- Inspect data
- Select and filter variables
- Rename columns
- Create new variables
- Handle missing values
- Summarise grouped data

---

**Next Chapter**  
➡️ Chapter 4: Descriptive Statistics and Exploratory Data Analysis

