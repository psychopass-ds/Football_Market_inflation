# Football Market inflation

## Overview
This project explores how well on-pitch performance statistics explain Premier League player market values, comparing a **Linear Regression** baseline against a **Random Forest** model. It serves as the foundation for a broader objective: understanding how individual player valuations shift over time relative to league-wide "market inflation."

---

## Why This Project
Player market values are often treated as intuitive or reputation-driven, but they represent a classic pricing problem—a market assigning monetary value to a player based on measurable output, age, and positional role, layered with external variables like transfer hype, contract duration, and scarcity. 

This project tests two core ideas:
1. **Explainability:** How much of a player's valuation can be explained strictly by performance data, and where does that explanation hit a wall?
2. **Market Baseline:** Establishing a reliable single-season valuation model as a reference point to eventually track valuation trends over time—similar to evaluating asset pricing against a market index.

---

## Key Results

| Model | RMSE | R² Score |
| :--- | :---: | :---: |
| **Linear Regression** | *1.847380e+07* | *0.275184* |
| **Random Forest** | *1.483286e+07* | *0.532733* |

### Key Findings

* **Non-Linear Dynamics:** Random Forest significantly outperformed Linear Regression, demonstrating that market value relies on non-linear patterns and feature interactions—for instance, the way age impacts value varies sharply depending on a player's position or career stage.
* **Unexplained Variance (~47%):** Even with ensemble decision trees, nearly half of the variance in market values remains unexplained by on-pitch statistics alone.
* **The Off-Pitch Gap:** The remaining gap confirms that transfer market pricing is heavily driven by factors outside standard match metrics—including contract length, brand marketability, commercial value, and media hype.

## Part 2: Uncertainty Bands (Quantile Regression)

Instead of a single point estimate, three `GradientBoostingRegressor(loss='quantile')`
models (alpha = 0.1, 0.5, 0.9) give each player a valuation range. They use
the same train/test split as Part 1 and are trained on the log1p target.
Predictions are converted back to euros with `expm1`, which is valid because
quantiles are preserved under monotonic transformations (means are not).

![Valuation bands vs actual value](images/bands_plot.png)

| Check | Result |
|---|---|
| Calibration, 10th percentile | 13.4% of actuals below the line (target 10%) |
| Calibration, 50th percentile | 47.0% (target 50%) |
| Calibration, 90th percentile | 83.5% (target 90%) |
| Quantile crossing (pred_50 ≥ pred_90) | 5 of 351 players (1.4%) |
| Median below actual, top 10% by value | 30 of 35 players (85.7%) |

**Main finding:** the bands widen for expensive players, as hoped, but they
are too low at the top. The 90th percentile line under-covers the upper tail,
and the median underpredicts most top-10% players.

* ## Limitations & Future Work

### Limitations

* **Single-League Scope:** Trained exclusively on Premier League data. Other leagues in the source dataset lacked sufficient player-season samples (~15–20 per season), making cross-league inflation comparisons unfeasible without broader data.
* **Unexplained Variance (~47%):** The Random Forest captures ~53% of value variance. The missing signal is structural rather than statistical—driven by off-pitch variables like contract duration, transfer hype, brand marketability, and injury history.
* **Data Thinness at Career Extremes:** Players aged 30+ are underrepresented in the sample, leading to higher prediction variance and lower reliability for veteran players compared to those in their 20s.
* The bands are
least reliable for high-value players, probably because reputation, contract
and scarcity are not in the feature set.

---

### Future Work

1. **League Inflation Index:** Construct a season-by-season baseline (e.g., median value by position group) to track player valuation movements relative to macro market inflation over time.
2. **Cross-League Expansion:** Incorporate balanced multi-league datasets to test whether performance-to-value models hold consistently across different European leagues.
