# ATLAS-TERMINAL: RESEARCH SPECIFICATION MEMO
**Document ID:** SPEC-ATLAS-2026-V1.2  
**Version:** v1.2_REVISED_POST_REVIEW  
**Date:** 2026-11-18  
**Project:** Project CODEX Multi-Asset Quantitative House  
**Asset Class:** US Equities (S&P 500 / Russell 1000 Large & Mid-Cap Common Stocks)  
**Target Horizon:** 2000-01-01 to 2026-03-31 (26 Calendar Years)  
**Execution Gateway:** Research & Paper Simulation Only (Live Execution De-Scoped & Locked)  
**Project Operational Status:** REJECTED (Terminated due to Gate 0 Survivorship Failure & Gate 3 Sharpe Failure)  

---

## 1. Executive Summary, Scope & Calibrated Targets

Atlas-Terminal is a systematic quantitative cross-sectional equity momentum research engine designed for the US equity market. The engine investigates dual-horizon volatility-adjusted equity momentum across liquid US common stocks, combined with a macro market regime filter (200-day simple moving average on `SPY`) to manage downside tail risk.

### 1.1 Realistic Target Calibration
Earlier draft projections (+18.0% to +22.0% USD Net CAGR, Sharpe $\ge 1.00$) were uncalibrated and inconsistent with historical long-only US equity market premia. 
Over the 26-year historical period (2000–2026):
- Passive S&P 500 cap-weighted (`SPY`) compounded at ~7.6% to 8.5% net CAGR (Sharpe ~0.30 to 0.40).
- Passive Nasdaq-100 (`QQQ`) compounded at ~10.2% net CAGR (Sharpe ~0.40 to 0.45).
- Equal-weighted S&P 500 (`RSP`) compounded at ~11.0% to 11.2% net CAGR (Sharpe ~0.45).

Accordingly, Atlas-Terminal's realistic quantitative targets are pre-registered as:
- **Target Net CAGR (USD):** +12.0% to +15.0% across full market cycles (an excess hurdle of +300 to +500 bps over passive market beta).
- **Target Sharpe Ratio:** 0.60 to 0.75 (annualized, net of realistic transaction frictions; revised from the initial 1.00 target).
- **Target Sortino Ratio:** $\ge 0.85$.
- **Maximum Peak-to-Trough Drawdown Ceiling:** Strictly constrained below **27.5%** via systematic macro regime switching.
- **Maximum Annual Turnover:** $< 250\%$ via hysteresis drift buffering.

### 1.2 Empirical Performance Findings & Gate Failures (2000–2026 Backtest)
The 26-year historical simulation (classified as an unverified exploratory simulation due to Gate 0) revealed four fundamental empirical realities:
1. **Underperformance vs. Equal-Weight Baselines:**
   Atlas-Terminal generated a **10.98% Net CAGR**, which:
   - Lost to the passive equal-weight ETF benchmark (**Baseline 2 / RSP: 11.12% CAGR**, -14 bps lag).
   - Severely lagged the Equal-Weighted Eligible Universe (**Baseline 5: 14.39% CAGR**, -341 bps / -3.41% annual drag).
2. **Negative Momentum Stock Selection Alpha (-3.28%):**
   Unfiltered momentum (**Baseline 4: 11.11% CAGR**) underperformed the Equal-Weighted Eligible Universe (**Baseline 5: 14.39% CAGR**) by **3.28 percentage points (-328 bps/year)**. The dual-horizon volatility-adjusted momentum metric added negative return alpha compared to simply equal-weighting the broad universe of eligible liquid US common stocks.
3. **Macro 200-SMA Gate as Sole Source of Downside Defense:**
   The strategy's only demonstrable advantage was drawdown containment (-21.29% MaxDD vs -49.19% for Baseline 5 and -55.19% for SPY). This reduction was driven entirely by the macro market trend gate (`SPY` > 200-SMA, where Baseline 3 achieved -18.62% MaxDD without any stock picking), not by momentum stock selection.
4. **Gate 3 Hard Reject (Observed Sharpe 0.52 < 0.60 Revised Hurdle):**
   The strategy generated an annualized Sharpe ratio of **0.52**, failing to meet the revised Gate 3 hard-reject hurdle of $\ge 0.60$ (revised down from the uncalibrated initial 1.00 target). Gate 3 is officially **FAILED (Hard Reject)**.
5. **Gate 6 Multi-Testing Insignificance:**
   The nominal permutation test $p = 0.038$ fails significance once corrected for the 36-cell parameter neighborhood grid ($p_{\text{Bonferroni}} = 1.00 \gg 0.00139$).

### 1.3 Formal Strategy Rejection & Termination Notice
1. **Live Capital Execution De-Scoped:** Atlas-Terminal is strictly designated as a **Research & Paper Simulation Engine**. Live routing of real capital is programmatically locked.
2. **Formal Strategy Rejection:** Atlas-Terminal is formally **REJECTED**. The candidate specification is terminated due to fatal hard-reject failures on Gate 0 (data survivorship/delisting omissions) and Gate 3 (failure to achieve the 0.60 revised Sharpe hurdle), compounded by negative momentum stock selection alpha (-3.28%) and multi-testing failure on Gate 6.
3. **Funding Pre-Condition for Reopening:** The project will NOT be deployed or reconsidered unless commercial point-in-time delisting-inclusive data (e.g., CRSP, Compustat, Norgate Data) is funded and integrated to eliminate survivorship bias. Without funded delisting data, Atlas-Terminal is permanently terminated.

---

## 2. Gate 0: Data Provenance, Survivorship Bias & Delisting Drag

### 2.1 The Delisting Bias Mandate
Before any backtest result on US equities can be evaluated or accepted as production-grade quantitative evidence, it must pass **Gate 0: Data Provenance & Survivorship Audit**.

1. **Point-in-Time Delisted Securities Prerequisite**:
   - For any 26-year (2000–2026) backtest of individual US stocks, the candidate universe must include securities that were subsequently delisted due to bankruptcy, distress, liquidation, or regulatory action (e.g., Enron, WorldCom, Global Crossing, Lehman Brothers, Washington Mutual, Silicon Valley Bank, First Republic Bank).
   - Relying on free data feeds (such as Yahoo Finance) introduces lookback survivorship bias. Free endpoints provide historical series only for entities currently trading or selectively preserved, omitting companies that failed during the 2000–2002 DotCom crash, the 2007–2009 Global Financial Crisis, and the 2023 regional banking crisis.

2. **Literature Correction on Delisting Drag**:
   - Prior drafts cited Elton, Gruber, and Blake (1996) and Carhart et al. (2002). Both citations have been removed: EGB (1996) and Carhart et al. addressed mutual fund attrition and performance, not common stock survivorship.
   - For common stocks, academic precedent rests on **Shumway (1997)** (*"The Delisting Bias in CRSP Data"*, Journal of Finance):
     - Shumway demonstrated that the average delisting return for performance-related/bankrupt delistings is **-30% to -55%**.
     - Omitting delisting returns significantly overstates historical returns for distressed and low-performing cohorts, creating artificial upward performance bias in databases that ignore delistings.
     - Unsubstantiated annual percentage ranges (e.g., "3% to 6%") have been deleted from this specification.

3. **Gate 0 Pre-Condition Status & Unverified Exploratory Classification**:
   - Because commercial point-in-time data containing delisted securities (e.g., CRSP, Compustat, Norgate Data) is not currently funded or integrated, the 26-year individual-stock backtest in this repository is formally classified as **Unverified Exploratory Research**.
   - Gate 0 status is **FAILED / EXPLORATORY ONLY**. The individual-stock backtest results cannot be accepted as validated production alpha.
   - Definitive validation without delisting data is strictly restricted to broad index ETFs (such as `SPY`, `QQQ`, and `RSP`), where constituent survivorship, additions, and deletions are handled point-in-time by the index provider's methodology.

---

## 3. Pre-Registered Mathematical Architecture

### 3.1 Universe Screening & Liquidity Gating
At each rebalance date $t$ (the final trading day of calendar month $m$):
1. **Asset Eligibility:** US common stocks traded on NYSE, NASDAQ, or AMEX. Excludes ADRs, CEFs, ETFs, unit trusts, and preferred shares. Twin/duplicate share classes are filtered (retaining the primary voting class with higher liquidity, e.g., GOOGL over GOOG).
2. **Liquidity Floor:** 60-day Median Daily Traded Dollar Volume ($\text{MDTV}_{60D}$) $\ge \$20,000,000$:
   $$\text{MDTV}_{60D}(i, t) = \text{median}\left(\{P_{i, \tau} \times V_{i, \tau}\}_{\tau=t-59}^t\right) \ge \$20,000,000$$
3. **Nominal Price Gate:** Unadjusted closing price $P_{i, t} \ge \$5.00$ to eliminate penny stock bid-ask bounce and micro-cap market microstructure anomalies.
4. **Historical Continuity:** Security must have $\ge 252$ valid trading days of historical adjusted prices preceding date $t$.

### 3.2 Dual-Horizon Volatility-Adjusted Momentum Metric
For each eligible security $i$ at month-end date $t$:

1. **Lagged 12-Month Momentum**:
   $$\text{Ret}_{12M-1M}(i, t) = \frac{P_{i, t-21}^{\text{adj}}}{P_{i, t-252}^{\text{adj}}} - 1$$

2. **Lagged 6-Month Momentum**:
   $$\text{Ret}_{6M-1M}(i, t) = \frac{P_{i, t-21}^{\text{adj}}}{P_{i, t-126}^{\text{adj}}} - 1$$

   *(The 21-trading-day lag $t-21$ is mandatory per Jegadeesh & Titman 1993 to eliminate 1-month short-term liquidity reversal drag).*

3. **Composite Raw Momentum Score**:
   $$\text{RawScore}(i, t) = 0.50 \times \text{Ret}_{12M-1M}(i, t) + 0.50 \times \text{Ret}_{6M-1M}(i, t)$$

4. **60-Day Realized Volatility Normalization**:
   Calculated from daily log returns $r_{i, \tau} = \ln(P_{i, \tau}^{\text{adj}} / P_{i, \tau-1}^{\text{adj}})$ over the prior 60 trading days:
   $$\sigma_{60D}(i, t) = \sqrt{\frac{252}{59} \sum_{\tau=t-59}^t \left(r_{i, \tau} - \bar{r}_i\right)^2}$$
   A volatility floor of $\sigma_{\text{min}} = 0.05$ (5% annualized) is applied.

5. **Risk-Adjusted Momentum Score**:
   $$\text{Score}(i, t) = \begin{cases}
   \frac{\text{RawScore}(i, t)}{\max\left(\sigma_{60D}(i, t), 0.05\right)} & \text{if } \text{RawScore}(i, t) \ge 0 \\
   \text{RawScore}(i, t) \times \max\left(\sigma_{60D}(i, t), 0.05\right) & \text{if } \text{RawScore}(i, t) < 0
   \end{cases}$$
   *(When raw momentum is negative, multiplying by volatility ensures high-volatility losers are penalized more heavily rather than mathematically rewarded).*

Eligible securities are ranked descending by $\text{Score}(i, t)$. Securities with $\text{RawScore}(i, t) \le 0$ are disqualified from initial entry.

### 3.3 Macro Trend Regime Filter (200-Day SMA Gate)
Let $P_t^{\text{SPY}}$ be the unadjusted close price of `SPY` at date $t$, and let:
$$\text{SMA}_{200}(t) = \frac{1}{200} \sum_{k=0}^{199} P_{t-k}^{\text{SPY}}$$

The macro regime indicator $M_t \in \{0, 1\}$ is determined as:
$$M_t = \begin{cases} 
1 & \text{if } P_t^{\text{SPY}} > \text{SMA}_{200}(t) \quad (\text{BULL REGIME}) \\ 
0 & \text{if } P_t^{\text{SPY}} \le \text{SMA}_{200}(t) \quad (\text{DEFENSIVE REGIME}) 
\end{cases}$$

- **When $M_t = 1$ (Bull):** Strategy allocates across target equity slots.
- **When $M_t = 0$ (Defensive):** Equity exposure is reduced to 0%. All capital sweeps to short-term US Treasury bills (proxy: `BIL` / effective Federal Funds cash yield).

### 3.4 Sizing, Drift Band Symmetry & Buffer Retention Rules
Let $N = \text{TARGET\_SLOTS} = 30$.
- **Target Weight per Slot**:
  $$\omega^* = \frac{1}{N} = \frac{1}{30} \approx 3.333\%$$
- **Rebalance Drift Tolerance Band**:
  The rebalance drift bounds are formally defined with an asymmetric operational band:
  $$\omega_{\text{floor}} = 2.00\%, \quad \omega_{\text{ceiling}} = 6.00\%$$
  *(Expressed around target $\omega^* \approx 3.33\%$ with $\Delta_{\text{down}} = 1.33\%$ and $\Delta_{\text{up}} = 2.67\%$ to accommodate natural equity compounding).*

- **Formal Rebalance Rules on Day $t$ Decision / Day $t+1$ Open Execution**:
  1. **Defensive Regime ($M_t = 0$):**
     - Liquidate 100% of all open equity positions at $T+1$ Open. Allocate 100% to Cash/Treasury yield.
  2. **Bull Regime ($M_t = 1$):**
     - Rank all eligible assets descending by momentum score.
     - **Top Target Cohort:** Ranks $1$ through $N$ ($1$ to $30$).
     - **Hysteresis Buffer Cohort:** Ranks $N+1$ through $2N$ ($31$ to $60$).
     - **Disqualified Cohort:** Ranks $> 2N$ ($> 60$).

  **Position-Level Actions for Existing Holding $h$**:
  - **Rule A (Buffer Retention - HOLD):** If holding $h$ is ranked within $[1, 2N]$ (ranks $1$ to $60$) AND its current weight $\omega_h \in [\omega_{\text{floor}}, \omega_{\text{ceiling}}] = [2.00\%, 6.00\%]$, execute **ZERO TRADES** (HOLD).
  - **Rule B (Ceiling Breach - TRIM):** If holding $h$ is ranked within $[1, 2N]$ but its weight exceeds $\omega_{\text{ceiling}} = 6.00\%$, **TRIM** the position back to target weight $\omega^* = 3.333\%$ at $T+1$ Open.
  - **Rule C (Floor Breach - TOPUP):** If holding $h$ is ranked in the top target cohort $[1, N]$ (ranks $1$ to $30$) but its weight has fallen below $\omega_{\text{floor}} = 2.00\%$, **TOPUP** the position back to target weight $\omega^* = 3.333\%$ (subject to available cash). Buffer holdings ranked $N+1$ to $2N$ are NOT topped up.
  - **Rule D (Buffer Exit - FULL LIQUIDATION):** If holding $h$ drops below rank $2N = 60$, execute **FULL LIQUIDATION** at $T+1$ Open.
  - **Rule E (New Slot Entry):** Open vacant slots ($N - \text{retained positions}$) are filled sequentially by the highest-ranked unheld candidates from ranks $1$ through $N$, each sized to target weight $\omega^* = 3.333\%$.

---

## 4. Execution Model, Cost Stress Ladder & Decimalization

### 4.1 Next-Bar Open Execution Protocol
All simulated fills enforce strict zero-lookahead next-bar timing:
- **Signal Calculation Time:** Date $t$ Close (16:00 EST, month-end).
- **Execution Time:** Date $t+1$ Open (09:30–09:35 EST, first trading day of next month).
- **Execution Price:**
  $$P_{\text{fill, buy}} = P_{t+1}^{\text{open}} \times (1 + \text{slippage})$$
  $$P_{\text{fill, sell}} = P_{t+1}^{\text{open}} \times (1 - \text{slippage})$$

### 4.2 Expanded 5-Tier Cost Stress Ladder
To ensure robust survivability under market microstructure friction, the cost ladder is expanded across five tiers:

| Tier | Slippage Assumption | Statutory Fees | Total Round-Trip Friction | Scenario Description |
| :---: | :---: | :---: | :---: | :--- |
| **Tier 1 (Base)** | 5 bps ($0.0005$) | SEC + FINRA | ~10 bps + fees | Standard electronic large-cap liquid trading. |
| **Tier 2 (2x)** | 10 bps ($0.0010$) | SEC + FINRA | ~20 bps + fees | Stressed liquidity / elevated market volatility. |
| **Tier 3 (3x)** | 15 bps ($0.0015$) | SEC + FINRA | ~30 bps + fees | High-impact rebalance / mid-cap slippage. |
| **Tier 4 (5x)** | 25 bps ($0.0025$) | SEC + FINRA | ~50 bps + fees | Fractional trading / illiquid rebalance regimes. |
| **Tier 5 (10x)** | 50 bps ($0.0050$) | SEC + FINRA | ~100 bps + fees | Severe market disruption / pre-decimalization friction. |

### 4.3 Pre-Decimalization Spread Stress Note (April 2001)
Prior to April 9, 2001, US equity markets operated under fractional pricing systems where minimum quotation increments (ticks) were \$1/16 (\$0.0625) or \$1/8 (\$0.1250). Bid-ask spreads on large-cap US common stocks frequently averaged **20 to 50 basis points** before decimalization. Any backtest spanning 2000–2001 must demonstrate profitability under Tier 4 (25 bps) and Tier 5 (50 bps) friction tiers.

---

## 5. Compulsory Baselines to Beat

To prove that momentum generates genuine orthogonal alpha and is not an artifact of cap-weighting, equal-weighting, or macro timing alone, performance must be benchmarked side-by-side against **Five Compulsory Baselines**:

| Baseline ID | Description | Benchmark Function |
| :--- | :--- | :--- |
| **B1: SPY Buy-and-Hold** | S&P 500 Market Cap Weighted Index ETF | Passive market beta benchmark. |
| **B2: RSP Buy-and-Hold** | Invesco S&P 500 Equal Weight ETF | Tests whether equal-weighting alone explains returns. |
| **B3: SPY + 200-SMA Gate** | SPY when SPY > 200-SMA, else 100% Cash/BIL | Isolates the risk/return contribution of macro trend timing alone. |
| **B4: Unfiltered Momentum** | Top 30 Momentum without 200-SMA Gate | Measures pure stock-selection alpha independent of macro gating. |
| **B5: Equal-Weighted Eligible Universe** | Monthly equal-weighted basket of all eligible universe stocks (price $\ge \$5$, MDTV $\ge \$20\text{M}$) | Isolates the momentum ranking factor edge from the underlying investable universe beta. |

### 5.1 26-Year Historical Full-Cycle Backtest Audit (2000–2026)
*Important Classification Note: Per Gate 0, the following 26-year simulation is classified as an Unverified Exploratory Simulation because free-tier data omitting point-in-time delisted securities was used.*

Simulated across 26 calendar years (January 2000 to March 2026, 6,599 trading days) with next-bar open fills and transaction frictions:

| Strategy / Baseline | Net CAGR | Sharpe Ratio | Sortino Ratio | Max Drawdown | Calmar Ratio | Annual Turnover |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Atlas-Terminal Core Strategy** | **10.98%** | **0.52** | **0.44** | **-21.29%** | **0.52** | **172.5%** |
| **Baseline 1: SPY Cap-Weighted** | 7.66% | 0.30 | 0.28 | -55.19% | 0.14 | 0.0% |
| **Baseline 2: RSP Equal-Weighted** | 11.12% | 0.45 | 0.43 | -59.92% | 0.19 | 0.0% |
| **Baseline 3: SPY + 200-SMA Gate** | 7.44% | 0.38 | 0.32 | -18.62% | 0.40 | 0.0% |
| **Baseline 4: Unfiltered Momentum** | 11.11% | 0.42 | 0.39 | -55.13% | 0.20 | 129.6% |
| **Baseline 5: Equal-Weighted Eligible Universe** | 14.39% | 0.62 | 0.59 | -49.19% | 0.29 | 0.0% |

### 5.2 Critical Factor Attribution & Empirical Audit Findings:
1. **Defeat Against Passive Equal-Weight Baselines:**
   - Atlas Core Strategy (**10.98% Net CAGR**) loses to the passive equal-weighted S&P 500 ETF (**Baseline 2 / RSP: 11.12% CAGR**, -14 bps lag).
   - Atlas severely lags the unmanaged Equal-Weighted Eligible Universe (**Baseline 5: 14.39% CAGR**, a -341 bps / -3.41% annual drag). An investor holding an unmanaged equal-weight portfolio of all liquid universe constituents generated superior compound wealth over the 26-year horizon.
2. **Negative Momentum Stock Selection Alpha (-3.28%):**
   - Comparing **Baseline 4 (Unfiltered Momentum: 11.11% CAGR)** directly against **Baseline 5 (Equal-Weighted Eligible Universe: 14.39% CAGR)** isolates the stock selection effect:
     $$\Delta_{\text{selection}} = 11.11\% - 14.39\% = -3.28\% \quad (-328\text{ bps/year})$$
   - Dual-horizon volatility-adjusted momentum selection subtracted 328 bps/year relative to naive universe equal-weighting. Cross-sectional momentum added zero positive return alpha.
3. **Macro 200-SMA Gate Explains 100% of Downside Defense:**
   - Atlas-Terminal constrained maximum drawdown to **-21.29%**, compared to -49.19% for Baseline 5 and -55.19% for SPY.
   - However, this tail-risk suppression was entirely produced by the macro market trend gate (`SPY` > 200-SMA). Baseline 3 (SPY + 200-SMA Gate) achieved **-18.62% Max Drawdown** with zero active stock picking. Active momentum selection provided no downside protection.

### 5.3 Acceptance Criteria Audit:
1. **Net CAGR:** **FAILED**. Atlas (10.98%) failed to beat Baseline 5 (14.39%) by +300 bps; it lagged B5 by -341 bps and also lagged RSP (11.12%).
2. **Maximum Drawdown:** **PASSED**. $|\text{MaxDD}_{\text{Atlas}}| = 21.29\% \le \mathbf{27.5\%}$ ceiling.
3. **Risk-Adjusted Ratios:** **FAILED (Hard Reject)**. Observed Sharpe is **0.52** (revised hurdle was $\ge 0.60$, revised down from the uncalibrated initial 1.00 target); Sortino is **0.44** (hurdle was $\ge 0.85$).
4. **Permutation Significance:** **FAILED (Grid-Corrected)**. While the nominal unadjusted $p = 0.038 < 0.05$, adjusting for the 36-cell parameter neighborhood grid (Gate 5) yields a Bonferroni critical alpha of $\alpha = 0.00139$ (or $p_{\text{Bonferroni}} = \min(1.0, 36 \times 0.038) = 1.00$). The result is not statistically significant after multi-testing correction.

---

## 6. The 9-Gate Robustness Protocol & Multi-Testing Governance

Every quantitative evaluation of Atlas-Terminal must be audited through the **9-Gate Protocol** (Gate 0 through Gate 8).

### 6.1 Gate Hierarchy & Rejection Taxonomy
Gates are categorized into **Hard Reject Gates** and **Diagnostic Gates**:
- **Hard Reject Gates (Gates 0, 1, 3, 4):** A failure on any hard reject gate immediately terminates the candidate specification and rejects deployment.
- **Diagnostic Gates (Gates 2, 5, 6, 7, 8):** A failure does not trigger automatic rejection but requires an explicit research memorandum detailing the source of risk and empirical degradation.

| Gate ID | Classification | Gate Name | Acceptance Criteria | Empirical Result | Gate Status |
| :---: | :---: | :--- | :--- | :--- | :---: |
| **Gate 0** | **HARD REJECT** | Data Provenance & Survivorship Audit | Point-in-time delisted stock inclusion documented; else exploratory. | Point-in-time delisted stocks absent from free-tier data. | **FAILED (EXPLORATORY ONLY)** |
| **Gate 1** | **HARD REJECT** | Next-Bar Open Fill Verification | $100\%$ of trade fills execute at $T+1$ Open with statutory SEC/FINRA fees. | Verified against next-bar open prices; fees accounted. | **PASSED** |
| **Gate 2** | Diagnostic | 5-Tier Cost Stress Ladder | Profitability maintained through 25 bps; degradation $< 250$ bps from 5 to 25 bps. | Profitable through 50 bps; degradation 5-to-25 bps is 2.18%. | **PASSED** |
| **Gate 3** | **HARD REJECT** | 10,000-Path Monte Carlo & Andrew Lo SE Audit | Point estimate Sharpe $> 0.60$ (revised hurdle); Lo (2002) 95% lower bound $> 0.15$. | Observed Sharpe = **0.52** (< 0.60 revised hurdle); Lo 95% lower bound = **0.11**. | **FAILED (HARD REJECT)** |
| **Gate 4** | **HARD REJECT** | Walk-Forward Train/Test Validation | OOS (2015–2026) Sharpe achieves $\ge 60\%$ of IS (2000–2014) Sharpe; OOS CAGR $> 8.0\%$. | OOS CAGR = 10.42%, OOS Sharpe = 0.68 ($\ge 60\%$ of IS Sharpe). | **PASSED** |
| **Gate 5** | Diagnostic | Parameter Neighborhood & FDR Audit | 36-cell parameter grid tested; Bonferroni / Benjamini-Hochberg FDR verified. | Stable surface across 36 parameter variations. | **PASSED** |
| **Gate 6** | Diagnostic | Randomized-Entry Permutation Test | Empirical $p < 0.05$ (corrected for 36-cell parameter search grid). | Nominal $p = 0.038$; Grid-corrected $p_{\text{Bonferroni}} = 1.00$ ($p \gg 0.00139$). | **FAILED (Multi-Testing Grid Snooping)** |
| **Gate 7** | Diagnostic | Sub-Period Regime Decomposition | Max drawdown in all bear regimes (2000–02, 2008–09, 2022) $\le \mathbf{27.5\%}$. | Bear regime MaxDD strictly below 27.5% in DotCom, GFC, and 2022. | **PASSED** |
| **Gate 8** | Diagnostic | Turnover & Capacity Analysis | Annual turnover $< 250\%$; holding period $\ge 4$ months; capacity $\ge \$15\text{M}$. | Turnover = 172.5%; average holding duration = 5.2 months. | **PASSED** |

### 6.2 Gate 3 Hard Reject Audit: Andrew Lo (2002) Standard Error Correction
Per **Andrew Lo (2002)** (*"The Statistics of Sharpe Ratios"*, Financial Analysts Journal):
$$\text{SE}(\text{SR}) = \sqrt{\frac{1 + \frac{1}{2}\text{SR}^2}{T}}$$

For a 26-year horizon ($T = 26$) and an observed annualized Sharpe ratio of $\text{SR} = 0.52$:
$$\text{SE}(\text{SR}) = \sqrt{\frac{1 + 0.5 \times (0.52)^2}{26}} = \sqrt{\frac{1.1352}{26}} \approx 0.2089 \approx 0.21$$
The 95% two-sided confidence interval is:
$$\text{SR}_{\text{lower}, 95\%} = 0.52 - 1.96(0.2089) \approx 0.52 - 0.4094 = 0.1106 \approx \mathbf{0.11}$$
$$\text{SR}_{\text{upper}, 95\%} = 0.52 + 1.96(0.2089) \approx 0.52 + 0.4094 = 0.9294 \approx 0.93$$

Both the summary table and the formula confirm an exact 95% lower bound of **0.11**.

**Gate 3 Failure Determination:**
- Revised Hard Reject Hurdle: Point Estimate Sharpe Ratio $\mathbf{\ge 0.60}$ (revised down from the uncalibrated initial 1.00 claim).
- Actual Empirical Point Estimate: $\mathbf{0.52}$.
- Outcome: **Gate 3 FAILED (Hard Strategy Reject)**. The engine does not meet the minimum statistical hurdle for standalone quantitative deployment.

### 6.3 Multi-Testing Governance & Gate 5 False Discovery Rate (FDR)
1. **One-Shot Governance Rule:**
   - Parameter selections, lookbacks, and thresholds are pre-registered once.
   - Modifying parameters post-hoc after inspecting backtest results constitutes data snooping, invalidates pre-registration, and transitions the engine back to unverified exploratory status.
2. **Multiple Hypothesis Correction (Benjamini-Hochberg / Bonferroni):**
   - The parameter sensitivity grid evaluates 36 variations:
     - Lookback 12M: $[210, 252, 294]$ days (3 levels)
     - Volatility Window: $[40, 60, 90]$ days (3 levels)
     - Portfolio Slots: $[20, 25, 30, 35]$ positions (4 levels)
     - Total parameter tests: $3 \times 3 \times 4 = 36$ configurations.
   - Under the Bonferroni adjustment for family-wise error rate $\alpha = 0.05$, the individual test significance threshold is:
     $$\alpha_{\text{Bonferroni}} = \frac{0.05}{36} \approx 0.00139$$
   - The parameter surface must exhibit smooth topological continuity without isolated Sharpe peaks or step-function cliffs.

### 6.4 Gate 6 Multi-Testing & Grid-Search Correction Audit
In earlier drafts, Gate 6 reported an empirical permutation $p$-value of $0.038$ and claimed to pass the nominal $p < 0.05$ hurdle. However, this nominal $p$-value was calculated on the single chosen configuration without adjusting for the 36-cell parameter search grid explored in Gate 5.

When an alpha search space spans 36 configurations, evaluating nominal significance on the selected variant without family-wise correction is classic data snooping:
- **Nominal Unadjusted $p$-value:** $p = 0.038$
- **Bonferroni-Adjusted Critical Threshold:** $\alpha = 0.05 / 36 \approx 0.00139$
- **Bonferroni-Adjusted $p$-value:**
  $$p_{\text{Bonferroni}} = \min(1.0, 36 \times 0.038) = 1.00$$

Because $p = 0.038 \gg 0.00139$ and $p_{\text{Bonferroni}} = 1.00$, the strategy's edge over randomized portfolios **evaporates under multiple-testing correction**. Gate 6 is officially marked **FAILED (Multi-Testing Grid Snooping)**.

---

## 7. Cross-Border Indian Regulatory & Tax Advisory Note

Because Project CODEX operates capital allocations for Indian resident individual and corporate accounts, all cross-border US equity allocations are subject to Indian foreign exchange and direct tax laws:

### 7.1 Reserve Bank of India (RBI) - Liberalised Remittance Scheme (LRS)
- **Annual Limit:** Indian resident individuals are permitted to remit up to **\$250,000 USD per financial year** (April 1 to March 31) for permissible capital and current account transactions.
- **Margin / Leverage Prohibition:** Under RBI Master Direction on LRS, remitted funds cannot be used to trade leveraged derivatives or speculative margin products. Atlas-Terminal operates exclusively on a cash equity basis with zero leverage.

### 7.2 Tax Collected at Source (TCS) under Section 206C(1G)
- Remittances under LRS for investing in foreign equities attract **20% TCS** on aggregate remittances exceeding **₹10 Lakh** in a financial year (revised threshold under Section 206C(1G)).
- While TCS is creditable against final Indian income tax liability, it creates an immediate 20% cash drag on capital deployment that must be modeled in working capital schedules.

### 7.3 Mandatory Foreign Asset Disclosure (Schedule FA & Black Money Act)
- Any Indian resident holding shares or beneficial interest in US equities (including via Alpaca or Interactive Brokers) must mandatorily disclose all foreign accounts, depository holdings, custodial balances, and peak investments in **Schedule FA (Foreign Assets)** of the Indian Income Tax Return (ITR-2 / ITR-3).
- Failure to report foreign assets attracts penalties of **₹10 Lakh per undisclosed year** and potential prosecution under the **Black Money (Undisclosed Foreign Income and Assets) and Imposition of Tax Act, 2015**.

### 7.4 Indian Capital Gains Taxation on Foreign Equities
- Foreign equities held by Indian residents are treated as unlisted securities for Indian tax purposes.
- Gains on foreign shares held for $\le 24$ months are classified as Short-Term Capital Gains (STCG) and taxed at applicable **ordinary income slab rates (up to 39% with surcharges)**.
- Long-term gains (> 24 months) are taxed at 12.5% without indexation benefits (Finance Act 2024 revisions).
- Dividends received from US stocks are subject to 25% US withholding tax under the US-India Double Tax Avoidance Agreement (DTAA), creditable via Form 67 against domestic tax liabilities.

---

## 8. Strategy Lifecycle Status: REJECTED

| Stage | Milestone / Focus | Status | Disposition |
| :--- | :--- | :---: | :--- |
| **Phase 1** | Mathematical Specification & Pre-Registration (v1.2) | **COMPLETED** | Archival Record |
| **Phase 2** | Data Pipeline, Benchmark Suite & Parquet Caching | **COMPLETED** | Free-Tier Data Only (Survivor-Biased) |
| **Phase 3** | Backtesting Engine & 9-Gate Robustness Battery | **COMPLETED** | Evaluated vs 5 Baselines |
| **Phase 4** | Pytest Comprehensive Unit Test Suite (15 tests passing) | **COMPLETED** | Verification Suite Operational |
| **Phase 5** | Alpaca Paper Simulation Runner & ASCII Dashboard | **COMPLETED** | Live Trading Programmatically Locked |
| **GATE 0** | **Data Provenance & Delisting Bias Audit** | **FAILED** | Delisted Equities Absent |
| **GATE 3** | **Sharpe Ratio Hurdle (>= 0.60 Revised Hurdle)** | **FAILED** | Observed Sharpe = 0.52 (Hard Reject) |
| **GATE 6** | **Randomized-Entry Permutation Test (Grid-Corrected)** | **FAILED** | $p_{\text{Bonferroni}} = 1.00$ across 36 cells |
| **ALPHA** | **Cross-Sectional Momentum Selection Alpha** | **NEGATIVE** | -328 bps/year vs Naive Equal Weight (B5) |
| **LIFECYCLE** | **Formal Strategy Rejection** | **REJECTED** | **Candidate Specification Terminated** |
| **CONDITIONAL** | **Point-in-Time Delisted Data Integration (CRSP/Norgate)** | **UNFUNDED** | **Strategy Dead Unless Funded** |

---
*Signed: Quantitative Research Lead, Project CODEX.*  
*Specification Status: SPEC-ATLAS-2026-V1.2 (v1.2_REVISED_POST_REVIEW - REJECTED).*
