# AI PROMPT: FX HEDGING SPREADSHEET MODEL

## GOAL
Create a professional Excel spreadsheet that models forward contracts, money market hedges, and currency option hedges for a EUR receivable. The model must calculate net USD receipts for each strategy and include sensitivity analysis showing outcomes across different future spot rates.

## CONTEXT FILES (GitHub)
Use the logic and specifications from:
- https://github.com/example/fin321/stage2-spec.md
- https://github.com/example/fin321/stage3-skeleton.xlsx

## CONTEXT
I am a US-based company expecting to receive EUR 2,850,000 in 360 days. I need to evaluate four hedging strategies:
1. Unhedged (convert at future spot rate)
2. Forward contract hedge
3. Money market hedge
4. Currency option hedges (put and collar)

The model should help determine which strategy maximizes expected USD receipts while managing downside risk.

## INPUT VARIABLES (Real Market Data)

FC_AMT = 2,850,000  
Spot = 1.0934  
Forward = 1.0875  
r_USD = 5.10%  
r_EUR = 2.71%  
K_put = 1.0900  
K_call = 1.0900  
Premium_put = 0.017  
Premium_call = 0.022  
T_days = 360

### Detailed Variable Breakdown
- **FC_AMT** = 2,850,000 EUR (foreign currency receivable)
- **T_DAYS** = 360 days (time to receipt)
- **T_YRS** = 1.0 years (360/360 day count)

### Current Market Rates (as of prompt execution date)
- **S0_in** = 1.0934 USD/EUR (current spot rate)
- **F0_in** = 1.0875 USD/EUR (360-day forward rate)
- **R_USD** = 0.0510 or 5.10% (US dollar interest rate, annual)
- **R_FC** = 0.0271 or 2.71% (EUR interest rate, annual)

### Option Contract Terms
- **K_PUT** = 1.0900 USD/EUR (put option strike price)
- **K_CALL** = 1.0900 USD/EUR (call option strike price)
- **PREM_PUT** = 0.0170 USD/EUR (put option premium per EUR)
- **PREM_CALL** = 0.0220 USD/EUR (call option premium per EUR)

## SPREADSHEET REQUIREMENTS

Use named ranges.  
Color code inputs yellow, formulas green, outputs gray, assumptions blue.  
Include forward hedge, money market hedge, option hedge, and sensitivity analysis.

### Detailed Specifications

### Named Ranges (Must Use These Exact Names)
Define the following named ranges for all input variables:
- `FC_AMT` (foreign currency amount)
- `S0_in` (spot rate at inception)
- `F0_in` (forward rate at inception)
- `R_USD` (US dollar interest rate)
- `R_FC` (foreign currency interest rate)
- `K_PUT` (put option strike)
- `K_CALL` (call option strike)
- `PREM_PUT` (put premium per unit)
- `PREM_CALL` (call premium per unit)
- `T_DAYS` (days to maturity)
- `T_YRS` (years to maturity)

### Color Coding System (Required)
- **YELLOW** = Input cells (user can modify)
- **BLUE** = Assumptions (market data, fixed parameters)
- **GREEN** = Formula cells (calculated values)
- **GRAY** = Output cells (final KPIs and results)

### Worksheet Structure
Create a single worksheet with clearly labeled sections:

#### Section 1: INPUT VARIABLES (Top of sheet)
- Yellow cells: FC_AMT, T_DAYS, T_YRS
- Blue cells: S0_in, F0_in, R_USD, R_FC, K_PUT, K_CALL, PREM_PUT, PREM_CALL
- All cells must use named ranges

#### Section 2: HEDGE STRATEGY CALCULATIONS

**Strategy A: Unhedged Position**
- Green cell: Calculate expected USD receipt = FC_AMT × S0_in
- Note: Actual receipt depends on future spot rate ST

**Strategy B: Forward Contract Hedge**
- Green cell: USD_receipt_forward = FC_AMT × F0_in
- This locks in the forward rate today

**Strategy C: Money Market Hedge (3 Steps)**
1. Green cell: Borrow_EUR_today = FC_AMT / (1 + R_FC × T_YRS)
   - Borrow present value of EUR receivable
2. Green cell: Convert_to_USD_today = Borrow_EUR_today × S0_in
   - Convert borrowed EUR to USD at spot
3. Green cell: Future_USD_value = Convert_to_USD_today × (1 + R_USD × T_YRS)
   - Invest USD and calculate future value
   - This amount repays EUR loan and equals net USD receipt

**Strategy D1: Put Option Hedge**
- Green cell: Premium_cost_total = FC_AMT × PREM_PUT
- Green cell: Premium_FV = Premium_cost_total × (1 + R_USD × T_YRS)
- Payoff logic (must use IF formula):
  - If ST < K_PUT: Exercise put, receive FC_AMT × K_PUT
  - If ST ≥ K_PUT: Let expire, receive FC_AMT × ST
- Green cell: Net_USD_receipt = Payoff - Premium_FV

**Strategy D2: Collar Hedge (Buy Put, Sell Call)**
- Green cell: Net_premium = (PREM_PUT - PREM_CALL) × FC_AMT
- Green cell: Net_premium_FV = Net_premium × (1 + R_USD × T_YRS)
- Payoff logic (must use nested IF formula):
  - If ST < K_PUT: Exercise put, receive FC_AMT × K_PUT
  - If K_PUT ≤ ST ≤ K_CALL: Convert at spot, receive FC_AMT × ST
  - If ST > K_CALL: Call exercised against you, receive FC_AMT × K_CALL
- Green cell: Net_USD_receipt = Payoff - Net_premium_FV

#### Section 3: SENSITIVITY ANALYSIS TABLE
Create a data table showing net USD receipts for all strategies across future spot rates:

**Table Structure:**
- Row headers (Gray): Strategy names (Unhedged, Forward, MM Hedge, Put, Collar)
- Column headers (Blue): Future spot rates from 0.95×S0_in to 1.05×S0_in in 1% increments
  - Example: 1.0387, 1.0497, 1.0607, 1.0717, 1.0827, 1.0934, 1.1041, 1.1151, 1.1261, 1.1371, 1.1481
- Cell values (Green): Calculate net USD receipt for each strategy at each spot rate
- Use Excel Data Table feature or replicate with formulas

#### Section 4: KEY PERFORMANCE INDICATORS (Bottom of sheet)
Gray cells showing:
- Best strategy at current spot (max USD receipt)
- Worst-case scenario for each strategy (min value from sensitivity table)
- Break-even spot rates where strategies have equal outcomes

## MODEL LOGIC

Insert pseudocode from Stage 2 spec.

### Detailed Pseudocode

```
// INPUTS
FC_AMT = 2,850,000
S0 = 1.0934
F0 = 1.0875
r_USD = 5.10%
r_EUR = 2.71%
T = 1 year

// FORWARD HEDGE
USD_forward = FC_AMT × F0

// MONEY MARKET HEDGE
PV_EUR = FC_AMT / (1 + r_EUR × T)
USD_today = PV_EUR × S0
USD_MM = USD_today × (1 + r_USD × T)

// PUT OPTION HEDGE
Premium_put_FV = FC_AMT × PREM_PUT × (1 + r_USD × T)
IF ST < K_PUT THEN
    Payoff = FC_AMT × K_PUT
ELSE
    Payoff = FC_AMT × ST
END IF
Net_USD_put = Payoff - Premium_put_FV

// COLLAR HEDGE
Net_prem = (PREM_PUT - PREM_CALL) × FC_AMT
Net_prem_FV = Net_prem × (1 + r_USD × T)
IF ST < K_PUT THEN
    Payoff = FC_AMT × K_PUT
ELSE IF ST <= K_CALL THEN
    Payoff = FC_AMT × ST
ELSE
    Payoff = FC_AMT × K_CALL
END IF
Net_USD_collar = Payoff - Net_prem_FV

// SENSITIVITY TABLE
FOR each ST in [0.95×S0 to 1.05×S0]:
    Calculate all strategies at that ST
    Store in table
END FOR
```

## FORMATTING & LAYOUT

1. **Professional appearance**: Use clear section headers, borders, and alignment
2. **Cell borders**: Use light gray borders to separate sections
3. **Number formatting**:
   - Currency: $#,##0.00
   - Percentages: 0.00%
   - Exchange rates: 0.0000
4. **Font**: Use Calibri or Arial, 11pt for data, 12pt bold for headers
5. **Column widths**: Auto-fit or set to readable widths

## VERIFICATION REQUIREMENTS

Validate parity and confirm all named ranges.

### Detailed Verification Steps

The AI must validate:

1. **Interest Rate Parity Check**:
   - Green cell: Calculate theoretical forward = S0 × (1 + R_USD × T) / (1 + R_FC × T)
   - Green cell: Compare to actual F0_in
   - Gray cell: Show parity difference (should be near zero for efficient markets)

2. **Money Market vs Forward Equivalence**:
   - Green cell: Calculate difference = USD_MM - USD_forward
   - Gray cell: Show percentage difference (should be minimal due to parity)

3. **Named Range Audit**:
   - Confirm all 11 named ranges exist and reference correct cells
   - Provide list of named ranges with cell addresses

4. **Formula Mapping**:
   - Document all major formulas used in each calculation
   - Show cell references for traceability

## OUTPUT REQUIREMENTS

1. Return a downloadable `.xlsx` file with all specifications implemented
2. Include a summary comment box on the worksheet explaining:
   - Purpose of the model
   - How to use the sensitivity table
   - Which strategy is recommended based on risk preferences
3. Ensure all formulas are visible in formula bar (not hardcoded values)
4. Test file by changing one input and confirming all dependent cells update

## EXPORT FORMAT

Return a downloadable Excel file.

### Export Specifications

Generate a complete, working Excel file (.xlsx format) that can be:
- Downloaded immediately
- Opened in Microsoft Excel or Google Sheets
- Modified by the user for their own scenarios
- Audited for accuracy and compliance

---

**Execution Note**: This prompt should be run with current market data. Update S0_in and interest rates to reflect real-time market conditions at the time of model generation.
