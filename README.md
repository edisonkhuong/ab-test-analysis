# E-commerce Landing Page A/B Test

**Should we ship the new landing page? A rigorous statistical analysis of a 3-week experiment across 294K users — with a clear recommendation backed by math.**

![Dashboard](dashboard.png)

**[Interactive Tableau Dashboard](YOUR_TABLEAU_URL_HERE)**

---

## Executive Summary

An e-commerce retailer ran a 3-week A/B test comparing a redesigned landing page (treatment) against the existing version (control) to evaluate whether the new design improved conversion rate. After cleaning ~7,800 rows affected by an implementation bug and user duplicates, a two-proportion z-test found **no statistically significant difference** between the two designs (p = 0.1899). The observed point estimate was actually slightly negative — the new page converted 11.88% of visitors vs. 12.04% for the control.

**Recommendation: Do not ship the new landing page.**

At the observed point estimate, rolling out the new design would represent an estimated **-$395K in annual revenue impact** (assuming 5M annual visitors, $50 AOV). Beyond the revenue loss, shipping would consume engineering rollout resources and forgo the opportunity to test more promising variants. The experiment successfully did its job: it caught a no-lift change before engineering invested in a full rollout.

---

## Business Context

**Who is this analysis for?**

- **Product team** — deciding whether to ship the new design
- **Engineering leadership** — evaluating rollout resource allocation
- **CMO** — informing broader design strategy decisions

**What's at stake?**

- **5M+ annual visitors** on the landing page
- **~12% baseline conversion rate**
- **A 1pp lift would generate ~$2.5M/yr** in additional revenue (assuming $50 AOV)
- **A false positive (shipping a no-lift change)** costs engineering resources, introduces new bugs, and blocks the test pipeline for other candidate changes

---

## Methodology

| Step | Decision | Rationale |
|---|---|---|
| Data integrity: assignment vs. landing page | Removed 3,893 mismatched rows | Users assigned to one group but shown the wrong page — cannot determine which experience they had, so cannot attribute their behavior |
| Duplicate users (3,894 users appearing multiple times) | Kept first visit per user only | Preserves the statistical assumption of independent observations; the first visit reflects the user's initial exposure |
| Group balance check | 145,274 control vs. 145,310 treatment after cleaning | Randomization held — no systematic bias introduced during cleanup |
| Statistical test | Two-proportion z-test, two-sided, α = 0.05 | Standard test for comparing conversion rates between two groups |

---

## Findings

### Primary result
- **Control conversion rate:** 12.039% (17,489 / 145,274)
- **Treatment conversion rate:** 11.881% (17,264 / 145,310)
- **Absolute difference:** -0.158 percentage points
- **Z-statistic:** 1.311
- **P-value:** 0.1899
- **Verdict:** Not statistically significant at α = 0.05

### Secondary checks
- **Daily conversion rates** for the two groups weave together throughout the 21-day experiment, with no systematic gap — visual confirmation of the statistical result.
- **Cumulative sample sizes** grew evenly throughout, confirming randomization held through the cleaning process.

### Business translation

At the observed point estimate, extrapolated to 5M annual visitors at $50 AOV:

| Scenario | Annual revenue |
|---|---|
| Keep control (status quo) | $30.10M |
| Ship treatment at observed rate | $29.70M |
| **Net impact of shipping** | **-$395K** |

Even ignoring statistical significance, the point estimate is negative so there is no positive case for shipping the new design.

---

## Recommendation

**Do not roll out the new landing page.**

### Suggested next steps

1. **Investigate why the redesign didn't outperform** — qualitative research (user interviews, session recordings, heatmaps) may reveal which specific changes hurt vs. helped
2. **Test more dramatic variants** — incremental design changes often fail to move conversion; more aggressive redesigns (new hero imagery, restructured value proposition, different CTA language) may produce larger effect sizes
3. **If the new design has non-conversion benefits** (brand consistency, accessibility, mobile experience), those should be evaluated separately — but the redesign should not be shipped as a *conversion-improvement* change

---

## Stack

- **Python** (pandas, numpy) — data cleaning and manipulation
- **statsmodels** — two-proportion z-test, confidence intervals
- **matplotlib** — in-notebook visualizations
- **Tableau Public** — published interactive dashboard
- **Jupyter Lab** — analysis notebooks
