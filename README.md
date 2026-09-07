# Olist Order Growth & Review Score — Diagnosis

**Narender Kumar** · M.Sc. Mathematics, IIT Guwahati

An end-to-end diagnostic analysis of why order growth has flattened and why review
scores swing across sellers/regions for a Brazilian e-commerce marketplace, using the
public Olist dataset (~100,000 orders, 2016–2018).

## Problem Statement

Order growth has flattened mainly because new-customer acquisition has plateaued —
repeat customers hold steady at just **~3.0%** of orders every month (vs. a **20–30%**
industry benchmark), so a shift in repeat behavior alone cannot explain the slowdown.

Within that small repeat channel, **delivery delay** is the dominant driver of review
score (2.58-pt spread across delay buckets, vs. 1.16 for seller and 0.89 for category)
and measurably suppresses repeat purchases (2.53% repeat rate after a late first order
vs. 3.03% after on-time, p = 0.026 — statistically real, not noise). Decomposing delay
further, **carrier transit time** (corr. −0.30 with score) is the fixable root cause —
not seller handoff speed (corr. −0.08) — so the fix should target carrier performance
and at-risk-order recovery, not a vague "logistics" catch-all.

## Repository Structure

```
olist-growth-diagnosis/
├── README.md
├── data/
│   ├── 01_olist_dataset_core/
│   └── 01_olist_geolocation/
├── notebooks/
│   └── analysis.ipynb              # Full EDA, assumptions log, key findings
├── reports/
│   ├── action_plan.pdf             # Ranked recommendations + KPIs
│   └── growth_diagnosis_deck.pptx  # Findings summary deck
├── presentation/
│   └── pitch_deck.pptx             # Client-facing pitch deck
└── media/
    └── pitch_video_link.md         # Link to recorded pitch video
```

## Key Findings

1. **Growth is a new-customer problem, not a repeat-behavior problem.** Repeat
   purchase rate is flat at ~3.0%/month, far below the 20–30% industry benchmark.
2. **Delivery delay is the strongest driver of review score** — a 2.58-point spread
   across delay buckets, more than double the effect of seller (1.16) or category
   (0.89).
3. **Carrier transit time, not seller handoff, is the fixable root cause** of delay
   (corr. −0.30 vs. −0.08), and a late first order significantly cuts repeat odds
   (2.53% vs. 3.03%, p = 0.026).

*Note: growth also appears to flatten partly due to a data-collection cutoff
(Sep–Oct 2018 show only 16 and 4 orders respectively) — this is a dataset artifact,
not a business signal, and is excluded from the recommendations below.*

## Recommendations (Impact vs. Effort)

| # | Recommendation | Impact | Effort | Why it works |
|---|---|---|---|---|
| 1 | **Recover at-risk first-time buyers** — proactive delay alerts + win-back offer for customers whose first order arrives late | High | Low | Late first order cuts repeat odds significantly (2.53% vs 3.03%, p=0.026) — most targeted fix for growth flattening |
| 2 | **Tighten carrier SLAs** — renegotiate contracts / add backup carriers, track on-time performance by carrier and route | High | Medium | Carrier delay correlates far more with review score (−0.30) than seller handoff (−0.08) — the real, fixable root cause |
| 3 | **Tiered seller performance program** — quality SLAs focused on top-revenue sellers first | Medium | Low | Top 20% of sellers drive 82.7% of revenue — modest quality gains move the needle disproportionately |
| 4 | **Fix the pre-shipping funnel leak** — automated alerts on orders stuck in invoiced/processing/created status | Low | Low | 0.63% of orders never reach a carrier — small but pure, cheap-to-close loss |
| 5 | **Deprioritize** response-time & category-level fixes for now | Low | — | Response time to 1–2★ reviews already matches average (72.9 vs 75.5 hrs); category effect size is weakest of the three drivers |

## KPIs to Track Post-Implementation

| KPI | Current Baseline | Direction of Success |
|---|---|---|
| Repeat-purchase rate, on-time vs. late first order | 3.03% vs. 2.53% | Gap narrows toward 0 |
| Carrier on-time delivery rate (by carrier/route) | Corr. w/ score: −0.30 | Fewer late carrier legs |
| Avg. review score by delivery-delay bucket | 4.30 (Very Early) → 1.72 (Very Late) | Late-bucket scores rise |
| Orders stuck pre-shipping (never reach carrier) | 0.63% | Trend toward 0% |
| Overall repeat-purchase rate | 3.00% (benchmark ~20–30%) | Steady upward trend |

## Deliverables

- **Analysis Notebook** — full EDA with assumptions log, statistical tests, and
  charts backing each finding.
- **Action Plan (PDF)** — one-page ranked recommendations with impact/effort
  reasoning and KPIs.
- **Pitch Deck & Video** — client-facing summary structured as
  situation → actions → deliverables.

## Dataset

Public [Olist Brazilian E-Commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
(~100,000 orders, 2016–2018).
