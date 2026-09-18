# QuantCalc Retirement Engine — Claude plugin

Gives Claude the retirement engine behind [quantcalc.app](https://quantcalc.app),
so a projection is computed rather than estimated.

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
- **Skill `setup`:** connection steps, what a licence key changes, and
  troubleshooting.

## Tools

| Tool | Returns |
|---|---|
| `run_retirement_projection` | Success rate, ending-portfolio distribution, and the assumptions that produced them |
| `compare_return_assumptions` | The same plan under each published capital market assumption set |
| `list_return_assumption_sources` | Which assumption sets the engine carries, and what each publisher provides |
| `explain_methodology` | What the engine models and what it deliberately leaves out |

All four are read-only: they compute and return, and change nothing. Inputs are
not kept after the run, except that a failed request is kept for two days to
diagnose it.

## What comes back with every number

A success rate is meaningless without its assumptions, and a summary will drop a
caveat it was not handed explicitly. So each result states:

- the return model that actually ran — reported by the engine, not by the request;
- the number of paths and the real trial count behind the rate;
- the income it assumed, **including when it assumed none**;
- whether a correlation matrix had to be adjusted before running;
- a plain warning when a run is not precise enough to show a client.

## Licence key

Optional. Without one, projections run at 2,000 paths, which puts the 95%
interval around a success rate at about ±1.5 points. A QuantCalc PRO key raises
that to 10,000 paths (about ±0.7). The same key unlocks the portfolio optimizer,
glide paths, custom capital market assumptions and multi-period planning in the
[QuantCalc app](https://quantcalc.app/app.html). Those features are not tools in
this plugin.

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
