# Tanzania Water Wells — Presentation Script
## 9-Slide Non-Technical Deck

---

### SLIDE 1 — Title
**Title:** Predicting Water Pump Conditions in Tanzania
**Subtitle:** A Machine Learning Approach to Proactive Maintenance
**Your name, team members, date: May 2026**

---

### SLIDE 2 — Overview
**Headline:** Tanzania has 74,000 water points. 46% need attention.

**3 bullets:**
- 38% of pumps are completely non-functional
- 7% are working but will fail soon without repair
- Manual inspection of every pump is too expensive and slow

**Speaker note:** Open with the human impact — millions of Tanzanians
rely on these pumps for drinking water. Connect the data problem to
the human problem immediately.

---

### SLIDE 3 — Business Problem
**Headline:** The Ministry needs to act before pumps fail — not after.

**Visual:** Simple before/after diagram
- LEFT box: "Reactive approach — inspect after failure — costly delays"
- RIGHT box: "Predictive approach — flag at-risk pumps — targeted action"

**1 sentence:** We built a model that predicts which pumps are at risk
using data the Ministry already collects.

---

### SLIDE 4 — The Data
**Headline:** 59,400 pumps. 40 features. 3 possible conditions.

**Visual:** Insert images/eda_01_class_distribution.png

**3 bullets:**
- Source: Tanzania Ministry of Water via DrivenData
- Features include: pump age, location, water quantity, payment type
- Target: functional / needs repair / non functional

**Speaker note:** Explain class imbalance briefly — "only 7% need repair,
which makes them the hardest — and most important — to catch."

---

### SLIDE 5 — What the Data Reveals
**Headline:** Three patterns predict failure clearly.

**Visual:** Insert images/eda_03_categorical_vs_status.png
  (or images/eda_02_numeric_vs_status.png — whichever is clearer)

**3 bullets:**
- DRY quantity reading → almost always non-functional
- NEVER PAY payment type → significantly higher failure rate
- OLDER construction year → higher probability of failure

**Speaker note:** These three findings alone give field officers
actionable triage criteria even without running the model.

---

### SLIDE 6 — Our Approach
**Headline:** We tested 4 models. Here is what we found.

**Visual:** Simple 4-row table (no technical jargon):

| Attempt | What We Tried | Result |
|---------|---------------|--------|
| 1 | Simple baseline model | Too simple — misses patterns |
| 2 | Decision tree (full depth) | Memorized training data — unreliable |
| 3 | Decision tree (pruned) | Better balance — generalizes well |
| 4 | Random Forest (100 trees) | Best performance — our final model |

**Speaker note:** Use the analogy — "asking 100 experts to vote is
more reliable than asking one expert to decide alone."

---

### SLIDE 7 — Results
**Headline:** Our model correctly identifies pump condition
6-7 times out of 10.

**Visual:** Bar chart of Test Macro F1 for all 4 models
  (simple 4-bar chart, no axis jargon — just the model names
  and their scores as plain percentages)

**Key callout box:**
"The final model is 30% more accurate than random guessing
and catches pumps needing repair BEFORE they fully fail."

**Speaker note:** Translate F1 into plain language for a non-technical
audience. Never say "macro F1" — say "correctly identified across all
three categories."

---

### SLIDE 8 — Recommendations
**Headline:** 4 actions the Ministry can take starting now.

**4 bullets (large text, one per line):**
1. 🔍 TRIAGE — Prioritise old pumps with dry quantity readings
2. 🗺️ DEPLOY — Send maintenance teams to high-failure regions first
3. 💰 REFORM — Introduce community payment at never-pay waterpoints
4. 🔄 RETRAIN — Update the model annually with fresh survey data

---

### SLIDE 9 — Next Steps + Thank You
**Headline:** The model is ready. Here is what comes next.

**3 bullets:**
- Integrate predictions into field officer data collection tablets
- Expand model to predict time-to-failure (not just current status)
- Partner with NGOs to collect more "needs repair" labeled examples

**Closing line:** "Every pump we predict correctly is a community
that keeps its clean water access."

**Thank you. Questions?**

---

## Slide Style Rules (apply before exporting to PDF)
- Use a clean, professional template (Google Slides "Simple Light"
  or similar)
- Maximum 40 words of text per slide
- All chart images inserted at 80% width, centered
- Font size: titles 28pt minimum, body text 20pt minimum
- Color scheme: steelblue (#4682B4) as primary, tomato (#FF6347)
  as accent
- No slide should have more than 5 bullet points
