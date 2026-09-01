---
title: "Luke Bornn"
type: entity
tags: [person, researcher, ai-research, university, sports-analytics, spatiotemporal, pitch-control, off-ball, space-creation, reliability, evaluation, statistics]
sources: [raw/papers/wide_open_spaces_creation_football.md, raw/papers/multiresolution-stochastic-process-model-nba-possessions.md, raw/papers/expected_value_possession_framework.md, raw/papers/understanding-sports-metric-statistical-properties.md]
confidence: 0.9
provenance:
  extracted: 70%
  inferred: 26%
  generated: 2%
  imported: 2%
  ambiguous: 0%
lifecycle: reviewed
created: 2026-07-23
updated: 2026-08-29
---

# Luke Bornn

Statistician working across basketball and football spatial analytics. Simon Fraser University and the Sacramento Kings at the time of the 2018 paper; Assistant Professor at Simon Fraser at the time of the 2016 one.

**Four held sources**, and the connective figure in the vault's possession-value literature.

| Year | Work | With | Contribution |
|---|---|---|---|
| 2016 | [[multiresolution-stochastic-process-nba-possessions\|Multiresolution EPV]] | Cervone, D'Amour, Goldsberry | [[martingale-epv]] |
| **2016** | **[[meta-analytics-sports-metrics\|Meta-Analytics]]** | **Franks, D'Amour, Cervone** | **[[metric-discrimination]], [[metric-stability]], [[metric-independence]]** |
| 2018 | [[wide-open-spaces-space-creation\|Wide Open Spaces]] | [[javier-fernandez\|Fernández]] | [[pitch-control]] (Gaussian), [[pitch-value-model]], [[space-occupation-gain\|SOG/SGG]] |
| 2020/21 | [[expected-value-possession-framework\|Soccer EPV framework]] | Fernández, Cervone | Decomposed tracking-based EPV, [[soccermap]] |

## ⚠️ He Co-Wrote the Standard His Own Football Metrics Do Not Meet

> **Added 2026-08-29** on ingest of [[meta-analytics-sports-metrics|Franks et al. (2016)]].

In the same year as the basketball EPV paper, Bornn co-authored the framework defining **[[metric-discrimination|discrimination]], [[metric-stability|stability]] and [[metric-independence|independence]]** as measurable properties of a player metric — the vault's top acquisition priority for twelve ingests.

He then went on to build [[wide-open-spaces-space-creation|Wide Open Spaces]] and the [[expected-value-possession-framework|soccer EPV framework]]. **Neither reports any of the three.**

> ### `the-framework-and-the-metrics-share-authors-and-never-meet`
> **The absence of reliability figures in football valuation is not explained by the field lacking the tools. Two of the four authors who defined how to measure metric quality went on to build football value surfaces and reported none of those properties for them.**
> ^[generated: declared on [[meta-analytics-sports-metrics]]. rests-on: source:franks-author-list, source:fernandez-bornn-wide-open-spaces]

**This overturned a reading three prior ingests had built up** — that reliability was missing because it belonged to a different discipline. It does not. It belongs to the same people. See [[reliability-layers]].

The [[space-occupation-gain|SOG]] case is the sharpest: the page below notes SOG and SGG "have not been extended by anyone", and the vault has no reliability figure for either. **Their co-author had, two years earlier, published the method for producing one.**

## The Through-Line

**Turn positional tracking into a spatial field, then reason over the field rather than over events.** In basketball that yields spatial bases and defender-assignment models; in football, control surfaces, value surfaces, and eventually full-pitch [[probability-surface|value surfaces]].

Also co-author on the defensive-structure work (Franks et al., 2015) underlying the EPV microtransition model, and the factorised point-process work (Miller et al., 2013) informing its NMF spatial bases.

⚠️ **The 2015 Franks collaboration is now an acquisition target.** *Counterpoints: advanced defensive metrics for NBA basketball* is a tracking-derived defensive metric by the same group that then wrote the metric-quality framework — the most likely place in the corpus where the two registers actually meet. See [[alexander-franks]] and [[defensive-valuation]].

## What Wide Open Spaces Established

> **Updated, 2026-07-27.** This page previously described the 2018 paper as unheld and as "the vault's other route into space-creation". It is now held, and it did more than one thing.

Three models composing into one quantity: a **Gaussian influence [[pitch-control|pitch-control]] model**, a **[[pitch-value-model|ball-relative pitch value model]]** learned from where defenders stand, and the **[[space-occupation-gain|SOG/SGG]]** metrics built on their product.

Two of the three have outlived the third. The pitch-control model became the substrate for the [[expected-value-possession-framework|2020 EPV framework]] and is one of the vault's two control traditions; the pitch-value model is the only value surface here trained without outcome labels. SOG and SGG have not been extended by anyone.

## The Two Pitch-Control Traditions

Bornn's construction is one of two the vault holds, and the pairing is instructive:

| | Bornn & Fernández | [[william-spearman\|Spearman]] |
|---|---|---|
| Origin | Spatial dominance | A **pass-reception** model |
| Mechanism | Gaussian influence density | Arrival-time contest, Poisson control |
| Parameters | **Expert-set** | **MLE-fitted, with stated errors** |
| Validated against | Expert video review | **5,471 held-out pass receivers** |

Neither cites the other; both position against [[voronoi-tessellation]]. See [[pitch-control-traditions-compared]].

## The Basketball-to-Football Trade

Bornn is on both sides of a methodological reversal the vault records in detail. [[martingale-epv|The 2016 basketball model]] is a Bayesian [[stochastic-process|stochastic process]] with a [[martingale]] guarantee, costing 461 processors. [[expected-value-possession-framework|The 2020 soccer framework]] abandons that guarantee for nine supervised components, gaining real-time inference and off-ball coverage.

His presence on both makes that **a considered change of approach rather than an oversight by newcomers.** See [[martingale-epv]] for the trade.

Funded at the time by DARPA, the US Army Research Office, Amazon AWS, and NSERC.

## See Also

- [[pitch-control]] · [[pitch-value-model]] · [[space-occupation-gain]] · [[space-creation]] · [[voronoi-tessellation]]
- [[martingale-epv]] · [[expected-possession-value]] · [[soccermap]] · [[probability-surface]] · [[off-ball-value]]
- [[gaussian-process]] · [[optical-tracking-data]] · [[multiresolution-modelling]] · [[car-prior]] · [[point-process]]
- [[javier-fernandez]] · [[daniel-cervone]] · [[kirk-goldsberry]] · [[william-spearman]] · [[fc-barcelona]]
- [[wide-open-spaces-space-creation|Wide Open Spaces]] · [[expected-value-possession-framework|Soccer EPV Framework]] · [[multiresolution-stochastic-process-nba-possessions|Basketball EPV]]
