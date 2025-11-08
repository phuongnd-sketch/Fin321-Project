# FX Hedge Analysis Model Technical Specification

**Created by:** Phuong Christina Doan  
**Updated by:** Phuong Christina Doan  
**Date Created:** November 7, 2025  
**Date Updated:** November 7, 2025  
**Version:** 1.0

---

## 1. Objective

This specification defines the quantitative framework for analyzing hedging strategies for our EUR 4,500,000 receivable due in one year. The model will compute net USD proceeds for four strategies—unhedged, forward contract, money market hedge, and currency put option—across 101 exchange rate scenarios. Outputs will identify breakeven points, visualize risk-return trade-offs through payoff diagrams, and support treasury's recommendation to the CFO on optimal hedge selection balancing cost certainty against opportunity preservation.

---

## 2. Scope

### In-scope:
* Net USD proceeds calculation for all four hedging strategies across exchange rate scenarios from 1.0500 to 1.1500 USD/EUR
* Transaction cost modeling (spot conversion fees, forward contract fees, option premiums)
* Interest rate parity validation for money market hedge vs. forward contract equivalence
* Breakeven analysis identifying crossover points between strategy pairs
* Payoff diagram visualization showing net proceeds by future spot rate
* Sensitivity analysis on key inputs (interest rates ±100 bps, option premium ±20%)
* Summary dashboard with conditional formatting highlighting optimal strategy under different risk preferences

### Out-of-scope:
* Hedge accounting treatment under ASC 815 (effectiveness testing, documentation requirements)
* Partial hedge strategies or dynamic rebalancing over time
* Exotic option structures (collars, knock-outs, range forwards, participating forwards)
* Tax implications and after-tax cash flow analysis
* Counterparty credit risk modeling or CVA adjustments
* Natural hedges from operational EUR-denominated payables elsewhere in the business
* Multi-period or portfolio optimization across multiple FX exposures

---

## 3. Inputs

### Market Data (as of November 7, 2025)
* **Spot Rate (S₀):** 1.0800 USD/EUR
* **1-Year Forward Rate (F):** 1.0875 USD/EUR
* **EUR Interest Rate (r_EUR):** 3.50% per annum (continuous compounding basis)
* **USD Interest Rate (r_USD):** 5.25% per annum (continuous compounding basis)
* **EUR Put Option Premium:** $0.015 per EUR (at-the-money strike)
* **EUR Put Strike Price (K):** 1.0800 USD/EUR
* **Implied Volatility:** 8.5% (for reference; premium already quoted)

### Transaction Parameters
* **Receivable Amount:** EUR 4,500,000
* **Maturity:** T = 1.0 year (365 days)
* **Spot Conversion Cost:** 0.10% of gross proceeds
* **Forward Contract Cost:** 0.05% of gross proceeds

### Scenario Analysis Parameters
* **Future Spot Rate Range (S_T):** 1.0500 to 1.1500 USD/EUR
* **Increment:** 0.01 (generating 101 scenarios)
* **Key Scenarios:** 1.0500 (stress depreciation), 1.0800 (current spot), 1.1500 (stress appreciation)

### Data Sources
* Bloomberg terminal (FX spot, forward points, implied volatility)
* Internal treasury desk (transaction cost schedules)
* Money market rates (Euribor 12M, SOFR 12M)

---

## 4. Workflow / Steps

### Step 1: Input Parameter Setup
Define all market data, transaction parameters, and scenario ranges in a dedicated inputs section. Ensure interest rates are expressed consistently (continuous compounding). Calculate forward points spread: (F - S₀) and interest rate differential: (r_USD - r_EUR).

### Step 2: Strategy A – Unhedged Position Calculation
For each future spot rate scenario S_T from 1.0500 to 1.1500:
* Gross USD proceeds = EUR 4,500,000 × S_T
* Spot conversion cost = Gross proceeds × 0.10%
* Net USD proceeds = Gross proceeds - conversion cost

### Step 3: Strategy B – Forward Contract Hedge Calculation
* Lock in forward rate F = 1.0875 regardless of future spot outcome
* Gross USD proceeds = EUR 4,500,000 × 1.0875 = $4,893,750
* Forward contract cost = Gross proceeds × 0.05%
* Net USD proceeds = $4,893,750 - ($4,893,750 × 0.05%) = $4,891,305 (constant across all scenarios)

### Step 4: Strategy C – Money Market Hedge Calculation
Synthetic forward construction through borrowing and investing:
* Present value of EUR receivable = EUR 4,500,000 ÷ e^(0.035 × 1) = EUR 4,345,447
* Convert borrowed EUR to USD at spot: EUR 4,345,447 × 1.0800 = $4,693,083
* Invest USD for one year at 5.25%: $4,693,083 × e^(0.0525 × 1) = $4,947,100
* Deduct transaction costs on initial conversion
* At maturity: EUR receivable exactly repays EUR loan; harvest USD investment
* Net USD proceeds = Final USD value - all transaction costs
* Compare to forward hedge to verify interest rate parity holds

### Step 5: Strategy D – Currency Put Option Hedge Calculation
For each future spot rate scenario S_T:
* Upfront premium paid = EUR 4,500,000 × $0.015 = $67,500
* Future value of premium = $67,500 × e^(0.0525 × 1) = $71,152
* Option payoff logic:
  - If S_T < 1.0800: Exercise put, convert at strike K = 1.0800
  - If S_T ≥ 1.0800: Let put expire worthless, convert at favorable spot S_T
* Gross proceeds = MAX(S_T, 1.0800) × EUR 4,500,000
* Spot conversion cost = Gross proceeds × 0.10%
* Net USD proceeds = Gross proceeds - conversion cost - $71,152

### Step 6: Breakeven Analysis
Calculate crossover spot rates where strategies yield equal net proceeds:
* **Forward vs. Unhedged:** Solve for S_T where unhedged net proceeds = $4,891,305
* **Option vs. Forward:** Solve for S_T where option net proceeds = $4,891,305 (identifies upside participation threshold)
* **Option Premium Recovery:** Solve for S_T where option proceeds = forward proceeds + original premium ($67,500), showing full premium breakeven

### Step 7: Sensitivity Testing
Construct two-way data tables:
* **Table 1:** Option net proceeds with S_T (rows: 1.05 to 1.15) × Option premium (columns: $0.012, $0.015, $0.018)
* **Table 2:** Money market net proceeds with S_T (rows) × Interest rate differential (columns: 1.25%, 1.75%, 2.25%)

Stress test extreme scenarios:
* EUR collapse to 1.0000 (calculate all strategy proceeds)
* EUR surge to 1.2000 (calculate all strategy proceeds)

### Step 8: Output Generation
Create summary comparison table showing net USD proceeds for all four strategies at 11 key exchange rates (1.05, 1.06, 1.07, 1.08, 1.09, 1.10, 1.11, 1.12, 1.13, 1.14, 1.15). Generate payoff diagram with future spot rate on X-axis (1.05 to 1.15) and net USD proceeds on Y-axis, plotting four strategy lines with breakeven points annotated. Format executive dashboard with conditional highlighting showing optimal strategy under three risk tolerances: conservative (maximum certainty), balanced (moderate cost/flexibility), aggressive (maximum upside).

---

## 5. Expected Outputs

### Table 1: Strategy Comparison Matrix
11 rows (spot scenarios: 1.0500, 1.0600, ..., 1.1500) × 4 columns (Unhedged, Forward, Money Market, Put Option). All values formatted as USD currency with two decimals. Example row at S_T = 1.1000:

| Future Spot | Unhedged | Forward | Money Market | Put Option |
|-------------|----------|---------|--------------|------------|
| 1.1000 | $4,945,500 | $4,891,305 | $4,893,000 | $4,878,850 |

### Table 2: Hedging Cost Summary
Four-row table showing all-in hedging costs:

| Strategy | Guaranteed Minimum | Maximum Upside | All-in Cost |
|----------|-------------------|----------------|-------------|
| Forward | $4,891,305 | $4,891,305 | $2,445 (fees) |
| Money Market | $4,893,000 | $4,893,000 | $0 (synthetic) |
| Put Option | $4,828,153 | Unlimited | $71,152 (premium FV) |

### Table 3: Breakeven Analysis
| Comparison | Breakeven Spot Rate | Interpretation |
|------------|---------------------|----------------|
| Forward vs. Unhedged | 1.0876 | Forward outperforms if EUR depreciates below this level |
| Option vs. Forward | 1.0966 | Option begins outperforming above this spot rate |
| Option Full Recovery | 1.0966 | Spot rate where option premium is fully recovered |

### Chart 1: Payoff Diagram
Line chart with:
* **X-axis:** Future spot rate from 1.0500 to 1.1500 (continuous)
* **Y-axis:** Net USD proceeds from $4,700,000 to $5,150,000
* **Four lines:** Unhedged (upward slope), Forward (horizontal), Money Market (horizontal, nearly overlapping forward), Put Option (kinked at strike 1.0800)
* **Annotations:** Breakeven crossover points marked with vertical dashed lines and callout boxes

### Chart 2: Sensitivity Heatmap
Two-way data table visualized as colored heatmap showing option net proceeds with:
* **Rows:** 11 spot rate scenarios (1.05 to 1.15)
* **Columns:** 3 premium levels ($0.012, $0.015, $0.018)
* **Color gradient:** Red (lower proceeds) to green (higher proceeds)

### Executive Dashboard Summary
One-page summary panel displaying:
* **Recommended Strategy Box:** Conditional formatted cell showing "FORWARD" (if conservative), "PUT OPTION" (if balanced/aggressive)
* **Key Metrics:**
  - Guaranteed Minimum: $4,891,305 (forward) vs. $4,828,153 (option floor)
  - Expected Proceeds: Weighted average across scenarios assuming probability distribution
  - Protection Cost: $71,152 for downside protection with unlimited upside
* **Risk Appetite Alignment:**
  - Conservative → Forward (eliminate uncertainty)
  - Balanced → Put Option (protect downside, retain 80% of upside)
  - Aggressive → Unhedged (full market exposure)

---

## 6. Evaluation Criteria

### Accuracy (40%)
* Money market hedge replicates forward contract within $5,000 (validates interest rate parity)
* Put option payoff correctly applies MAX(S_T, K) logic with zero payoff below strike
* All transaction costs consistently applied across strategies
* Breakeven calculations verified: manual check that strategy proceeds are equal at stated crossover rates

### Clarity (30%)
* All formulas use named ranges (e.g., `Spot_Rate`, `EUR_Amount`, `Forward_Rate`) rather than cell references
* Summary tables formatted with currency symbols, thousand separators, and two decimal precision
* Chart axes labeled with units (USD/EUR for X-axis, USD for Y-axis)
* Executive dashboard uses plain language interpretations, not jargon ("Guarantees $4.89M" vs. "Fixed proceeds via forward arbitrage")

### Reproducibility (20%)
* Another treasury analyst can rebuild the model from this specification without additional guidance
* Inputs clearly separated from calculations (dedicated input cells, no hard-coded constants in formulas)
* Calculation flow follows logical sequence: inputs → strategy calculations → scenarios → outputs
* Sensitivity tables use Excel data table functionality (Table feature) for dynamic recalculation

### Completeness (10%)
* All four strategies fully specified with step-by-step logic
* Breakeven analysis includes both forward vs. unhedged and option vs. forward comparisons
* Outputs include both tabular summaries and visual payoff diagram
* Sensitivity analysis addresses at least two key input variables (option premium, interest rate differential)

---

## 7. AI Prompts (Examples)

### Example 1: Initial Model Build Prompt
```
Build an Excel financial model to analyze FX hedging strategies for a EUR 4,500,000 receivable due in 1 year. 

INPUTS (create dedicated input section):
- Spot rate: 1.0800 USD/EUR
- Forward rate: 1.0875 USD/EUR  
- EUR interest rate: 3.50% continuous compounding
- USD interest rate: 5.25% continuous compounding
- Put option premium: $0.015 per EUR, strike 1.0800
- Transaction costs: 0.10% spot conversion, 0.05% forward contract

CALCULATIONS (four strategies):
1. Unhedged: Convert EUR at future spot S_T, deduct 0.10% cost
2. Forward: Lock 1.0875 rate, deduct 0.05% cost  
3. Money market: Borrow PV of EUR at 3.50%, convert at spot, invest USD at 5.25%
4. Put option: Pay $67,500 premium upfront, MAX(S_T, 1.0800) conversion, deduct costs and premium FV

SCENARIOS: Calculate net USD proceeds for S_T from 1.0500 to 1.1500 in 0.01 increments (101 scenarios)

OUTPUTS:
- Comparison table: 11 key scenarios × 4 strategies showing net USD proceeds
- Breakeven analysis: Spot rates where forward = unhedged and option = forward
- Payoff diagram: Line chart with S_T on X-axis, net proceeds on Y-axis, four strategy lines

Format professionally with currency formatting, named ranges, and clear section headers.
```

### Example 2: Sensitivity Analysis Enhancement Prompt
```
Add sensitivity analysis to the existing FX hedge model:

REQUIREMENT 1 - Option Premium Sensitivity:
Create a two-way data table showing option strategy net proceeds with:
- Rows: Future spot rates 1.05, 1.06, ..., 1.15 (11 scenarios)  
- Columns: Option premiums $0.012, $0.015, $0.018 per EUR
- Apply conditional formatting: green scale for higher proceeds

REQUIREMENT 2 - Interest Rate Sensitivity:
Create a two-way data table showing money market hedge proceeds with:
- Rows: Future spot rates 1.05 to 1.15
- Columns: Interest rate differentials 1.25%, 1.75%, 2.25% (r_USD - r_EUR)
- Show how money market proceeds change as rate differential varies

REQUIREMENT 3 - Stress Testing:
Add stress scenario calculations showing all four strategy proceeds at:
- EUR collapse: S_T = 1.0000 (extreme depreciation)
- EUR surge: S_T = 1.2000 (extreme appreciation)
Display results in a dedicated stress test summary table with max downside and upside for each strategy.

Use Excel's Data Table feature for automatic recalculation. Format tables with clear headers and professional styling.
```

---

**End of Specification**  
*This document provides complete technical requirements for Stage 3 Excel model construction and Stage 4 AI prompt engineering.*
