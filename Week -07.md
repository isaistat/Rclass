# Regression & Epidemiology in R
## PART I-Regression modelling

This chapter introduces **regression models and epidemiology measures** using a practical, health-focused approach.

You will learn:
- Which tests to check before modeling
- How to run linear, logistic, and Poisson regression
- Diagnostics and goodness-of-fit interpretation
- How to compute epidemiological measures (OR, RR, IRR, VE) separately using packages
- How to plot and interpret results without ggplot
- How to write manuscript-ready results

---

## Example Health Datasets

```r
data(infert)   # Case–control study
```
- `case`: infertility status (0 = control, 1 = case)
- `age`: continuous exposure
- `education`, `parity`: categorical covariates

---

## Regression Modeling

### 1. Pre-Model Checks

**Linear Regression:**
- Outcome: continuous
- Check linearity (plot outcome vs predictors, `car::crPlots`)
- Check normality of residuals (`shapiro.test`, QQ plot)
- Check multicollinearity (`car::vif`)
- Check homoscedasticity (residuals vs fitted plot)

**Logistic Regression:**
- Outcome: binary (0/1)
- Sufficient events per predictor (rule of thumb ≥10 events/predictor)
- No perfect separation


---

### 2. Linear Regression

```r
library(car)
fit_lm <- lm(age ~ parity + education, data = infert)
summary(fit_lm)
```
**Interpretation:**
> Each additional parity level increased age by 1.2 years (β = 1.2, 95% CI: 0.3–2.1, p = 0.01).

**Diagnostics:**
```r
crPlots(fit_lm)            # linearity
plot(fit_lm$fitted.values, fit_lm$residuals) # homoscedasticity
qqPlot(fit_lm$residuals)  # normality
shapiro.test(fit_lm$residuals)
vif(fit_lm)                # multicollinearity
```
**How to interpret diagnostics (important):**
- *Linearity (crPlots)*: Curved pattern → linear model inappropriate or transformation needed.
- *Residuals vs Fitted*: Funnel shape → heteroscedasticity; random cloud → OK.
- *QQ Plot / Shapiro test*: Strong deviation or p < 0.05 → residuals not normal (large samples tolerate this).
- *VIF*: >5 (or >10) → problematic multicollinearity.

**Goodness-of-fit:**
- **R²** indicates proportion of variance explained (0–1, higher = better)
- Residuals close to zero and evenly spread indicate good fit

**Plotting (base R):**
```r
plot(infert$parity, infert$age, main="Age vs Parity", xlab="Parity", ylab="Age")
abline(fit_lm, col="red")
```
Final clean result to present
```r
library(broom)
tidy(fit_lm, conf.int = TRUE)
```
---

### 3. Logistic Regression

**How to interpret diagnostics:**
- *Hosmer–Lemeshow*: p > 0.05 → model fits data adequately; p < 0.05 → poor fit.
- *Deviance residuals*: Large absolute values → influential observations.
- *Events per variable*: Too few events → unstable OR estimates.

```r
fit_logit <- glm(case ~ age + parity + education, data = infert, family = binomial)
summary(fit_logit)
```

**Diagnostics:**
```r
library(ResourceSelection)
hoslem.test(infert$case, fitted(fit_logit))  # Hosmer-Lemeshow goodness-of-fit
```
- p > 0.05 indicates good fit
- Deviance residuals near zero indicate good fit

**Confidence intervals:**
```r
confint(fit_logit)
```

**Interpretation Example:**
> Each year increase in age raised odds of infertility (OR = 1.08, 95% CI: 1.02–1.15).

**Plotting fitted probabilities (base R):**
```r
fitted_probs <- fitted(fit_logit)
plot(fitted_probs, jitter(infert$case), xlab="Fitted probability", ylab="Observed outcome", main="Observed vs Fitted")
```

---

### 4. Poisson Regression

**How to interpret diagnostics:**

*Overdispersion statistic*: ≈1 then Poisson is OK; >1.5 then use negative binomial.

*Residual pattern*: Systematic trend, missing covariate or wrong functional form.

```r
library(MASS)
fit_poisson <- glm(events ~ age + parity, offset=log(person_years), data=infert, family=poisson)
```

**Base R plot:**
```r
plot(infert$age, infert$events/person_years, xlab="Age", ylab="Incidence rate", main="Incidence rate vs Age")
```
Final clean result to present
```r
library(broom)
tidy(fit_logit, conf.int = TRUE)
tidy(fit_poisson, conf.int = TRUE)

```

---

## PART II: Epidemiology Measures (Separate from Regression)

## Epidemiology Measures Using Packages

### Packages and 2x2 Tables

```r
library(epiR)
# Example: exposed vs outcome
# Create a simple 2x2 dataset (clear and beginner-friendly)
epi_data <- data.frame(
  exposure = c("Yes", "Yes", "No", "No"),
  outcome  = c("Case", "Non-case", "Case", "Non-case"),
  count    = c(20, 180, 10, 290)
)

# Convert to 2x2 table
tab <- xtabs(count ~ exposure + outcome, data = epi_data)
tab
```

### Odds Ratio (OR)

**When to use:** Case–control studies

**How to interpret:**
- OR = 1 → no association
- OR > 1 → exposure increases odds
- OR < 1 → exposure is protective

```r
epi.2by2(tab, method="case.control", conf.level=0.95)
```

### Risk Ratio (RR)

**When to use:** Cohort studies

**How to interpret:**
- RR = 1 → no risk difference
- RR > 1 → increased risk
- RR < 1 → reduced risk

```r
epi.2by2(tab, method="cohort.count", conf.level=0.95)
```

### Incidence Rate Ratio (IRR)

**When to use:** Person-time data

**How to interpret:**
- IRR = 1 → equal incidence rates
- IRR > 1 → higher incidence in exposed

```r
library(MASS)
# For Poisson regression output
exp(coef(fit_poisson))   # IRR per unit increase
confint(fit_poisson)
```

### Vaccine / Intervention Efficacy (VE)

**When to use:** Prevention studies

**How to interpret:**
- VE = 0% → no protection
- VE = 50% → 50% risk reduction

```r
RR <- 0.5  # example from epi.2by2
VE <- (1 - RR) * 100
VE
```

**Plotting (base R)**
```r
barplot(tab, beside=TRUE, names.arg=c("Exposed","Unexposed"), col=c("red","blue"), main="Cases by Exposure")
```

**Interpretation:**
- OR > 1 → increased odds
- RR > 1 → increased risk
- IRR > 1 → higher incidence rate
- VE = 1 − RR × 100, proportionate reduction in risk

---


## Key Takeaways

- Always check assumptions before modeling
- Use diagnostics: residuals, VIF, Hosmer-Lemeshow
- R² measures variance explained in linear regression
- Epidemiology measures computed via **epiR**, plotted in base R
- Keep regression interpretation separate from epidemiology measures

---

**End of Regression & Epidemiology (Beginner)**


