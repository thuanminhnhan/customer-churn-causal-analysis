# Dataset Information

## Data Source

This project uses the customer churn dataset distributed through the R package `modeldata`.

- Source: https://modeldata.tidymodels.org/reference/mlc_churn.html
- Package: `modeldata`
- Original dataset: 5,000 observations and 20 variables.

## Data Availability

The raw dataset is not included in this repository.

Users who wish to reproduce the analysis should obtain the dataset from the original source and save it as:

`data/churn.csv`

## Data Preparation

The analysis code expects a CSV file named `churn.csv` inside the `data/` directory.

The variable `customer_service_contact` is derived from `number_customer_service_calls`:

- 1: At least one customer service call
- 0: No customer service calls

Please check the original dataset source for applicable terms of use.
