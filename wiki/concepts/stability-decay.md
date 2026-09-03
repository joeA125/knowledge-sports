---
title: "Stability Decay"
type: concept
tags: [reliability, evaluation, statistics, player-evaluation, recruitment, model-selection, sports-analytics]
sources: [raw/papers/metric-stability-elite-football.md]
confidence: 0.75
provenance:
  extracted: 44%
  inferred: 20%
  generated: 34%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Stability Decay

**[[metric-stability|Stability]] is not a property of a metric. It is a property of a metric *at a lag*.**

The year-to-year correlation $r(t \to t+1)$ that [[metric-stability]] reports is the first point on a curve. At two years it is lower. Nobody has measured the third point for football.

## The Only Measurement

[[year-to-year-metric-stability-football|Shaikh (2026)]], across 24 position-metric combinations on 4,844 $t \to t+2$ observations:

**All 24 decay.** Median $\Delta r = -0.097$, IQR $[-0.118, -0.083]$.

| Metric | $t \to t+1$ | $t \to t+2$ | $\Delta r$ |
|---|---|---|---|
| Pass completion % (MID) | 0.824 | 0.768 | −0.056 |
| Pass completion % (DEF) | 0.824 | 0.754 | −0.070 |
| Progressive passes (DEF) | 0.529 | 0.405 | **−0.124** |
| Tackles won | 0.414 | 0.293 | **−0.120** |
| Interceptions | 0.476 | 0.357 | **−0.119** |
| Blocks | 0.399 | 0.28 | −0.119 |

## Decay Is Steepest Where Stability Is Already Lowest

The most stable metric loses the least; the least stable lose the most. Pass completion drops 0.056–0.070; the four defensive volume counts drop 0.119–0.124, roughly double.

> ### `unstable-metrics-decay-faster-than-stable-ones`
> **Decay rate and stability level are not independent. A metric already dominated by context loses what little individual signal it carries faster than a metric measuring a durable trait, so the gap between good and bad metrics widens as the forecasting horizon lengthens. Choosing a metric for a two-year projection is a more consequential decision than choosing one for a one-year projection.**
> ^[generated: the paper reports the per-metric decays but does not observe that decay correlates with level. rests-on: source:shaikh-two-year-decay-table]

⚠️ **Beware a regression-to-the-mean reading.** Correlations near zero have less room to fall than correlations near one, so *some* of this pattern is arithmetic rather than substantive. The Fisher-$z$ scale is the right one to test it on and the paper does not report it, so the claim above should be treated as a pattern worth checking rather than an established one.^[generated: the caveat is raised here; the paper does not address it]

**Below 0.30, individual-level signal is arguably negligible.** Tackles won and blocks are there at two years. The author's recommendation follows: statistics older than one season should be discounted, and for the least stable metrics essentially disregarded.

## Why This Matters More Than the Headline Numbers

Real decisions use multi-season histories. A scout looking at a 24-year-old has three or four seasons of data and weights them somehow — and **the near-universal default is to weight them equally**, or to take a simple average.

That default is wrong in a specific, quantifiable direction: **it over-weights old seasons for exactly the metrics where old seasons carry least.** A three-season average of tackles won is closer to noise than a one-season figure, because two of the three seasons contribute almost nothing while adding variance.

> **Averaging over seasons is not free.** It reduces sampling noise ($\tau^2_M$ on [[metric-variance-components]]) and simultaneously imports stale signal. For a metric with fast decay the second effect can dominate, and no held source has worked out where the crossover sits for any football metric.
> ^[generated: no source frames multi-season averaging as this trade-off. rests-on: source:shaikh-two-year-decay-table, claim:unstable-metrics-decay-faster-than-stable-ones]

This is a concrete, unrun calculation the vault can specify: **for each metric, find the number of past seasons that minimises prediction error for season $t+1$.** The answer will differ by metric and, given the age results, by player age.

## The Interaction With Shrinkage Nobody Has Worked Out

[[empirical-bayes-shrinkage]] pulls a player's estimate toward his career average — that is, toward a quantity computed from **exactly the old seasons this page says carry decayed signal.**

The two prescriptions are in tension:

| | Prescribes |
|---|---|
| **Shrinkage** | Weight the career history *more* — it stabilises the estimate |
| **Decay** | Weight the career history *less* — it is stale |

⚠️ **They are not straightforwardly reconcilable**, and the resolution is not obvious. A defensible synthesis is that shrinkage should target a **recency-weighted** career average rather than a flat one, with the discount rate set by the measured decay for that metric. **No source does this**, and the vault should not pretend the tension is resolved.^[generated: neither source addresses the other; the synthesis is proposed here, not established]

## What Is Not Known

- **The third point on the curve.** Nothing beyond $t \to t+2$ for any football metric. Whether decay is linear, exponential or plateaus is unmeasured.
- **Decay for any tracking-derived metric.** [[obso|OBSO]], [[c-obso|C-OBSO]], [[drso|DRSO]] and [[space-occupation-gain|SOG]] have no $t \to t+1$ figure, let alone a decay curve.
- **Whether decay is player change or context change.** A player who moves club, ages, or has his role altered will decay for reasons that have nothing to do with the metric. Shaikh's within-league design removes transfers but not managerial or tactical change.
- **Age-specific decay.** Given that U23 players are less stable at one year, their decay curve is plausibly steeper — but this is not reported, and it is the population where projection matters most.

## See Also

- [[year-to-year-metric-stability-football|Shaikh (2026)]] — the only source
- [[metric-stability]] — the one-year property · [[metric-discrimination]] · [[metric-variance-components]] · [[metric-independence]]
- [[empirical-bayes-shrinkage]] — the unresolved tension · [[reliability-layers]] · [[split-half-reliability]]
- [[recruitment]] · [[player-development-curve]] · [[player-rating-time-series]] · [[capability-profiling]] · [[performance-volatility]]
- [[rare-event-proxy-targets]] · [[vdep]] · [[off-ball-value]] · [[predictive-validity]]
