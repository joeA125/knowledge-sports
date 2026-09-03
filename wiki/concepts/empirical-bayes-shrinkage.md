---
title: "Empirical Bayes Shrinkage"
type: concept
tags: [statistics, reliability, evaluation, player-evaluation, model-selection, sports-analytics, hierarchical-model, uncertainty-quantification]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/metric-stability-elite-football.md]
confidence: 0.8
provenance:
  extracted: 54%
  inferred: 16%
  generated: 28%
  imported: 2%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Empirical Bayes Shrinkage

**Pull each player's noisy estimate toward a prior, and accept bias to buy a large reduction in variance.**

The prior is estimated from the data itself — hence *empirical* Bayes — typically a player's own career average or the population mean. Players with few observations move a long way; players with many barely move.

From [[meta-analytics-sports-metrics|Franks et al. (2016)]], who use it as their worked example of improving a metric without changing what it measures.

## The Demonstration

Three-point percentage is the least reliable metric in their NBA set: **over half the between-player variance in a season is chance.** They fit a hierarchical Beta-Binomial model,

$$X^{3P\%}_{sp} = \frac{z_{sp}}{n_{sp}}, \qquad z_{sp} \sim \text{Bin}(n_{sp}, \pi_{sp}), \qquad \pi_{sp} \sim \text{Beta}\!\left(r_p \pi^0_p,\ r_p(1 - \pi^0_p)\right)$$

where $\pi^0_p$ is the player's career average and $r_p$ — estimated from the data — controls how hard each player is shrunk.

| | [[metric-discrimination\|Discrimination]] | [[metric-stability\|Stability]] |
|---|---|---|
| Raw 3P% | 0.43 | 0.30 |
| **Shrunken** | **0.53** | **0.64** |

**Both properties improve at once**, which no other route on [[split-half-reliability]] achieves. The estimator is biased for every single player and has lower mean squared error anyway — the standard variance-reduction result.

## Why It Matters Here Specifically

The vault's complaint is that six off-ball mechanisms have no reliability figures and would probably score badly if they had them — see the derivation-depth argument on [[off-ball-value]].

> **Shrinkage is the only remedy on the table that is post-hoc.** Restricting scope and withholding outcome information both require redesigning what the metric measures. Shrinkage takes an existing metric's outputs and a player history and needs **no change to the mechanism at all** — no retraining, no new data, no access to the model.
> ^[generated: the contrast against the vault's other two routes is drawn here. rests-on: source:franks-empirical-bayes-3pt]

For [[c-obso|C-OBSO]] or [[obso|OBSO]], where the pipeline is five transformations deep and expensive to rerun, that asymmetry is decisive.

## ⚠️ It Assumes Away the Thing the Vault Is Arguing About

Shrinking toward a career average encodes a prior belief that **a player's true level is roughly constant.** That is exactly what [[performance-volatility]] denies and what [[within-season-variation-noise-or-signal]] exists to adjudicate.

> ### `shrinkage-buys-reliability-by-assuming-the-answer`
> **Shrinkage raises stability by construction, because it suppresses precisely the between-season movement that stability measures. If form and development are real, the improved number reflects signal that has been erased rather than noise that has been removed — and the statistic cannot distinguish the two, because the assumption is in the prior rather than in the data.**
> ^[generated: no source raises this; drawn from the estimator's construction against the vault's volatility material. rests-on: source:franks-empirical-bayes-3pt, claim:reliability-volatility-identity]

Franks et al. are not wrong to use it — three-point shooting really is close to a fixed ability, which is why free-throw percentage scores 0.98 on stability. **The question is whether football's off-ball metrics are that kind of quantity**, and the vault's own material leans the other way: [[gps-deceleration-reliability|Jones et al.]] found an elite player cannot reproduce his own maximal braking a week later.

**A metric measuring something genuinely volatile should not be shrunk**, and shrinking it will make it look better by every meta-metric while making it worse for the decision it serves.

### ⚠️ Where the Assumption Fails Hardest Is Where Shrinkage Is Most Wanted

> **Added 2026-08-29** on ingest of [[year-to-year-metric-stability-football|Shaikh (2026)]].

Veterans (30+) are more stable than U23 players in **18 of 24** position-metric combinations, 15 at $p < 0.001$. Forwards' progressive passes run 0.621 for U23 against 0.825 for veterans; xG runs 0.643 against 0.809.

**A career average is a well-estimated prior for a 32-year-old and a poor one for a 20-year-old** — because the young player's history is short *and* because he is genuinely still changing. Shrinking a U23 rating toward it suppresses developmental trajectory, which is the signal a scouting operation is specifically trying to read.

> **The population where shrinkage would most improve the numbers is the population where it most destroys the meaning.** Squad-building attention concentrates on young players precisely because their futures are uncertain, and shrinkage answers that uncertainty by assuming it away.
> ^[generated: the paper reports the age effect but does not connect it to shrinkage. rests-on: source:shaikh-fisher-z-age, claim:shrinkage-buys-reliability-by-assuming-the-answer]

See [[player-development-curve]] and [[recruitment]].

⚠️ **One qualification, from the same paper.** The age effect is **absent for defenders** — tackles, blocks, interceptions and clearances show no significant U23-versus-veteran difference. Shaikh's reading is that where team context dominates individual signal, there is nothing for age-related convergence to act on.

So the caution above applies to *attacking and midfield* metrics. For defensive volume counts the prior is equally poor at every age, which is a different and worse problem: **shrinkage cannot help a metric whose instability is contextual rather than sampling-driven**, because the career average is contaminated too.^[generated: drawn from the null age result. rests-on: source:shaikh-def-age-null]

### And the prior itself decays

Shrinking toward a career average pulls toward seasons whose signal has already faded — see [[stability-decay]], where all 24 combinations lose a median $\Delta r = -0.097$ by the two-year horizon. **The two prescriptions point opposite ways** and nobody has reconciled them.

## Choosing the Prior Is Choosing What You Believe

The two natural priors encode different claims and are not interchangeable:

| Prior | Claim | Effect |
|---|---|---|
| **Player's career average** | Ability is durable; this season is a noisy read of it | Preserves between-player differences; suppresses form |
| **Population mean** | Extreme values are mostly luck | Compresses toward average; penalises genuine outliers |
| **Position-group mean** | Ability is durable *within role* | ⚠️ Bakes positional structure in — see below |

The third is tempting in football and dangerous. [[metric-discrimination]] records that discrimination rewards measuring position; shrinking toward a **position-group** mean would remove between-position variance from the metric while raising its scores, which is the same confound entering through a different door.^[generated: no source considers position-group priors]

## Where the Vault Already Has This and Did Not Name It

[[gvdep|GVDEP]] rescales VDEP's weighting using VAEP-derived values rather than raw event frequencies — a data-informed adjustment of a component that was previously asserted. That is not shrinkage, but it is the same instinct: **replace a noisy or arbitrary quantity with one borrowed from a larger pool.**^[inferred: the connection is drawn here; GVDEP does not describe itself in these terms]

More directly, `no-held-football-source-uses-shrinkage` — **no held football source applies hierarchical shrinkage to a player metric**, despite the technique being standard in baseball for decades and demonstrated here on basketball.^[generated: an absence claim, newly declared. rests-on: source:franks-empirical-bayes-3pt]

## See Also

- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the source
- [[metric-discrimination]] · [[metric-stability]] · [[metric-variance-components]] · [[metric-independence]]
- [[split-half-reliability]] — the three routes · [[reliability-layers]] · [[regularization]]
- [[performance-volatility]] · [[within-season-variation-noise-or-signal]] · [[player-rating-time-series]]
- [[obso]] · [[c-obso]] · [[vdep]] · [[gvdep]] · [[off-ball-value]] · [[recruitment]]
