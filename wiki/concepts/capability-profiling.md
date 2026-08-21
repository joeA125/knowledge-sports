---
title: "Capability Profiling"
type: concept
tags: [evaluation, model-decomposition, predictive-validity, construct-validity, action-valuation, player-evaluation, class-imbalance, sports-analytics]
sources: [raw/papers/epv_control_and_duel_skills_football.md, raw/papers/on-ball-actions-football-xt-vs-vaep.md, raw/papers/expected_value_possession_framework.md, raw/papers/football_defence_evaluation.md]
confidence: 0.75
provenance:
  extracted: 30%
  inferred: 35%
  generated: 25%
  imported: 10%
  ambiguous: 0%
lifecycle: draft
created: 2026-07-27
updated: 2026-08-14
---

# Capability Profiling

Evaluating something by **decomposing its performance into components and reporting the vector**, rather than aggregating to a single score.

> **Re-sourced 2026-08-14.** This page was previously built on Hendrycks et al. (2025), *A Definition of AGI*, which now lives in the general vault. The argument is re-grounded on the four football cases below, which evidence it independently. The AGI material is retained as an external illustration and marked `imported:` throughout — it is **cited, not held** here.

## The Core Claim

> ### `aggregates-assume-substitutability`
> **An aggregate is a fair summary only when its components are substitutes for one another. Where they are not, the composite is the least informative number available — and the one most likely to be quoted.**
> ^[generated: no held source states this; drawn across the four cases below. rests-on: source:shelopugin-receiving-efficiency, source:vanroy-ivaep-ovaep, source:fernandez-epv-decomposition, source:vdep-f1-zero]

## Four Cases in This Vault

Each derived independently, each reaching the same structural point.

| Case | The composite | What the decomposition showed |
|---|---|---|
| [[receiving-efficiency]] | Defenders 1.06, midfielders 1.03 — near-identical | Split by reception and interception, the two groups do entirely different things |
| [[intent-vs-outcome-valuation]] | [[vaep\|VAEP]] conflates decision and execution | I-VAEP and O-VAEP separate them, and rank players differently |
| [[expected-possession-value]] | "EPV" as one term | **Four distinct quantities** under one name |
| [[class-imbalance-evaluation]] | Brier score alone | F1 exposes a classifier finding nothing — [[vdep\|VDEP]] measures VAEP's conceding model at 0.000 |

The [[expected-possession-value|EPV]] case is the strongest, because there the aggregation happened in the *vocabulary* rather than in a formula. Four research groups used one term for four different quantities, and nothing about the term signalled the difference.

## Where It Bites Hardest Here

Almost every framework in this vault ends by **summing per-action values into a per-90 rating** — see the aggregation-step discussion on [[action-valuation]]. That step discards exactly what a profile would report.

Two consequences the vault has documented:

- **[[player-rating-time-series|A season of within-player variation is discarded]]** by the sum, and that variation turns out to be the same quantity [[performance-volatility]] treats as signal. See [[within-season-variation-noise-or-signal]].
- **Off-ball metrics disagree with each other** at $\rho = 0.182$ while both report a single number called "off-ball contribution". A profile would have shown they rank different position groups; the composite hid it. See [[construct-validity]].

**No framework here reports a per-player capability profile**, despite the machinery existing in every one of them. The components are computed and then summed away.

## Jaggedness

^[imported: this framing and the term are from Hendrycks et al. (2025), cited not held]

A **smooth** profile means an aggregate is a fair summary. A **jagged** one means the aggregate is a weighted average over things that are not substitutes.

The practical point: **the more jagged the profile, the more misleading the composite** — and nothing in a single score tells you which case you are in. A footballer strong in progression and weak in retention has a jagged profile; his per-90 rating cannot express that, and two players with identical ratings may be jagged in opposite directions.

## Workarounds That Mask a Missing Capability

^[imported: the "capability contortions" argument is from Hendrycks et al. (2025), cited not held. The football application below is generated here]

The external argument: a workaround that substitutes for a missing capability inflates an aggregate while leaving the deficit in place — large context windows standing in for memory storage, retrieval standing in for memory recall.

**The football analogue is direct.** A metric that cannot see off-ball contribution but correlates with team success *via* possession volume will score well without measuring the thing. That is the [[rare-event-proxy-targets|proxy-target problem]] arriving from the evaluation side rather than the training side: **the proxy becomes the definition, and no evaluation that only measures the proxy can detect it.**

[[obso|OBSO]] predicting next-match goals at 0.26 is the counter-case worth holding onto — an external criterion, outside the pipeline, which a proxy cannot satisfy by construction. See [[predictive-validity]].

## Limitations

- **Equal weighting across components is a choice, not a finding.** Whatever weights a profile is collapsed under, nothing here shows rankings are stable across them — the same objection recorded on [[free-parameters-load-bearing]].
- **A profile needs its components to be separately valid.** Decomposing an unreliable metric produces several unreliable numbers, and no off-ball metric here reports [[split-half-reliability|reliability]] at all.
- **No held source reports a football capability profile.** The four cases above each decompose *one* dimension; none produces a vector.

## See Also

- [[receiving-efficiency]] · [[intent-vs-outcome-valuation]] · [[expected-possession-value]] · [[class-imbalance-evaluation]] · [[structured-model-decomposition]]
- [[action-valuation]] · [[player-rating-time-series]] · [[performance-volatility]] · [[within-season-variation-noise-or-signal]]
- [[construct-validity]] · [[predictive-validity]] · [[split-half-reliability]] · [[uncertainty-quantification]] · [[model-selection]]
- [[rare-event-proxy-targets]] · [[free-parameters-load-bearing]] · [[obso]] · [[vaep]] · [[vdep]]
- [[epv-control-duel-skills-football|Shelopugin Summary]] · [[on-ball-actions-football-xt-vs-vaep|xT/VAEP Summary]] · [[expected-value-possession-framework|EPV Summary]] · [[football-defence-evaluation-vdep|VDEP Summary]]
