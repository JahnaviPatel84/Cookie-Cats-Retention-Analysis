# Cookie Cats Retention Analysis: A/B Testing, Predictive Modeling, and Behavioral Insight

This project analyzes how in-game ad placement affects player retention in a mobile game. Specifically, we assess whether showing an ad after level 30 (Group A) or level 40 (Group B) impacts Day 1 and Day 7 retention.

We treat this as a product experimentation case, combining hypothesis testing with machine learning and model interpretability to guide potential design decisions.

---

## Project Objectives

- Evaluate the causal impact of ad placement timing on player retention
- Build a predictive model to estimate likelihood of 7-day retention
- Use SHAP to interpret model decisions and surface key behavioral drivers
- Quantify behavioral thresholds to inform segmentation or personalization strategy

---

## Dataset

- ~90,000 user records from the mobile game *Cookie Cats*
- Columns include:
  - `userid`
  - `version` (gate_30 or gate_40)
  - `sum_gamerounds` (gameplay activity before Day 1)
  - `retention_1`, `retention_7` (binary retention labels)
- Additional features generated in this project:
  - `version_encoded` (for modeling)
  - `model_pred` (XGBoost probability)
  - `shap_gamerounds`, `shap_version` (SHAP explanations)
  - `engagement_level` (binned early activity levels)

---

## Methodology

1. **A/B Testing**  
   Two-sample t-tests to compare Day 1 and Day 7 retention across experiment groups.

2. **Effect Size Estimation**  
   Calculated Cohen’s d to measure practical impact in addition to statistical significance.

3. **Predictive Modeling**  
   Trained an XGBoost classifier to predict 7-day retention using early gameplay metrics.

4. **Explainability**  
   Used SHAP to quantify feature importance and explain individual predictions.

5. **Segmentation**  
   Binned users by gameplay behavior and analyzed retention patterns across segments.

---

## Results Summary

### A/B Test

- **Day 1 retention**: No statistically significant difference (p > 0.05)
- **Day 7 retention**: Statistically significant difference (p < 0.01), but **Cohen’s d = 0.021**, indicating a **very small practical effect**

### Predictive Modeling

- **AUC (XGBoost)**: ~0.89
- `sum_gamerounds` was the dominant predictor of retention
- SHAP confirmed that user engagement drives model decisions more than ad group

---

## Retention by Early Gameplay: Behavioral Threshold Analysis

To better understand the relationship between early engagement and long-term retention, we grouped users based on the number of game rounds played before Day 1. We then calculated the 7-day retention rate within each group.

| Engagement Level (Game Rounds) | Users  | Retained | Retention Rate |
|-------------------------------|--------|----------|----------------|
| 0–5                           | 21,725 | 259      | 1.2%           |
| 6–10                          | 12,594 | 319      | 2.5%           |
| 11–25                         | 19,204 | 1,337    | 6.9%           |
| 26–50                         | 13,604 | 2,227    | 16.9%          |
| 51–100                        | 8,753  | 3,079    | 35.2%          |
| 101–250                       | 2,893  | 1,795    | 63.6%          |
| 250+                          | 3,720  | 3,303    | 88.7%          |

### Interpretation

- The majority of users churn quickly: players who engage in fewer than 10 rounds have a <3% chance of returning by Day 7.
- There is a significant behavioral inflection point between 25 and 50 rounds. Players in this range are ~6x more likely to return than those in the lowest bin.
- Users playing more than 100 rounds retain at levels above 60%, indicating high product-market fit.
- The top-tier segment (250+ rounds) has an exceptionally high retention rate (~89%), suggesting a small but extremely committed user group likely responsible for a disproportionate share of long-term engagement and potential revenue.

---

## Product Recommendations

- Move the ad gate to **level 40** to avoid penalizing users who are still ramping up engagement.
- Focus retention efforts on users with 25–100 rounds — this is the conversion zone where incremental lifts are most likely.
- Avoid targeting users under 10 rounds for retention or monetization — their churn rate is too high to justify cost.
- Consider real-time modeling to assign users into engagement tiers and adjust ad timing, tutorial content, or bonus structures accordingly.
