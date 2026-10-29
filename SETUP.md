# Setup

1. Install the plugin from the Claude plugin directory. It installs switched
   off. Enable it when you want to use it.
2. When asked for a **QuantCalc PRO licence key**, enter one or leave the field
   blank. Without a key each run uses 2,000 paths, and you get 40 runs a day. A
   tax-aware run counts as one run. A key raises each run to 10,000 paths. The
   key is kept in your system keychain and sent to the server as a connection
   header, never through the conversation.
   The 2026 return estimate (`estimate_2026_tax_return`) and the every-state
   ranking (`state_tax_in_every_state`) work only with a PRO or Tax Season
   Pass key, and the Social Security claiming search
   (`optimize_social_security_claiming`) only with a PRO key; every other tool
   runs without one.
3. Ask Claude something that needs a real projection, for example: *"I'm 58
   with $1.4M, retiring at 63 on $80k a year, with $32k of Social Security at
   67. What's the chance the money lasts to 95?"*

   *"Same household, but $900k is in a traditional IRA, $300k in a Roth and
   $200k in a brokerage account with 40% basis, single filer in California —
   which account should I draw first, and should I convert?"*

   *"We're moving from New Jersey to Florida in July 2028 — what does that do
   to our lifetime state tax?"* (the tax-aware tool's `move_state`)

   *"Married, 67 and 64, $42k Social Security, $70k IRA withdrawals and a
   $40k state pension: which states would tax us least in 2026?"* (with a key)

   *"Single, 45, $30k of military retirement pay and $60k of wages: which
   states tax my military pension, and how much?"* (with a key; the tool's
   `military_pension`)

There is no account and no sign-in. To use the server without the plugin, add
`https://mcp.quantcalc.app` as a custom connector (streamable HTTP).

Claude also gets a `setup` skill with the same steps and a troubleshooting list,
so you can ask it for help with connecting.

Full documentation: <https://quantcalc.app/advisors/mcp/> ·
Privacy: <https://quantcalc.app/privacy.html> ·
Terms: <https://quantcalc.app/terms.html> ·
Support: hello@quantcalc.app
