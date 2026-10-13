# A/B Test: Cookie Cats Gate Placement

## Overview
An A/B test analyzing whether moving the first gameplay gate in the mobile game 
Cookie Cats from level 30 to level 40 affects player retention and engagement.

## Dataset
- Source: [Cookie Cats dataset, Kaggle](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing)
- ~90,189 users, randomly split into two groups: gate_30 and gate_40
- Columns: userid, version, sum_gamerounds, retention_1, retention_7

## Goal
Test whether moving the gate from level 30 to level 40 changes:
1. 1-day retention
2. 7-day retention
3. Number of game rounds played

## Methods
- **Data cleaning:** removed one extreme outlier (49,854 rounds) that was 
  skewing the game rounds distribution
- **Proportion z-test** for retention_1 and retention_7, since these are 
  binary (yes/no) outcomes
- **Bootstrap resampling** (2,000 iterations) to build a 95% confidence 
  interval for the retention_7 difference, as a cross-check on the z-test
- **Mann-Whitney U test** for sum_gamerounds, since the data is heavily 
  right-skewed (not appropriate for a t-test)

## Results
| Metric | gate_30 | gate_40 | p-value | Significant? |<img width="1050" height="600" alt="retention_chart" src="https://github.com/user-attachments/assets/9922cb97-2094-4256-beba-45f5054d2f2f" />

| 1-day retention | 44.82% | 44.23% | 0.0739 | No |
| 7-day retention | 19.02% | 18.20% | 0.0016 | Yes |
| Game rounds (Mann-Whitney) | median 17 | median 16 | 0.0509 | No (borderline) |

Bootstrap 95% CI for the 7-day retention difference (gate_30 − gate_40): 
[0.0032, 0.0132] — excludes 0, confirming the result.

## Recommendation
Keep the gate at level 30. Moving it to level 40 did not improve 1-day 
retention and significantly reduced 7-day retention.

## Limitations
- Only 1-day and 7-day retention were measured
- The effect size is small (~0.8 percentage points)
- No revenue data, so business impact isn't estimated
- Two retention metrics were tested; a stricter significance threshold 
  (0.025 each) would still keep the 7-day result significant

## Tools
Python, pandas, numpy, scipy, statsmodels, matplotlib

## How to run
1. Clone this repo
2. Install dependencies: `pip install pandas numpy scipy statsmodels matplotlib`
3. Open `ab_test_cookie_cats.ipynb` in Jupyter
