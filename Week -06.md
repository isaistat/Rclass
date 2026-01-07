# Inferential Statistics in R (Beginner Level)

This chapter introduces **inferential statistics for beginners** using a clear, decision-based flow.

You will learn how to:
- Choose the correct statistical test based on outcome type and study design
- Apply common parametric and non-parametric tests
- Interpret results correctly (what to check and how to report)

**Modeling (regression, GLM, survival analysis) is handled in a separate chapter.**

---

## Example Data (Health-related Built-in R Datasets)

To keep this chapter reproducible and health-focused, we use built-in datasets:

```r
data(esoph)   # Case–control data on esophageal cancer
data(infert)  # Infertility case–control study
```

- `esoph`: categorical outcomes and associations (public health example)
- `infert`: continuous + categorical variables (clinical example)

---

## Par-I: Continuous Outcomes: Decision Flow

To make results easy to understand, this chapter focuses on **direct test output and interpretation**.

We intentionally avoid reproducible-table frameworks here, so beginners can concentrate on:
- choosing the correct test
- reading the output
- writing results in words


### 1. Check Normality

Normality matters mainly for **small samples**.

```r
shapiro.test(infert$age)
```

Visual check (always recommended):

```r
qqnorm(infert$age)
qqline(infert$age)
```

**How to interpret**:
- p > 0.05 → no strong evidence against normality
- Q–Q points roughly on the line → normality is reasonable

---

### 2. One Group vs Reference Value

#### Parametric: One-Sample t-test

```r
t1 <- t.test(mtcars$mpg, mu = 20)

```

**Interpretation**:
- Estimate = sample mean
- CI shows plausible range
- p < 0.05 → mean differs from reference

**Example result (manuscript-ready)**:
> The mean mileage was significantly different from the reference value of 20 (mean = 20.1, 95% CI 18.6–21.6; one-sample t-test, p = 0.04).

#### Non-Parametric: One-Sample Wilcoxon Signed-Rank Test

```r
wilcox.test(mtcars$mpg, mu = 20)
```

**When to use**:
- Outcome is continuous or ordinal
- Distribution is clearly non-normal or sample size is small
- Median (not mean) is the parameter of interest

**Example result (manuscript-ready)**:
> Median mileage differed from the reference value (Wilcoxon signed-rank test, p = 0.03).

- Non-normal or ordinal data



```r
wilcox.test(mtcars$mpg, mu = 20)
```

**When to use**:
- Outcome is continuous or ordinal
- Distribution is clearly non-normal or sample size is small

---

### 3. Two Independent Groups

#### Parametric: Independent (Two-Sample) t-test

```r
t2 <- t.test(mpg ~ am, data = mtcars)

```

**Interpretation**:
- Estimate = mean difference
- CI not crossing 0 → meaningful difference

#### Non-Parametric: Mann–Whitney U Test (Wilcoxon Rank-Sum)

```r
wilcox.test(mpg ~ am, data = mtcars)
```

**Why Mann–Whitney is used**:

- Does not assume normality
- Compares distributions (often interpreted as median difference)
- Preferred when data are skewed or contain outliers

**Example result (manuscript-ready)**:
> Median mileage differed between automatic and manual transmission cars (Mann–Whitney U test, p = 0.002).

---

### 4. Paired Data

#### Parametric: Paired t-test

```r
# Example paired data for reproducible output
set.seed(1)
before <- rnorm(20, 50, 10)
after  <- before + rnorm(20, -3, 5)

t.test(before, after, paired = TRUE)
```

**Interpretation**:
- Tests mean difference within the same subjects

**Example result (manuscript-ready)**:
> The post-intervention measurement was significantly lower than baseline (mean paired difference = −3.1, 95% CI −5.6 to −0.7; paired t-test, p = 0.02).

#### Non-Parametric: Wilcoxon Signed-Rank Test (Paired)

```r
# Non-parametric paired alternative
wilcox.test(before, after, paired = TRUE)
```

**When to use**:
- Paired data with non-normal differences

---

### 5. More Than Two Groups (One-way ANOVA)

#### Parametric: One-way ANOVA

```r
anova1 <- aov(weight ~ group, data = PlantGrowth)
summary(anova1)
```

**Interpretation**:
- p < 0.05 → at least one group mean differs
- ANOVA does **not** tell which groups differ

**Example result (manuscript-ready)**:
> Mean plant weight differed across treatment groups (one-way ANOVA, F = 4.8, p = 0.015).

---

### Post-hoc Tests (After Significant ANOVA)

Post-hoc tests are required **only if ANOVA is significant**.

They answer the question:
> *Which specific groups are different?*

#### Tukey HSD (Parametric Post-hoc)

```r
TukeyHSD(anova1)
```

**When to use Tukey HSD**:
- ANOVA assumptions are met
- Equal or similar variances
- Pairwise mean comparisons

**How to interpret**:
- Each row compares two groups
- CI not crossing 0 → significant difference
- Adjusted p-values control multiple testing

**Example result (manuscript-ready)**:
> The treatment group showed significantly higher mean weight than control (Tukey HSD, p = 0.01).

#### Non-Parametric Post-hoc (After Kruskal–Wallis)

If Kruskal–Wallis is significant, use **pairwise Wilcoxon tests** with p-value adjustment.

```r
pairwise.wilcox.test(PlantGrowth$weight, PlantGrowth$group,
                     p.adjust.method = "bonferroni")
```

**When to use**:
- Non-normal outcome
- Kruskal–Wallis test is significant

**How to interpret**:
- Each comparison reports adjusted p-value
- Focus on direction using group medians

**Example result (manuscript-ready)**:
> Median plant weight differed between treatment and control groups (pairwise Wilcoxon test with Bonferroni correction, p = 0.02).

---

### 6. Two-Way ANOVA

Use when:
- One continuous outcome
- Two categorical independent variables

```r
anova2 <- aov(age ~ factor(case) * factor(education), data = infert)
summary(anova2)
```

**How to interpret**:
- Main effects: overall effect of each factor
- Interaction: whether the effect of one factor depends on the other
- If interaction is significant, interpret interaction first

---

### Non-Parametric Alternative

```r
kruskal.test(age ~ parity, data = infert)
```

**How to interpret**:
- Tests differences in distributions across groups
- Appropriate when normality assumption is violated

---

## Part-II: Categorical Outcomes: Association vs Independence

Categorical tests assess **association**, often described as **lack of independence** between variables.

### 1. Contingency Table

```r
tab <- xtabs(ncases ~ alcgp + tobgp, data = esoph)
tab
```

---

### 2. Chi-square Test of Independence

```r
chi <- chisq.test(tab)

```

**Interpretation**:
- p < 0.05 → evidence of association

**Example result (manuscript-ready)**:
> Alcohol consumption was significantly associated with tobacco use (χ² test of independence, p < 0.001).

Check expected counts:

```r
chi$expected
```
 counts:

```r
chisq.test(tab)$expected
```

---

### 3. Fisher’s Exact Test

```r
fisher.test(tab)
```

**How to interpret**:
- Same null hypothesis as chi-square
- Preferred when expected counts are small

---

## Part-III: Correlation (Continuous vs Continuous)

Correlation measures **strength and direction of association**, not causation.

### 1. Pearson Correlation

```r
cor.test(infert$age, infert$parity, method = "pearson")
```

**When to use Pearson**:
- Both variables are continuous
- Relationship is approximately linear
- No major outliers
- Data are roughly normally distributed

**Interpretation**:
- r ranges from −1 to +1
- Sign indicates direction
- p < 0.05 → evidence of linear association

**Example result (manuscript-ready)**:
> Age was weakly positively correlated with parity (Pearson r = 0.21, p = 0.03).

---

### 2. Spearman Rank Correlation

```r
cor.test(infert$age, infert$parity, method = "spearman")
```

**When to use Spearman**:
- Variables are ordinal or continuous
- Relationship is monotonic (consistently increasing or decreasing)
- Outliers are present
- Normality assumption is violated

**Example result (manuscript-ready)**:
> Age showed a monotonic association with parity (Spearman ρ = 0.24, p = 0.02).

---

### 3. Kendall’s Tau Correlation

```r
cor.test(infert$age, infert$parity, method = "kendall")
```

**When to use Kendall’s tau**:
- Small sample sizes
- Many tied values
- Ordinal measurements
- More robust but less powerful than Spearman

**Example result (manuscript-ready)**:
> Age was positively associated with parity (Kendall’s τ = 0.18, p = 0.04).

---

## Interpreting Results: What to Check

For any inferential test:
- State the null hypothesis
- Identify the estimate (mean, difference, association)
- Check the p-value and confidence interval
- Mention the test used (parametric / non-parametric)
- Write a conclusion in plain language

---

**End of Inferential Statistics**



