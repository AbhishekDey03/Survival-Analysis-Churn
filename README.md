# Survival Analysis: Customer Churn & Retention EV

Exploratory survival analysis on the IBM Telco Churn dataset, framed as a business decision problem: **for which customers is a retention intervention worth the cost?**

## Results

Under a moderate treatment effect (γ = 0.75), RSF-estimated individual survival curves (mean E[T] = 53.7 months) identify 4,803 customers as positive-EV intervention targets, representing £343k/month monthly recurring revenue (MRR); the remaining 1,723 customers (£79k/month) do not clear the cost threshold under any gamma assumption and should not be targeted.


## Pipeline

| Model | Purpose |
|---|---|
| Kaplan-Meier | Nonparametric population baseline, log-rank test by contract type |
| Cox PH | Interpretable hazard ratios; PH assumption tested |
| Random Survival Forest | Individual survival curves |

Individual survival curves from RSF are integrated to expected LTV per customer. An intervention EV framework then computes the net gain from a retention action under three assumed treatment effects ($\gamma$ = 0.9 / 0.75 / 0.5), assigning each customer a tier based on whether $\Delta$ EV > 0 across scenarios.

## Key design decisions

- **Event**: voluntary churn only. Deceased customers dropped; "Moved" customers
  right-censored (single-market California provider — relocation = geographic exit,
  not product churn).
- **Time origin**: account open date (Tenure Months used directly).
- **Censoring cutoff**: 72 months — maximum observed tenure, calendar-driven.
- **Leakage removed**: Churn Score, CLTV, Total Charges (encodes tenure),
  and all location columns.


## Requirements

```bash
pip install -r requirements.txt
```

Run `Survival_Analysis.ipynb` top to bottom.
