---
title: "Layers of Reliability"
type: concept
tags: [reliability, evaluation, statistics, sports-analytics, player-evaluation, event-stream-data, optical-tracking-data, model-selection, construct-validity]
sources: [raw/papers/stats_reliability_football_champdas.md, raw/papers/test_retest_reliability_soccer_positioning_and_movement.md, raw/papers/on-ball-actions-football-xt-vs-vaep.md, raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/metric-stability-elite-football.md]
confidence: 0.8
provenance:
  extracted: 46%
  inferred: 22%
  generated: 30%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Layers of Reliability

**"Reliability" in football analytics names at least three different things**, measured with different statistics, failing for different reasons, and fixable by different means. The vault treated it as one quantity until 2026-08-29, when two sources arriving in succession forced the distinction.

## The Three Layers

| | **1. Coding** | **2. Instrument & protocol** | **3. Metric** |
|---|---|---|---|
| Question | Same match → same event log? | Same test → same physical value? | Same player → same rating across samples? |
| Statistic | Kappa, ICC, typical error | ICC, CV%, LOA, TEE | Split-half $\rho$, test–retest |
| Fails from | Operator error, ambiguous definitions, camera angle | Device sampling rate, derivation depth, **athlete variability** | Rare events, small samples, genuine inconsistency |
| Fixed by | Better definitions, training, automation | Better hardware, simpler variables, more trials | More data, or a different metric |
| **Held source** | [[champdas-validity-reliability\|Gong et al. (2019)]] | [[gps-deceleration-reliability\|Jones et al. (2024)]] | [[on-ball-actions-football-xt-vs-vaep\|Van Roy et al. (2020)]] |
| Covers | One system, one match | One club, 16 players, a capacity test | xT and VAEP only |

**They compose downward.** Coding noise and instrument noise both flow into $\sigma^2_\varepsilon$ in the decomposition on [[split-half-reliability]]:

$$\rho = \frac{\sigma^2_\theta}{\sigma^2_\theta + 2\sigma^2_\varepsilon / n}$$

A metric can be no more reliable than its inputs. **The converse does not hold**, and that asymmetry is the reason the layers must be kept apart: clean inputs do not buy a stable metric.

## ⚠️ The Standing Gap Is Layer 3, and It Is Larger Than Recorded

> **Revised 2026-08-29** on ingest of [[meta-analytics-sports-metrics|Franks et al. (2016)]]. Layer 3 is not one property but **three**, and the vault holds one of them for two metrics.

`no-reliability-for-off-ball-metrics` has survived **twelve ingests.**

**Layers are one axis. Properties are the other.** Within the metric layer:

| Property | Question | Design | Held for football? |
|---|---|---|---|
| **[[metric-discrimination\|Discrimination]]** | Does it separate players *within* a season, beyond chance? | Split a season; bootstrap the sampling variance | ✅ xT (0.89) and VAEP (0.25), **nothing else** |
| **[[metric-stability\|Stability]]** | Does it measure the same thing *across* seasons? | Multiple seasons per player, chance removed | ✅ **24 FBref counting stats** ([[year-to-year-metric-stability-football\|Shaikh 2026]]) — **no value model, no tracking metric** |
| **[[metric-independence\|Independence]]** | Does it say anything the others do not? | Copula over several metrics on shared players | ❌ nothing, and the vault had no word for it |

⚠️ **The stability row changed 2026-08-29** and the change is instructive. One ingest after this page declared *"nothing, for any football metric"*, a source arrived with 24 stability figures — **none for a metric the vault values.** The claim narrows again rather than dying: *no tracking-derived football metric, and no value model of any kind, has a stability figure.*

Stability also turns out not to be one number: it decays with lag, universally, and fastest for the least stable metrics. See [[stability-decay]].

All three properties are ratios of the same four variances — season, player, player-season interaction, and sampling. See [[metric-variance-components]], where the difference between discrimination and stability turns out to be **which side of the ratio the interaction term sits on.**

⚠️ **The vault's own phrasing was wrong.** The absence claim read *"split-half or test–retest reliability"*, treating those as two ways of measuring one property. They are **two different properties**: split-half estimates discrimination, test–retest estimates stability.

So [[split-half-reliability]]'s $\rho = 0.25$ for VAEP is a **discrimination** figure. **No football metric here has a stability figure at all** — including xT, which the [[recruitment]] recommendation leans on precisely because season-to-season dependability is what recruitment needs.

> **Four reliability papers arrived, and the fourth revealed that the layer which mattered was itself subdivided.** The correction ran the same way each time: the gap was never mis-identified, only described too coarsely to be checked against an arriving source.
> ^[generated: drawn from the outcome of four consecutive ingests against the claim as successively worded]

See [[metric-independence]] for the third property, which is the cheapest of the three to run and the only one needing neither a bootstrap nor repeated seasons.

## What the Two New Layers Contributed Anyway

Neither closed the gap. Both narrowed it.

### Coding noise is ruled out as the cause of VAEP's instability

[[operator-reliability|Gong et al.]] show event-coding agreement at ICC ≥ 0.93. And the decisive argument needs no source at all: **xT and VAEP were computed on the same event stream**, so coding noise would depress both, and xT returns $\rho = 0.89$.

### The athlete is a real source of variance, not just the instrument

[[gps-deceleration-reliability|Jones et al.]] found their **criterion radar device** only moderately reliable — ICC 0.62–0.78 across deceleration variables — and attributed the error to *deceleration strategy* rather than measurement technique.

An elite academy player, under maximal effort in a standardised protocol one week apart, **does not reproduce his own braking.** If a physical capacity under laboratory-like control behaves that way, expecting high test–retest stability from an in-match value metric is optimistic.

This bears directly on [[within-season-variation-noise-or-signal]], which asks whether low reliability is measurement noise or genuine player inconsistency. **It does not settle that question** — a capacity test is not a match, and the transfer is an inference — but it moves the prior toward the player.^[inferred: no source connects the deceleration result to metric-level reliability]

## Derivation Depth Predicts Reliability

The strongest cross-layer regularity available, and it comes from layer 2:

| Quantity | Distance from the sampled signal | ICC |
|---|---|---|
| Peak velocity | **0** — read directly | 0.81–0.82, CV 1.4% |
| Max deceleration | 1 — derivative, extremum | 0.84–0.86 |
| Mean deceleration | 1 — derivative, mean | 0.57 |
| Distance to stop | 2 — integral over a detected segment | 0.53 |
| Time to stop | 2 — detected segment duration | 0.48 |

> ### `each-layer-of-derivation-costs-reliability`
> **A quantity computed from a sampled signal is less reliable than the signal, and the loss compounds with each transformation.**
> ^[generated: declared on [[gps-deceleration-reliability]]. rests-on: source:jones-table1-reliability-column]

**Every football-value metric in this vault sits further out than any variable in that table.** [[c-obso|C-OBSO]] runs tracking → trajectory prediction → pitch control → scoring surface → counterfactual difference. That is four or five transformations past a sampled position, each with a fitted model attached.

The regularity is not a prediction — nobody has measured it at that depth — but it is the only empirical handle the vault has on **why** layer 3 might be worse than practitioners assume, and it argues the standing gap matters more rather than less.^[generated: the extrapolation is drawn here and is explicitly not measured]

### The two worst variables share a cause

$TTS$ and $DTS$ both depend on **detecting where a phase ends**. Their velocity trace yields a stable $V_{Max}$ from the same data — so the instability is plausibly in **segment-boundary detection**, not in the signal.

⚠️ That is a *segmentation* problem, and segmentation is what possession- and event-based pipelines do constantly: where a possession starts, when pressing begins, which frames belong to an attack. **No held source reports reliability of any segmentation decision in football.**^[generated: an absence claim, newly declared. rests-on: source:jones-tts-dts-icc]

## Aggregate Statistics Flatter Agreement

Recorded here because it has now appeared twice in unrelated places.

[[gps-deceleration-reliability|Jones et al.]] found GPS and radar **equivalent on mean difference** and **outside equivalence bounds on limits of agreement** — group-equivalent, individually unreliable. Distance-to-stop correlates with its own criterion at $r = 0.22$.

[[pitch-control-traditions-compared]] independently reaches the same shape: a single global correlation between two pitch-control surfaces would understate practical disagreement, because most of the pitch is empty and both models agree trivially there.

> **Where two measures of one quantity are compared, the aggregate statistic systematically flatters them.** Any comparison in this vault reporting a single correlation should be read as an upper bound on agreement.
> ^[generated: constructed from two independent instances. rests-on: source:jones-loa-outside-bounds, claim:pitch-control-stratification]

## What Would Actually Close Layer 3

Restated by property, 2026-08-29:

1. **Discrimination for a tracking-derived value metric.** Split a season, compute OBSO or C-OBSO player ratings on each half, correlate. Nobody has done it. ⚠️ **Compute it conditional on position** — [[meta-analytics-sports-metrics|Franks et al.]] show blocks and rebounds score highly largely because they indicate position rather than ability.
2. **Stability for any football metric.** Needs multiple seasons per player, which the vault's football sources mostly lack — C-OBSO uses one season, SOG a single match. **The framework is cheap; the data is not held.**
3. **Independence across the six off-ball mechanisms.** The cheapest of the three: no bootstrap, no repeated seasons, only several metrics over one shared set of players. See [[metric-independence]].
4. **The coding-noise upper bound** — code $n$ matches twice, compute VAEP under each, correlate ratings. Proposed on [[operator-reliability]].
5. **Reliability of a segmentation decision** — the gap opened above.

And a route that needs none of them: **empirical Bayes shrinkage improves discrimination and stability simultaneously**, post-hoc, without changing the metric. Franks et al. demonstrate it on three-point percentage (0.43 → 0.53 and 0.30 → 0.64). See [[split-half-reliability]].

⚠️ **A caution on all five.** Meta-metrics measure *internal* quality only. Franks et al. note that birthplace zip code would score perfectly on all three and be useless — so a metric passing them still needs an external anchor. That is the same wall [[construct-validity]] reaches from the other side.

[[data-driven-team-sports-behaviors|Fujii's survey]] contains no discussion of reliability at all, which suggested the closing source would come from **outside** the machine-learning literature — supported by three of the four arrivals, from psychology, sports physiology and quantitative-analysis-of-sports journals.

⚠️ **But the fourth complicates that reading.** [[daniel-cervone|Cervone]] and [[luke-bornn|Bornn]] co-wrote this framework *and* two of the vault's football value surfaces, reporting none of these properties for the latter. **The tools were never outside the field — they were outside the paper.** See [[meta-analytics-sports-metrics]].

## See Also

- [[champdas-validity-reliability]] · [[gps-deceleration-reliability]] · [[on-ball-actions-football-xt-vs-vaep]] — one source per layer
- [[metric-discrimination]] · [[metric-stability]] · [[metric-independence]] · [[metric-variance-components]] · [[empirical-bayes-shrinkage]] — the properties within layer 3
- [[meta-analytics-sports-metrics]] · [[operator-reliability]] · [[split-half-reliability]] · [[within-season-variation-noise-or-signal]] · [[performance-volatility]]
- [[predictive-validity]] · [[construct-validity]] · [[probability-calibration]] · [[selection-bias]]
- [[event-stream-data]] · [[optical-tracking-data]] · [[spadl]] · [[obso]] · [[c-obso]] · [[drso]] · [[space-occupation-gain]]
- [[action-valuation-frameworks-compared]] · [[model-selection]] · [[recruitment]]
