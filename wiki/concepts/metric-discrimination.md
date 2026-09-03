---
title: "Metric Discrimination"
type: concept
tags: [reliability, evaluation, statistics, player-evaluation, model-selection, sports-analytics, uncertainty-quantification, off-ball, defensive-valuation]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/on-ball-actions-football-xt-vs-vaep.md, raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.85
provenance:
  extracted: 58%
  inferred: 16%
  generated: 24%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Metric Discrimination

**Does the metric separate players, or is it separating noise?**

$$\mathcal{D}_{sm} = 1 - \frac{E_{sm}[V_{spm}[X]]}{V_{sm}[X]}$$

One minus the ratio of average sampling variability to total between-player variance, within a single season. It is the fraction of observed player-to-player spread that reflects real differences. From [[meta-analytics-sports-metrics|Franks et al. (2016)]].

Under the mixed-effects decomposition on [[metric-variance-components]] it reduces to

$$\mathcal{D}_m = \frac{\sigma^2_{PM} + \sigma^2_{SPM}}{\sigma^2_{PM} + \sigma^2_{SPM} + \tau^2_M}$$

— player variance plus player-season interaction, over the same plus sampling noise.

## This Is What the Vault Has Been Measuring

⚠️ **[[split-half-reliability]]'s figures are discrimination figures.** Splitting one season into two halves and correlating player ratings estimates exactly this quantity. The vault called it "reliability" and treated it as interchangeable with test–retest, which measures [[metric-stability|something else]].

| Metric | $\mathcal{D}$ | Source |
|---|---|---|
| [[expected-threat\|xT]] | **0.89** | [[on-ball-actions-football-xt-vs-vaep\|Van Roy et al.]] |
| [[vaep\|VAEP]] | **0.25** | Van Roy et al. |
| VAEP, attacking actions only | 0.59 | Van Roy et al. |
| **Off-ball defensive, best of twelve** | **0.656** *(0.280 role-adjusted)* | Recovered in-vault — see below |
| **Off-ball defensive, worst of twelve** | **0.342** *(0.077 role-adjusted)* | Recovered in-vault |

⚠️ **The first three and the last two are not strictly comparable** — different estimators (split-half against one-way ICC), different populations, and different [[aggregation-denominator|denominators]]. They are indicative of scale, not a ranking.

## Two Ways to Score Well, Only One of Them Good

This is the property's central trap and the paper is explicit about it.

Franks et al. find rebounds, blocks and assists highly discriminative — **because they indicate position.** Centres rebound; guards do not. The between-player variance is large and real, and almost none of it is about *ability*.

> ### `discrimination-rewards-measuring-position`
> **A metric that separates player types scores identically to a metric that separates player quality, because both produce large true between-player variance. Discrimination cannot tell them apart, and in a sport with strongly differentiated roles the positional signal is the larger one. A high score is therefore evidence the metric measures *something* stable, not evidence it measures skill.**
> ^[generated: the paper states the position confound but does not frame it as a general limit of the property. rests-on: source:franks-blocks-rebounds-position]

**The fix is stated and cheap:** recompute conditional on player type. Franks et al. note the meta-metrics are defined through expectations and variances, so conditional versions require only replacing marginal moments with conditional ones.

⚠️ **This lands directly on football**, where positional differentiation is severe and the vault's metrics are known to sort by position. See [[construct-validity]] on the ρ = 0.182 problem, which this test resolves.

### ✅ Measured, 2026-08-29 — and It Is About Half the Signal

> Computed in-vault from [[off-ball-defensive-performance-blame|Bischofberger et al.'s]] committed World Cup data, which ships per-player per-match values for twelve off-ball defensive metric variants.

| | Unadjusted ICC(1,1) | After removing role means |
|---|---|---|
| Best (raw fault per pass against) | 0.656 | **0.280** |
| Worst (valued contribution per pass) | 0.342 | **0.077** |
| Range across twelve variants | 0.34 – 0.66 | **0.08 – 0.30** |

^[generated: computed in-vault, not reported by any source; full table and caveats on [[off-ball-defensive-performance-blame]]. rests-on: source:bischofberger-committed-player-level-csv]

**Every metric falls by 0.21 to 0.39, and the drop is largest for the metrics that score best unadjusted.** Raw fault per 90 loses 0.394 — more than half its apparent reliability.

> ### `off-ball-defensive-metrics-are-poor-once-position-is-removed`
> **Roughly half the apparent reliability of off-ball defensive metrics is positional rather than individual. Unadjusted they reach "moderate"; role-adjusted, every one is "poor" on Koo & Li's bands.**
> ^[generated: declared on [[off-ball-defensive-performance-blame]]]

This is the concrete version of the warning above. **A metric can look moderately reliable and be measuring, half of the time, that a centre-back is a centre-back.**

⚠️ **Caveats.** Subtracting role means is blunter than Franks' covariate adjustment and **probably over-corrects**, so the adjusted column is indicative rather than exact. World Cup only, median three matches per unit, no confidence intervals. **The direction and magnitude of the drop are the finding; the levels are not precise.**

## What Would Raise It, and What That Costs

$\mathcal{D}$ rises when $\tau^2_M$ falls — less sampling noise per player-season. Three levers:

| Lever | Mechanism | Cost |
|---|---|---|
| **More observations** | $\tau^2_M$ shrinks with sample size | None, if the data exists |
| **[[empirical-bayes-shrinkage\|Shrinkage]]** | Borrows strength from a prior | Bias; assumes a stable underlying level |
| **Restrict scope** | Drop the noisiest action types | Measures less |
| ⚠️ **Aggregate over playing time** | Total-based metrics inherit appearance-count variance | **Inflates $\mathcal{D}$ without adding signal** |

That last row is a warning, not a lever. Franks et al. find win shares and VORP outscore rate metrics **primarily because minutes played is itself highly discriminative** — not because they carry more information about ability. Minutes played scores 0.96.

**Any football metric summed over a season inherits the discriminative power of appearance count**, which is largely a selection and availability signal. Rate and total metrics should not be compared on this score at all.

## Why 0.25 Is Worse Than It Looks

At $\mathcal{D} = 0.25$, **three quarters of the observed spread in VAEP between players in a season is chance.** Franks et al. reach the same conclusion about three-point percentage at 0.43 and call it the least reliable metric they examine.

VAEP is lower than the worst metric in their NBA set.

^[generated: the comparison across sports is drawn here; the two figures come from different papers, different sports and different estimators — Van Roy's split-half against Franks' bootstrap — so the ordering is indicative, not a measurement. rests-on: source:vanroy-vaep-splithalf, source:franks-3pt-discrimination]

⚠️ **The estimators are not identical.** A split-half correlation and a bootstrap-based variance ratio target the same estimand but need not agree numerically, and Van Roy et al. do not report whether a Spearman–Brown correction was applied. **Reconciling them on one dataset is itself an unrun check.**

## See Also

- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the source · [[metric-stability]] · [[metric-independence]] · [[metric-variance-components]]
- [[split-half-reliability]] — the estimator the vault has been using · [[reliability-layers]] · [[operator-reliability]]
- [[empirical-bayes-shrinkage]] — the cheapest way to raise it
- [[construct-validity]] · [[predictive-validity]] · [[within-season-variation-noise-or-signal]] · [[performance-volatility]]
- [[vaep]] · [[expected-threat]] · [[off-ball-value]] · [[c-obso]] · [[obso]] · [[recruitment]]
