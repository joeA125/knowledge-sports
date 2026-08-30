---
title: "Concurrent agreement and test-retest reliability of a GPS device for measuring maximal horizontal deceleration"
type: summary
tags: [reliability, evaluation, statistics, sports-analytics, player-evaluation, optical-tracking-data, model-selection, single-source]
sources: [raw/papers/test_retest_reliability_soccer_positioning_and_movement.md]
confidence: 0.85
provenance:
  extracted: 71%
  inferred: 14%
  generated: 13%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Concurrent Agreement and Test–Retest Reliability of GPS for Maximal Horizontal Deceleration

**Lewys Jones, Cedric Leduc, Kurt Greatorex, Samuel Callaghan, Dan Weaving, Damian Harper & Alex Bliss.** *International Journal of Sports Physiology and Performance*, Brief Report, IJSPP.2024-0212.R3. St Mary's University Twickenham, Leeds Beckett, UCLan, and **Crystal Palace FC Academy**.

> ⚠️ **This is the third reliability paper in three ingests and the third distinct layer. It does not close the vault's top priority either.**
>
> `no-reliability-for-off-ball-metrics` asks for split-half or test–retest reliability of a **tracking-derived football-value metric** — [[obso|OBSO]], [[c-obso|C-OBSO]], [[drso|DRSO]], [[space-occupation-gain|SOG]]. This measures **device and protocol reliability of a physical-capacity test**: how hard a player can brake, in a standardised 20 m sprint-and-stop, on a training pitch. See [[reliability-layers]].

## What Was Done

Thirty-two elite youth academy players (18.1 ± 1.6 yrs) performed an **acceleration–deceleration ability test** — maximal 20 m sprint through timing gates, then maximal braking to a stop. Velocity was captured simultaneously by a **47 Hz radar** (criterion) and a **10 Hz STATSports APEX GPS unit**.

Sixteen players repeated the test **one week later** for test–retest reliability; only sixteen because of fixture congestion.

Five variables, computed in R from Butterworth-filtered velocity (4th order, zero-lag, 1 Hz cutoff):

| Variable | What it is |
|---|---|
| $V_{Max}$ | Peak velocity — **directly sampled** |
| $DEC_{Max}$ | Largest instantaneous deceleration — **first derivative, extremum** |
| $DEC_{Ave}$ | Mean deceleration over the braking phase — **first derivative, mean** |
| $TTS$ | Time to stop — **duration of a segment** |
| $DTS$ | Distance to stop — **integral of velocity** |

Three data streams were compared: radar, `GPS_Raw` (custom R processing of 10 Hz velocity), and `GPS_Export` (STATSports' own Sonra software).

## The Result That Matters to This Vault

Reliability degrades systematically as the quantity moves further from what the device actually samples.

| Variable | Derivation | GPS ICC | GPS CV% | Rating |
|---|---|---|---|---|
| $V_{Max}$ | **Sampled directly** | 0.81–0.82 | **1.4–1.5** | Good |
| $DEC_{Max}$ | Derivative, extremum | 0.84–0.86 | 4.5–5.5 | Good |
| $DEC_{Ave}$ | Derivative, mean | **0.57** | 6.1 | Moderate |
| $DTS$ | Integral | **0.53** | 6.6 | Moderate |
| $TTS$ | Segment duration | **0.48** | 5.8 | Moderate |

> ### `each-layer-of-derivation-costs-reliability`
> **A quantity computed from a sampled signal is less reliable than the signal, and the loss compounds with each transformation. Peak velocity — read straight off the device — reproduces at CV 1.4%; quantities derived from it by differentiation, integration or segment-boundary detection fall to ICC 0.48–0.57. The football-value metrics in this vault sit several transformations further out than any variable measured here.**
> ^[generated: the paper reports the per-variable figures without framing them as a derivation gradient. rests-on: source:jones-table1-reliability-column]

⚠️ **$TTS$ and $DTS$ both depend on detecting where the deceleration phase ends** — the timepoint of minimum velocity after $V_{Max}$. Their low ICCs are plausibly **boundary-detection instability** rather than noise in the velocity trace itself, since $V_{Max}$ from the same trace is stable.^[inferred: the paper does not diagnose the cause; the shared dependency on segment boundaries is drawn here from the supplementary R script] That is a *segmentation* problem, and segmentation is exactly what [[spadl|event]] and possession-based pipelines do constantly.

## The Finding the Authors Half-State

**The criterion device is not much more reliable than the GPS.** Radar test–retest: $TTS$ ICC 0.67, $DTS$ 0.62, $DEC_{Ave}$ 0.73, $DEC_{Max}$ 0.78 — *moderate* on their own scale, for three of four.

The authors draw the right conclusion and then move on from it:

> This suggests error from the testing procedure (e.g., deceleration strategy) rather than the measurement technique itself.

**The player is the unstable component, not the instrument.** Under maximal effort, in a standardised protocol, on the same surface, one week apart, an elite academy footballer does not reproduce his own braking.

> This is the cleanest evidence the vault holds on the $\sigma^2_\theta$ versus $\sigma^2_\varepsilon$ question that [[split-half-reliability]] and [[within-season-variation-noise-or-signal]] are built around — and it points at **genuine within-player variability** rather than measurement noise.
> ^[generated: the paper makes the attribution but does not connect it to metric-level reliability. rests-on: source:jones-radar-reliability-comparable, source:jones-testing-procedure-attribution]

⚠️ **Transfer carefully.** This is a maximal physical capacity under laboratory-like control; a match-derived value metric is a different quantity in a different setting. The result does not license concluding that VAEP's $\rho = 0.25$ is mostly true player variation. **What it does is make that branch of the disjunction substantially more plausible**, where the field has tended to assume the noise reading. See [[performance-volatility]].

## Agreement and Reliability Are Different, Demonstrated Cleanly

For $DEC_{Max}$ and $DEC_{Ave}$, the **mean difference** between radar and GPS fell inside the pre-registered equivalence bounds — but the **95% limits of agreement did not.**

The authors state it plainly: devices are practically equivalent on average, with a potential lack of agreement **at the individual level**.

| | $DEC_{Max}$ | $DTS$ | $TTS$ |
|---|---|---|---|
| Standardised TEE | 0.79–0.85 | **4.50** | **2.80** |
| Pearson $r$ vs radar | 0.76–0.78 | **0.22** | 0.34 |

**$DTS$ correlates with its own criterion at $r = 0.22$.** Two devices measuring the same physical event, over the same seconds, disagree almost entirely.

> **A metric can be group-equivalent and individually useless.** Mean difference is the wrong summary when the decision is about a person.
> ^[generated: the paper reports both statistics and flags the LOA problem, but does not generalise. rests-on: source:jones-loa-outside-bounds]

This is the same structure as the caution already recorded on [[pitch-control-traditions-compared]] — that a single global correlation between two pitch-control surfaces would *understate* practical disagreement, because it averages over regions where the models agree trivially. **Two independent instances now.** Where two measures of one quantity are compared, the aggregate statistic systematically flatters them.

## Limitations

- **$n = 16$ for the reliability half**, from 32 recruited — fixture congestion, not design. Confidence intervals are correspondingly wide: $DEC_{Ave}$ ICC 0.57 has a 95% CI of 0.13 to 0.83, which spans *poor* to *good*.
- **One club, one age group, one surface.** Elite youth academy players at Crystal Palace, artificial turf, studded footwear, in-season.
- **A capacity test, not match play.** Nothing here concerns behaviour during a game.
- **GPS, not optical tracking.** The vault's football sources use optical systems; this is a wearable. Error models differ, and the standing absence claim on [[optical-tracking-data]] is untouched.
- **The two GPS streams are not independent** — `GPS_Raw` and `GPS_Export` come from the same unit and the same satellites, differing only in processing. Their agreement says nothing about hardware.
- **Deceleration from ~8 m·s⁻¹.** The authors note GPS deceleration detection may degrade at higher velocities, and that a comparable prior study used decelerations below 7 m·s⁻¹.

## See Also

- [[reliability-layers]] — the three-layer taxonomy this ingest created
- [[operator-reliability]] · [[split-half-reliability]] · [[within-season-variation-noise-or-signal]] · [[performance-volatility]] · [[predictive-validity]]
- [[champdas-validity-reliability|Gong et al. (2019)]] — the coding layer
- [[optical-tracking-data]] · [[player-rating-time-series]] · [[recruitment]] · [[construct-validity]]
- [[pitch-control-traditions-compared]] — the same aggregate-flatters-agreement structure
- [[action-valuation-frameworks-compared]] · [[model-selection]] · [[selection-bias]]
