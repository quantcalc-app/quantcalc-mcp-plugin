# QuantCalc Retirement Engine — Claude plugin

Gives Claude the retirement engine behind [quantcalc.app](https://quantcalc.app),
so a projection is computed rather than estimated.

A language model cannot run ten thousand correlated return paths, a
Guyton-Klinger guardrail rule, or a tax-aware withdrawal order and be right
about it. It can ask something that can.

## Install

Install from the Claude plugin directory, or point your client at the remote
server directly:

```
https://mcp.quantcalc.app
```

Setup, the full tool list and the limits are documented at
<https://quantcalc.app/advisors/mcp/>.

## Tools

| Tool | Returns |
|---|---|
| `run_retirement_projection` | Success rate, ending-portfolio distribution, and the assumptions that produced them |
| `compare_return_assumptions` | The same plan under each published capital market assumption set |
| `list_return_assumption_sources` | Which assumption sets the engine carries, and what each publisher provides |
| `explain_methodology` | What the engine models and what it deliberately leaves out |

All four are read-only. Nothing is written, and no client data is stored.

## What comes back with every number

A success rate is meaningless without its assumptions, and a summary will drop a
caveat it was not handed explicitly. So each result states:

- the return model that actually ran — reported by the engine, not by the request;
- the number of paths and the real trial count behind the rate;
- the income it assumed, **including when it assumed none**;
- whether a correlation matrix had to be adjusted before running;
- a plain warning when a run is not precise enough to show a client.

## Licence key

Optional. Without one, projections run at 2,000 paths — enough that the 95%
interval around a success rate is roughly a point and a half wide. A QuantCalc
PRO key raises that to 10,000 paths and unlocks the portfolio optimizer, glide
paths, custom capital market assumptions and multi-period planning.

Set it in the plugin's configuration. It is sent as a connection header and
never passes through the conversation.

## Not advice

QuantCalc is calculation software, not financial advice. Methodology and the
source of every figure: <https://quantcalc.app/methodology.html>. Tax scope and
its explicit exclusions: <https://quantcalc.app/tax-methodology/>.

## Licence

MIT — see [LICENSE](LICENSE). This repository contains the plugin manifest and
documentation; the engine itself is a hosted service, published to the Official
MCP Registry as `app.quantcalc/retirement-engine` under a DNS-verified
namespace.
