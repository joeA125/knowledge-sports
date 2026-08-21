---
title: "TrueSkill"
type: concept
tags: [bayesian, ranking-system, matchmaking, gaming, statistics, reinforcement-learning, evaluation, simulator, agent-based-simulation]
sources: [raw/papers/bayesian-true-skill-rating.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.9
provenance:
  extracted: 74%
  inferred: 18%
  generated: 5%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# TrueSkill

A Bayesian skill rating system developed by [[microsoft-research]] (Herbrich, Minka & Graepel, 2006) that generalises the [[elo-rating-system]]. Built for matchmaking in online gaming; held here because [[ai-football-reinforcement-learning|Scott et al.]] use it to rank RL agents.

## Key Properties

- **Uncertainty tracking:** skill is a Gaussian belief $\mathcal{N}(\mu, \sigma^2)$, not a point estimate.
- **Draw modelling:** an explicit draw margin $\epsilon$ models tied outcomes.
- **Team support:** individual skills inferred from team results, modelling team performance as the sum of individual performances.
- **Fast convergence:** approaches target skill in ~10 games for 8-player matches, near the information-theoretic limit.

The displayed rating is deliberately conservative — $\mu - 3\sigma$ — so leaderboard tops hold only players who are both highly skilled *and* well-measured.

Inference runs as [[message-passing]] on a graphical model, with each game's posterior becoming the next game's prior. The machinery is covered in the general vault; what matters here is that **the output is a single number on a common scale with a calibrated uncertainty attached.**

## Rating Agents Rather Than People

The vault's only application of TrueSkill to non-human competitors, and the reason this page is football-side at all.

[[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] rank **15 RL agents** — five training checkpoints from each of three curricula — via **50 round-robin tournaments, 5,250 matches**, in [[google-research-football|GFootball]].

The fit is unusually good, for a reason the original paper could not have anticipated:

| TrueSkill assumes | Human leagues | **RL checkpoints** |
|---|---|---|
| Skill is latent and unobservable | Yes | Yes |
| Matches are expensive to run | Yes | **No — arbitrarily many** |
| Skill drifts over time | Yes | **No — a checkpoint is frozen** |
| Competitors are numerous | Yes | 15 |

**The uncertainty machinery that justifies TrueSkill for matchmaking is largely wasted here.** With frozen agents and 5,250 cheap matches, $\sigma$ collapses and the ranking is essentially a well-regularised round-robin win rate. What it supplies is a *principled single number* — which is what the paper needs, since its whole analysis correlates play-style statistics against that number.

> ### `latent-skill-models-suit-frozen-agents-better-than-people`
> **Skill-rating systems built for humans transfer unusually cleanly to model checkpoints, because checkpoints satisfy the stationarity assumption that humans violate. The cost is that the uncertainty apparatus, which is the reason to prefer such systems over win rates, becomes redundant once matches are cheap.**
> ^[generated: no source states this; drawn from the assumption set of the original paper read against Scott et al.'s use. rests-on: source:herbrich-trueskill-assumptions, source:scott-trueskill-tournament]

## The Anomaly That Followed

Scott et al.'s ranking produced a result they call counter-intuitive and do not resolve: **agents trained against the *easy* bot rank 1, 2 and 3**, above agents trained four times longer against the hard bot.

This matters beyond their paper because **every correlation they report is measured against this axis** — including the finding that better agents shoot more, which contradicts [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] on real football. See [[observed-versus-optimal-decisions]].

Two readings, undistinguished by anything held:

- **The ranking is right and the curriculum intuition is wrong** — training against weak opponents may produce more transferable play than over-fitting to one strong opponent's exploitable habits.
- **The ranking measures something narrower than competence** — round-robin performance against *this particular pool of 15* rather than football ability.

The second is what TrueSkill's own framing warns about: a rating is defined relative to the population it was estimated in. **A leaderboard of fifteen agents that only ever played each other is a closed system**, and nothing anchors it to external football quality.

## Where It Sits Among the Rating Systems Held Here

| | Origin | Use in this vault |
|---|---|---|
| [[elo-rating-system\|Elo]] | Chess | League and duel strength |
| [[glicko-rating-system\|Glicko]] | Generalises Elo with uncertainty | [[league-strength-rating]], Sirotkin's duel work |
| [[bradley-terry-model\|Bradley-Terry]] | Paired comparison | [[duel-skill-rating]] |
| **TrueSkill** | Xbox matchmaking | **Ranking RL agents** |

TrueSkill is the outlier: the other three rate *footballers*, and this one rates *models*.

## See Also

- [[elo-rating-system]] · [[glicko-rating-system]] · [[bradley-terry-model]] · [[league-strength-rating]] · [[duel-skill-rating]]
- [[bayesian-inference]] · [[expectation-propagation]] · [[message-passing]] · [[uncertainty-quantification]]
- [[agent-based-simulation]] · [[google-research-football]] · [[proximal-policy-optimization]] · [[social-network-analysis]] · [[reinforcement-learning]] · [[observed-versus-optimal-decisions]]
- [[microsoft-research]] · [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]]
- [[bayesian-true-skill-rating|TrueSkill Summary]] · [[ai-football-reinforcement-learning|Scott et al. Summary]]
