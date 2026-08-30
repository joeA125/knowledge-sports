---
title: "Operator Reliability (Event Coding)"
type: concept
tags: [reliability, evaluation, statistics, event-stream-data, sports-analytics, player-evaluation, model-selection, selection-bias]
sources: [raw/papers/stats_reliability_football_champdas.md]
confidence: 0.8
provenance:
  extracted: 58%
  inferred: 20%
  generated: 19%
  imported: 2%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Operator Reliability (Event Coding)

Whether two analysts watching the same match produce the same event log — and whether one analyst watching it twice produces the same log twice.

This is **upstream of everything else in the vault.** Every held event-based framework — [[vaep]], [[expected-threat|xT]], [[vdep]], [[nmstpp]], [[gvdep]] — consumes an event stream that a person produced by pressing keys while watching football. **Until 2026-08-29 the vault held no source on whether that process is repeatable.**

## The Layer Distinction

The vault had been treating "reliability" as one quantity. It is **three**, and they fail for unrelated reasons — full taxonomy on [[reliability-layers]]. This page covers the first.

| | **Coding layer** *(this page)* | Instrument & protocol | Metric layer |
|---|---|---|---|
| Question | Same match → same event log? | Same test → same value? | Same player → same rating? |
| Fails because of | Operator error, ambiguous definitions, camera angle | Sampling rate, derivation depth, **athlete variability** | Rare events, small samples, genuine inconsistency |
| Statistics | **Kappa, ICC, typical error** | ICC, CV%, LOA, TEE | **Split-half $\rho$, test–retest** |
| Fix | Better definitions, training, automation | Better hardware, simpler variables | More data, or a different metric |
| Held source | ✅ [[champdas-validity-reliability\|Gong et al. (2019)]] | ✅ [[gps-deceleration-reliability\|Jones et al. (2024)]] | ✅ xT and VAEP only ([[split-half-reliability]]) |

They compose. In the decomposition on [[split-half-reliability]],

$$\rho = \frac{\sigma^2_\theta}{\sigma^2_\theta + 2\sigma^2_\varepsilon / n}$$

**coding noise is one contributor to $\sigma^2_\varepsilon$.** A metric cannot be more reliable than the data it is computed from — but the converse does not hold, and that asymmetry is what makes the distinction useful rather than pedantic.

## What Is Known

From [[champdas-validity-reliability|Gong et al.]], the only held source, on the [[champdas-master-system|Champdas Master System]]:

| | Kappa | ICC | Standardised TE |
|---|---|---|---|
| **Intra-operator**, 2 weeks apart | 0.87–0.93 | 0.98–1.00 | 0.01–0.15 |
| **Inter-operator** | 0.89, 0.97 | 0.93–1.00 | 0.01–0.29 |

**Very good agreement**, on Altman's scheme, throughout. Event-coding times agreed to under a second.

### Where it is weakest, and why that is the interesting part

Agreement is not uniform across action types:

| Group | Inter-operator ICC | Inter-operator TE |
|---|---|---|
| Passing | **1.00** | **0.01** |
| Attacking | 0.99–1.00 | 0.06–0.08 |
| **Defending and goalkeeper** | **0.93, 0.95** | **0.24, 0.29** |

> ### `coding-agreement-is-worst-where-the-vault's-hardest-metrics-live`
> **Operator agreement is near-perfect on passes and weakest on defensive and goalkeeping actions — the same actions the vault's defensive-valuation line is built from, and the same actions that are rare, ambiguous and outcome-dependent. The measurement layer degrades in the same places the modelling layer does, so the two sets of difficulties are correlated rather than independent.**
> ^[generated: the paper reports the per-group figures without connecting them to anything downstream. rests-on: source:gong-table5-defensive-cell]

This matters for [[vdep]] and [[gvdep]], which value team defence from **ball recovery and effective attack** — proxies chosen because goals are too rare. Those proxies are built from tackles, interceptions and clearances, which is precisely the weakest coding cell here.

⚠️ **Not a refutation.** TE of 0.29 is still "small" on Hopkins' scale, and this is one match on one system. But the vault has been treating [[rare-event-proxy-targets|proxy substitution]] as a purely statistical move — trading a rare target for a frequent one — and this suggests the frequent proxies are also **the noisier ones to observe**, which the trade does not account for.^[inferred: no source raises this; drawn across the two lines]

## The Aggregation Problem in Kappa

A Kappa of 0.97 across all event types is dominated by whichever category is most frequent. Passing accounted for roughly 182 events per team per collection against 7–14 for defensive actions.

**The headline agreement figure is therefore mostly a statement about passes.** The per-group table above is the informative one, and any future source reporting only an aggregate Kappa should be read with that in mind.^[generated: the paper reports both, but leads with the aggregate. rests-on: source:gong-event-counts]

## Reliability Within a System Is Not Agreement Between Systems

Gong et al. measured both, and they came apart.

Their operators agreed with each other, and **disagreed with OPTA on short passes** — while recording similar *total* pass counts. The cause is definitional: a different length threshold. They report the same problem for pass *directions*, caused by marking on a miniaturised on-screen pitch, where the system auto-labels a pass lateral if its angle to the sideline is under 15°.

> **Two operators sharing a definition agree; two systems with different definitions do not. High intra-system reliability is fully compatible with cross-system disagreement on the same quantity, and neither predicts the other.**
> ^[generated: the paper reports both results separately. rests-on: source:gong-opta-short-pass-discrepancy]

⚠️ **This is a caution about [[spadl]].** SPADL unifies provider vocabularies at the level of **action type** — a pass is a pass. The disagreement observed here is in the **attributes**: length band, direction category. Those are exactly the features a value model reads. A pipeline can be provider-agnostic at the type level and still provider-dependent at the level where it matters.

## What Is Still Unmeasured

- **Every provider the vault's sources actually use.** Champdas serves the Chinese Super League and the K-League. The held corpus runs on StatsBomb, Wyscout, Opta and [[data-stadium]]. Reliability figures do not transfer between systems by right — and Gong et al.'s own OPTA comparison is the evidence that they do not.
- **Tracking data.** All of the above concerns manual event coding. Optical tracking has its own error model, propagated by nobody — see the standing absence claim on [[optical-tracking-data]].
- **Automatic event detection.** Increasingly the actual pipeline, and unvalidated here.
- **Whether coding noise moves any downstream metric.** Nobody has recomputed a rating under two independent codings of the same matches. That is the test this page most wants and it is now cheap to specify: **code $n$ matches twice, compute VAEP or xT under each, and correlate player ratings.** The result is an upper bound on how much of the metric-layer instability is inherited from the coding layer.^[generated: no source proposes this test]

## See Also

- [[champdas-validity-reliability|Gong et al. (2019)]] — the only source · [[champdas-master-system]]
- [[split-half-reliability]] — the metric layer · [[within-season-variation-noise-or-signal]] · [[predictive-validity]] · [[construct-validity]]
- [[event-stream-data]] · [[spadl]] · [[optical-tracking-data]] · [[data-stadium]] · [[stats-perform]]
- [[vdep]] · [[gvdep]] · [[vaep]] · [[expected-threat]] · [[rare-event-proxy-targets]] · [[defensive-valuation]]
- [[action-valuation-frameworks-compared]] · [[model-selection]] · [[selection-bias]]
