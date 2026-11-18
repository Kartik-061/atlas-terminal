# Atlas-Terminal: Global US Equity Momentum Engine

**Project:** Project CODEX Multi-Asset Quantitative House  
**Specification:** `SPEC-ATLAS-2026-V1.2` (`v1.2_REVISED_POST_REVIEW`)  
**Target Market:** US Equities (S&P 500 / Russell 1000 Large & Mid-Cap Common Stocks)  
**Execution Mode:** Research & Paper Simulation Only (Live Execution Programmatically Locked)  
**Quantitative Status:** **REJECTED** (Failed Gate 0 and Gate 3; negative momentum selection alpha; terminated unless delisting-inclusive data is funded)  

---

## 1. Overview, Empirical Findings & Formal Strategy Status

Atlas-Terminal is a systematic quantitative cross-sectional equity momentum research engine designed for the US equity market. The engine tests dual-horizon volatility-adjusted equity momentum across liquid US common stocks, coupled with a macro market regime filter (200-day simple moving average on `SPY`) to mitigate downside tail risk.

### 1.1 Empirical Audit Summary (26-Year Simulation: 2000–2026)
Following rigorous quantitative backtesting and robustness evaluation, Atlas-Terminal has been evaluated against five compulsory baselines. The empirical results reveal four primary conclusions:

1. **Defeat Against Passive Equal-Weight Baselines:**
   Atlas-Terminal generated a **10.98% Net CAGR**, which:
   - Lost to the passive equal-weighted S&P 500 ETF (**Baseline 2 / RSP: 11.12% CAGR**, -14 bps lag).
   - Severely lagged the unmanaged Equal-Weighted Eligible Universe (**Baseline 5: 14.39% CAGR**, -341 bps / -3.41% annual drag). Naive equal-weighting of all eligible liquid US common stocks outperformed the active momentum portfolio.
2. **Negative Momentum Stock Selection Alpha (-3.28%):**
   Comparing **Baseline 4 (Unfiltered Momentum: 11.11% CAGR)** directly against **Baseline 5 (Equal-Weighted Eligible Universe: 14.39% CAGR)** reveals that momentum stock ranking subtracted **328 bps/year** ($\Delta = -3.28\%$). Dual-horizon volatility-adjusted momentum selection provided negative alpha relative to the underlying investable equity beta.
3. **Macro 200-SMA Gate as Sole Source of Downside Defense:**
   The strategy's only demonstrable advantage was drawdown containment (-21.29% MaxDD vs -49.19% for Baseline 5 and -55.19% for SPY). However, this tail-risk reduction was produced entirely by the macro market trend gate (`SPY` > 200-SMA, where Baseline 3 achieved **-18.62% MaxDD** with zero stock selection), not by momentum stock selection.
4. **Gate 3 Hard Reject (Observed Sharpe 0.52 < 0.60 Revised Hurdle):**
   Atlas achieved an annualized Sharpe ratio of **0.52**, failing its revised Gate 3 hard-reject hurdle of $\ge 0.60$ (revised down from the uncalibrated initial 1.00 claim). Consequently, the engine triggered a **Hard Reject** under quantitative governance rules.
5. **Gate 6 Multi-Testing Insignificance:**
   The nominal permutation test $p = 0.038$ fails significance once corrected for the 36-cell parameter neighborhood grid ($p_{\text{Bonferroni}} = 1.00 \gg 0.00139$).

### 1.2 Formal Strategy Rejection Notice
Atlas-Terminal is formally **REJECTED**. The candidate specification is terminated due to hard-reject failures on Gate 0 (survivorship/delisting bias) and Gate 3 (Sharpe failure), negative momentum selection alpha (-3.28%), and multi-testing failure on Gate 6. The strategy will NOT be deployed or reconsidered unless commercial point-in-time delisting-inclusive data (e.g., CRSP, Compustat, Norgate Data) is funded and integrated.

---

## 2. Gate 0: Data Provenance, Survivorship Bias & Delisting Audit

Before any backtest result on US equities can be evaluated or accepted as production-grade quantitative evidence, it must pass **Gate 0: Data Provenance & Survivorship Audit**.

1. **Delisted Securities Mandate**:
   - For any 26-year (2000–2026) backtest of individual US stocks, the candidate universe must include securities that were subsequently delisted due to bankruptcy, distress, liquidation, or regulatory action (e.g., Enron, WorldCom, Global Crossing, Lehman Brothers, Washington Mutual, Silicon Valley Bank, First Republic Bank).
   - Free data feeds (such as Yahoo Finance) omit entities that failed during the 2000–2002 DotCom collapse, the 2007–2009 Global Financial Crisis, and the 2023 regional banking crisis, inducing survivorship bias.
2. **Academic Precedent on Delisting Drag**:
   - **Shumway (1997)** (*"The Delisting Bias in CRSP Data"*, Journal of Finance): Demonstrated that average delisting returns for performance-related/bankrupt delistings are **-30% to -55%**, and databases that omit delisting returns significantly overstate historical returns for distressed and low-performing cohorts.
   - *(Note: Citations to Elton, Gruber, & Blake 1996 and Carhart et al. 2002 have been removed, as both evaluate mutual funds rather than common stock delisting survivorship).*
3. **Gate 0 Status & Unverified Exploratory Classification**:
   - Because commercial point-in-time data containing delisted securities (e.g., CRSP, Compustat, Norgate Data) is not currently funded or integrated, the 26-year individual-stock backtest is formally classified as **Unverified Exploratory Research**. Gate 0 is marked **FAILED / EXPLORATORY ONLY**.
   - Definitive validation without delisting data is strictly restricted to broad index ETFs (`SPY`, `QQQ`, `RSP`), where constituent survivorship is handled point-in-time by the index provider.

---

## 3. Mathematical Architecture & Rebalance Governance

### 3.1 Universe & Dual-Horizon Momentum Math
At month-end close $t$:
$$\text{Return}_{12M-1M}(i, t) = \frac{P_{i, t-21}^{\text{adj}}}{P_{i, t-252}^{\text{adj}}} - 1, \quad \text{Return}_{6M-1M}(i, t) = \frac{P_{i, t-21}^{\text{adj}}}{P_{i, t-126}^{\text{adj}}} - 1$$
$$\text{Raw Score}(i, t) = 0.50 \times \text{Return}_{12M-1M}(i, t) + 0.50 \times \text{Return}_{6M-1M}(i, t)$$
*(The 21-trading-day lag $t-21$ is mandatory per Jegadeesh & Titman 1993 to eliminate 1-month reversal drag).*

$$\text{Score}(i, t) = \begin{cases}
\frac{\text{RawScore}(i, t)}{\max(\sigma_{60D}, 0.05)} & \text{if } \text{RawScore} \ge 0 \\
\text{RawScore}(i, t) \times \max(\sigma_{60D}, 0.05) & \text{if } \text{RawScore} < 0
\end{cases}$$
*(Disqualifies non-positive momentum candidates; penalizes high-volatility losers).*

### 3.2 Macro Regime Filter (200-Day SMA Gate)
- **When $P_t^{\text{SPY}} > \text{SMA}_{200}(t)$ (BULL):** Allocate across target equity slots.
- **When $P_t^{\text{SPY}} \le \text{SMA}_{200}(t)$ (DEFENSIVE):** Equity allocation drops to 0%. Capital sweeps to short-term US Treasury bills (`BIL` / cash yield).

### 3.3 Drift Band Symmetry & Buffer Retention Rules
- **Target Slots:** $N = 30$ equal-weighted positions ($\omega^* = 1/N \approx 3.333\%$).
- **Drift Tolerance Band:** $[\omega_{\text{floor}}, \omega_{\text{ceiling}}] = [2.00\%, 6.00\%]$.
- **Position Actions on Day $t$ Decision / Day $t+1$ Open Execution:**
  - **Rule A (Buffer Retention - HOLD):** If holding $h$ is ranked in $[1, 2N]$ ($1$ to $60$) AND weight $\in [2.0\%, 6.0\%]$, execute **ZERO TRADES** (HOLD).
  - **Rule B (Ceiling Breach - TRIM):** If holding $h$ is ranked in $[1, 2N]$ but weight $> 6.0\%$, **TRIM** back to target weight $3.333\%$.
  - **Rule C (Floor Breach - TOPUP):** If holding $h$ is ranked in top target cohort $[1, N]$ ($1$ to $30$) but weight $< 2.0\%$, **TOPUP** back to target weight $3.333\%$. Buffer holdings ranked $N+1$ to $2N$ are NOT topped up.
  - **Rule D (Buffer Exit - FULL LIQUIDATION):** If holding $h$ drops below rank $2N = 60$, execute **FULL LIQUIDATION**.
  - **Rule E (Defensive Sweep):** In defensive regime, execute **FULL LIQUIDATION** of all equity holdings.

---

## 4. Five Compulsory Baselines to Beat

| Baseline ID | Description | Benchmark Function |
| :--- | :--- | :--- |
| **B1: SPY Buy-and-Hold** | S&P 500 Market Cap Weighted Index ETF | Passive market beta benchmark. |
| **B2: RSP Buy-and-Hold** | Invesco S&P 500 Equal Weight ETF | Tests whether equal-weighting alone explains returns. |
| **B3: SPY + 200-SMA Gate** | SPY when SPY > 200-SMA, else 100% Cash/BIL | Isolates risk/return contribution of macro trend timing alone. |
| **B4: Unfiltered Momentum** | Top 30 Momentum without 200-SMA Gate | Measures pure stock-selection alpha independent of macro gating. |
| **B5: Equal-Weighted Eligible Universe** | Monthly equal-weighted basket of all eligible universe stocks (price $\ge \$5$, MDTV $\ge \$20\text{M}$) | Isolates momentum factor edge from broad eligible universe beta. |

### 4.1 26-Year Historical Backtest Results (2000–2026, Unverified Exploratory)
Simulated across 26 calendar years (January 2000 to March 2026, 6,599 trading days) with next-bar open fills and transaction frictions:

| Strategy / Baseline | Net CAGR | Sharpe Ratio | Sortino Ratio | Max Drawdown | Calmar Ratio | Annual Turnover |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Atlas-Terminal Core Strategy** | **10.98%** | **0.52** | **0.44** | **-21.29%** | **0.52** | **172.5%** |
| **Baseline 1: SPY Cap-Weighted** | 7.66% | 0.30 | 0.28 | -55.19% | 0.14 | 0.0% |
| **Baseline 2: RSP Equal-Weighted** | 11.12% | 0.45 | 0.43 | -59.92% | 0.19 | 0.0% |
| **Baseline 3: SPY + 200-SMA Gate** | 7.44% | 0.38 | 0.32 | -18.62% | 0.40 | 0.0% |
| **Baseline 4: Unfiltered Momentum** | 11.11% | 0.42 | 0.39 | -55.13% | 0.20 | 129.6% |
| **Baseline 5: Equal-Weighted Eligible Universe** | 14.39% | 0.62 | 0.59 | -49.19% | 0.29 | 0.0% |

#### Critical Empirical Takeaways:
- **Loses to RSP & Baseline 5:** Atlas (10.98%) loses to RSP (11.12%) and significantly underperforms Baseline 5 (14.39% by -341 bps).
- **Negative Momentum Stock Selection Alpha:** Baseline 4 (11.11%) lags Baseline 5 (14.39%) by -3.28% (-328 bps/year), showing that momentum stock ranking destroyed value relative to simple naive equal weighting.
- **Drawdown Reduction is 100% Macro 200-SMA:** Atlas's low drawdown (-21.29%) is achieved entirely by the SPY 200-SMA trend gate (Baseline 3 achieved -18.62% with no stock picking). Stock selection provided zero downside risk benefit.

---

## 5. The 9-Gate Robustness Protocol & Multi-Testing Governance

### 5.1 Gate Hierarchy & Rejection Taxonomy
- **Hard Reject Gates (Gates 0, 1, 3, 4):** Failure immediately terminates candidate strategy.
- **Diagnostic Gates (Gates 2, 5, 6, 7, 8):** Failure requires research memo explaining risk.

| Gate ID | Classification | Gate Name | Acceptance Criteria | Empirical Result | Gate Status |
| :---: | :---: | :--- | :--- | :--- | :---: |
| **Gate 0** | **HARD REJECT** | Data Provenance & Delisting Audit | Point-in-time delisted stock inclusion documented; else exploratory. | Point-in-time delisted stocks absent from free data. | **FAILED (EXPLORATORY ONLY)** |
| **Gate 1** | **HARD REJECT** | Next-Bar Open Fill Verification | $100\%$ trade fills execute at $T+1$ Open with statutory SEC/FINRA fees. | $100\%$ fills verified at $T+1$ Open with fees. | **PASSED** |
| **Gate 2** | Diagnostic | 5-Tier Cost Stress Ladder | Profitability maintained through 25 bps; pre-decimalization stress noted. | Profitable through 50 bps; degradation 5-to-25 bps is 2.18%. | **PASSED** |
| **Gate 3** | **HARD REJECT** | 10,000-Path MC & Andrew Lo SE Audit | Point estimate Sharpe $> 0.60$ (revised hurdle); Lo (2002) 95% lower bound $> 0.15$. | Observed Sharpe = **0.52** (< 0.60 revised hurdle); Lo lower bound = **0.11**. | **FAILED (HARD REJECT)** |
| **Gate 4** | **HARD REJECT** | Walk-Forward Train/Test Split | OOS (2015–2026) Sharpe achieves $\ge 60\%$ of IS; OOS CAGR $> 8.0\%$. | OOS CAGR = 10.42%, OOS Sharpe = 0.68 ($\ge 60\%$ of IS Sharpe). | **PASSED** |
| **Gate 5** | Diagnostic | Parameter Neighborhood & FDR Audit | 36 parameter grid tested; Bonferroni $\alpha = 0.00139$ reported. | Smooth surface across 36 parameter configurations. | **PASSED** |
| **Gate 6** | Diagnostic | Permutation Test ($p < 0.05$) | Empirical $p < 0.05$ (corrected for 36-cell parameter search grid). | Nominal $p = 0.038$; Grid-corrected $p_{\text{Bonferroni}} = 1.00$ ($p \gg 0.00139$). | **FAILED (Multi-Testing Grid Snooping)** |
| **Gate 7** | Diagnostic | Sub-Period Regime Decomposition | Max drawdown in all bear regimes $\le \mathbf{27.5\%}$. | Bear regime MaxDD strictly below 27.5% across DotCom, GFC, and 2022. | **PASSED** |
| **Gate 8** | Diagnostic | Turnover & Capacity Analysis | Annual turnover $< 250\%$; holding duration $\ge 4.0$ months; capacity $\ge \$15\text{M}$. | Turnover = 172.5%; average holding duration = 5.2 months. | **PASSED** |

### 5.2 Andrew Lo (2002) Standard Error Correction & Gate 3 Hard Reject
Per **Andrew Lo (2002)** (*"The Statistics of Sharpe Ratios"*):
$$\text{SE}(\text{SR}) = \sqrt{\frac{1 + 0.5 \times \text{SR}^2}{T}}$$
For $T = 26$ years and observed $\text{SR} = 0.52$:
$$\text{SE}(\text{SR}) = \sqrt{\frac{1 + 0.5 \times (0.52)^2}{26}} \approx 0.2089 \approx 0.21$$
The 95% two-sided lower confidence bound is:
$$\text{SR}_{\text{lower}, 95\%} = 0.52 - 1.96(0.2089) \approx \mathbf{0.11}$$
*(Matches the summary table exactly).*

**Gate 3 Failure Determination:** The revised hard-reject hurdle required a point estimate Sharpe ratio $\ge 0.60$ (revised down from the uncalibrated initial 1.00 claim). With an observed Sharpe ratio of **0.52**, Atlas-Terminal **FAILED Gate 3 (Hard Reject)**.

### 5.3 Gate 6 Multi-Testing & Grid Correction Audit
While the unadjusted nominal permutation test achieved $p = 0.038$, Gate 5 evaluated 36 parameter combinations. Under the Bonferroni family-wise error rate correction:
$$\alpha_{\text{Bonferroni}} = \frac{0.05}{36} \approx 0.00139, \quad p_{\text{Bonferroni}} = \min(1.0, 36 \times 0.038) = 1.00$$
Because $0.038 \gg 0.00139$ and $p_{\text{Bonferroni}} = 1.00$, the strategy's apparent permutation edge fails multiple-testing correction.

---

## 6. Cross-Border Indian Regulatory & Tax Advisory Note

1. **RBI Liberalised Remittance Scheme (LRS):** Remittance ceiling of **\$250,000 USD per financial year**. Leveraged derivatives/margin trading are strictly prohibited.
2. **Tax Collected at Source (TCS):** **20% TCS** under Section 206C(1G) on aggregate foreign remittances exceeding **₹10 Lakh** in a financial year (revised threshold).
3. **Mandatory Foreign Asset Disclosure (Schedule FA & Black Money Act):** All foreign depository accounts and equity holdings must be mandatorily disclosed in **Schedule FA** of ITR-2/ITR-3. Failure to disclose attracts penalties of **₹10 Lakh per year** and prosecution under the **Black Money Act, 2015**.
4. **Capital Gains Taxation:** Foreign equities are treated as unlisted securities. Short-term gains ($\le 24$ months) are taxed at **ordinary slab rates (up to 39%)**. Long-term gains (> 24 months) are taxed at **12.5%** without indexation.

---

## 7. Execution CLI Commands

```powershell
# Run unit test suite (15 tests passing)
pytest tests/test_atlas_engine.py -v

# View status dashboard (shows REJECTED status & tax advisory)
python atlas_runner.py --status

# Preview monthly rebalance orders (Dry Run)
python atlas_runner.py --dry-run

# Run 26-year historical backtest vs ALL 5 COMPULSORY BASELINES
python atlas_runner.py --backtest --start 2000-01-01 --end 2026-03-31

# Run 9-Gate Robustness Protocol (v1.2)
python atlas_runner.py --audit-8gates
```

---
*Project CODEX — Atlas-Terminal Quantitative Architecture*  
*Specification: SPEC-ATLAS-2026-V1.2 (v1.2_REVISED_POST_REVIEW - REJECTED)*
