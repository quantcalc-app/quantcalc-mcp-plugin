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
does not — AMT, the 199A deduction, NUA and estate tax among them. If a question
depends on something out of scope, say so rather than letting the projection
imply it was covered.

## Not advice

This is calculation software. Results depend entirely on the assumptions listed
with them, and nothing here is financial advice.
