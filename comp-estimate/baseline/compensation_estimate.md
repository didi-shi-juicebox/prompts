Estimate yearly compensation for this candidate, broken down by component and in total, based on their profile, market data and compensation priors.
If the profile's compensation should exceed the available data points (e.g. because of seniority or niche), adjust accordingly.
Conversely, the profile may require lower compensation than available data points (e.g. Site Reliability Engineer and DevOps should be lower than pure software engineers).

Be extremely conservative in using very senior levels (e.g. ict5+ at Apple, L7+ at Amazon, L6+ at Google, 66+ at Microsoft) as these are rare even amongst long-tenured employees.
To reference these, the profile data MUST clearly indicate an ultra-senior role, like if they lead an org or they have a Staff/Distinguished title.

You MUST give every amount as an integer in USD per year, rather than English.

Estimate each component as a min-max range, then the total:
- base: fixed salary, or hourly rate x expected hours.
- stock: equity annualized over its vesting period. Use 0 when there is none.
- bonus: cash bonus, profit share, and other variable cash that is not commission. Use 0 when there is none.
- commission: sales commission. Use 0 when there is none.
- The total min must equal the sum of the component mins, and the total max the sum of the component maxes.
- When the market data gives only "Additional Pay" without a stock/bonus split, split it using the compensation priors and say so in Assumptions.

Confidence
Give estimate_confidence as a decimal from 0 to 1 with two decimal places. It is how likely the candidate's actual total compensation falls inside your range. Base it on how well the market data covers this candidate, not on how sure you feel:
- 0.80-1.00: several rows match the candidate's job family, level and country, and the estimate sits within or next to them.
- 0.50-0.79: at least one row matches the job family and country, but the level is one step off, the city differs, or the rows rest on few samples. Ordinary adjustments like these are not extrapolation.
- 0.30-0.49: you extrapolated (see "Extrapolating beyond the data"), and the rows still tell you how this company pays.
- 0.00-0.29: the estimate rests mostly on general knowledge: the rows say little about this candidate, or the title is vague or the work history is missing.
Confidence is below 0.50 exactly when the Explanation starts with "Extrapolated:". Use the whole scale; do not default to the middle.
Illiquid equity does not lower confidence by itself.

Output format:
min_base_yearly_compensation: [number]
max_base_yearly_compensation: [number]
min_stock_yearly_compensation: [number]
max_stock_yearly_compensation: [number]
min_bonus_yearly_compensation: [number]
max_bonus_yearly_compensation: [number]
min_commission_yearly_compensation: [number]
max_commission_yearly_compensation: [number]
estimate_confidence: [decimal from 0.00 to 1.00]
min_total_yearly_compensation: [number]
max_total_yearly_compensation: [number]

Explanation: [concise, 1-3 sentences on key factors; start with "Extrapolated:" only when the estimate meets the definition of extrapolation below]
Assumptions: [one short sentence on any component mapping, split or illiquid equity you assumed, or "none"]
Compensation examples used: [list of compensation entry #, comma separated]

<edge_cases>
The market data is often thin. Handle these cases as follows.

Extrapolating beyond the data
An estimate is extrapolated only when at least one of these holds:
- Wrong job family: no usable row is in the candidate's job family (e.g. only engineering rows for a sales or support role). Use the rows only as a signal of how the company pays, then adjust to the candidate's function.
- Far level: the nearest row in the candidate's job family is two or more levels away from the candidate. Scale from it. Seniority must be supported by the profile (scope, title, tenure), not assumed.
- Wrong country: every usable row is from a different country than the candidate.
- Outside the rows: your range lies entirely above or entirely below every row you cite.
It is NOT extrapolation when a row matches the job family and you adjust one level up or down, move between cities in the same country, pick a point within the rows' range, rely on a row with few samples, or split or add pay components using the compensation priors. Describe those adjustments in the Explanation without the "Extrapolated:" prefix.
- When you extrapolate, the Explanation MUST start with "Extrapolated:" and say what you extrapolated from and why. Still list the nearest rows under "Compensation examples used".
- Sparse data: one or two usable rows, or rows based on very few samples, are a weak signal. Use your own knowledge of the market for this role, company type and location to set the estimate, and widen the range. Sparse data by itself lowers confidence but is not extrapolation.
- Never say or imply that you searched the web or consulted a source you were not given.

Reading the rows
- A stock or bonus of 0 means "not reported", not "none". Add that component only when the comp priors clearly imply it for this industry and company type, and say so in Assumptions.
- Variable-pay roles (sales, recruiting, trading): when rows show base only, treat them as base and add on-target variable pay as commission. Mark it as assumed.
- Rows from another city or country: adjust to the candidate's local market and say so. Never apply another country's rows unadjusted.
- All amounts are USD per year, the currency of the market data, even when the candidate is outside the US. Never present the estimate in local currency.
- Private-company equity: report stock at paper value, with no illiquidity discount. Note in Assumptions that the equity is illiquid. Do not mention liquidity in the Explanation.

Reading the profile
- Abbreviated, non-English or multi-role titles: resolve to the primary role using the company and work history.
- Vague titles or missing work history: place the level from what is available and widen the range.
- Intern, part-time, contract, advisor or board roles: annualize at a stated assumption. Do not price them as full-time staff.
</edge_cases>

<comp_priors>
Structure by industry (map all pay into base / stock / bonus / commission):

Tech
- Public: base + RSU (annualize over vest, typically 4y) + bonus 10-20%
- Private late-stage: report equity at paper value (annualized grant value at the latest known valuation); do not discount it
- Early startup: below-market base, usually no bonus; report options at paper value

Finance
- Banking/trading/buy-side: bonus may be 50-150%+ of base; carry at PE/VC → stock (flag as highly uncertain)

Law
- Large firms: associate base follows a lockstep scale by class year, plus year-end bonus; level = class year, not title
- Partners: equity partners paid via profit share (→ bonus); non-equity partners salaried + bonus
- In-house: base + bonus; equity if at a tech/public company
- Small firm, government, public interest: substantially lower, base-heavy

Healthcare
- Physicians: specialty dominates (surgical/procedural >> primary care); pay is salary or productivity-based (wRVU) → productivity pay maps to bonus; private-practice owners also receive distributions (→ bonus)
- Academic medicine: lower than private practice
- Residents/fellows: fixed stipend by training year
- Nurses/allied health: hourly; shift differentials and overtime add materially (→ bonus); often union scales; travel roles carry stipends
- Pharma/biotech: base + bonus + equity; pre-revenue biotech equity is option-heavy and speculative
- Medical device sales: commission-heavy

Consulting & accounting
- Base + performance bonus with clear level tiers (analyst → partner); partners paid via profit share (→ bonus)

Government & public sector
- Published pay scales by grade/step + locality; no bonus or equity
- Note pension as non-cash benefit, but exclude it from the estimate

Education & academia
- K-12: step-and-lane schedules (years + degree)
- Faculty: 9-10 month base; summer salary from grants (→ bonus)

Sales-driven (real estate, insurance, recruiting)
- Commission-dominant; may have no base; high variance → widen ranges

Hourly sectors (retail, hospitality, manufacturing, trades, energy)
- Hourly base x expected hours; overtime, shift and field premiums, tips → bonus; union scales where applicable

Nonprofit
- Base-heavy, generally below for-profit equivalents; rarely bonus or equity

Mapping rule: if pay doesn't fit a category, use the closest one and state it in assumptions (e.g. "profit share reported as bonus").
</comp_priors>

<profile>
{{profile_data}}
</profile>


<market_data>
{{compensation_data}}
</market_data>

