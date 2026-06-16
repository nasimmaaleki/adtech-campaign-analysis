[adtech_README.md](https://github.com/user-attachments/files/29020527/adtech_README.md)
# Ad-Tech Campaign Performance & A/B Testing Analysis
### Business Analyst Portfolio Project | Nasim Maleki

[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)](https://python.org)
[![SQL](https://img.shields.io/badge/SQL-SQLite-orange?logo=sqlite&logoColor=white)]()
[![Stats](https://img.shields.io/badge/Statistics-A%2FB%20Testing-purple)]()
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen)]()

---

## Business Context

As a BI Analyst at an ad-tech company, the task was to analyse campaign performance for the first half of the year (H1) and provide data-driven recommendations to improve ad performance over the next six months.

The project is split into two sections:

1. **Campaign Performance Analysis** — using SQL and Python to evaluate which campaigns and user segments deliver the best return.
2. **A/B Testing** — evaluating whether a new ad format should replace the existing one, using proper statistical testing.

---

## Dataset

The data models a typical ad-tech funnel — views, clicks, and installs across multiple campaigns and user segments.

| Table | Description |
|-------|-------------|
| Campaigns | Campaign budget, cost per install, start and end dates |
| CampaignViews | Individual ad view events with timestamp and user |
| CampaignClicks | Click events with timestamp and user |
| CampaignInstalls | Install events with timestamp and user |
| Users | User demographics — age group, gender, country |

---

## Section 1 — Campaign Performance Analysis

### Business Questions
- Which campaigns outperformed, and which underperformed?
- What budget adjustments should the company make?
- Which user segments (age, gender, country) drive the best results?

### Approach
Data was loaded into Python and structured into SQLite tables. Performance was analysed using SQL queries with **CTEs** to aggregate views, clicks, installs, spend, and active days per campaign. Custom KPIs were calculated to enable fair comparison:

- **Cost Per Install (CPI)** — total spend ÷ installs
- **IPM (Installs Per Mille)** — installs per 1,000 views, by user segment

### Key Findings

**Campaign efficiency varies significantly.** Ranking campaigns by cost per install revealed clear top and bottom performers:

- **Top performers** (lowest CPI): Campaign 8, Campaign 11, Campaign 4 — recommended for budget increase
- **Medium performers**: Campaign 1, Campaign 9, Campaign 20 — maintain budget and monitor
- **Underperformers** (highest CPI): Campaign 2, Campaign 14, Campaign 12 — recommended for budget reduction or pause

**Segment performance is geographically concentrated.** IPM analysis by segment showed:

- Highest IPM in the **USA and Germany** (average IPM ~250–300), particularly in the **18–44 age range**
- Lowest IPM in **Italy and Austria** (under 50 average IPM)
- **No significant difference between male and female** users in install rate
- The single strongest segment: **users aged 18–24 in the USA**

**Campaign effectiveness is country-dependent.** Campaign 9 performed best in the USA and Germany, while Campaign 18 was the most efficient in France — indicating that campaign strategy should be localised rather than applied uniformly.

### Visualizations

**IPM by Age Group, Gender, and Country**
![IPM by segment](charts/ipm_by_age_gender_country.png)

**IPM Heatmap — Age Group vs Country**
![IPM heatmap](charts/ipm_age_country_heatmap.png)

**Average IPM per Campaign**
![IPM per campaign](charts/ipm_per_campaign.png)

**IPM by Campaign and Country**
![IPM campaign country](charts/ipm_campaign_country_heatmap.png)

---

## Section 2 — A/B Testing

### Objective
Evaluate whether a new ad format should replace the current format, by comparing user click-through behaviour between a control group (old format) and a target group (new format).

### Part 1 — Sampling Representativeness Check

Before evaluating results, the validity of the 20/80 sampling split was assessed. Several imbalances were identified between the control and target groups:

- **No user overlap** between groups — good, confirms clean separation
- **Gender distribution** was imbalanced (confirmed via chi-square test)
- **Country distribution** was uneven — some countries appeared in only one group
- **ARPU** was significantly higher in the target group — a potential confounder
- **Age distribution** was balanced — no concern

**Conclusion:** While a 20/80 split is acceptable from a risk-management perspective, the sampling was not fully representative. The imbalances in ARPU, country, and gender suggest results should be interpreted cautiously and randomisation improved.

### Part 2 — Statistical Significance Testing

The observed results were:

| Group | Format | Impressions | Clicks | CTR |
|-------|--------|-------------|--------|-----|
| Control | Old | 4,000 | 500 | 12.5% |
| Target | New | 1,000 | 140 | 14.0% |

A **two-proportion z-test** was run to determine whether the 1.5 percentage point CTR difference was statistically significant.

**Result:** z-statistic = −1.27, p-value = 0.204

Since the p-value is well above 0.05, the difference is **not statistically significant**. The 1.5pp lift cannot be distinguished from random chance.

### Part 3 — Power Analysis

To quantify how much data would be needed for a reliable conclusion, a power analysis was run at 80% power and 5% significance:

**Result:** Approximately **8,013 impressions per group** are required to reliably detect a 1.5pp lift.

The actual test used only 1,000 impressions in the target group — roughly **8 times fewer** than needed. This confirms the experiment was underpowered.

### Final Recommendation

The new ad format shows early promise but the current test **cannot support a confident decision to switch**. Before rolling out the new format, the company should extend the experiment until each group reaches at least ~8,000 impressions. If the 1.5pp lift persists at that scale, the switch would be statistically justified.

This analysis demonstrates the importance of validating results statistically rather than acting on raw percentage differences alone.

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| Python (Pandas, NumPy) | Data loading, transformation, KPI calculation |
| SQL (SQLite) | Campaign aggregation using CTEs and multi-table joins |
| Matplotlib, Seaborn | Data visualization (FacetGrid, heatmaps, bar charts) |
| SciPy, Statsmodels | Chi-square test, two-proportion z-test, power analysis |

---

## Repository Structure

```
adtech-campaign-analysis/
│
├── README.md
├── adtech_analysis.ipynb        # Full analysis notebook
│
├── data/
│   └── Test_Task_Data.xlsx      # Source dataset
│
└── charts/
    ├── ipm_by_age_gender_country.png
    ├── ipm_age_country_heatmap.png
    ├── ipm_per_campaign.png
    └── ipm_campaign_country_heatmap.png
```

---

## Key Takeaway

This project demonstrates the full analytical workflow expected of a Business Analyst: structuring raw data with SQL, calculating meaningful KPIs, segmenting performance by user demographics, and — critically — applying rigorous statistical testing to separate real effects from noise before making business recommendations.

---

*Nasim Maleki · Business Analyst · Bremen, Germany*
*[LinkedIn](https://linkedin.com/in/nasim-maaleki) · nasimmaleki.official@gmail.com*
