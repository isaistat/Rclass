# Data Visualization (`ggplot`)
This chapter introduces **basic data visualization using ggplot2** for beginners.

You will learn:

- Which plot to use for which type of data
- How to create common charts using `ggplot2`
- What each plot shows and when it may fail
- How to interpret plots for health and epidemiology data

---

### What is `ggplot2` and How It Works

`ggplot2` is a plotting system based on the **Grammar of Graphics**.

Instead of choosing a chart type directly, you **build a plot layer by layer**.

#### How ggplot2 Thinks (Algorithm / Logic)

Every ggplot follows the same logic:

1. **Data** – what dataset you are using
2. **Aesthetics (**``**)** – how variables map to axes, color, size, etc.
3. **Geometries (**``**)** – what type of plot (bar, point, line)
4. **Scales** – how values are displayed (colors, sizes)
5. **Labels & Themes** – how the plot is presented

You do **not** write a new function for each plot — you **reuse the same structure**.

---

#### Basic Structure of a ggplot

```r
ggplot(data = dataset, aes(x = xvar, y = yvar)) +
  geom_type()
```

- `ggplot()` initializes the plot
- `aes(x,y)` defines variable mapping
- `geom_*()` decides the chart type

If any of these are missing, the plot may fail or be misleading.

---

We use built-in, health-relevant datasets.

```r
library(ggplot2)
```
---

### **1. Bar Plot (Counts)**

#### When to use

- Categorical variable
- Show **counts** of observations

#### When it may fail

- Too many categories → cluttered plot

```r
ggplot(msleep, aes(x = vore)) +
  geom_bar()
```

**Interpretation:**

> The bar height represents the number of animals in each feeding category.

---

### **2. Column Plot (Pre-computed Values)**

#### When to use

- Categorical x
- Numeric y already summarized (mean, total, %)

```r
msleep_sum <- aggregate(sleep_total ~ vore, data = msleep, mean)

ggplot(msleep_sum, aes(x = vore, y = sleep_total)) +
  geom_col()
```

**Careful:** Do NOT use `geom_col()` on raw data.

---

### **3. Box Plot (Distribution & Outliers)**

#### When to use

- Compare distributions across groups
- Identify outliers visually

#### What it shows

- Median, IQR, extreme values

```r
ggplot(msleep, aes(x = vore, y = sleep_total)) +
  geom_boxplot()
```

**Interpretation:**

> Median sleep duration differs by feeding type; points beyond whiskers indicate potential outliers.

---

### **4. Histogram (Continuous Distribution)**

#### When to use

- Single continuous variable
- Check shape (normality, skewness)

```r
ggplot(msleep, aes(x = sleep_total)) +
  geom_histogram(bins = 20)
```

**Careful:** Bin width strongly affects appearance.

---

### **5. Scatter Plot (Two Continuous Variables)**

#### When to use

- Assess relationship between two numeric variables

```r
ggplot(msleep, aes(x = bodywt, y = sleep_total)) +
  geom_point()
```

**Interpretation:**

> Heavier animals tend to sleep less, though variability is high.

---

### **6. Line Plot (Trends Over Time)**

#### When to use

- Time-series data

```r
ggplot(economics, aes(x = date, y = unemploy)) +
  geom_line()
```

**Interpretation:**

> Unemployment shows long-term trends and short-term fluctuations.

---

### **7. Stacked Bar Plot (Composition)**

#### When to use

- Show subgroup composition within categories

```r
ggplot(msleep, aes(x = vore, fill = conservation)) +
  geom_bar()
```

**Careful:** Hard to compare across bars when many categories exist.

---

### **8. Bubble Plot (Third Variable)**

#### When to use

- Two continuous variables + one magnitude variable

```r
ggplot(msleep, aes(x = bodywt, y = sleep_total, size = brainwt)) +
  geom_point(alpha = 0.6)
```

**Interpretation:**

> Bubble size represents brain weight.

---

### **9. Pie Chart (Proportions – Use Sparingly)**

#### When to use

- Very few categories
- Simple proportion display

```r
msleep_pie <- aggregate(sleep_total ~ vore, data = msleep, length)

ggplot(msleep_pie, aes(x = "", y = sleep_total, fill = vore)) +
  geom_col(width = 1) +
  coord_polar("y")
```

**Warning:** Pie charts make comparison difficult; bar charts are often better.

---

### **10. Correlation Visualization**

#### When to use

- Assess linear relationship visually

```r
ggplot(msleep, aes(x = bodywt, y = brainwt)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

**Interpretation:**

> Positive linear association between body and brain weight.

---


## **Basic Customization in ggplot2 (Learn Once, Use Everywhere)**

This section introduces **essential customizations** that apply to *all* plots.

#### Color vs Fill

- `color` → points, lines, borders
- `fill` → bars, boxes, areas

```r
ggplot(msleep, aes(x = vore, fill = vore)) +
  geom_bar(color = "black")
```

---

#### Shape and Size

```r
ggplot(msleep, aes(x = bodywt, y = sleep_total, shape = vore)) +
  geom_point(size = 3)
```
When useful: Distinguishing groups in scatter plots.

---

#### Labels and Titles

```r
ggplot(msleep, aes(x = bodywt, y = sleep_total)) +
  geom_point() +
  labs(
    title = "Body Weight vs Sleep Duration",
    x = "Body Weight (kg)",
    y = "Total Sleep (hours)",
    caption = "Source: msleep dataset"
  )
```
Always label axes for manuscript-quality figures.

---

#### Text Annotations

```r
ggplot(msleep, aes(x = bodywt, y = sleep_total)) +
  geom_point() +
  annotate("text", x = 50, y = 18, label = "Higher weight, less sleep")
```
Use sparingly to highlight key findings.

---

#### Manual Colors (Publication Friendly)

```r
ggplot(msleep, aes(x = vore, fill = vore)) +
  geom_bar() +
  scale_fill_manual(values = c("#4DAF4A", "#377EB8", "#E41A1C"))
```
**Tip:** Use color-blind–friendly palettes for publications.

Referral Link for more learning on ggplot: https://r-graph-gallery.com/ 

---


**End of Data Visualization (Beginner)**


