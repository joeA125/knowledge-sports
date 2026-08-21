---
title: "Bayesian Inference"
type: concept
tags: [bayesian, statistics, inference, uncertainty-quantification, ranking-system]
sources: [raw/papers/bayesian-true-skill-rating.md]
confidence: 0.85
provenance:
  extracted: 40%
  inferred: 48%
  generated: 8%
  imported: 0%
  ambiguous: 4%
lifecycle: draft
created: 2026-05-08
updated: 2026-08-14
---

# Bayesian Inference

Updating beliefs about unknown quantities in light of observed data, via [[bayes-theorem]]:

$$p(\theta \mid \text{data}) = \frac{p(\text{data} \mid \theta) \, p(\theta)}{p(\text{data})}$$

The posterior combines the likelihood with a prior belief. Three properties matter for how it is used here:

- **The posterior is a distribution, not a point estimate** — so uncertainty comes out of the inference rather than being bolted on.
- **Sequential updating:** today's posterior is tomorrow's prior, which is what makes online rating systems work at all.
- **Principled model comparison** via marginal likelihoods.

## Where It Runs in This Vault

Bayesian machinery appears in more football pages than its profile suggests:

| Framework | What is inferred |
|---|---|
| [[trueskill]] | Player and agent skill, updated game by game via [[expectation-propagation]] |
| [[glicko-rating-system\|Glicko]] | Skill plus a rating deviation that widens with inactivity |
| [[martingale-epv]] | Possession value, via [[inla]] and a [[car-prior]] over pitch locations |
| [[gaussian-process]] | Nonparametric surfaces over spatial data |
| [[football-event-sequences-point-process-mixture\|Possession clustering]] | Component membership under a mixture model |

The sequential-updating property is the one doing the most work. A rating system is a Bayesian filter with a domain-specific likelihood, and that framing is what lets [[ai-football-reinforcement-learning|Scott et al.]] apply a human matchmaking system to frozen RL checkpoints without modification — see the stationarity argument on [[trueskill]].

## The Gap Between Computing and Using

Worth recording because it recurs. Every framework above produces a posterior; several then **discard the variance and pass a point estimate downstream.** [[league-strength-rating]] and [[duel-skill-rating]] both track rating deviation and feed a number to the next stage.

A club rated 1700 on thin continental evidence is not the same input as one rated 1700 on a full season, and nothing in the pipeline distinguishes them. See [[uncertainty-quantification]].

> **Scope note.** General Bayesian methodology, and the graphical-model inference machinery around it, live in the general vault. This page keeps what the football frameworks rest on.

## See Also

- [[bayes-theorem]] · [[expectation-propagation]] · [[message-passing]] · [[uncertainty-quantification]]
- [[trueskill]] · [[glicko-rating-system]] · [[elo-rating-system]] · [[league-strength-rating]] · [[duel-skill-rating]]
- [[martingale-epv]] · [[inla]] · [[car-prior]] · [[gaussian-process]] · [[mixture-model]]
- [[bayesian-true-skill-rating|TrueSkill Summary]]
