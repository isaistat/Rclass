# **Chapter 2: Importing, Understanding, and Cleaning Data in R**

In this chapter, you will learn how to **bring data into R**, understand **different types of data**, and perform **basic data cleaning**.  

---

The most commonly used data structure is **data frame**, which is very similar to an Excel sheet.

- **Rows** represent observations (eg. patients, visits, samples)
- **Columns** represent variables (eg. age, sex, blood pressure, outcomes)

---

#### **About the data**

A tertiary-care hospital conducted a study on **200 adult patients** to understand:
Demographic characteristics, Cardio metabolic risk factors, Lifestyle behaviors,Treatment outcomes

This same dataset will be used in **all future chapters** for statistics and analysis.


---

---
Install the package`'openxlsx'`
```r
install.packages("tidyverse")
library(tidyverse)
```
### Importing Data into R

In real projects, data usually comes from **CSV or Excel files**.

#### To import .csv file
```r
data <- read.csv("data.csv")
```

#### To import Excel files. the required package is `'openxlsx'`

```r
install.packages("openxlsx")
library(openxlsx)
data <- read.xlsx("data.xlsx")
```
#### To import SPSS file. the required package is `'haven'`
```r
library(haven)
data <- read_sav("data.sav")
```

#### To import STATA file
```r
data <- read_dta("data.dta")
```
After importing, the dataset will appear in the **Environment pane**.


---

### Different Types of Data in R

Understanding data types is essential because R treats numbers and text differently.

| Type | Description | Example |
|-----|------------|--------|
| numeric | Continuous numbers | age, bmi |
| character | Text data | "Male", "Yes" |
| logical | TRUE / FALSE | TRUE |
| factor | Categorical data | Normal, Obese |
| Date | Calendar dates | 2024-03-15 |

---

### Loading the tidyverse Package

The **tidyverse** is a collection of packages for data manipulation and visualization.

```r
library(tidyverse)
```

If not installed:

```r
install.packages("tidyverse")
```

---

### Viewing the Data

#### Viewing the Dataset

- Double-click `data` in the **Environment pane**
- Or use:

```r
View(data)
```

---

### Viewing the Structure of the Data

To understand the type of each variable:

```r
str(data)
```

This shows whether variables are numeric, character, factor, or date.

---

### Viewing Column Names

```r
names(data)
```

Useful for checking spelling while writing code.

---

### Renaming Variables

#### Rename a Single Variable

```r
data <- data %>% rename(sbp = systolic_bp)
```

#### Rename Multiple Variables

```r
data <- data %>% rename(
  dbp = diastolic_bp,
  chol = cholesterol
)
```

---

### Cleaning Variable Names (Recommended)

Messy variable names can cause errors.

#### Using the janitor Package

```r
install.packages("janitor")
library(janitor)

data <- data %>% clean_names()
```

This will:
- Convert names to lowercase
- Replace spaces with `_`
- Remove special characters

---

### Skimming the Data (Quick Overview)

The **skimr** package provides a quick summary of the dataset.

```r
install.packages("skimr")
library(skimr)

skim(data)
```

---

### Keeping and Dropping Variables

#### Keep Selected Variables

```r
data %>% select(age, sex, bmi)
```

#### Drop Variables

```r
data %>% select(-cholesterol)
```

---

### Sorting the Data

```r
data %>% arrange(age)
```

Descending order:

```r
data %>% arrange(desc(age))
```

---

### Modifying Data (Creating New Variables)

#### Example: Creating Risk Group

```r
data <- data %>%
  mutate(
    risk_group = ifelse(age >= 60 | diabetes == "Yes",
                        "High Risk",
                        "Low Risk")
  )
```

---

### Chapter Summary

In this chapter, you learned how to:

- Create and import data into R
- Understand different data types
- View and inspect datasets
- Rename and clean variables
- Select, sort, and modify data

---

**End of Chapter 2**  
➡️ Next, we move to **Chapter 3: Descriptive Statistics and Data Exploration**.

