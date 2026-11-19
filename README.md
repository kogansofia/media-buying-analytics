# iGaming Media Buying Analytics — Q3 2026

A portfolio case study: one quarter of paid user acquisition for an in-house media buying team, analysed end to end — from data validation to a gated test plan for the next launch cycle.

**Stack:** Python · pandas · NumPy · Matplotlib · seaborn · Jupyter
**Data:** real, anonymized — 266 launches in 120 campaigns, 4 buyers, 4 GEOs (BR, CL, MX, PE), 5 ad platforms, about 8,000 daily acquisition cohorts

## The business question

The team buys traffic on Meta, Google and TikTok and is paid under two models:

- **CPA** — a fixed fee per approved first-time depositor (FTD).
- **Conditional spend fee** — ad spend is reimbursed, and a 30% fee is earned only if 30-day deposits reach 60% of spend within an offer / GEO / month group.

Where does advertising spend earn its return, where does the data mislead, and how can a fixed budget be redistributed without losing paying users?

## Key findings

**1. One landing page drags down CPA results.**
LP_4529 converts registrations into first deposits worse than the other landing pages of the same campaign in 37 of 38 campaigns (median 20.8% vs 34.5%; exact sign test p ≈ 3 × 10⁻¹⁰). It carries 16.4% of CPA spend at a 33% return, against 65–112% for the other five pages. At their average return, the same spend would have produced about USD 22K more in the quarter — an upside benchmark, not a forecast.

**2. Facebook reporting discrepancies are concentrated in September 4–7.**
On September 4–7 the platform reported 24 FTDs against 82 in product data (−71%) and 85 registrations against 251 (−66%). Outside that window, FTD totals match exactly.

**3. The headline return is biased by missing data.**
Complete records show a 56.7% return after ad costs. The excluded 7% of spend belongs entirely to the lower-return conditional model, so a sensitivity scenario puts the full quarter near 54.8%. The shift is uneven across buyers: B_1 moves from 66.7% to 62.2%.

**4. Buyer rankings depend on landing mix.**
Without LP_4529 traffic, B_3's CPA return rises from 63.8% to 79.4% — nearly level with B_1 at 81.7%.

**5. Half of the spend depends on a deposit threshold.**
Conditional offers account for 48.9% of spend. The September OFF_6137 / CL group sits at a deposit-to-spend ratio of 0.71 against the 0.60 threshold, with about USD 2.1K of fee at stake.

## Recommendations

1. **Audit LP_4529** and run a randomized landing routing test at fixed spend.
2. **Repair Facebook event reporting** for September 4–7, recover seven missing cost records, and monitor conditional settlement headroom with an early-warning level of 0.75.
3. **Run a staged, budget-neutral replication test.** For the next launch cycle, reserve an illustrative amount equal to 10% of historical CPA Search spend excluding LP_4529 (about USD 3.6K, 0.75% of recorded quarterly spend), split equally among BR, PE and CL CPA TikTok. Confirm the planned funding allocation and delivery capacity before releasing funds. Release each allocation in thirds: BR requires a review of rising approved-FTD costs, CL requires both cost and repeat-deposit reviews, and PE requires a repeat-deposit review. Funds remain reserved when release conditions are unmet.

## What the notebook covers

| Step | What is done |
|---|---|
| Data validation | Keys, 12 exact duplicate ad rows, accounting identities, payout rules rechecked for 29 resolved settlement groups |
| Metric design | Every metric has an explicit record scope; one reusable `summarize()` function produces all tables |
| Missing-data sensitivity | Scenarios for seven missing cost records and four pending settlement groups |
| Segment analysis | Buyers in shared segments, GEO × platform economics, upper funnel (CTR, CPC) |
| Landing diagnostic | Paired within-campaign comparison on shared dates, sign test, campaign bootstrap (95% interval −15.8 to −9.8 pp) |
| Launch age | Cost per approved FTD rises from USD 46.91 in days 0–6 to USD 51.53 after day 30 on a common set of launches |
| Feasibility | Actual spend is about 25% of stated daily budgets; only 21 of 266 launches have observed activity on the last day |
| Test plan | Two separate interventions, an expense ledger, effect scenarios, and pause rules |

## Data

The dataset is real media buying data, anonymized before publication. Buyers, campaigns, offers, brands and landing pages appear only as coded identifiers, and all figures are daily aggregates — the files contain no player-level or personal data.

| File | Grain | Rows | Content |
|---|---|---|---|
| `campaigns.csv` | launch | 266 | Launch settings and offer payment terms |
| `ad_stats_daily.csv` | launch × day | 7,983 | Impressions, clicks, spend, platform-reported events |
| `product_cohorts_daily.csv` | launch × acquisition day | 7,978 | Registrations, FTDs, 30-day deposits, receivables |

## Repository structure

| File | Purpose |
|---|---|
| `Media_Buying_Analytics.ipynb` | Analysis, saved tables and charts |
| `campaigns.csv` | Launch settings and offer terms |
| `ad_stats_daily.csv` | Daily advertising statistics |
| `product_cohorts_daily.csv` | Daily product acquisition cohorts |
| `README.md` | Project overview and run instructions |

## How to run

Tested with Python 3.9.7, pandas 1.3.4, NumPy 1.20.3, Matplotlib 3.4.3 and seaborn 0.11.2.

```bash
# Install the tested library versions
pip install pandas==1.3.4 numpy==1.20.3 matplotlib==3.4.3 seaborn==0.11.2 notebook

# Open the notebook; the three CSV files must be in the same folder
jupyter notebook Media_Buying_Analytics.ipynb
```

Then choose **Run All**. The notebook reads the three CSV files and reproduces every table and chart.

## Limitations

- The data covers one team and one quarter; generalization to other teams, markets or periods requires further validation.
- All comparisons are observational. The landing effect needs a randomized test before any causal claim.
- Financial results are accrued amounts after advertising costs; other operating costs are out of scope.
- Many segments contain only one or two campaigns.
