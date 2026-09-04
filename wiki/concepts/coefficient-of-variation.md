---
title: "Coefficient of Variation"
type: concept
tags: [statistics, reliability, evaluation, volatility, model-selection, sports-analytics, uncertainty-quantification]
sources: [raw/papers/match-level-variability-high-speed-actions-football.md]
confidence: 0.85
provenance:
  extracted: 46%
  inferred: 14%
  generated: 38%
  imported: 2%
  ambiguous: 0%
lifecycle: draft
created: 2026-09-04
updated: 2026-09-04
---

# Coefficient of Variation

**Standard deviation divided by the mean.** A dimensionless dispersion measure that expresses variability relative to the size of the thing being measured, so quantities on different scales can be compared.

$$CV = \frac{\sigma}{\mu}$$

Usually reported as a percentage. In the sports-science tradition it is the default statistic for [[match-to-match-variability]]: for each player, the SD of a quantity across their matches divided by that player's mean, then averaged over players. From [[match-to-match-variability-high-speed|Gregson et al. (2010)]].

## Why It Is Not a Reliability Coefficient

This is the distinction the vault most needs from this page, because CV and ICC look interchangeable and are not.

| | Measures | Depends on between-player spread? |
|---|---|---|
| **CV** | dispersion of one unit around **its own** mean | **No** |
| **ICC / split-half / stability** | how much of the **between-unit** spread is real | **Yes** |

> ### `cv-is-within-unit-and-icc-is-between-unit`
> **A coefficient of variation is computed per player and averaged; a reliability coefficient is computed across players. The first says how much an individual bounces around their own level; the second says whether individuals can be told apart. A metric can have high CV and high reliability at once — many players, each noisy, but noisy around widely separated means.**
> ^[generated: no single source states the contrast; drawn between Gregson's CV and Franks' ICC. rests-on: source:gregson-cv-definition, source:franks-metametric-definitions]

**Consequence for the vault:** Gregson's 16–31% CVs and [[off-ball-defensive-performance-blame|Bischofberger's]] 0.34–0.66 ICCs **cannot be placed on one axis**, even though both are "variability" numbers about football tracking data. CV going up does not entail ICC going down. Any future page tempted to rank the two together should stop here.

## The Ratio Property That Makes It Useful and Dangerous

Being a ratio, CV is scale-free — its virtue — but that also means **it inflates as the mean approaches zero.** A rarely-performed action has a small mean, so its CV balloons even if its absolute SD is modest.

This is visible in Gregson's own table: **total sprint distance** (mean 145–307 m, the smallest quantities) shows the highest CV (30.8%), while **total high-speed running** (mean 604–1162 m) shows the lowest (17.7%). Some of that ordering is real inconsistency; some is arithmetic, because sprinting is rarer.

⚠️ **This is a direct caution for the vault's rare-event metrics.** [[rare-event-proxy-targets|Proxy substitution]] moves toward frequent events precisely to raise counts — and a CV computed on a rare defensive action will look catastrophic partly because the denominator is small, not only because the action is genuinely inconsistent. **Low counts inflate CV the way they inflate everything.** Compare per-90 against per-event framing on [[aggregation-denominator]].^[generated: the transfer to rare-event metrics is drawn here. rests-on: source:gregson-tsd-highest-cv]

## Relation to the Vault's Existing Volatility Material

[[performance-volatility]] treats the SD of a player's rating series as a signal about form. CV is that SD normalised by the mean, which makes it comparable across players and metrics — an improvement for cross-metric work, but it **discards the level**, and level is often what a scout cares about. The two are complementary: CV for "how noisy relative to size", raw SD (or [[metric-variance-components|the variance components]]) for decomposition into signal and noise.

## The Sample-Size Use

CV's original purpose in Gregson et al. is **research power**, not evaluation. Given a CV, one can compute how large an effect must be, or how many subjects are needed, to detect a real change. Their nomogram-based estimate — ~80 players to detect a 10% effect at a 20% CV — is a worked instance.

This is worth carrying because it inverts cleanly: **a metric's CV sets a floor on the change it can detect in an individual.** At CV 23%, an individual's true improvement must exceed ~64% to be distinguishable from noise at 95% confidence. Any single-observation player judgement on a high-CV metric is, in that light, mostly reading noise. See [[recruitment]].

## See Also

- [[match-to-match-variability]] — the property CV measures in this literature · [[match-to-match-variability-high-speed|Gregson et al.]]
- [[metric-variance-components]] · [[metric-stability]] · [[metric-discrimination]] · [[split-half-reliability]] · [[reliability-layers]]
- [[performance-volatility]] · [[rare-event-proxy-targets]] · [[aggregation-denominator]] · [[within-season-variation-noise-or-signal]]
- [[recruitment]] · [[uncertainty-quantification]]
