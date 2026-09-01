---
title: "Metric Variance Components"
type: concept
tags: [statistics, reliability, evaluation, player-evaluation, model-selection, sports-analytics, uncertainty-quantification, hierarchical-model]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/on-ball-actions-football-xt-vs-vaep.md]
confidence: 0.85
provenance:
  extracted: 60%
  inferred: 17%
  generated: 21%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Metric Variance Components

**All three meta-metrics are ratios of the same four variances.** Getting the decomposition straight is what makes [[metric-discrimination|discrimination]], [[metric-stability|stability]] and [[metric-independence|independence]] distinguishable rather than three names for "reliability".

From [[meta-analytics-sports-metrics|Franks et al. (2016)]]. Arrange every metric as a three-dimensional array $X_{spm}$ — **season × player × metric** — and model one metric as

$$X_{spm} = \mu_m + Z_{sm} + Z_{pm} + Z_{spm} + \epsilon_{spm}$$

| Term | Variance | What it is |
|---|---|---|
| $Z_{sm}$ | $\sigma^2_{SM}$ | **Season effect** — league-wide drift, rule changes, scoring environment |
| $Z_{pm}$ | $\sigma^2_{PM}$ | **Player effect** — the durable thing a scout wants |
| $Z_{spm}$ | $\sigma^2_{SPM}$ | **Player-season interaction** — form, team context, role change |
| $\epsilon_{spm}$ | $\tau^2_M$ | **Sampling variability** — would vanish in an infinitely long season |

The distinction between $Z_{spm}$ and $\epsilon_{spm}$ is definitional and load-bearing: **$\epsilon$ is what a longer season would average away; $Z_{spm}$ is not.** A player who genuinely played differently this year is $Z_{spm}$; a player whose finishing percentage swung on 30 shots is $\epsilon$.

## The Three Meta-Metrics Are Three Slices

$$\mathcal{D}_m = \frac{\sigma^2_{PM} + \sigma^2_{SPM}}{\sigma^2_{PM} + \sigma^2_{SPM} + \tau^2_M} \qquad \mathcal{S}_m = \frac{\sigma^2_{PM}}{\sigma^2_{PM} + \sigma^2_{SM} + \sigma^2_{SPM}}$$

Read them side by side and the difference is exactly one term's placement:

- **Discrimination** counts $\sigma^2_{SPM}$ as **signal** — form is a real difference between players this season, and for attribution that is what you want.
- **Stability** counts $\sigma^2_{SPM}$ as **noise** — form is precisely what will not repeat, and for acquisition that is what you want to exclude.

> ### `form-is-signal-or-noise-depending-on-the-decision`
> **The player-season interaction term sits in the numerator of one meta-metric and the denominator of the other. There is no reading of the data on which a metric is simply "reliable" — the same variance is signal for attribution and noise for acquisition, and a single reliability figure has silently chosen one.**
> ^[generated: the paper defines both statistics and the attribution/acquisition distinction but does not point at the shared term. rests-on: source:franks-metametric-definitions]

This is the sharpest available statement of why [[within-season-variation-noise-or-signal]] cannot be settled by measurement alone. **The question is not what $\sigma^2_{SPM}$ *is*; it is what decision the number is for.**

## Reconciling With the Vault's Existing Identity

[[split-half-reliability]] carries a different decomposition, inherited from [[player-rating-time-series|Mendes-Neves et al.]]:

$$\rho = \frac{\sigma^2_\theta}{\sigma^2_\theta + 2\sigma^2_\varepsilon/n}$$

These are the same object viewed at different resolution:

| Vault identity | Franks components |
|---|---|
| $\sigma^2_\theta$ — true player level | $\sigma^2_{PM} + \sigma^2_{SPM}$, within one season |
| $\sigma^2_\varepsilon$ — match-to-match noise | $\tau^2_M$, plus coding and instrument noise |
| — | $\sigma^2_{SM}$ — **absent; the vault's identity is single-season** |

⚠️ **The vault's identity has no season term**, so it cannot express stability at all. That is the formal reason the absence claim was mis-worded for twelve ingests: **the vocabulary the vault was reasoning in could not represent the property it was asking for.**^[generated: the reconciliation is drawn here; neither source states the other's form]

$\sigma^2_{SM}$ is not negligible in football. A league-wide scoring shift, a rule change, or a provider changing its event definitions moves every player's metric together — and [[champdas-validity-reliability|the provider-definition problem]] means that last one is real.

## Estimation Is Nonparametric and the Assumption Is in the Resampling

Franks et al. do **not** fit the mixed model. It is offered as an interpretive aid; the metrics have incompatible supports — probabilities, counts, signed reals — so a single exchangeable additive model is inappropriate.

Instead, $\tau^2_M$ is estimated by **bootstrap**: for each team, resample the season's games with replacement, recompute end-of-season metrics, take the sample variance. Everything else comes from sample moments.

⚠️ **This assumes games are exchangeable within a season**, and in football they are conspicuously not. Opponent strength varies enormously across a league; home and away differ; scoreline state changes how a team plays; congestion and rotation cluster. **A naive game bootstrap will understate $\tau^2_M$** by treating a fixture list as an i.i.d. sample, which inflates every meta-metric computed from it.^[generated: the paper states the bootstrap design but not this limitation; the football-specific objection is drawn here]

The fix is standard — stratify or block the resampling by opponent strength and venue — and it is a real design decision anyone running these statistics on football has to make and report.

## What This Buys the Vault

**A vocabulary for saying which variance a construct is trying to isolate**, which several held pages need and none has:

- [[c-obso|C-OBSO]] and [[drso|DRSO]] difference against a counterfactual baseline. **A difference of two estimates has the sum of their variances** — so counterfactual constructs inflate $\tau^2_M$ by design. See the derivation-depth argument on [[off-ball-value]].
- [[vdep|VDEP]] and [[gvdep|GVDEP]] change the prediction target to raise event frequency, which shrinks $\tau^2_M$ directly. **That is the actual mechanism** behind [[rare-event-proxy-targets|proxy substitution]], stated in variance terms rather than as "more positives".
- [[empirical-bayes-shrinkage|Shrinkage]] trades bias for a reduction in $\tau^2_M$, raising both $\mathcal{D}$ and $\mathcal{S}$.

## See Also

- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the source
- [[metric-discrimination]] · [[metric-stability]] · [[metric-independence]] · [[empirical-bayes-shrinkage]]
- [[split-half-reliability]] · [[reliability-layers]] · [[operator-reliability]] · [[uncertainty-quantification]]
- [[within-season-variation-noise-or-signal]] · [[performance-volatility]] · [[player-rating-time-series]]
- [[rare-event-proxy-targets]] · [[off-ball-value]] · [[c-obso]] · [[drso]] · [[vdep]] · [[recruitment]]
