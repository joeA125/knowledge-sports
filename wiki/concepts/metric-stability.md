---
title: "Metric Stability"
type: concept
tags: [reliability, evaluation, statistics, player-evaluation, model-selection, sports-analytics, uncertainty-quantification, recruitment, event-stream-data]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/metric-stability-elite-football.md]
confidence: 0.8
provenance:
  extracted: 52%
  inferred: 18%
  generated: 28%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Metric Stability

**Does the metric measure the same thing this season that it measured last season?**

$$\mathcal{S}_m = 1 - \frac{E_m[V_{pm}[X] - V_{spm}[X]]}{V_m[X] - E_m[V_{spm}[X]]}$$

The fraction of total variance — **with sampling variability already subtracted from both numerator and denominator** — that is between players rather than within a player over time. Under the decomposition on [[metric-variance-components]]:

$$\mathcal{S}_m = \frac{\sigma^2_{PM}}{\sigma^2_{PM} + \sigma^2_{SM} + \sigma^2_{SPM}}$$

From [[meta-analytics-sports-metrics|Franks et al. (2016)]]. If a metric never changes for a player, $\mathcal{S} = 1$; if within-player variation is as large as total variation, $\mathcal{S} = 0$.

## ⚠️ Football Now Has Stability Figures — But Not for Anything the Vault Values

> **Corrected 2026-08-29**, one ingest after this page was written. It stated flatly that **"no football metric in this vault has a stability figure at all."** That is now false.

[[year-to-year-metric-stability-football|Shaikh (2026)]] reports year-to-year Pearson $r$ and ICC for **24 position-metric combinations** across 8,207 player-season pairs, five leagues, eight seasons.

| Tier | Metrics | $r$ |
|---|---|---|
| Very high | Pass completion % (MID, DEF) | **0.824** |
| High | Progressive carries / passes / passes into box — **forwards** | 0.707–0.733 |
| Moderate | xG 0.664, npxG 0.635, goals 0.593, SCA, key passes, GCA | 0.52–0.68 |
| Low | **Assists 0.428**, interceptions 0.476, **tackles 0.414**, **blocks 0.399** | < 0.50 |

**These are FBref counting statistics.** Not [[vaep|VAEP]], not [[expected-threat|xT]], and none of the six mechanisms on [[off-ball-value]]. `no-reliability-for-off-ball-metrics` survives its **thirteenth** ingest.

The claim narrows rather than dies: *no **tracking-derived** football metric has a stability figure, and no **value model** of any kind has one.*

### The process-to-outcome gradient

Progression metrics (0.71–0.73) > xG (0.664) > goals (0.593) > assists (0.428). **The ordering runs from what a player did to what resulted** — the first empirical support for [[intent-vs-outcome-valuation]]'s untested proposal that withholding outcome information should raise reliability.

### Defensive metrics are worst here too

Tackles, blocks and interceptions occupy the bottom three. [[champdas-validity-reliability|Gong et al.]] independently find defensive actions the worst-coded cell at a **different layer** with a **different statistic**. See `defensive-actions-degrade-at-every-measurement-layer` on [[year-to-year-metric-stability-football]] and the second leg it supplies to [[rare-event-proxy-targets]].

### Stability at one year is not stability

All 24 combinations decay at two years, median $\Delta r = -0.097$, and the least stable decay fastest. See [[stability-decay]].

> **The gap is worst exactly where it is quietly assumed.** [[recruitment]] recommends xT over VAEP for season-long squad decisions on the grounds that it "replicates better" — citing $\rho = 0.89$, a **within-season** figure. Recruitment is a between-season decision. **The property the recommendation needs is the one nobody has measured for either metric.**
> ^[generated: no source notes the mismatch between the figure cited and the decision it supports. rests-on: source:vanroy-vaep-splithalf, claim:recruitment-prefers-xt]

The recommendation may well survive — xT is a coarser, zone-based construct and plausibly steadier across seasons. But it currently rests on the wrong statistic.

⚠️ **And [[year-to-year-metric-stability-football|Shaikh's]] results give a reason to doubt it.** xT is built from ball progression between zones, and progression metrics are among the *most* stable things measured — which supports the recommendation. But VAEP's largest components are shot and outcome-adjacent, and **outcome metrics are consistently the least stable.** The two metrics therefore sit at opposite ends of the one gradient that has been measured, which makes the recommendation plausible **and** makes the missing measurement more embarrassing, not less.^[generated: the mapping of xT and VAEP onto Shaikh's gradient is drawn here; neither metric appears in his data]

## Why It Is Not Merely Discrimination Measured Twice

The two properties can move in opposite directions, and Franks et al.'s data shows it.

| Metric | Discrimination | Stability | Reading |
|---|---|---|---|
| Free-throw % | 0.65 | **0.98** | Poor at separating players; nearly constant for each |
| Minutes played | **0.96** | 0.40 | Separates players sharply; swings year to year |
| 3-point % | 0.43 | 0.30 | Bad at both |
| Total rebound % | 0.93 | 0.94 | Good at both — **because it tracks position** |

Free-throw shooting requires little athleticism and is isolated from team strategy, so it barely changes. Minutes played is driven by injury, squad depth and managerial preference, so it moves constantly while still sorting players cleanly.

> ### `stability-and-discrimination-are-independent-axes`
> **A metric can separate players well and be worthless for prediction, or separate them poorly and be perfectly dependable. The two properties answer different decisions — attribution versus acquisition — and a single "reliability" figure conflates them.**
> ^[generated: the paper reports both columns and the attribution/acquisition distinction separately; the framing as independent axes is drawn here. rests-on: source:franks-ft-mp-contrast]

## Instability Is Not a Defect

The paper is careful here and the vault should be too. A metric can be unstable for two quite different reasons:

1. **Context dependence** — performance varies with teammates, system, opponent quality
2. **Genuine change in the player** — offseason development, injury, ageing

**Neither is measurement error.** Franks et al. define stability *after removing* sampling variability precisely so that what remains is real.

This connects to [[performance-volatility]] and [[within-season-variation-noise-or-signal]]: a low stability score is evidence that **something real changed**, not that the metric is broken. Whether that something is the player or his circumstances is a further question the statistic does not answer.

⚠️ **And it is why [[empirical-bayes-shrinkage|shrinkage]] is double-edged.** Shrinking toward a career average raises stability by construction — it suppresses exactly the between-season movement this statistic measures. If form and development are real, that is signal being erased to buy a better number.

## The Data the Vault Does Not Have

Stability needs **multiple seasons per player.** The vault's football corpus mostly cannot supply them:

| Source | Coverage |
|---|---|
| [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | One season |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 14 players, one club |
| [[wide-open-spaces-space-creation\|SOG]] | A single match |
| [[team-defense-positioning-counterfactuals\|DRSO]] | One season |

> **This is an acquisition problem, not an analysis one**, and it separates the three meta-metrics sharply. [[metric-discrimination|Discrimination]] and [[metric-independence|independence]] are runnable on data the vault already describes. **Stability is not runnable on anything held here.**
> ^[generated: drawn from the coverage of the held sources against the estimator's requirements]

That reframes what to acquire. A multi-season tracking dataset with a value metric computed on it is worth more than another single-season paper introducing a seventh mechanism.

## Attribution Versus Acquisition

Franks et al.'s framing, and it is the cleanest statement of why one reliability number cannot serve:

| Decision | Is chance signal or noise? | Is context signal or noise? | Property needed |
|---|---|---|---|
| **Attribution** — awards, credit for a season | **Signal** — it happened | **Signal** | Discrimination |
| **Acquisition** — will he do it again? | **Noise** | **Noise** | **Stability** |

For an end-of-season award, whether a player could repeat his performance is irrelevant; he did it. For a transfer, repeatability is the entire question.

**Nearly every use the vault discusses — [[recruitment]], [[capability-profiling]], [[player-development-curve]] — is acquisition-shaped**, and is being served by discrimination figures.

## See Also

- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the source · [[metric-discrimination]] · [[metric-independence]] · [[metric-variance-components]]
- [[split-half-reliability]] · [[reliability-layers]] · [[empirical-bayes-shrinkage]] · [[operator-reliability]]
- [[performance-volatility]] · [[within-season-variation-noise-or-signal]] · [[player-rating-time-series]] · [[player-development-curve]]
- [[recruitment]] · [[capability-profiling]] · [[predictive-validity]] · [[construct-validity]]
- [[vaep]] · [[expected-threat]] · [[off-ball-value]] · [[c-obso]] · [[obso]]
