---
title: Sensitivity analysis practical
author: Gibran Hemani
output: word_document
---

## 1. Recap

Use the Stata command `paramed` to estimate the role of BMI in mediating the association between adult social class and physical function, using the same covariates as in your analyses above. Before running the analysis, you may need to install the command by typing `ssc install paramed` in the command line. The syntax to run this model is shown below. Use the help file (`help paramed` to make sure you understand what each part of the code is doing:

```
paramed phys_function, avar(asoc) mvar(bmi) a0(0) a1(1) m(22) yreg(linear) mreg(linear) cvars(csoc pa) boot reps(1000) seed(1234)
```

---

## 2. Confounding of the total effect

a) What unmeasured confounders could be biasing the total effect of adult social class on physical function?

- Genetic factors
- Geographic location
- Year of birth

b) How large would the U->X and U->Y effects have to be to explain the total effect?

- Total effect = -0.29
- Effect of $-0.29 = \delta\gamma$
- e.g. if $\delta = 0.6$ then $\gamma = -0.29 / 0.6 = 0.49$

c) How much would the total effect attenuate if there was a similar confounding effect of physical activity at age 30? How many confounders of similar magnitude would be required to explain the association?

Total effect including confounder

```
regress phys_function asoc csoc pa
```

Gives -0.29

Total effect excluding confounder

```
regress phys_function asoc csoc
```

Gives -0.32

Confounder has a 0.03 influence. Would need to have 10 independent unmeasured confounders of similar magnitude

---

## 3. Effect of mediator-outcome confounders on the natural direct and natural indirect effects

a) What unmeasured mediator-outcome confounders might exist?

- e.g. Health factors


b) What is the proportion of the total effect mediated by BMI?

- NDE = -0.1
- NIE = -0.2
- prop = -0.2 / (-0.1 + -0.2) = 0.67


c) An unmeasured confounder has an influence of 0.3 on BMI and 0.2 on physical function. What is the revised proportion of the total effect explained by mediation by BMI?

- $\delta\gamma = 0.3 \times 0.2 = 0.06$
- $NDE = -0.1 + 0.06 = -0.04$
- $NIE = -0.2 - 0.06 = -0.26$
- $prop = -0.26 / (-0.26 + -0.04) = 0.87$

---

## 4. Measurement error

a) If BMI was self reported what problem might this incur?

Underestimate of the mediating effect

b) What is the variance of BMI

```
summarize bmi, detail
```

Gives 6.17

c) Suppose that 20% of the variance in BMI is due to measurement error. What would the true variance in BMI be?

- measured variance = 6.17
- true variance = 0.8 * 6.17 = 4.94
- error variance = 0.2 * 6.17 = 1.23

d) Let's create a new variable with even higher measurement error. What is the difference in the variances between these two measures?

```
gen bmim=bmi+invnorm(uniform())
lab var bmim "Self-Reported BMI in adulthood"
```

```
summarize bmi, detail
summarize bmim, detail
```

e) Calculate the proportion of the effect of adult social class on physical function mediated by BMI, and then again but this time using self reported BMI.


```
paramed phys_function, avar(asoc) mvar(bmi) a0(0) a1(1) m(22) yreg(linear) mreg(linear) cvars(csoc pa) boot reps(1000) seed(1234)
```

```
paramed phys_function, avar(asoc) mvar(bmim) a0(0) a1(1) m(22) yreg(linear) mreg(linear) cvars(csoc pa) boot reps(1000) seed(1234)
```


The proportion changes to $-0.16/-0.29 = 0.55$
