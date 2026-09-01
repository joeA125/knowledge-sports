---
title: "Daniel Cervone"
type: entity
tags: [person, researcher, ai-research, university, sports-analytics, stochastic-process, optical-tracking-data, reliability, evaluation, statistics]
sources: [raw/papers/multiresolution-stochastic-process-model-nba-possessions.md, raw/papers/expected_value_possession_framework.md, raw/papers/understanding-sports-metric-statistical-properties.md]
confidence: 0.9
provenance:
  extracted: 70%
  inferred: 25%
  ambiguous: 5%
lifecycle: reviewed
created: 2026-07-23
updated: 2026-08-29
---

# Daniel Cervone

Moore-Sloan Data Science Fellow at the Center for Data Science, New York University (at time of publication). Lead author of [[multiresolution-stochastic-process-nba-possessions|A Multiresolution Stochastic Process Model for Predicting Basketball Possession Outcomes]], which introduced [[martingale-epv]] and the [[multiresolution-modelling]] framework for estimating it from [[optical-tracking-data]].

The work is cited as related work in [[evaluating-football-player-actions|Decroos et al. (2019)]], the soccer [[vaep]] paper, as one of the foundational Markov-process approaches to valuing player actions — and by [[epv-control-duel-skills-football|Shelopugin]] as the origin of the EPV idea itself.

## Two Models, Opposite Design Philosophies

Cervone is an author on both the basketball construction and its soccer successor, [[expected-value-possession-framework|Fernández, Bornn & Cervone (2020)]] — and the two differ on nearly every methodological choice:

| | 2016 basketball | 2020 soccer |
|---|---|---|
| Estimation | One Bayesian process model | Nine supervised components |
| [[martingale]] guarantee | **Yes** — the defining feature | No |
| Interpretability from | Stochastic consistency | [[structured-model-decomposition\|Decomposition]] |
| Cost | 461 processors | Real-time |

The 2016 paper argues explicitly that regressing game-state features onto outcomes cannot produce an interpretable value curve. The 2020 framework does structurally that. Cervone's presence on both makes this a **considered revision of what interpretability requires** rather than a lapse — from a mathematical property of the estimator to a structural property of the model. See [[martingale-epv]] for the full comparison.

Reading the two together is the best available illustration in this vault that the martingale property is a cost as well as a virtue: it is what forced 461 processors, and abandoning it is what made a usable real-time coaching tool possible.

## A Third Paper, in a Register the Other Two Never Use

> **Added 2026-08-29** on ingest of [[meta-analytics-sports-metrics|Franks, D'Amour, Cervone & Bornn (2016)]].

Alongside the basketball EPV model, Cervone co-authored the framework defining **[[metric-discrimination|discrimination]], [[metric-stability|stability]] and [[metric-independence|independence]]** as measurable properties of a player metric. Same year, same core group — Franks, D'Amour and Bornn all appear on both.

He then co-authored the [[expected-value-possession-framework|soccer EPV framework]], which **reports none of the three.**

> ### `the-framework-and-the-metrics-share-authors-and-never-meet`
> **The absence of reliability figures in football valuation is not explained by the field lacking the tools.**
> ^[generated: declared on [[meta-analytics-sports-metrics]]. rests-on: source:franks-author-list, source:cervone-bornn-epv]

**The contrast is sharper here than for [[luke-bornn|Bornn]]**, because Cervone's whole methodological signature is uncertainty-awareness. The 2016 basketball model's defining feature is a [[martingale]] guarantee — a *mathematical* commitment that the value process is coherent over time. That is a statement about the estimator's internal consistency, and it is precisely the kind of concern the meta-metrics formalise empirically.

⚠️ **So the omission is not obliviousness but a difference in what counts as evidence.** The martingale property is proved; discrimination and stability are measured. Cervone's football work carries the proof-shaped guarantee where it can and reports no measured properties at all — which is consistent with [[metric-variance-components]]'s point that the vault's constructs mostly declare their variance behaviour rather than estimating it.^[inferred: no source characterises the choice; drawn from the three papers' evidence types]

**Three held sources**, and the only vault author on both a proof-based and a measurement-based account of metric quality.

Earlier vault notes dated the soccer framework to the MIT Sloan conference version (2019). With the arXiv preprint now held, the sequence is 2019 conference → 2020 arXiv → 2021 *Machine Learning* journal. All three are the same work.

Maintains a public demo repository with sample tracking data and R code for reproducing EPV calculations.

## See Also

- [[martingale-epv]] · [[expected-possession-value]] · [[multiresolution-modelling]]
- [[meta-analytics-sports-metrics]] · [[metric-discrimination]] · [[metric-stability]] · [[metric-independence]] · [[metric-variance-components]] · [[reliability-layers]]
- [[expected-value-possession-framework|Soccer EPV Framework Summary]] · [[structured-model-decomposition]]
- [[luke-bornn]] · [[javier-fernandez]] · [[alex-damour]] · [[kirk-goldsberry]] · [[alexander-franks]]
