---
title: "Match-to-Match Variability"
type: concept
tags: [reliability, evaluation, statistics, optical-tracking-data, volatility, player-evaluation, sports-analytics, model-selection]
sources: [raw/papers/match-level-variability-high-speed-actions-football.md]
confidence: 0.8
provenance:
  extracted: 54%
  inferred: 16%
  generated: 28%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-09-04
updated: 2026-09-04
---

# Match-to-Match Variability

**How much a single player's output swings between one match and the next, for reasons that are not measurement error and not season-scale change.**

The sports-science register for a quantity the vault has been circling from three other directions. Its statistic is the **[[coefficient-of-variation|coefficient of variation]]** — per-player SD across matches over that player's mean — reported by [[match-to-match-variability-high-speed|Gregson et al. (2010)]] at 16–31% for high-speed running in the EPL.

## The Fourth Register for One Family of Questions

The vault has now met the "is this metric consistent" question in four vocabularies, from four literatures that barely cite each other:

| Register | Term | Statistic | Source |
|---|---|---|---|
| Psychometrics | split-half **reliability** | correlation of two halves | [[split-half-reliability]] |
| Meta-analytics | **stability** / **discrimination** | variance-component ratios | [[meta-analytics-sports-metrics]] |
| Sports science | **match-to-match variability** | coefficient of variation | **this page** |
| Finance-flavoured | **[[performance-volatility|volatility]]** | SD of a rating series | [[performance-volatility]] |

> ### `one-property-four-vocabularies`
> **Metric consistency has been studied independently in psychometrics, sports-metric meta-analytics, sports physiology and performance rating, each with its own term, statistic and citation graph. Searching any one register makes the others invisible, which is why the vault repeatedly judged the area emptier than it was.**
> ^[generated: assembled across the four held reliability literatures; no source connects them. rests-on: source:gregson-cv-table, source:franks-metametric-definitions]

This is now a **standing search heuristic**, not a per-ingest observation: a query in one register should be repeated in the other three before concluding a gap exists. It has cost the vault at least twice — see the overview's acquisition-priority notes.

⚠️ **They are not identical, and the differences matter.** CV is a pure dispersion measure: it says how much a quantity bounces around its own mean, and it does **not** partition that bounce into real between-player differences and noise. [[metric-discrimination|Discrimination]] and [[metric-stability|stability]] do partition it. **A high CV is compatible with high stability** if every player bounces the same relative amount around very different means. So CV answers "how noisy is one player's series" and the meta-metrics answer "can we tell players apart" — related, not interchangeable.

## What CV Buys and What It Hides

**Buys:** it needs only repeated observations of one player, no population model, no cross-player pooling. That makes it computable where stability is not — a single player's match log suffices, and the vault's [[metric-stability|stability]] gap is precisely that most held sources lack the multi-*season* panels stability needs. CV needs multiple *matches*, which every tracking source has.

**Hides:** it is silent on whether the variation is signal or noise. A 30% CV could be a player genuinely doing different things each week (tactical, and real) or the same player measured noisily (error). Gregson et al. lean toward the former — they attribute it to tactics, possession and opposition, explicitly *not* to instrument error — which is the same reading [[within-season-variation-noise-or-signal]] is trying to adjudicate for value metrics.

## The Noise-Floor Role

The reason this concept earns a page rather than a mention: **it is the closest thing the vault has to a floor on the noise every downstream metric inherits.**

[[gps-deceleration-reliability|Jones et al.]] gave a floor under laboratory-like control (peak velocity CV 1.4%). Gregson gives it for actual match play (16–31%). Every value metric — [[vaep|VAEP]], [[expected-threat|xT]], [[off-ball-value|the off-ball mechanisms]] — is computed on quantities produced in the match-play regime, so **16% is roughly the best-case variability of the raw material**, before any modelling adds its own.

⚠️ This is a floor on the *inputs*, not a prediction of the *outputs*. A well-constructed metric could in principle be more stable than its noisiest input, if it aggregates over many inputs whose errors partly cancel. But it is a reason to expect downstream value metrics to be *at least* this variable absent such averaging, and the [[off-ball-defensive-performance-blame|recovered off-ball ICCs]] (0.34–0.66) are consistent with that expectation.^[generated: the floor framing is drawn here; Gregson measures physical output only]

## Position and Possession, the Two Modifiers

Gregson found variability **higher for central players** and **higher in possession**. Both recur elsewhere in the vault:

- Position: the third independent appearance of a position effect on metric consistency, after [[metric-discrimination]] and [[year-to-year-metric-stability-football|Shaikh]]. Here it acts on the *raw signal*, a distinct mechanism from position-as-construct.
- Possession: the attacking-value metrics sit in the noisier (~30%) regime and the defensive ones in the quieter (~23%). See [[off-ball-value]].

## See Also

- [[match-to-match-variability-high-speed|Gregson et al. (2010)]] — the source · [[coefficient-of-variation]] — the statistic
- [[reliability-layers]] · [[split-half-reliability]] · [[metric-stability]] · [[metric-discrimination]] · [[performance-volatility]]
- [[within-season-variation-noise-or-signal]] · [[gps-deceleration-reliability]] · [[off-ball-defensive-performance-blame]]
- [[off-ball-value]] · [[recruitment]] · [[player-development-curve]] · [[optical-tracking-data]]
