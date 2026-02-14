# Predicting Recessions

**Machine learning classification for recession forecasting across 17 economies (1870–2016).**

> **Source Available** — This repository is published for portfolio review and educational reference only. See [LICENSE](LICENSE) for terms.

---

## Overview

Trains classification models on 146 years of macroeconomic data spanning 17 national economies to predict the probability of an approaching recession. The core insight is that false negatives (failing to predict a recession) carry asymmetric downside — estimated at 50–100% asset loss — versus false positives (unnecessary hedging) at ~10% cost. The evaluation metric is designed accordingly.

**Best model: Logistic Regression with regularization and balanced class weights — 75% accuracy, F-beta(7) score of 0.673 on held-out data (2002–2015).**

## Technical Architecture

### Data Pipeline
- **Source**: Schularick & Taylor (2012) macroeconomic dataset — 2,499 country-year observations across 17 economies
- **Feature Engineering**: 44 raw features reduced to 13 core predictors after correlation analysis and domain filtering
- **Class Imbalance**: 94.7% non-recession vs. 5.3% recession — handled via balanced class weights (outperformed oversampling)
- **Temporal Split**: Train (1870–1971) / Test (1972–2001) / Holdout (2002–2015) — no future data leakage

### Key Features
| Feature | Type |
|---|---|
| Real GDP per capita | Continuous |
| Yield curve spread | Continuous |
| Stock prices | Continuous |
| Debt-to-GDP ratio | Continuous |
| Current account balance | Continuous |
| Crude oil prices | Continuous |
| Population | Continuous |
| USD exchange rate | Continuous |
| Financial crisis indicator | Binary |

### Models Evaluated
| Model | F-beta(7) Holdout | Notes |
|---|---|---|
| **Logistic Regression (regularized, balanced)** | **0.673** | Best generalization |
| Logistic Regression (oversampled) | 0.588 | Slightly worse on holdout |
| Random Forest (600 trees) | 0.121 | Poor temporal generalization |
| XGBoost | 0.0 | Failed to predict any positives |

### Evaluation Metric
**F-beta with beta=7** — weights recall 7x more than precision, reflecting the asymmetric cost structure of recession prediction. A missed recession is catastrophic; a false alarm is a manageable hedge.

### Stack
- **scikit-learn** — Logistic regression, Random Forest, preprocessing, cross-validation
- **XGBoost** — Gradient boosting (evaluated, underperformed)
- **imbalanced-learn** — Oversampling strategies
- **pandas / NumPy** — Data manipulation
- **matplotlib / seaborn** — Visualization
- **Tableau** — Interactive dashboards

## Key Findings

1. **Regularized logistic regression** outperforms tree-based models for macroeconomic time series — captures relative relationships that generalize across time periods
2. **Balanced class weights** outperform oversampling for this problem — oversampling slightly improved training scores but reduced holdout performance
3. **Tree-based models** (Random Forest, XGBoost) suffer from absolute year-value splits that fail to generalize to future periods
4. Applied to 2018 data, the logistic model predicted **98.6% recession probability** (trade war period) vs. Random Forest at 52%

## Legal Notice

Copyright (c) 2019-2026 Clarence Stephen. All rights reserved.

This repository is **source available**, not open source. Viewing is permitted for educational and portfolio review purposes. Commercial use, redistribution, and derivative works are prohibited without written authorization. See [LICENSE](LICENSE) for full terms.
