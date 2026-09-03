---
title: "Defensive Pressure Area"
type: concept
tags: [off-ball, defensive-valuation, optical-tracking-data, tactical-analysis, player-evaluation, evaluation, spatiotemporal, sports-analytics, single-source]
sources: [raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.8
provenance:
  extracted: 62%
  inferred: 14%
  generated: 22%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Defensive Pressure Area

**A geometric region around a pass, within which defenders are held answerable for it.**

From [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]]. The DPA is how the paper converts a pass — an *attacking* event — into per-defender blame or credit, without any model of what the defenders were trying to do.

## Four Shapes, One Per Pass Outcome

The area's shape depends on what the pass did, which is the design's cleverest part.

| Pass | DPA | Rationale |
|---|---|---|
| **Successful, raised threat** | Circles ($r$ = 5 m) around passer **and** receiver, plus a rectangle along the lane | Three ways to have failed: pressure the passer, contest the reception, intercept en route |
| **Successful, lowered threat** | Circle around the **passer only** | Typically a backward pass — the defenders who forced it are the ones near the ball |
| **Unsuccessful, out of play** | Circle around the **passer only** | Same logic |
| **Intercepted** | **None** | Value goes directly to the intercepting defender |

Within the area, a defender's share scales with proximity: $R_{p,i} = (r - d_i)/r$, where $d_i$ is distance to the DPA edge. Multiply by the pass's defensive value and you get $V_{p,i}$.

The sign of the pass value renames the same quantity:

| $xDT$ | Name | Meaning |
|---|---|---|
| Positive | **Contribution** | Was near, and threat fell |
| Negative | **Fault** | Was near, and threat rose anyway |

**Involvement** is the sum of absolute values — activity regardless of outcome.

## Why the Radius Is Handcrafted, and Why That Argument Is Good

5 metres, chosen by judgement. The authors' defence of not fitting it is unusually careful and worth recording in full force:

> Since there is no ground truth for defensive involvement or the individual attribution of offensive value, the specific areas are handcrafted rather than determined empirically. Data-driven modelling or parameter optimization with respect to unreliable benchmarks would risk overfitting and reproducing biases present in the selected target variable.

> ### `fitting-a-parameter-to-a-bad-benchmark-is-worse-than-asserting-it`
> **Where no ground truth exists, optimising a free parameter against a proxy does not remove the arbitrariness — it launders it, replacing a visible judgement with an invisible inheritance of the proxy's biases. An asserted parameter can be argued about; a fitted one carries the authority of having been "learned".**
> ^[generated: the paper states the reasoning for its own case but does not generalise it. rests-on: source:bischofberger-handcrafted-dpa]

⚠️ **This is a genuine complication for [[free-parameters-load-bearing]]**, which catalogues eighteen asserted parameters across seven kinds and treats assertion as the failure. **Here assertion is the considered choice and fitting would be the error.** The page's implicit standard — that a parameter should be swept or fitted — is not universal, and this is the counter-case.

The distinction that survives: **a parameter should be swept for *sensitivity* even where it cannot be fitted for *value*.** Knowing whether 4 m or 6 m changes the rankings is separable from knowing which is right, and this paper does not report it either.^[generated: the distinction is drawn here]

The authors also rejected **motion-model reachable areas**, on the grounds that physically reachable is not tactically permitted — a defender constrained by team shape cannot go where his legs could take him. That is a substantive argument against the [[pitch-control]] tradition's usual approach, made in passing.

## What It Buys and What It Costs

**Buys:** no trajectory prediction, no fitted control surface, no training. The whole attribution is distance arithmetic on synchronised tracking and event data, which makes it cheap enough to run over 78 million frames.

**Costs:** the DPA has no notion of **time, velocity, body orientation or pressure intensity** — all named in limitations. A defender sprinting away from a pass and one closing it down score identically if they are equidistant at the moment of release.

> Contrast [[c-obso|C-OBSO]] and [[drso|DRSO]], which spend a trajectory-prediction model to ask comparable questions. This is the same trade recorded on [[network-cohesion]] for edge connectivity — **a cheap counterfactual that answers a narrower question** — and the [[off-ball-defensive-performance-blame|robustness results]] suggest the shallow version is *more repeatable* for exactly that reason.
> ^[inferred: the paper does not compare against trajectory-based approaches; the connection to the vault's derivation-depth material is drawn here]

## The Attribution Is Not Zero-Sum

Nothing normalises a pass's value across the defenders it touches. Two defenders both within 1 m of the passer each receive close to the full proximity weight, so **total attributed blame for a pass can exceed the pass's value**, and for a pass with no defender nearby it is zero.

That is defensible — the paper is measuring per-player accountability, not decomposing a fixed quantity — but it means **involvement scores are not comparable across passes** and only become interpretable after aggregation. It also means the metric rewards defensive *crowding*, which the responsibility model on [[role-conditioned-baseline]] is partly designed to correct.^[inferred: the paper does not discuss normalisation; drawn from the equations]

## See Also

- [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]] — the source
- [[role-conditioned-baseline]] — the expected-involvement correction · [[aggregation-denominator]]
- [[off-ball-value]] · [[defensive-valuation]] · [[c-obso]] · [[drso]] · [[space-occupation-gain]] · [[pitch-control]]
- [[expected-threat]] · [[counterfactual-baseline]] · [[free-parameters-load-bearing]] · [[network-cohesion]]
- [[optical-tracking-data]] · [[tactical-analysis]] · [[metric-discrimination]] · [[reliability-layers]]
