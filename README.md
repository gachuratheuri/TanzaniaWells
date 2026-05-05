# Tanzania Water Wells — Pump Condition Classification

## Overview
This project predicts the operational condition of rural water pumps across Tanzania
(`functional`, `functional needs repair`, or `non functional`) so the Ministry of Water
can shift from reactive to proactive maintenance. We follow the CRISP-DM framework
end-to-end on the DrivenData "Pump It Up" dataset and iterate through four classifiers,
landing on a **Random Forest (100 trees, max_depth=15)** as the final model with a
**test macro F1-score of 0.6677**. Evaluation uses a held-out 20% stratified test set
because the target is class-imbalanced.

## Business and Data Understanding

### Stakeholder
Tanzanian Ministry of Water and rural NGOs.

### Business Problem
Tanzania has more than 74,000 rural water points and a substantial share are
non-functional or in need of repair. Manually inspecting every pump is infeasible, so
maintenance crews currently react only after pumps fail. A classification model that
flags at-risk pumps from existing administrative and geographic data lets the Ministry
target inspections, reduce downtime, and maximise water access at lower cost.

### Dataset
- **Source:** DrivenData "Pump It Up: Data Mining the Water Table" competition
  (https://www.drivendata.org/competitions/7/)
- **Files:**
  - `training_values.csv` — 59,400 rows, 40 features (X)
  - `training_labels.csv` — 59,400 rows, target `status_group` (y)
  - `test_values.csv` — 14,358 rows (no labels, competition submission)
- **Target:** `status_group` with 3 classes
  (`functional` 54.3%, `non functional` 38.4%, `functional needs repair` 7.3%)

### Why This Dataset
The dataset combines geographic, administrative, mechanical, and managerial features
across nearly 60,000 real waterpoints, which closely mirrors the operational data the
Ministry already collects. The three-class target maps directly to the maintenance
decisions stakeholders need to make, making it an ideal multiclass classification
problem for a data-driven triage system.

## Modeling

Four models were built iteratively following the CRISP-DM framework:

| # | Model | Train F1 | Test F1 | Overfit Gap | Selected |
|---|-------|----------|---------|-------------|----------|
| 1 | Logistic Regression (Baseline) | 0.4454 | 0.4423 | 0.0031 | |
| 2 | Decision Tree (Default depth) | 0.9844 | 0.6487 | 0.3357 | |
| 3 | Decision Tree (Tuned: max_depth=15) | 0.6281 | 0.5766 | 0.0515 | |
| 4 | Random Forest (100 trees) | 0.7651 | 0.6677 | 0.0974 | ✅ FINAL |

**Iteration rationale:**
- Model 1 → 2: Logistic Regression underfits (F1=0.44). Decision Tree
  captures non-linear patterns but severely overfits (gap=0.34)
- Model 2 → 3: Pruning (`max_depth=15`) reduces overfit gap to 0.05
  while maintaining higher test F1 than baseline
- Model 3 → 4: Random Forest aggregates 100 pruned trees via majority
  vote, reducing variance further — justified by remaining overfit gap

All models use `class_weight='balanced'` to handle the minority "functional needs repair"
class, and are evaluated on the same stratified 80/20 holdout split.

## Evaluation

**Primary metric: Macro F1-score**
Chosen because the target is class-imbalanced — "functional needs repair"
is only 7.3% of all pumps. Accuracy is misleading on imbalanced data:
a model that always predicts "functional" achieves 54.3% accuracy but
identifies zero pumps needing repair.

**Final model test macro F1: 0.6677** (Random Forest, 100 trees,
`max_depth=15`). The model correctly classifies approximately 6–7 out of
10 pumps across all three condition categories on data it has never seen.

**Top predictive features** (from feature importance analysis):
- `quantity` — dry reading is the strongest single signal of failure
- `gps_height` — altitude correlates with gravity-fed reliability
- `construction_year` — older pumps fail more frequently
- `latitude` / `longitude` — geographic clustering of failures

**Key limitation:** The minority class "functional needs repair" achieves
the lowest per-class F1. These misclassifications delay maintenance until
full failure — the most costly real-world outcome. Additional labeled
examples of this class would directly improve model performance.

## Conclusion

Tanzania's water pump failure problem is predictable from existing
administrative data. Our Random Forest model identifies pump condition
with a macro F1-score of **0.6677**, meaningfully above the 0.4423 baseline,
enabling the Ministry of Water to shift from reactive to proactive maintenance.

**Top 4 recommendations:**
1. **Triage by age + quantity** — pumps older than the median
   construction year showing dry quantity readings are the
   highest-risk combination
2. **Geographic deployment** — concentrate maintenance teams in
   the regions with the highest density of predicted non-functional
   pumps identified in the geographic map
3. **Payment scheme reform** — never-pay waterpoints have
   significantly higher failure rates; community payment
   incentivizes local maintenance accountability
4. **Annual retraining** — refresh the model yearly with new
   survey data to maintain prediction accuracy as pump conditions
   change over time
