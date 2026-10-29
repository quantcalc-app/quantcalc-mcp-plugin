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

## A move to another state

When the household plans to move, pass `move_state` to
`run_tax_aware_projection` (`state` moved to, `year`, optional `month`, default
July), with `state` the one it lives in now. The result compares the lifetime
state tax with the move against staying and against moving now; quote that
comparison as returned, with its assumptions.

## Which state taxes this income least

When the question is *which states would tax my retirement income least* or
*what would I pay in another state this year*, use `state_tax_in_every_state`
with the household's 2026 income (and `current_state` for the difference from
where it lives now). It needs a licence key; without one, say so and give the
free page link it returns. It is one year (2026) of state income tax only — not
property, sales or estate tax, and not a projection; for a lifetime answer use
`run_tax_aware_projection` with `move_state`. Relay its county-tax note and its
"assumed because not given" defaults with the ranking.

Military retirement pay goes in `military_pension`, never in `pension`: the
federal return taxes it like any pension, but many states exempt it in full or
in part (a cap, an age test or a credit) beyond what they allow other pensions,
and only `military_pension` reaches each state's military rule. Put a civilian
or government pension the same household also receives in `pension` (with
`pension_type`). On a joint return `military_pension` follows `spouse_split`
with the withdrawals, pension and wages (one setting moves them all): when one
spouse is the retiree and most of the income is theirs, `filer` or `spouse`
puts it in that person's name; say which split ran when you quote a state.

## A 2026 tax return estimate

When the question is *what will I owe or get back for 2026*, *what will my
return look like*, or a line of this year's Form 1040, use
`estimate_2026_tax_return`. It needs a licence key; without one, say so and give
the free calculator link it returns. It is an estimate for the 2026 tax year,
not a filed return and not tax advice — say that. Relay its "assumed because not
given" defaults and its not-modelled items with the refund or amount owed, and
ask for withholding and estimated payments before quoting a refund: without
them the result treats nothing as already paid.

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
does not — the 199A deduction, NUA and estate tax among them. The
tax-aware result already carries this same not-modelled list in its own text,
so relay it from there rather than calling `explain_methodology` again when a
tax-aware run is what produced the number. If a question depends on something
out of scope, say so rather than letting the projection imply it was covered.

## Not advice

This is calculation software. Results depend entirely on the assumptions listed
with them, and nothing here is financial advice.
