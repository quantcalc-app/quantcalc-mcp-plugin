---
name: setup
description: Setup and troubleshooting for the QuantCalc connector — installing and enabling it, configuring the optional licence key, which tools are free and which need a licence, and what each error message means. Relevant when the connector is being installed or configured, when its tools are missing or failing, or when someone asks what a QuantCalc licence key changes.
---

# Setting up the QuantCalc connector

The plugin connects to a hosted server at `https://mcp.quantcalc.app`. There is
no account to create and no sign-in step.

## Prerequisites

None beyond enabling the plugin. It installs switched off; enabling it is the
user's choice.

## Licence key (optional)

When the plugin is enabled, it asks for a QuantCalc PRO licence key. It is
optional and can be left blank.

- **Without a key:** each run uses 2,000 paths, each installation gets 40 runs a
  day (15 an hour), and the tax-aware tool returns the first retirement
  year of its schedule. A tax-aware run counts as one run.
- **With a key:** each run uses 10,000 paths and the tax-aware tool returns the
  full year-by-year schedule.
- **The 2026 tax tools** run on QuantCalc's licensed federal and state tax APIs
  and need a PRO or Tax Season Pass key; without one they compute nothing and
  name the free web page for the same question. `irmaa_check`, the RMD in
  `calculate_rmd` and the 2027 benchmark, cliff and headroom in
  `aca_premium_tax_credit` are free. `optimize_social_security_claiming` needs
  a PRO key.

The key is stored by the client as a secret and sent as a connection header. It
never passes through the conversation, so do not ask the user to paste it into
chat. If they want to add or change one, point them to the plugin's
configuration.

## What the tools do

"Free" runs without a key; "Licence" needs a PRO or Tax Season Pass key.

| Tool | Key | What it returns |
|---|---|---|
| `run_retirement_projection` | Free | Success rate with its 95% interval, the ending-portfolio distribution, and the assumptions that ran |
| `run_tax_aware_projection` | Free | The recommended withdrawal order and Roth-conversion programme, its present-value advantage against traditional-first, and the after-tax success rate; `move_state` plans a move to another state |
| `compare_return_assumptions` | Free | The same plan under each published capital market assumption set |
| `list_return_assumption_sources` | Free | The assumption sets available and what each publisher provides |
| `explain_methodology` | Free | What the engine models and what it leaves out |
| `optimize_social_security_claiming` | PRO | The tax-aware plan at every claiming age from 62 to 70 on the same markets, and the best age with its range |
| `estimate_2026_tax_return` | Licence | A 2026 federal and state return estimate (refund or amount owed, main Form 1040 lines, state tax), including self-employment profit for each spouse |
| `state_tax_in_every_state` | Licence | One household's 2026 state income tax in all 50 states and DC, ranked, with each state's difference from the current one; military retirement pay goes in `military_pension` |
| `calculate_2026_income_tax` | Licence | 2026 federal and state income tax, payroll tax, effective and marginal rates, take-home pay, and the refund or amount owed given withholding |
| `pretax_contribution_savings` | Licence | Tax saved by a health FSA or HSA contribution, 2026 or projected 2027 |
| `business_deduction_tax_savings` | Licence | Tax saved by a self-employed business deduction (2026 business miles or a dollar expense) |
| `calculate_rmd` | Free RMD; licence for the tax | The 2026 or 2027 RMD, deadline, start age and a projection; with filing status and other income, the tax the RMD adds |
| `roth_conversion_tax_cost` | Licence | The federal and state tax a 2026 Roth conversion adds, bracket and IRMAA room, torpedo and phase-out flags |
| `qcd_tax_savings` | Licence | Tax and IRMAA saved by giving through a QCD instead of cash |
| `irmaa_check` | Free | The IRMAA tier a MAGI sets for 2026, and the projected 2027 and 2028 tiers, with the surcharge and the distance to the next tier |
| `social_security_taxable_amount` | Licence | The taxable part of 2026 Social Security and the tax torpedo rate |
| `capital_gains_tax` | Licence | 2026 tax on capital gains and losses, the 0% band room, the carryover, and the saving from a harvested loss |
| `estimated_tax_payments_2026` | Licence | The 2026 safe-harbor payment, due dates and Form 2210 penalty |
| `aca_premium_tax_credit` | Free 2027 benchmark and cliff; licence for the credit | The ACA benchmark, poverty-line percentage, 400% cliff and headroom; the premium tax credit with a key |

All nineteen are read-only and store nothing about the user.

## Troubleshooting

- **The tools do not appear.** Check that the plugin is enabled, then restart
  the session so the connector is loaded.
- **"Hourly simulation limit reached" or a daily limit.** The installation has
  used its allowance for now. Say so plainly, including when it resets if the
  message gives a time. Do not retry in a loop.
- **"The engine refused this run (HTTP 400): …"** The text after the colon is the
  engine's reason, for example a negative balance or a life expectancy below the
  current age. Relay it and ask for the corrected input.
- **"at least one of traditional_balance, roth_balance, taxable_balance must be
  greater than zero"** or **"state must be a two-letter US state code"**: the
  tax-aware tool validates before running; ask for the missing figure or the
  state and call again.
- **"Nothing was computed"** from a 2026 tax tool: no key is configured (or
  it was refused); the tool runs on the licensed tax APIs and needs a PRO or
  Tax Season Pass key. Say so, and point to the free page the message names;
  the free tools still work. `calculate_rmd` still returns the RMD and
  `aca_premium_tax_credit` the 2027 benchmark and cliff without a key; only the
  tax and the credit are left out. `optimize_social_security_claiming` needs a
  PRO key.
- **Anything else.** The service status and setup notes are at
  <https://quantcalc.app/advisors/mcp/>. Support: hello@quantcalc.app.
