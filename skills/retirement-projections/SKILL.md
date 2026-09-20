---
name: retirement-projections
description: Use when working on a retirement plan, a withdrawal strategy, a safe spending figure or a portfolio's chance of lasting — anything where a success rate, a sustainable withdrawal, or the effect of return assumptions is being discussed.
---

# Retirement projections

A retirement success rate is the output of thousands of correlated return paths
run against a spending schedule, taxes, Social Security timing and required
distributions. It cannot be estimated in prose, and an estimate that sounds
reasonable is the failure mode to avoid here: someone may act on it.

Use `run_retirement_projection` and report what it returns.

When the question is about *which account to draw from*, *Roth conversions*,
*what tax retirement will cost*, *IRMAA* or *state tax*, or the person has
given balances by account type, use `run_tax_aware_projection` instead. It
needs the three balances (traditional, Roth, taxable), the filing status, and
the state; ask for any of those rather than guessing them — a guessed filing
status or state moves the answer more than most inputs, and the tool will say
"assumed because not given" for whatever you leave out. Its `annual_spending`
is **after tax**: what the household keeps. Never repeat its success rate as if
the spending figure were a gross withdrawal, and never describe the plain
tool's figure as after tax.

Report the tax-aware result in this order, in the tool's own words: the
recommended order and any conversion programme; the advantage sentence exactly
as returned (the tool chooses between several carefully different sentences
depending on what the run actually showed, and it explains itself with a
breakdown whose parts sum to the advantage; paraphrasing any of them into
"saves $X in tax" can turn a true sentence into a false one); the "Assumed because not given" line; the "Not modelled" line. A
recommendation whose advantage is inside the stated range's downside is a
comparison of close alternatives, not a finding.

## Carry the assumptions with the number

Every result states the model that ran, the number of paths, and the income it
assumed — **including when it assumed none**. Repeat those alongside any figure
you quote. An unstated Social Security assumption moves a plan more than most
inputs, and a success rate quoted bare is the figure most likely to be acted on
and least likely to be checked.

If a result carries a precision warning, quote the warning too. Do not present a
low-path run as a finding.

## When the path count matters

Every result says how many paths it ran and how wide the resulting interval is.
That interval is not decoration — it is the difference between two questions:

- **"Does this plan work?"** A 2,000-path run answers this. An interval of ±2
  points does not change whether 88% is a healthy plan.
- **"Is this plan better than that one?"** It often does not. If two plans come
  back 3 points apart and the interval is ±2, the run cannot tell them apart,
  and presenting one as better than the other is reading noise. Say so rather
  than ranking them.

When a comparison is the actual question — a different claiming age, a different
allocation, more or less spending — either run enough paths to separate them or
report the gap as inside the margin. A licensed run uses 10,000 paths and about
±0.7, which resolves differences a 2,000-path run cannot.

## When one number would mislead

A single success rate implies more precision than published return forecasts
support. Where the answer would change materially depending on whose forecast is
used, run `compare_return_assumptions` and show the spread instead of picking
one. The gap between houses is usually larger than the gap the client is asking
about.

## Say what is out of scope

`explain_methodology` returns what the engine models and what it deliberately
does not — AMT, the 199A deduction, NUA and estate tax among them. The
tax-aware result already carries this same not-modelled list in its own text,
so relay it from there rather than calling `explain_methodology` again when a
tax-aware run is what produced the number. If a question depends on something
out of scope, say so rather than letting the projection imply it was covered.

## Not advice

This is calculation software. Results depend entirely on the assumptions listed
with them, and nothing here is financial advice.
