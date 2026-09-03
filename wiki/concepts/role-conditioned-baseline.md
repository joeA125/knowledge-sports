---
title: "Role-Conditioned Baseline"
type: concept
tags: [off-ball, defensive-valuation, counterfactual, tactical-analysis, player-evaluation, evaluation, reliability, optical-tracking-data, sports-analytics]
sources: [raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.8
provenance:
  extracted: 56%
  inferred: 16%
  generated: 26%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Role-Conditioned Baseline

**What a player in this role should have been doing, averaged over every comparable situation in the competition.**

From [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]], where it is called **responsibility**: the expected [[defensive-pressure-area|involvement]] for each triplet of *(passer role, receiver role, defender role)*.

A defender is charged his role's average involvement for a pass **whether or not he was anywhere near it.**

## Why It Exists

[[defensive-pressure-area|Proximity-based attribution]] has a hole the paper states plainly: a defender who abandons his position causes danger *elsewhere* and receives no blame, **because blame requires being close.** The metric that measures involvement cannot see non-involvement.

Responsibility inverts this. It asks not *were you there* but *should you have been*, and the answer comes from what players in that role typically do.

| | Charges you for | Blind to |
|---|---|---|
| **Involvement** | Being near a bad pass | Being absent from one you should have covered |
| **Responsibility** | Your role's expected presence | Individual variation within the role |

Roles come from **template-matching formation detection** — seven 11-player formations plus four short-handed ones, 20 roles in 6 groups, matched per possession phase by linear sum assignment and smoothed with a 7.5-minute Gaussian kernel to stop spurious switches.

## This Is a Counterfactual With No Counterfactual Model

Every other counterfactual construct in the vault builds a **model of the alternative**: [[c-obso|C-OBSO]] predicts trajectories, [[drso|DRSO]] searches for optimal positions, [[counterfactual-baseline|baseline-differencing]] generally requires a fitted comparator.

Responsibility gets its counterfactual from a **conditional average over observed play**. The alternative is not simulated; it is *what everyone else in that role did*.

> ### `population-averages-are-counterfactuals-you-can-afford`
> **Conditioning on role and averaging over a competition answers "what should have happened here" without any generative model, because the population supplies the comparator. It is cheaper than simulation and it inherits the population's biases wholesale — including systematic tactical tendencies that a model would have had to state explicitly.**
> ^[generated: no source frames responsibility as a counterfactual construct; the contrast with the vault's model-based counterfactuals is drawn here. rests-on: source:bischofberger-responsibility-definition]

The inherited-bias problem is not hypothetical, and the authors flag a version of it: the responsibility model **may be biased toward weaker teams and passive defending styles**, because those teams face more passes and are therefore overrepresented in the averages.

⚠️ **So the baseline is not neutral.** "What a left-back typically does against this pass" is dominated by left-backs on teams that defend a lot. A player at a dominant team is measured against a norm set largely by players in a different tactical situation.

## It Is Shrinkage, and It Does What Shrinkage Does

The paper reports that adding responsibility is one of two properties that **raise robustness** — replacing a player's measured involvement with his role's average lowers event-level variance while keeping players distinguishable.

That is [[empirical-bayes-shrinkage|pooling toward a group mean]], and the vault's page on it flags exactly this prior as dangerous: **shrinking toward a position-group mean bakes positional structure into the metric.**

> **The same operation is a confound in one framing and the construct in another.** [[metric-discrimination]] warns that discrimination rewards metrics that track position rather than skill. Responsibility deliberately encodes role — because defensive duty genuinely *is* role-determined. Which reading applies depends on whether role is the thing being measured or a nuisance to be removed, and **no statistic can settle that.**
> ^[generated: drawn across the two pages; neither source addresses the other. rests-on: claim:shrinkage-buys-reliability-by-assuming-the-answer, source:bischofberger-responsibility-stability]

## The Mbappé Case

The paper's own example of how far the two constructs diverge.

At the 2022 World Cup, **Kylian Mbappé ranks third best defending winger by raw fault** — he was rarely near dangerous passes — **and third weakest by raw fault responsibility**, because his role implied he should have been.

Two closely related metrics on one player, opposite conclusions, and both are defensible readings of "defensive performance". The authors note this contrasts with offensive metrics like pass completion and [[expected-threat|xT]], where one metric serves across roles.

> **Defensive metrics may be irreducibly role-specific in a way offensive ones are not**, because defensive duty is assigned by shape while attacking contribution is largely taken. If so, a single off-ball defensive number is not a coherent object, and the ρ = 0.182 disagreement on [[off-ball-value]] is what that incoherence looks like when two such numbers are correlated.
> ^[generated: the paper states the role-dependence and the contrast with offensive metrics but does not connect it to cross-metric disagreement]

## The Sparsity Constraint Nobody Escapes

Responsibility is estimated per *(passer role, receiver role, defender role)* triplet. With 20 roles that is up to 8,000 cells, and the authors are explicit that richer conditioning — ball position, team identity, pressing style — was rejected as a **trade-off between robustness and informativeness**.

That is the same bind [[metric-discrimination]] describes for conditional meta-metrics and [[network-cohesion]] describes for community detection: **conditioning buys correctness and costs sample size**, and football's unit counts are small. The fix the authors name for the weak-team bias — adding team identity — is exactly the conditioning they could not afford.

## See Also

- [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]] — the source · [[defensive-pressure-area]] · [[aggregation-denominator]]
- [[counterfactual-baseline]] · [[counterfactual-simulation]] · [[c-obso]] · [[drso]] · [[observed-versus-optimal-decisions]]
- [[empirical-bayes-shrinkage]] · [[metric-discrimination]] · [[metric-stability]] · [[reliability-layers]]
- [[off-ball-value]] · [[defensive-valuation]] · [[tactical-analysis]] · [[expected-threat]] · [[construct-validity]]
