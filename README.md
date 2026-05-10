# Dython — Categorical Variable Correlation

Measuring associations between categorical variables using Dython — because Pearson doesn't work on "yes" and "no."

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/wsamuelw/dython-demo/blob/main/dython_demo.ipynb)

## Problem

Standard correlation (Pearson, Spearman) only works on numeric data. But most real-world datasets have categorical variables — colour, type, status, gender. How do you measure the relationship between "Fire" and "Legendary" in a Pokemon dataset? You can't just subtract them.

Dython solves this by computing association values between all pairs of categorical (and mixed) variables, producing a correlation-style heatmap for non-numeric data.

## What It Does

1. **Identifies** categorical columns automatically
2. **Calculates** association values between all categorical pairs
3. **Visualises** the result as a heatmap with a colour gradient

## How Dython Works

Dython uses **Theil's U** (uncertainty coefficient) for nominal-nominal associations and **correlation ratio** for nominal-numeric. Both range from 0 (no association) to 1 (perfect association), so the matrix looks and feels like a Pearson correlation heatmap.

| Association Type | Method | Range |
|-----------------|--------|-------|
| Categorical ↔ Categorical | Theil's U | 0–1 |
| Categorical ↔ Numeric | Correlation ratio | 0–1 |
| Numeric ↔ Numeric | Pearson | -1 to 1 |

## Setup

### Google Colab

Click the badge above.

### Local

```bash
pip install dython pandas
git clone https://github.com/wsamuelw/dython-demo.git
cd dython-demo
jupyter notebook dython_demo.ipynb
```

## Key Code

```python
from dython.nominal import associations, identify_nominal_columns

# auto-detect categorical columns
categorical_features = identify_nominal_columns(df)

# compute the full association matrix
complete_correlation = associations(df, nominal_columns=categorical_features)

# extract and display
df_complete_corr = complete_correlation['corr']
df_complete_corr.style.background_gradient(cmap='coolwarm')
```

## Data

**Pokemon Dataset** — includes Type, Colour, Legendary status, and other categorical attributes. The association matrix reveals which attributes are most strongly related.

## When to Use Dython

- **EDA** — understand which categorical features are related before modelling
- **Feature selection** — drop highly correlated categorical features (just like you would with numeric)
- **Data profiling** — quickly see relationships in survey data, customer segments, or any categorical-heavy dataset

## When NOT to Use It

- **Only numeric data** — standard `df.corr()` is faster and sufficient
- **Very large datasets** — Theil's U computation scales with unique categories
- **Causation** — association ≠ causation, same as Pearson

## Tech Stack

- **dython** — categorical association measures
- **pandas** — data handling
- **matplotlib** — heatmap rendering (via dython)

## References

- [Dython documentation](http://shakedzy.xyz/dython/)
- [Finding correlation for categorical variables](https://medium.com/@knoldus/how-to-find-correlation-value-of-categorical-variables-23de7e7a9e26)

## License

MIT
