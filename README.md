Objective:
Test how well standard market risk tools worked during the 2007-2008 crisis, using an equal-weight portfolio of JPM, AIG, GS, MS, AXP and C. The project asks three questions:
Did daily rebalancing help or hurt compared with buy & hold?
Did Normal-distribution VaR and Expected Shortfall capture the real tail risk?
Did diversification protect the portfolio, and where did the risk come from?

Process
Data: downloaded daily adjusted close prices from Yahoo Finance (Jan 2007 to Dec 2008, 502 daily returns), plus the Dow Jones index as the beta benchmark.
Performance: built the daily rebalanced and buy & hold portfolios from $10,000. Calculated total returns, volatility, beta, maximum drawdown and the worst single day.
Tail risk: calculated 99% VaR and ES per stock, and for the portfolio under five methods: historical, parametric Normal, Monte Carlo Normal, fitted Student's t, and Monte Carlo t.
Stress event: looked at every stock's return on the Lehman bankruptcy day (Sep 15, 2008).
Extensions: added rolling 60-day correlation and risk contribution per stock.
Reporting: built a dashboard and a written report with findings, corrections to the original report, and limitations.

Techstack
Language: Python.
Libraries: pandas, NumPy, SciPy (Student's t fit, Monte Carlo), matplotlib, yfinance.
Environment: Jupyter Notebook.
Data source: Yahoo Finance.

Results

Portfolio performance ($10,000 invested, Jan 2007 to Dec 2008)

| Metric | Value |
|---|---|
| Daily rebalanced return | -74.64% ($10,000 to $2,536) |
| Buy & hold return | -70.04% ($10,000 to $2,996) |
| Maximum drawdown (rebalanced) | -82.70% |
| Worst single day | -20.11% (Sep 15, 2008, Lehman) |

Intepretation:
Daily rebalancing ended 4.60 percentage points below buy & hold, which is $460 less on $10,000.
Both portfolios lost more than two-thirds of their value: -74.64% and -70.04%.
The rebalanced portfolio fell -82.70% from its peak at the worst point.
The worst single day cost -20.11%, caused mainly by AIG falling -60.79% on Lehman day.

Portfolio 99% VaR and Expected Shortfall

| Method | 99% VaR | 99% ES | ES vs historical |
|---|---|---|---|
| Historical | -13.99% | -17.09% | 1.00x (baseline) |
| Parametric Normal | -9.94% | -11.36% | 0.66x |
| Monte Carlo Normal | -9.77% | -11.29% | 0.66x |
| Student's t (df = 1.80) | -14.14% | -31.86% | 1.86x |
| Monte Carlo t | -13.29% | -27.35% | 1.60x |

Intepretation:
Normal models understated VaR by about 4 percentage points(-9.94% vs -13.99%), roughly 29% too low.
Normal models understated ES by about 5.7 percentage points (-11.36% vs -17.09%), roughly 33% too low.
The fitted Student's t matched historical VaR almost exactly (-14.14% vs -13.99%), a 0.15 point difference).
The Student's t overshot ES by 14.77 percentage points, making it 1.86x the historical figure. Monte Carlo t overshot by 10.26 points (1.60x).
The fitted degrees of freedom was 1.80, which signals very heavy tails.

Key Takeaways
Normal-distribution VaR and ES understated crisis risk. A fat-tailed model matched historical VaR but overshot ES by almost 2x, so tail models need calibration and backtesting.
Daily rebalancing hurt in this one-way collapse, but it is a single period and not a general rule.
Diversification across six financial stocks was weak throughout the period, not only on the worst day.
Risk was concentrated: one stock, AIG, drove nearly a third of the worst-day losses.
Limits to keep in mind: about 500 days of data (about 5 observations in the 99% tail), stocks chosen with hindsight, and one crisis only.
