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
The case guidance below may narrow this scale for the candidate's situation; stay inside the range it gives.

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

<rules>
Extrapolating beyond the data
An estimate is extrapolated only when at least one of these holds:
- Wrong job family: no usable row is in the candidate's job family (e.g. only engineering rows for a sales or support role).
- Far level: the nearest row in the candidate's job family is two or more levels away from the candidate.
- Wrong country: every usable row is from a different country than the candidate.
- Outside the rows: your range lies entirely above or entirely below every row you cite.
It is NOT extrapolation when a row matches the job family and you adjust one level up or down, move between cities in the same country, pick a point within the rows' range, rely on a row with few samples, or split or add pay components using the compensation priors. Describe those adjustments in the Explanation without the "Extrapolated:" prefix.
- When you extrapolate, the Explanation MUST start with "Extrapolated:" and say what you extrapolated from and why. Still list the nearest rows under "Compensation examples used".
- Seniority must be supported by the profile (scope, title, tenure), not assumed.
- Never say or imply that you searched the web or consulted a source you were not given.

Reading the rows
- A stock or bonus of 0 means "not reported", not "none". Add that component only when the comp priors clearly imply it for this industry and company type, and say so in Assumptions.
- Variable-pay roles (sales, recruiting, trading): when rows show base only, treat them as base and add on-target variable pay as commission. Mark it as assumed.
- All amounts are USD per year, the currency of the market data, even when the candidate is outside the US. Never present the estimate in local currency.
- Private-company equity: report stock at paper value, with no illiquidity discount. Note in Assumptions that the equity is illiquid. Do not mention liquidity in the Explanation.
</rules>

<case_guidance>
{{> bucket_guidance}}
</case_guidance>

{{> comp_priors}}

<profile>
{{profile_data}}
</profile>


<market_data>
{{compensation_data}}
</market_data>
