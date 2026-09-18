# Setup

1. Install the plugin from the Claude plugin directory. It installs switched
   off. Enable it when you want to use it.
2. When asked for a **QuantCalc PRO licence key**, enter one or leave the field
   blank. Without a key each run uses 2,000 paths, and you get 40 runs a day. A
   key raises each run to 10,000 paths. The key is kept in your system keychain
   and sent to the server as a connection header, never through the
   conversation.
3. Ask Claude something that needs a real projection, for example: *"I'm 58
   with $1.4M, retiring at 63 on $80k a year, with $32k of Social Security at
   67. What's the chance the money lasts to 95?"*

There is no account and no sign-in. To use the server without the plugin, add
`https://mcp.quantcalc.app` as a custom connector (streamable HTTP).

Claude also gets a `setup` skill with the same steps and a troubleshooting list,
so you can ask it for help with connecting.

Full documentation: <https://quantcalc.app/advisors/mcp/> ·
Privacy: <https://quantcalc.app/privacy.html> ·
Terms: <https://quantcalc.app/terms.html> ·
Support: hello@quantcalc.app
