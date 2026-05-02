FX Hedging Model – Technical Specification

Created by: Aiden Rector
Updated by: Aiden Rector
Date Created: 04/04/2026
Date Updated: 04/17/2026
Version: 1.0
LLM Used: Claude (Anthropic)

Role: Financial Analyst / Treasury Analyst
Audience: CFO / Director of Treasury

Purpose: Provide a structured, quantitative specification for evaluating FX hedging strategies on a EUR-denominated receivable of €500,000, enabling reproducible analysis and improved model design.

---

## 1. Problem Statement

The firm expects to receive €500,000 from a European client on July 3, 2026 — approximately 90 days from the analysis date of April 4, 2026 — creating transaction exposure to fluctuations in the EUR/USD exchange rate. At the current spot rate of 1.09 USD/EUR, this receivable carries a notional USD value of approximately $545,000. If the euro depreciates relative to the U.S. dollar before settlement, the firm's USD proceeds will decline, negatively impacting revenue and cash flow. As a quantitative example, a move from 1.09 to 1.04 USD/EUR would reduce proceeds by approximately $25,000 (–4.6%) with no change to the underlying business transaction. The objective is to protect the USD value of the receivable, evaluate trade-offs between certainty, cost, and upside potential, and provide a quantitative comparison of hedging strategies to support treasury decision-making.

---

## 2. Inputs (Known Variables)

| Variable   | Description                      | Unit        | Value   | Source                               |
|------------|----------------------------------|-------------|---------|--------------------------------------|
| FC_AMT     | Foreign-currency receivable      | EUR         | 500,000 | Client invoice — Stage 1 memo        |
| S0_in      | Spot exchange rate               | USD/EUR     | 1.0900  | Market rate, April 4, 2026           |
| F0_in      | Forward rate (90-day)            | USD/EUR     | 1.0865  | S0 × (1+R_USD×T/360)/(1+R_FC×T/360) |
| R_USD      | USD interest rate                | Annual %    | 5.25%   | Fed funds / T-bill proxy             |
| R_FC       | EUR interest rate                | Annual %    | 3.90%   | ECB deposit facility rate            |
| T_DAYS     | Days to settlement               | Days        | 90      | April 4 → July 3, 2026              |
| K_PUT      | Put option strike                | USD/EUR     | 1.0800  | Near-ATM put; provides USD floor     |
| PREM_PUT   | Put premium per unit of FC       | USD per EUR | 0.0090  | ~0.90% of notional                   |
| K_CALL     | Call option strike               | USD/EUR     | 1.1100  | OTM call; included for comparison    |
| PREM_CALL  | Call premium per unit of FC      | USD per EUR | 0.0060  | ~0.55% of notional; OTM              |

---

## 3. Assumptions & Constraints

- Interest rates are based on macro benchmarks (Fed funds for USD, ECB deposit rate for EUR) rather than live market quotes
- Exchange rates are quoted as USD per EUR throughout
- Day-count convention is ACT/360 for both USD and EUR rates; all interest calculations apply the factor (T_DAYS / 360)
- Covered interest rate parity holds, implying forward hedge ≈ money market hedge proceeds; a residual difference under $50 is acceptable rounding
- No transaction costs, taxes, or bid-ask spreads are included
- No credit or counterparty risk is considered
- The put option is European-style, exercisable only at maturity (July 3, 2026)
- Option premiums are flat per-unit amounts paid upfront in USD at inception; not financed
- Real-world premiums would be priced using Black-Scholes or market-implied volatility models
- The firm is assumed to borrow EUR at the ECB rate (R_FC = 3.90%) with no credit spread
- Market liquidity is assumed to be perfect
- Model is static, evaluating a single terminal exchange rate (S_T) across a defined scenario range

---

## 4. Calculation Flow

**Forward Hedge**
- Lock in the 90-day forward exchange rate (F0_in = 1.0865)
- Multiply FC_AMT by F0_in to obtain fixed USD proceeds
- Output: $543,250 — eliminates FX uncertainty entirely; no premium required

**Money Market Hedge**
- Discount the EUR receivable: divide FC_AMT by (1 + R_FC × T_DAYS/360) to get the EUR borrowing amount
- Convert borrowed EUR to USD at the current spot rate (S0_in = 1.09)
- Invest USD at the USD interest rate for T_DAYS: multiply by (1 + R_USD × T_DAYS/360)
- Compare resulting USD value to forward hedge — expected outcome matches due to covered interest parity

**Option Hedge**
- Calculate total put premium cost: PREM_PUT × FC_AMT = $4,500
- At maturity, apply payoff condition:
  - If S_T < K_PUT (1.08): exercise put, receive K_PUT per EUR
  - If S_T >= K_PUT (1.08): sell EUR at market rate S_T
- Gross proceeds = MAX(K_PUT, S_T) × FC_AMT
- Net proceeds = Gross proceeds − total premium cost
- Result: downside floor protection with retained upside participation

---

## 5. Outputs

| Output            | Description                                       | Format     | Purpose                 |
|-------------------|---------------------------------------------------|------------|-------------------------|
| USD_forward       | Forward hedge proceeds ($543,250)                 | Numeric    | Baseline certainty      |
| USD_mm            | Money market hedge proceeds (≈ $543,250)          | Numeric    | Parity validation check |
| Parity_Check      | Forward − MM; should be ≈ $0                      | Numeric    | Rate-basis error flag   |
| USD_option        | Put hedge net proceeds at various S_T             | Table      | Sensitivity analysis    |
| Sensitivity_Table | USD proceeds vs. S_T across all strategies        | Table      | Risk comparison         |
| Chart_Comparison  | Strategy payoff curves vs. ending spot rate       | Line chart | Visual analysis         |
| Summary_Output    | All strategies vs. unhedged benchmark             | Dashboard  | CFO decision support    |

---

## 6. Model Review — What Worked & What to Improve

**What worked correctly:**
- All three hedge types are implemented with transparent, auditable formula logic using named ranges throughout
- The sensitivity table correctly generates 11 S_T scenarios (±5% in 1% steps) and all strategy payoffs update dynamically
- The parity check confirms forward and MM hedge results agree within rounding tolerance
- Color coding (Yellow = inputs, Blue = assumptions, Green = formulas, Gray = outputs) is consistently applied per assignment specification

**What should be improved in a more rigorous version:**
- Option premiums should be derived from a Black-Scholes model by adding a volatility input (SIGMA) rather than assumed as a flat rate
- A collar structure (buy put + sell call) should be modeled explicitly as a zero- or reduced-cost hedge alternative
- A credit spread input (CREDIT_SPREAD) should be added to R_FC to make the MM hedge borrowing cost realistic
- Sensitivity range should be extended to ±15% for stress-testing alongside the primary ±5% table
- A break-even analysis row showing the S_T at which each hedge outperforms the unhedged position would add direct decision support for the CFO

---

## 7. Sensitivity Plan

The sensitivity analysis evaluates the firm's exposure to exchange rate uncertainty by varying the terminal spot rate S_T over a ±5% range around the current spot rate (S0_in = 1.09) in 1% increments, producing 11 scenarios from 1.0355 to 1.1445. For each scenario, USD proceeds are calculated across all strategies — Unhedged, Forward, MM Hedge, Put Hedge (net), and Call Hedge (net) — enabling direct comparison under different market conditions. The baseline row (0% change, S_T = 1.09) is highlighted for reference. The accompanying line chart plots all five strategies against S_T, with the most analytically useful comparisons being Unhedged vs. Put Hedge (cost of optionality vs. downside protection), Forward vs. Unhedged (opportunity cost of hedging if EUR appreciates), and Forward vs. Put (the core decision between certainty and floor-plus-upside).

---

## 8. Limitations & Next Steps

The model is subject to several limitations, including the exclusion of exchange rate volatility, dynamic hedging strategies, transaction costs, and credit risk. It also relies on a static single-period framework and does not incorporate stochastic modeling or advanced option pricing techniques. Additional exclusions include accounting treatment (ASC 815 hedge designation), tax implications of FX gains or losses, and correlation with other EUR-denominated exposures in the firm's portfolio.

The next step is to translate this specification into a structured AI prompt for Stage 4, which will enhance the model's analytical depth, incorporate additional scenario analysis, and deliver a data-driven hedge recommendation to the CFO. The named ranges defined here (FC_AMT, S0_in, F0_in, etc.) will serve as direct inputs to the AI prompt, the calculation flow in Section 4 will guide formula generation, and the model review notes in Section 6 will instruct the AI to build the improved version rather than a direct replica.

---

*Prepared by: Aiden Rector | Shidler College of Business | April 2026*
*Stage 2 Model File: Rector-Aiden-stage2-model.xlsx*
