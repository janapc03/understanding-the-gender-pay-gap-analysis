# Understanding the Gender Pay Gap
## Description

The purpose of this analysis is to discern whether there is a significant difference in the proportions of men and women who earn more than 50K in the United States. The leading question in this analysis is: Is there a significant difference in the proportions of men and women who earn more than 50K in the United States?

This data comes from the 1994 Census and was used to predict whether an individual earns more than $50,000 USD annually.

The analysis involves conducting a hypothesis test using both bootstrapping and asymptotic methods, as well as calculating a confidence interval for the true value of the difference in proportions.  All code was completed in R using the platform JupyterNotebook.

---

## Hypothesis Testing

We will define the null and alternative hypotheses to be:
- Null hypothesis: the proportions of men and women who earn more than $50,000 annually are equal
- Alternative hypothesis: the proportions of men and women who earn more than $50,000 annually are *not* equal
  
---

## Bootstrapping Method

To produce the bootstrapped null distribution, we generate 1000 samples and calculate the proportion of men who earn more than 50K minus the proportion of women who earn more than 50K for each sample. Then, we calculate the p-value for the test.
```
# Generate bootstrapped null distribution using 1000 samples
bootstrapped_null <- adult_data %>%
    specify(formula = Income ~ Sex, success = ">50K") %>%
    hypothesize(null = "independence") %>% 
    generate(reps = 1000, type = "permute") %>%
    calculate(stat = "diff in props", order = c("Male", "Female"))

# Calculate the p-value for the bootstrap method
p_value_boot <- bootstrapped_null %>%
    get_p_value(obs_stat = obs_diff_prop, direction = "both")
```
Output:

<img width="364" height="129" alt="Screenshot 2025-11-19 at 3 09 49 PM" src="https://github.com/user-attachments/assets/4e469c66-622c-41f8-bbef-931698615339" />

---

## Two Sample Z-Test

We can also use an asymptotic method for the hypothesis test. After checking the appropriate assumptions (independent observations, sufficient sample size, independent selection of samples), we can conduct the test.
```
# Conduct the two sample z-test
z_test_result <- tidy(prop.test(x = table(adult_data$Sex, adult_data$Income),
                  n = nrow(adult_data),
                  alternative = "two.sided",
                  correct = FALSE))
```
Output:

<img width="992" height="156" alt="Screenshot 2025-11-19 at 3 15 44 PM" src="https://github.com/user-attachments/assets/89adfa4d-dfa0-4050-9479-af1adf1d6162" />


---

## Findings and Impact

Based on the results from both the bootstrap and asymptotic methods, at a 5% significance level, there is enough evidence to suggest that there is a statistically significant difference between the proportion of men and women who earn over 50K.

While this analysis suggests that there is a significant difference in the proportions of men and women who earn over 50K, further research can be done to investigate other facts that can affect income. These findings could raise more awareness and address the disaparity in income between genders, bringing more attention to issues regarding gender inequality and encourage an improvement in equity practices in various industries.
