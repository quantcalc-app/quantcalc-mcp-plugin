# QuantCalc Retirement Engine — Claude plugin

Gives Claude the retirement engine and the federal and state tax engines behind
[quantcalc.app](https://quantcalc.app), so a projection or a 2026 tax figure is
computed rather than estimated.

A language model cannot run thousands of correlated return paths against a
spending plan and get the odds right. It can ask something that can.

**[Privacy policy](https://quantcalc.app/privacy.html)** ·
**[Terms](https://quantcalc.app/terms.html)** ·
**Support: [hello@quantcalc.app](mailto:hello@quantcalc.app)** ·
**[Documentation](https://quantcalc.app/advisors/mcp/)**

## Install

Install from the Claude plugin directory. The plugin installs switched off, and
you enable it when you want it. Step-by-step instructions are in
[SETUP.md](SETUP.md). To use the server without the plugin, add it as a custom
connector:

```
https://mcp.quantcalc.app
```

## What's in the plugin

- **Connector:** the hosted QuantCalc engine over streamable HTTP. No account or
  sign-in is needed.
- **Skill `retirement-projections`:** loads when a plan, withdrawal rate or
  success rate comes up. It keeps the assumptions attached to every number and
  flags differences that fall inside the margin of error.
- **Skill `tax-calculations`:** loads when a 2026 tax question comes up — a
  return, a paycheck, an RMD, a Roth conversion, IRMAA, capital gains,
  estimated payments or the ACA credit. It maps each question to the tool that
  computes it and keeps the defaults and the not-modelled items with the figure.
- **Skill `setup`:** connection steps, what a licence key changes, and
  troubleshooting.

## Tools

Twenty-two tools. "Free" means the tool computes without a licence key;
"Licence" means it needs a connected QuantCalc PRO licence or Tax Season Pass
(PRO only where stated).

Since server 1.10.0 the connector lists tools by what the session can run.
With no key connected it lists the eleven that compute without one: the five
free projection tools, `calculate_rmd`, `irmaa_check`,
`retirement_plan_deferral_savings`, `ira_deduction_and_roth_limit`,
`standard_deduction_vs_itemizing` and `aca_premium_tax_credit` (coverage year
2027). A tool that is free in part is listed with the inputs its free part
takes. With a key connected it lists all twenty-two.

### Retirement projections

| Tool | Key | Returns |
|---|---|---|
| `run_retirement_projection` | Free | Success rate, ending-portfolio distribution, and the assumptions that produced them |
| `run_tax_aware_projection` | Free | The recommended withdrawal order and Roth-conversion programme, its present-value advantage and range against traditional-first, and the after-tax success rate, with federal and state tax, RMDs, IRMAA and Social Security taxation paid from the portfolio. With `move_state` (`state`, `year`, `month`) it plans a move to another state: the old state's tax before the move year, a part-year return in each state in the move year, the new state's after it, and the lifetime state tax compared with staying and with moving now |
| `compare_return_assumptions` | Free | The same plan under each published capital market assumption set |
| `list_return_assumption_sources` | Free | Which assumption sets the engine carries, and what each publisher provides |
| `explain_methodology` | Free | What the engine models and what it deliberately leaves out |
| `optimize_social_security_claiming` | PRO | The tax-aware plan run at every Social Security claiming age from 62 to 70 on the same simulated markets: each age's success rate and median paired difference against the plan, and the best age with its 10th–90th percentile range |

### 2026 tax returns and calculators

| Tool | Key | Returns |
|---|---|---|
| `estimate_2026_tax_return` | Licence | A 2026 federal and state return estimate: the refund or amount owed, the main Form 1040 lines, the state tax, every default assumed and what is not modelled. Takes self-employment (Schedule C) profit for the filer and the spouse in `self_employment_income` and `spouse_self_employment_income`: Schedule SE, the half-SE-tax deduction and the qualified business income deduction are figured |
| `state_tax_in_every_state` | Licence | One household's 2026 state income tax in all 50 states and DC, ranked from lowest to highest, with each state's difference from the state it lives in now, the federal tax (the same in every state), the county taxes included, every default assumed and what is not modelled. Military retirement pay goes in `military_pension` (not `pension`): a taxable pension on the federal return, and in each state that state's own military retirement rule (exempt, capped, age-tested or credited); on a joint return it follows `spouse_split` like the pension |
| `calculate_2026_income_tax` | Licence | 2026 federal income tax and, given a state, state income tax from wages (with the overtime and tips deductions), dependents and other common income; the Social Security and Medicare tax on the wages, the effective and marginal rates, take-home pay, and, given withholding, the refund or amount owed |
| `pretax_contribution_savings` | Licence | What a health FSA or HSA contribution saves in federal income tax, Social Security and Medicare tax and state tax, for 2026 or 2027; payroll or direct HSA contributions; the after-tax cost and an optional FSA forfeiture estimate |
| `business_deduction_tax_savings` | Licence | What a self-employed business deduction (2026 business miles at the half-year rates, or a dollar expense) saves in income tax, self-employment tax, earned income credit and state tax |
| `retirement_plan_deferral_savings` | Free limit; licence for the tax | For a 401(k), 403(b), 457(b), TSP or SIMPLE IRA in 2026 or projected 2027: the contribution limit at the saver's age with the age-50 and age-60–63 catch-ups, and the part that must go in as Roth — free; the federal income tax saved (and the 2026 Saver's Credit), $0 of Social Security and Medicare tax saved, and the state tax saved (none in Pennsylvania, and in New Jersey only for a 401(k)) — licence |
| `ira_deduction_and_roth_limit` | Free worksheets; licence for the tax | Given MAGI, IRS Publication 590-A's worksheets line by line for 2026 or projected 2027: how much of a traditional IRA contribution is deductible and how much may go into a Roth IRA, including the spousal IRA rule — free; MAGI worked out from income, and the federal and state tax the deduction saves — licence |
| `standard_deduction_vs_itemizing` | Free deduction; licence for the comparison | The 2026 or 2027 standard deduction piece by piece (basic amount, age-65 and blind additions, a dependent's limit) and the senior deduction's rule — free; the senior deduction after its phase-out, the itemized total, the federal tax both ways and which is better — licence |
| `calculate_rmd` | Free RMD; licence for the tax | The 2026 or 2027 required minimum distribution, its divisor and deadline, the SECURE 2.0 start age, the first-year April 1 option and a multi-year projection — free. With a filing status and the year's other income it adds the federal and state tax the RMD adds, the Social Security it makes taxable, the IRMAA effect and what a QCD would take off — licence |
| `roth_conversion_tax_cost` | Licence | The federal and state tax a 2026 Roth conversion adds, its share of the amount converted and the cost of the next $1,000; the room left in the bracket and below the next IRMAA tier; flags for an IRMAA tier crossing, the tax torpedo and the senior-deduction phase-out |
| `qcd_tax_savings` | Licence | A 2026 gift made as a qualified charitable distribution compared with the same gift in cash: the federal and state tax and the IRMAA surcharge saved |
| `irmaa_check` | Free | The Medicare IRMAA tier a MAGI sets: the official 2026 tier and the 2027 and 2028 tiers on projected tables (labelled projected), with the monthly Part B and Part D surcharge per person, the annual total, and the distance to the next tier |
| `social_security_taxable_amount` | Licence | How much of 2026 Social Security is taxable, the tax the benefits cost, the marginal rate on the next dollar of other income and where the household sits in the tax torpedo |
| `capital_gains_tax` | Licence | The 2026 federal and state tax on short- and long-term gains or losses given other income: the room left in the 0% band and before the net investment income tax, the loss deducted and carried to 2027, and, given `harvest_loss`, the tax a harvested loss saves |
| `estimated_tax_payments_2026` | Licence | The 2026 safe-harbor required payment and per-installment amount, the four due dates, the Form 2210 underpayment and penalty for the payments as made, and, with the January installment open, the amount still due and the penalty if it is paid as an estimate, withheld in December or not paid |
| `aca_premium_tax_credit` | Free 2027 benchmark and cliff; licence for the credit | For 2027 coverage: the county benchmark premium from the CMS plan-year-2027 files, the poverty line, the 400% cliff income and the headroom below it — free; the 2027 credit and the credit lost by being $1 over the cliff — licence. For 2026 coverage: the credit at the state-level or a given benchmark, the expected contribution, the Medicaid and coverage-gap regimes and the cliff — licence |

Since server 1.9.0 these tools take `tax_year` 2027, on the IRS's published
2027 amounts (Rev. Proc. 2026-35): `calculate_2026_income_tax`,
`pretax_contribution_savings`, `roth_conversion_tax_cost`, `qcd_tax_savings`,
`social_security_taxable_amount`, `capital_gains_tax`,
`standard_deduction_vs_itemizing`, the tax on an RMD in `calculate_rmd`, and
`business_deduction_tax_savings` for a dollar expense (federal only; the 2027
mileage rate is not published). A state line in a 2027 result stays on that
state's 2026 rules and is labelled so. The 2027 contribution limits and IRA
income ranges are projections until the IRS publishes its limits notice, and
are labelled as projections. `estimate_2026_tax_return`,
`state_tax_in_every_state` and `estimated_tax_payments_2026` are 2026 only.

All twenty-two are read-only: they compute and return, and change nothing. Inputs
are not kept after the run, except that a failed request is kept for two days to
diagnose it.

## What comes back with every number

A success rate is meaningless without its assumptions, and a summary will drop a
caveat it was not handed explicitly. So each result states:

- the return model that actually ran — reported by the engine, not by the request;
- the number of paths and the real trial count behind the rate;
- the income it assumed, **including when it assumed none**;
- whether a correlation matrix had to be adjusted before running;
- a plain warning when a run is not precise enough to show a client;
- for the tax-aware tool: the spending convention (after tax — what the household
  keeps), every default assumed for an input that was not given, and the tax
  items the engine does not model.
- for the every-state ranking: the engine versions, the federal tax it used for
  every state, the county taxes the state figures include, and every default
  assumed (spouse's age, how a couple's income is split, a private pension).
- for the 2026 tax calculators: every default assumed for an input that was not
  given, what is projected rather than published (the 2027 and 2028 IRMAA
  tables, a 2027 tax year), and that the figure is an estimate, not tax advice.

## Licence key

Optional. Without one, projections run at 2,000 paths (the 95% interval around a
success rate is about ±1.5 points) and the tax-aware tool returns the first
retirement year of its year-by-year schedule. A QuantCalc PRO key raises
that to 10,000 paths (about ±0.7) and returns the full schedule. The tax engine
inside the projections — withdrawal order, Roth conversions, state tax — needs
no key.

The 2026 tax tools run on QuantCalc's licensed federal and state tax APIs and
need a PRO or Tax Season Pass key: `estimate_2026_tax_return`,
`state_tax_in_every_state`, `calculate_2026_income_tax`,
`pretax_contribution_savings`, `business_deduction_tax_savings`,
`roth_conversion_tax_cost`, `qcd_tax_savings`,
`social_security_taxable_amount`, `capital_gains_tax`,
`estimated_tax_payments_2026`, the tax part of `calculate_rmd` and the credit in
`aca_premium_tax_credit`. Without a key the tools that only compute with one
are not listed; the free calculators for the same questions are on the website
(for example <https://quantcalc.app/2026-tax-return-calculator/>, and the full
list at <https://quantcalc.app/tools/>). `irmaa_check`, the RMD
itself, the 2027 ACA benchmark, cliff and headroom, the retirement-plan
contribution limit by age, the IRA deduction and Roth limit worksheets, and the
standard deduction amount are free; the tax each of those saves needs a key.
`optimize_social_security_claiming` needs a PRO key. The same key also unlocks
the portfolio optimizer, glide paths, custom capital market assumptions and
multi-period planning in the [QuantCalc app](https://quantcalc.app/app.html).
Those features are not tools in this plugin.

Set it in the plugin's configuration. It is sent as a connection header and
never passes through the conversation.

## Privacy

The server receives only the inputs of each tool call, never the conversation.
A random session identifier is used to count runs for each installation. A
licence key is removed before anything is logged. Details are in section 6 of
the [privacy policy](https://quantcalc.app/privacy.html).

## Not advice

QuantCalc is calculation software, not financial advice. Methodology and the
source of every figure: <https://quantcalc.app/methodology.html>. Tax scope and
its explicit exclusions: <https://quantcalc.app/tax-methodology/>.

## Licence

MIT — see [LICENSE](LICENSE). This repository contains the plugin manifest,
skills and documentation. The engine itself is a hosted service, published to the Official
MCP Registry as `app.quantcalc/retirement-engine` under a DNS-verified
namespace.
