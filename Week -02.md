# **Importing, Understanding the Data types in R**

In this chapter, you will learn how to **import data into R** and understand **different types of data**,  steps that are essential before any statistical analysis.

---

## Understanding Data Frames

The most commonly used data structure in R is the **data frame**, which is very similar to an Excel spreadsheet.

- **Rows** represent observations (e.g., patients, visits, samples)
- **Columns** represent variables (e.g., age, sex, blood pressure, outcomes)

---

## About the Dataset

A tertiary-care hospital conducted a study on **1000 adult patients** to understand:

- Demographic characteristics  
- Cardiometabolic risk factors  
- Lifestyle behaviors  
- Treatment outcomes

This same dataset will be used in **all subsequent chapters** for statistical analysis and interpretation.

###### **Data table:**

| id | age | sex    | height_cm | weight_kg | systolic_bp | diastolic_bp | cholesterol | smoking_status | treatment_group | diabetes | hospital_days | visit_date |
| -- | --- | ------ | --------- | --------- | ----------- | ------------ | ----------- | -------------- | --------------- | -------- | ------------- | ---------- |
| 1  | 65  | Female | 160       | 65        | 111         | 86           | 188         | Current        | Control         | No       | 1             | 2024-05-18 |
| 2  | 31  | Male   | 165       | 74        | 108         | 80           | 176         | Never          | Control         | Yes      | 6             | 2024-05-24 |
| 3  | 53  | Male   | 174       | 77        | 107         | 78           | 212         | Never          | Intervention    | Yes      | 5             | 2024-06-01 |
| 4  | 50  | Female | 160       | 78        | 106         | 98           | 227         | Never          | Intervention    | Yes      | 1             | 2024-06-18 |
| 5  | 58  | Female | 161       | 54        | 136         | 91           | 187         | Never          | Control         | No       | 3             | 2024-06-05 |
| 6  | 58  | Male   | 158       | 73        | 116         | 66           | 210         | Never          | Intervention    | No       | 3             | 2024-03-17 |

---

## Installing and Loading Required Packages

Before importing data, ensure that the required packages are installed and loaded.

```r
install.packages(c("tidyverse", "openxlsx", "haven", "janitor", "skimr"))

library(tidyverse)
library(openxlsx)
library(haven)
library(janitor)
library(skimr)
```

---

## **Importing Data into R**

In real-world projects, data commonly comes from **CSV, Excel, SPSS, or STATA files**.

#### Importing a CSV File

```r
data <- read.csv("data.csv")
```

#### Importing an Excel File

```r
library(openxlsx)
data <- read.xlsx("data.xlsx")
```

#### Importing an SPSS File

```r
library(haven)
data <- read_sav("data.sav")
```

#### Importing a STATA File

```r
librayr(haven)
data <- read_dta("data.dta")
```
After importing, the dataset will appear in the **Environment pane** in RStudio.s

## **Export the dataset to work directory**
```r
write.csv(data_1,"updated_data.csv",na="")
write.xlsx(data_1,"updated_data.xlsx",na="") #For excel format data

```
---

#### The first step is to understand, **how R has interpreted each variable**.


#### View the dataset

To see the dataset as spread sheet, to ensure the data is completely/correctly imported
```r
View(data) 
head(data) #the first 6 rows of data will display in console
tail(data) #the last 6 rows of data will display in console

```

#### Check the number of observations in data imported
```r
dim(data) #Returns the number of rows and columns.
nrow(data) #Number of rows in data
ncol(data) # Number of columns in data
```


#### View the variable names list

```r
names(data)
```
Useful for verifying variable names before analysis.

#### View the Structure of Data

```r
str(data)
```

This command shows:
- Variable names
- Data type of each variable (numeric, character, factor, Date, etc.)
- A few example values

This is the **most important command** to check data format.

---

### Different Types of Data in R

Understanding data types is important because R treats each type differently during analysis.

| Type      | Description             | Example            |
  |-----------|-------------------------|--------------------|
  | numeric   | Continuous numbers      | age, bmi           |
  | character | Text values             | "Male", "Yes"     |
  | logical   | TRUE or FALSE           | TRUE               |
  | factor    | Categorical variables   | Normal, Obese      |
  | Date      | Calendar dates          | 2024-03-15         |
  
---

## **Understanding and Changing Data Types**

R assigns data types automatically, but these may need correction.

---

#### **1.Numeric Data**

Numeric variables represent continuous or discrete numbers (e.g., age, BMI).

Check if a variable is numeric:

```r
is.numeric(data$age) #If the output is TRUE, the variable in numeric format
```

Convert to numeric:

```r
data$age <- as.numeric(data$age)
```

⚠️ Non-numeric characters will be converted to `NA`.

---

#### **2.Character Data**

Character variables store text values (e.g., sex, lifestyle habits).

Check:

```r
is.character(data$sex) #If the output is TRUE, the variable in character format
```

Convert to character:

```r
data$sex <- as.character(data$sex)
```

---

#### **3.Logical Data**

Logical variables contain `TRUE` or `FALSE` values.
Check:

```r
is.logical(data$smoker) #If the output is TRUE, the variable in factor format
```
Convert to logical:

```r
data$smoker <- as.logical(data$smoker)
```

---

#### **4.Factor (Categorical) Data**

Factors are used for categorical variables, especially in statistical models.

Check:

```r
is.factor(data$sex) #If the output is TRUE, the variable in factor format
```

Convert character to factor:

```r
data$sex <- as.factor(data$sex)
```

View levels:

```r
levels(data$sex)
```

---

#### **5.Date Data**

Dates are essential for time-based analysis.

Check:

```r
class(data$visit_date)
```

Convert to Date format:

```r
data$visit_date <- as.Date(data$visit_date)
```

Specify format if needed:

```r
data$visit_date <- as.Date(data$visit_date, format = "%d-%m-%Y")
data$visit_date <- as.Date(data$visit_date, format = "%d-%m-%Y %H:%M")
```

---

## **Identifying Missing Values**

Missing values in R are represented as `NA`.

#### Check for Missing Values in the Dataset

```r
any(is.na(data))
```

#### Count Missing Values per Variable

```r
colSums(is.na(data))
```

---

## **Skimming the Data (Exploratory Overview)**

The `skimr` package provides a structured summary of each variable type without performing data cleaning.

```r
skim(data)
```
## **Descriptive measures**

Basic descriptives measures to understand how the numerical values in the dataset
```r
summary(data)
summary(data$weight)
```
To check each descriptive measures
```r
mean(data$weight)
median(data$weight)
sd(data$weight)
range(data$weight)
IQR(data$weight)
```
⚠️ If there is missing value in the variable these will retun the output as `NA`. To avoid that include `na.rm=TRUE` argument in the funtion

```r
mean(data$weight,na.rm=TRUE)
median(data$weight,na.rm=TRUE)
sd(data$weight,na.rm=TRUE)
range(data$weight,na.rm=TRUE)
IQR(data$weight,na.rm=TRUE)
```
Similarly to understand how the categorical values in the dataset

Frequency table:
```r
table(data$sex)
table(data$sex,useNA = "ifany") #To include the missing to display
```

Cross tab:

```r
xtabs(~data$sex+data$status)
xtabs(~data$sex+data$status,addNA = TRUE) #To include the missing to display

```


This helps identify:
- Variable types
- Missing values
- Distribution patterns

⚠️ If the dataset is very big, the output can't display completely

---

## Key takeaways

In this chapter, you learned how to:

- Import data into R
- Understand different data types
- Check the structure and format of data
- Convert variables to appropriate data types
- Identify missing values
- Obtain a high-level overview using `skim()`

---

**End of Chapter 2**  
➡️ Next: **Chapter 3 – Data Cleaning and Data Preparation**
