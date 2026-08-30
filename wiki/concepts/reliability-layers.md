---
title: "Layers of Reliability"
type: concept
tags: [reliability, evaluation, statistics, sports-analytics, player-evaluation, event-stream-data, optical-tracking-data, model-selection]
sources: [raw/papers/stats_reliability_football_champdas.md, raw/papers/test_retest_reliability_soccer_positioning_and_movement.md, raw/papers/on-ball-actions-football-xt-vs-vaep.md]
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

## ⚠️ The Standing Gap Is Layer 3, and It Is Still Open

`no-reliability-for-off-ball-metrics` has survived **eleven ingests.** It asks for split-half or test–retest reliability of a **tracking-derived football-value metric** — [[obso|OBSO]], [[c-obso|C-OBSO]], [[drso|DRSO]], [[space-occupation-gain|SOG]].

Three reliability papers have now arrived and **none of them is that paper.** Two measured layers 1 and 2; the third measured layer 3 for two on-ball event metrics only.

> **The vault's acquisition list was not wrong about the gap — it was imprecise about it**, and imprecision let two near-misses look like hits. The claim is now stated by layer, which makes it falsifiable by inspection rather than by argument.
> ^[generated: drawn from the outcome of three consecutive ingests against the claim as previously worded]

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

Unchanged, and now precisely stated:

1. **Split-half or test–retest for a tracking-derived value metric.** Split a season, compute OBSO or C-OBSO player ratings on each half, correlate. Nobody has done it.
2. **The coding-noise upper bound** — code $n$ matches twice, compute VAEP under each, correlate ratings. Proposed on [[operator-reliability]].
3. **Reliability of a segmentation decision** — the gap this page just opened.

[[data-driven-team-sports-behaviors|Fujii's survey]] contains no discussion of reliability at all, which suggests the closing source will come from **outside** the machine-learning literature. Both new sources support that: one appeared in *Frontiers in Psychology* under Quantitative Psychology and Measurement, the other in a sports-physiology journal.

## See Also

- [[champdas-validity-reliability]] · [[gps-deceleration-reliability]] · [[on-ball-actions-football-xt-vs-vaep]] — one source per layer
- [[operator-reliability]] · [[split-half-reliability]] · [[within-season-variation-noise-or-signal]] · [[performance-volatility]]
- [[predictive-validity]] · [[construct-validity]] · [[probability-calibration]] · [[selection-bias]]
- [[event-stream-data]] · [[optical-tracking-data]] · [[spadl]] · [[obso]] · [[c-obso]] · [[drso]] · [[space-occupation-gain]]
- [[action-valuation-frameworks-compared]] · [[model-selection]] · [[recruitment]]
