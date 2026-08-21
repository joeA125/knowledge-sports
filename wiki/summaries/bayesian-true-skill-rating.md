---
title: "TrueSkill: A Bayesian Skill Rating System — Source Summary"
type: summary
tags: [bayesian, statistics, ranking-system, message-passing, matchmaking, gaming, inference, evaluation]
sources: [raw/papers/bayesian-true-skill-rating.md]
confidence: 0.9
provenance:
  extracted: 82%
  inferred: 12%
  generated: 4%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# TrueSkill: A Bayesian Skill Rating System

**Herbrich, Minka & Graepel**, [[microsoft-research]], NeurIPS 2006.

> **Held here because [[ai-football-reinforcement-learning|Scott et al.]] use it to rank RL agents**, and because it is the ancestor of the [[glicko-rating-system|Glicko]] machinery behind [[league-strength-rating]] and [[duel-skill-rating]]. The factor-graph inference detail lives in the general vault.

## The Model

- Each player has a skill $s_i \sim \mathcal{N}(\mu_i, \sigma_i^2)$ — **a belief, not a number.**
- In a game they exhibit a performance $p_i \sim \mathcal{N}(s_i, \beta^2)$ around that skill.
- Team performance is the sum of individual performances; outcomes are the ordering of team performances, with an explicit draw margin $\epsilon$.
- The posterior follows from [[bayes-theorem]] and is computed by [[message-passing]], with [[expectation-propagation]] approximating the non-Gaussian comparison factors by moment matching.

Efficient enough to run within twice the cost of an Elo update.

## What It Generalises

| | Uncertainty | Draws | Teams | Multi-player |
|---|---|---|---|---|
| [[elo-rating-system\|Elo]] | No | No | No | No |
| [[glicko-rating-system\|Glicko]] | **Yes** | No | No | No |
| **TrueSkill** | **Yes** | **Yes** | **Yes** | **Yes** |

## Results and Deployment

On Halo 2 beta data, TrueSkill beat Elo on prediction accuracy across most modes and converged in **~10 games against Elo's hundreds** — near the information-theoretic limit of about 5 for 8-player matches.

Deployed on Xbox Live at 2+ million subscribers, one of the largest applications of [[bayesian-inference]] at the time. Prior $\mu_0 = 25$, $\sigma_0 = 25/3$; displayed rating is the conservative $\mu_i - 3\sigma_i$.

## Why It Reaches Football

**Directly:** [[ai-football-reinforcement-learning|Scott et al.]] rank 15 RL agents by TrueSkill over 5,250 round-robin matches in GFootball, and every play-style correlation they report is measured against that axis.

The fit is unusually good for a reason the paper could not have anticipated. **Frozen model checkpoints satisfy the stationarity assumption humans violate** — but with cheap matches, the uncertainty machinery that justifies TrueSkill over a win rate is largely redundant. See `latent-skill-models-suit-frozen-agents-better-than-people` on [[trueskill]].

**Indirectly:** the Gaussian-belief lineage runs through Glicko into [[league-strength-rating]] and [[duel-skill-rating]], where it does real work — a club rated on thin evidence should not be treated like one rated on a full season.

## Two Deployment Observations Worth Keeping

**Matchmaking creates feedback loops** — players game the system to protect ratings. The football analogue is unaddressed: any rating fed back into team selection or scouting changes the behaviour it measures.

**Skill distribution shifts below the prior** if new entrants consistently lose early. Relevant to [[league-strength-rating]], where teams enter continental competition with priors set by domestic form.

## See Also

- [[trueskill]] · [[elo-rating-system]] · [[glicko-rating-system]] · [[bradley-terry-model]] · [[league-strength-rating]] · [[duel-skill-rating]]
- [[bayesian-inference]] · [[bayes-theorem]] · [[expectation-propagation]] · [[message-passing]] · [[uncertainty-quantification]]
- [[microsoft-research]] · [[agent-based-simulation]] · [[google-research-football]]
- [[ai-football-reinforcement-learning|Scott et al. Summary]]
