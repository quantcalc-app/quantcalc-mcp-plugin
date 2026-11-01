---
name: tax-calculations
description: How the QuantCalc connector's 2026 tax tools map to everyday tax questions — a return or refund estimate, income tax and take-home pay, FSA/HSA and business-deduction savings, RMDs, Roth conversion cost, QCDs, IRMAA tiers, taxable Social Security, capital gains and loss harvesting, estimated payments and the ACA premium tax credit — which of them are free and which need a licence, and what to carry with each figure. Relevant whenever a tax amount for 2026 (or 2027 coverage or premiums) is being discussed.
---

# 2026 tax calculations

A tax figure depends on brackets, phase-outs, credits and state rules that
interact; a figure worked out in prose that sounds right is the failure mode
to avoid, because someone may file, pay or convert on it. Each tool below files
the return (or the relevant part of it) on QuantCalc's federal and state tax
engines and reports the difference, so quote what it returns.

## Which tool answers which question

| Question | Tool | Key |
|---|---|---|
| What will I owe or get back for 2026; a line of Form 1040 | `estimate_2026_tax_return` | Licence |
| Income tax, payroll tax, take-home pay, refund from withholding; overtime and tips deductions; dependents | `calculate_2026_income_tax` | Licence |
| What an FSA or HSA contribution saves (2026, or projected 2027) | `pretax_contribution_savings` | Licence |
| What business miles or a business expense save a self-employed person | `business_deduction_tax_savings` | Licence |
| How much is my required minimum distribution | `calculate_rmd` | Free (the tax on it: licence) |
| What a Roth conversion costs this year, and how much bracket or IRMAA room is left | `roth_conversion_tax_cost` | Licence |
| A QCD or a cash gift to charity | `qcd_tax_savings` | Licence |
| Which Medicare IRMAA tier a MAGI lands in | `irmaa_check` | Free |
| How much of Social Security is taxable; the tax torpedo | `social_security_taxable_amount` | Licence |
| Tax on capital gains, the 0% band, loss harvesting and carryovers | `capital_gains_tax` | Licence |
| Estimated payments, the safe harbor, due dates and the underpayment penalty | `estimated_tax_payments_2026` | Licence |
| The ACA premium tax credit, the benchmark and the 400% cliff | `aca_premium_tax_credit` | 2027 benchmark, cliff and headroom free; the credit: licence |
| State income tax in every state, ranked | `state_tax_in_every_state` | Licence |

"Licence" means a connected QuantCalc PRO licence or Tax Season Pass. Without
one the tool computes nothing and its reply names the free web page that
answers the same question; relay that, rather than estimating the figure in its
place. A multi-year question — which account to draw first, a conversion
programme, a move between states — belongs to `run_tax_aware_projection`
(see the `retirement-projections` skill), not to a one-year calculator.

## What the inputs mean

- **Filing status and ages** move the answer more than most inputs (the age-65
  deductions, the senior deduction, the RMD start age). Ask for them rather than
  guessing; the tools state every default they assumed.
- **Social Security** is the gross benefit; the engine figures the taxable part.
- **Self-employment profit** (`self_employment_income`,
  `spouse_self_employment_income` on `estimate_2026_tax_return`; `net_profit`
  on `business_deduction_tax_savings`) is Schedule C profit after business
  expenses. The engine figures the self-employment tax and the deductions that
  follow from it. Some states' lines are not figured for it, and the result
  says which.
- **Withholding and estimated payments** decide whether a refund or a balance
  due can be quoted at all. Without them, the result treats nothing as paid.
- **On a joint return** `spouse_split` says whose the IRA money and pension are
  on the state return; it can change the tax in the states that tax a couple by
  each spouse's income. Say which split ran when quoting a state figure.

## Carry the caveats with the number

Each result lists the defaults it assumed for inputs not given, what it does
not model, and what is projected rather than published — the 2027 and 2028
IRMAA tables, a 2027 tax year. Repeat those with the figure. A projected tier or
a projected-year saving is an estimate of a table that does not exist yet, and
should be presented as one.

These are estimates for planning, not a filed return and not tax advice; say
so when quoting a refund, a balance due or a saving.
