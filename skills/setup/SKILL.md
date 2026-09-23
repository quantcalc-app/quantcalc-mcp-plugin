---
name: setup
description: Use when the QuantCalc connector is being installed, enabled or configured, when its tools are missing or failing, or when someone asks what a QuantCalc licence key changes.
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

The key is stored by the client as a secret and sent as a connection header. It
never passes through the conversation, so do not ask the user to paste it into
chat. If they want to add or change one, point them to the plugin's
configuration.

## What the tools do

| Tool | What it returns |
|---|---|
| `run_retirement_projection` | Success rate with its 95% interval, the ending-portfolio distribution, and the assumptions that ran |
| `run_tax_aware_projection` | The recommended withdrawal order and Roth-conversion programme, its present-value advantage against traditional-first, and the after-tax success rate |
| `compare_return_assumptions` | The same plan under each published capital market assumption set |
| `list_return_assumption_sources` | The assumption sets available and what each publisher provides |
| `explain_methodology` | What the engine models and what it leaves out |

All five are read-only and store nothing about the user.

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
- **Anything else.** The service status and setup notes are at
  <https://quantcalc.app/advisors/mcp/>. Support: hello@quantcalc.app.
