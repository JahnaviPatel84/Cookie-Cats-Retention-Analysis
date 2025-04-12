# Cookie Cats Retention Analysis — A/B Testing + Predictive Modeling

This project explores whether the timing of in-game ads impacts player retention. We treat this as a product decision-making problem: does showing an ad at level 30 vs. level 40 change how long players stick around?

The work follows a typical lifecycle for experimentation and modeling at scale — combining statistical testing, predictive modeling, and interpretability.

---

## Goals

- Quantify the impact of ad placement (level 30 vs level 40) on retention
- Build a predictive model for 7-day retention
- Interpret key drivers behind retention behavior using SHAP

---

## Summary of Findings

- There's **no meaningful difference** in Day 1 retention between the two groups (p > 0.05)
- There's a **significant drop** in Day 7 retention when ads are shown earlier (p < 0.01)
- Users who play more rounds before Day 1 are much more likely to return on Day 7
- SHAP confirms this: `sum_gamerounds` dominates in feature influence, while `version` has modest negative impact


