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

* ## Limitations & Future Work

### Limitations

* **Single-League Scope:** Trained exclusively on Premier League data. Other leagues in the source dataset lacked sufficient player-season samples (~15–20 per season), making cross-league inflation comparisons unfeasible without broader data.
* **Unexplained Variance (~47%):** The Random Forest captures ~53% of value variance. The missing signal is structural rather than statistical—driven by off-pitch variables like contract duration, transfer hype, brand marketability, and injury history.
* **Data Thinness at Career Extremes:** Players aged 30+ are underrepresented in the sample, leading to higher prediction variance and lower reliability for veteran players compared to those in their 20s.

---

### Future Work

1. **Uncertainty Bands:** Implement quantile regression (10th, 50th, and 90th percentiles) to output a valuation range rather than a single point prediction, reflecting real-world financial risk modeling.
2. **League Inflation Index:** Construct a season-by-season baseline (e.g., median value by position group) to track player valuation movements relative to macro market inflation over time.
3. **Cross-League Expansion:** Incorporate balanced multi-league datasets to test whether performance-to-value models hold consistently across different European leagues.
