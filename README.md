# Car Crash — Predicting Occupant Survival

![Course](https://img.shields.io/badge/Course-Machine%20Learning%20--%201-d62728)
![Grade](https://img.shields.io/badge/Grade-A%20(4.0)-success)
![Type](https://img.shields.io/badge/Type-Classification%20%C2%B7%20Imbalanced%20Data-blue)
![Tools](https://img.shields.io/badge/Tools-statsmodels%20%C2%B7%20scikit--learn-orange)

A binary classification project that predicts **whether an occupant survives a car crash**, and identifies the factors that most influence survival — so safety regulations can be targeted where they matter.

---

## Business Problem

Car crashes remain a leading cause of injury and death. The Department of Road Transport reported a **15% year-on-year rise in urban crashes**. They can record causes after the fact, but they want to **pre-empt risk**. Understanding which factors drive fatalities would let them design better safety regulations for manufacturers and road users.

## Objective

Analyse five years of historical crash data, build a predictive model for survival, and identify the most critical factors influencing survival outcomes.

## Data

| Attribute | Detail |
|---|---|
| Records | **11,217 crashes** |
| Attributes | **12** |
| Fields | impact speed band, observation weight, seatbelt use, frontal vs non-frontal impact, sex, occupant age, accident year, vehicle model year, airbag status (deploy / no deploy / unavailable), occupant role (driver/passenger), target `deceased` (yes/no) |

## Approach

1. **Data quality checks** — confirmed zero missing values and zero duplicates.
2. **Feature engineering** — created `veh_usage_duration = accident year − model year`, capturing how old the vehicle was at the time of the crash.
3. **EDA** — univariate distributions for every variable, then bivariate analysis of each factor against the target.
4. **Preprocessing** — deliberately minimal outlier treatment (the extreme values are genuine — very old vehicles and design weights). Dropped identifiers, one-hot encoded categoricals, converted the target to 0/1, and standardised features.
5. **Modelling** — 70/30 stratified split, then a logistic regression baseline (statsmodels) and a decision tree classifier.
6. **Improvement** — dropped high p-value variables from the logistic regression, found the optimal classification threshold from the ROC curve, and pre-pruned the decision tree with grid search.

## Key Findings

**The target is imbalanced:** **89.5% survived vs 10.5% died** — so accuracy is misleading and the model must be judged on **recall for the "deceased" class**.

| Factor | Finding |
|---|---|
| **Speed is the strongest risk gradient** | Fatality rate rises monotonically: **1.1%** at 1–9 km/h → **2.1%** at 10–24 → **9.0%** at 25–39 → **25.6%** at 40–54 → **51.3%** at 55+ km/h |
| **Seatbelts more than triple survival odds** | **20.2%** fatality without a belt vs **6.4%** with one |
| **Non-frontal crashes are deadlier** | **15.0%** vs **8.1%** for frontal, even though frontal crashes are more common (64.4%). Restraint systems are optimised for frontal protection; side and oblique impacts remain harder to mitigate |
| **Airbag availability matters more than deployment** | **16.1%** fatality when unavailable, **8.4%** when deployed, **5.3%** when present but not deployed (those were less severe crashes) |
| **Age matters** | Median age of the deceased ~**40** vs ~**32** for survivors |
| **Older vehicles are riskier** | Fatalities skew toward higher vehicle usage duration; survivors cluster in newer cars (0–5 years) |
| **Sex and role show modest gaps** | Male 11.8% vs female 9.0%; passengers 11.9% vs drivers 10.1% — likely driven by exposure and behaviour rather than biology |

**Model results**
- Logistic regression baseline reached ~**91.5% test accuracy but only ~38% recall** on the deceased class — a reminder that high accuracy on imbalanced data can hide poor detection of the cases that matter.
- The unpruned decision tree overfit completely (100% train, poor test), which pre-pruning addressed.

## Recommendations

1. **Enforce speed management**, especially in urban areas where even moderate speed increases sharply raise fatality risk.
2. **Keep seatbelt compliance enforcement** as a top priority — it is the most effective single intervention.
3. **Focus on non-frontal protection** — side airbags, reinforced door structures.
4. **Encourage fleet renewal** and safety upgrades for older vehicles, which lack modern protective systems.
5. **Prioritise recall of the "deceased" class** when tuning the model, since missing a high-risk case is the costliest error.

## Skills Demonstrated

Classification modelling · logistic regression with statsmodels · decision trees · ROC curve and threshold optimisation · pre-pruning with grid search · feature importance analysis · handling class imbalance · interpreting model metrics in a safety-critical context.

## Repository Structure

```
├── notebooks/
│   └── car_crash_survival_model.ipynb
├── reports/
│   └── Car_Crash_Business_Report.pdf
├── screenshots/
│   ├── notebook_chart_01.png … notebook_chart_10.png
│   └── report_page_01.png … report_page_04.png
└── README.md
```

---

*PGP in Data Science & Business Analytics · Great Lakes Executive Learning / The University of Texas at Austin, McCombs School of Business. Content drawn from the project notebook, business report and course transcript.*
