---
title: "Bayes' Theorem"
type: concept
tags: [bayesian, statistics, inference]
sources: [raw/papers/bayesian-true-skill-rating.md]
confidence: 0.9
provenance:
  extracted: 30%
  inferred: 58%
  generated: 8%
  imported: 0%
  ambiguous: 4%
lifecycle: draft
created: 2026-05-08
updated: 2026-08-14
---

# Bayes' Theorem

Relates conditional and marginal probabilities:

$$P(A \mid B) = \frac{P(B \mid A) \, P(A)}{P(B)}$$

In [[bayesian-inference]] it is the rule for updating a prior belief with observed data to obtain a posterior.

## In TrueSkill

[[trueskill]] derives the posterior over player skills given a game outcome and the team assignments:

$$p(\mathbf{s} \mid \mathbf{r}, A) = \frac{P(\mathbf{r} \mid \mathbf{s}, A) \, p(\mathbf{s})}{P(\mathbf{r} \mid A)}$$

The likelihood $P(\mathbf{r} \mid \mathbf{s}, A)$ is where the domain lives — it encodes how skills produce a ranking, including draws and team composition. **The theorem is generic; the modelling is entirely in the likelihood.** That is the useful thing to carry over: two rating systems differ not in their use of Bayes but in what they assume about how skill becomes a result.

## See Also

- [[bayesian-inference]] · [[trueskill]] · [[expectation-propagation]] · [[uncertainty-quantification]]
- [[glicko-rating-system]] · [[bradley-terry-model]] · [[elo-rating-system]]
- [[bayesian-true-skill-rating|TrueSkill Summary]]
