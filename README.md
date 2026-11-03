# Causal Analysis of Customer Churn

## Project Overview

Customer churn analysis is often approached as a prediction problem: identifying customers who are likely to leave.

This project takes a complementary approach by investigating how selected subscription and customer-service variables relate to churn under an explicit causal framework.

Using observational customer data and the DoWhy framework, the analysis defines causal assumptions, estimates Average Treatment Effects (ATE), conducts refutation checks, and examines sensitivity to potential unobserved confounding.

The goal is to translate analytical findings into practical business questions and recommendations, rather than claim that observational estimates alone establish definitive causal effects.

## Business Questions

The project investigates three questions:

1. How is having an International Plan associated with customer churn?
2. How is having a Voice Mail Plan associated with customer churn?
3. How is having contacted customer service at least once associated with customer churn?

The outcome variable is `churn`, encoded as 1 for churn and 0 otherwise.

## Dataset

The analysis uses the Customer Churn dataset distributed through the Tidymodels `modeldata` ecosystem.

- Observations: 5,000
- Original variables: 20
- Observed churn rate: 14.14%

The original dataset is not included in this repository. Refer to the official sources:

- [Tidymodels MLC Churn dataset](https://modeldata.tidymodels.org/reference/mlc_churn.html)
- [CRAN modeldata package](https://cran.r-project.org/web/packages/modeldata/index.html)

Please check the original source for dataset terms and licensing before use.

## Analytical Approach

### 1. Data Preparation

The workflow includes:

- Loading and validating required variables
- Checking missing values
- Encoding binary treatment and outcome variables
- Creating `customer_service_contact`, defined as 1 when a customer has made at least one customer-service call and 0 otherwise

No missing values were found in the variables used in the reported analysis.

### 2. Causal Framework

A separate Directed Acyclic Graph (DAG) is specified for each treatment.

The assumed structure is:

- Adjustment variables → Treatment
- Adjustment variables → Churn
- Treatment → Churn

The six adjustment variables are:

- `total_day_minutes`
- `total_day_calls`
- `total_eve_minutes`
- `total_eve_calls`
- `total_night_minutes`
- `total_night_calls`

These variables form the predefined adjustment set under the working causal assumptions.

The dataset does not establish the temporal ordering of all variables. Therefore, the adjustment set and DAG should be interpreted as assumptions that require domain justification, not facts proven by the observed data.

### 3. Effect Estimation

The analysis estimates the Average Treatment Effect (ATE) using a generalized linear model with a binomial family.

The reported effect is expressed as a risk difference:

`ATE = E[Y | do(T=1)] - E[Y | do(T=0)]`

Results are reported with 95% confidence intervals and expressed in percentage points for business interpretation.

These estimates are conditional on the specified causal assumptions and model.

### 4. Refutation Checks

Two refutation checks are conducted:

- Placebo treatment
- Random common cause

They are used to examine how the estimates respond to specific diagnostic changes. They do not prove that the DAG is correct or eliminate all potential bias.

### 5. Sensitivity Analysis

The analysis introduces simulated unobserved common causes at different effect-strength settings.

This is intended to explore how estimates may respond to potential unobserved confounding. The settings are implementation-specific and should not be interpreted directly as probabilities or real-world confounder prevalence.

## Key Results

| Treatment | ATE | 95% Confidence Interval |
|---|---:|---:|
| International Plan | +29.42 pp | +24.81 to +33.76 pp |
| Voice Mail Plan | -8.87 pp | -10.74 to -6.99 pp |
| Customer Service Contact | +3.25 pp | +1.22 to +5.36 pp |

### International Plan

The estimated ATE is +29.42 percentage points.

Under the specified model and causal assumptions, having an International Plan is associated with a substantially higher estimated churn risk.

This is the largest estimated effect among the three treatments and should be prioritized for further business investigation.

### Voice Mail Plan

The estimated ATE is -8.87 percentage points.

Under the specified model and assumptions, having a Voice Mail Plan is associated with a lower estimated churn risk.

This finding motivates further investigation into feature awareness, usage, and customer engagement. It does not establish the mechanism behind the observed relationship.

### Customer Service Contact

The estimated ATE is +3.25 percentage points.

Customers who contacted customer service at least once have a higher estimated churn risk under the specified model.

This variable may be useful as an operational signal for reviewing customer issues. The estimate should not be interpreted as evidence that contacting support itself causes churn.

## Business Implications

The findings suggest three areas for further investigation:

1. **International Plan:** Review pricing, usage patterns, plan communication, and customer experience among subscribers.
2. **Voice Mail Plan:** Investigate whether feature awareness and onboarding are related to customer engagement and retention.
3. **Customer Service Contact:** Review the reasons for support contact, first-contact resolution, and opportunities for proactive follow-up.

These are investigation priorities, not proven intervention effects. Business decisions should consider operational context and additional evidence.

## Proposed Business Validation

The observational findings can inform future controlled experiments:

- **International Plan:** Test alternative plan communication or service improvements among eligible customers.
- **Voice Mail Plan:** Test a feature-awareness or onboarding intervention against standard communication.
- **Customer Service Contact:** Compare proactive follow-up after support interactions with the existing process.

Each experiment should define eligibility, random assignment, the churn measurement window, and business guardrails such as revenue, customer satisfaction, and support cost.

These experiments are proposed future work. They have not been conducted in this project.

## Limitations

- The analysis uses observational data and relies on assumptions encoded in the DAG.
- The dataset does not provide sufficient temporal detail to verify the ordering of all variables.
- Unobserved confounding cannot be ruled out.
- The adjustment set may not capture every relevant factor.
- The customer-service count variable is simplified into a binary indicator of at least one contact.
- ATE describes an average estimated effect, not the response of every individual customer.
- Refutation and sensitivity checks do not establish causal validity.

The results should therefore be interpreted as model-based estimates under explicit assumptions, not guaranteed outcomes of business interventions.

## Reproducibility

Install dependencies:

```bash
pip install -r requirements.txt
```

Obtain the dataset from its public source and place the CSV file at:

```text
data/churn.csv
```

Run the analysis:

```bash
python src/causal_analysis.py
```

Generated outputs are saved in the `outputs/` directory.

The original dataset is excluded from version control.

## Tools

Python, Pandas, NumPy, Statsmodels, DoWhy, Matplotlib, Git, and GitHub.

## Project Scope

This portfolio project focuses on:

**Data Preparation → DAG → Adjustment → ATE → Refutation → Sensitivity → Business Interpretation**

The emphasis is on transparent assumptions, reproducible analysis, and business-oriented communication.
