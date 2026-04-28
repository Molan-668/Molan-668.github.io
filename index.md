# Project 1: Survival Analysis on Customer Churn

**Author:** Liuan LU | **Date:** 2026-04-28

## 1. Process of Survival Analysis
Due to the presence of "right-censored" data (customers who haven't churned by the end of the observation period), traditional regression models fall short. In this project, we implemented a complete survival analysis pipeline:

* **Data Preparation:** Used PySpark to extract `subscription_start` and `subscription_end`, calculating the `duration` in days. We encoded the event status where `is_churn` = 1 (churn event occurred) and 0 (right-censored).
* **Exploratory Assessment (Kaplan-Meier):** Applied the non-parametric Kaplan-Meier estimator to fit the survival function of the entire cohort, visually assessing the probability of retention over time.
* **Risk Factor Modeling (Cox Proportional Hazards):** Built a Cox semi-parametric model. We introduced covariates such as `monthly_fee`, `auto_renew`, and `age` to calculate the Hazard Ratio (HR), quantifying how specific variables accelerate or decelerate the churn rate.

## 2. Kaplan-Meier Survival Estimation Results
<img width="1726" height="1070" alt="Q2_1" src="https://github.com/user-attachments/assets/689668ea-d870-42f6-9954-3fc8e0145086" />

**Analysis Record:** The K-M survival curve reveals a steady decline in retention probability over time. During the first 200 days, the decline is relatively gentle, maintaining a retention rate above 80%. Observing the median threshold (50% survival rate), we found the **Median Survival Time is approximately 420 days**. This means half of the customer base is expected to churn after about 420 days of subscription.

## 3. Cox Proportional Hazards Model Results
<img width="1812" height="1082" alt="Q2_3" src="https://github.com/user-attachments/assets/101f1592-6608-4cd3-8d56-16c16d5ffa0d" />
<img width="1998" height="1126" alt="Q2_2" src="https://github.com/user-attachments/assets/4024b1be-ad58-4739-8e86-5e2adaaaba07" />

**Analysis Record:** * **Model Evaluation:** The Concordance Index is 0.52, indicating a limited discriminative ability of the selected covariates in the current sample distribution.
* **Feature Significance:** The p-values for `monthly_fee` (0.06), `auto_renew` (0.44), and `age` (0.82) are all > 0.05, meaning they do not reach conventional statistical significance.
* **Hazard Ratios (HR):** The confidence intervals for all features cross the zero-mark (log(HR)=0), and their `exp(coef)` values are extremely close to 1.00. Overall, in this dataset, monthly fee, auto-renew status, and age do not have a statistically significant accelerating or decelerating impact on the survival cycle.
