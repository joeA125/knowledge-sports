---
title: "Action-Space Design"
type: concept
tags: [action-space, reinforcement-learning, multi-agent, game-theory, counterfactual, action-valuation, off-ball, sports-analytics, feature-engineering, evaluation, simulator, domain-adaptation, policy-gradient]
sources: [raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/expected_value_possession_framework.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 58%
  inferred: 28%
  generated: 13%
  imported: 0%
  ambiguous: 1%
lifecycle: reviewed
created: 2026-08-07
updated: 2026-08-08
---

# Action-Space Design

The choice of *what counts as an action* — and, where the underlying behaviour is continuous, how it is discretised. Usually presented as an implementation detail; it is the single choice that determines which questions a valuation framework can answer.

## Five Held Action Spaces

| Framework | Action space | Size |
|---|---|---|
| [[expected-value-possession-framework\|Fernández et al.]] | Pass destination — a **[[probability-surface\|surface]]** | Effectively continuous |
| [[optimal-decisions-shot-taking-situations\|Yeung & Fujii]] | {Shoot, Pass} × {Block, Not Block} | **4 profiles** |
| **[[ai-football-reinforcement-learning\|Scott et al.]]** | **GFootball's native set** | **19** |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 8 directions + idle + sprint start/stop/release + pass + shot | **14** |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | 8 directions + idle + high pass, short pass, shot | **12** |

> **The 19-action figure is now confirmed first-hand.** The vault had it second-hand from Nakahara et al.; Scott et al. enumerate the set directly: 8 directions, shot, short/high/long pass, idle, sliding, dribble, stop-dribble, sprint, stop-moving, stop-sprint. See [[google-research-football]].

## The Genealogy Is Now Visible, and It Is a Series of Deletions

All three RL papers descend from the same 19-action vocabulary, and each **removes** things:

| | Scott (19) | Nakahara (14) | Fujii (12) |
|---|---|---|---|
| Movement directions | 8 | 8 | 8 |
| Idle | ✓ | ✓ | ✓ |
| Sprint state | sprint, stop-sprint | **start, stop, release** | **dropped** |
| Dribble state | dribble, stop-dribble | **dropped** | **dropped** |
| Sliding | ✓ | **dropped** | **dropped** |
| Passing | short, high, long | **one** pass | **high, short** |
| Shot | ✓ | ✓ | ✓ |

**Nakahara et al.'s sprint start / stop / release triple stops looking arbitrary once you see the source.** GFootball's movement actions are *sticky* — they persist until an explicit stop action — so the environment needs stop-sprint and stop-moving as first-class actions. Nakahara et al. inherit that structure and apply it to **tracking data, where nothing latches.** The triple is a vestige of an environment they do not use.

That is a specific, checkable instance of a general risk: **an action vocabulary carries assumptions about the environment that produced it, and those assumptions do not travel.**

## Two Overlapping Author Sets, Same Data, Different Spaces

Nakahara et al. and Fujii et al. share authors, use **the same 54 Meiji J1 2019 matches from [[data-stadium|Data Stadium]]**, and both adapt GFootball's vocabulary — to 14 and 12 actions respectively. **Neither mentions the other's choice.**

> ### `action-space-sets-the-question`
> **A framework's action space fixes which counterfactual questions it can pose, and therefore what "suboptimal" can mean within it. Frameworks with different action spaces are not measuring the same construct even when they report the same quantity.**
> ^[generated: drawn across five held sources; none states it. **Strengthened twice** — first because the divergence occurs within one group on one dataset, removing the explanation that spaces differ because data or goals differ; then because the shared 19-action ancestor is now held, so the divergence is demonstrably subtraction from a common source rather than independent design. rests-on: source:fernandez-pass-surface, source:yeung-four-profiles, source:nakahara-14-actions, source:fujii-12-actions, source:scott-gfootball-19-actions]

**Nakahara et al. cannot ask "should he have played the ball long or short?"** Fujii et al. can. **Fujii et al. cannot ask "should he have started sprinting?"** Nakahara et al. can. Neither limitation is stated by either paper.

## The State Representation Is a Parallel Choice

> **Added 2026-08-08.** Actions get discussed; states mostly do not.

| Framework | State |
|---|---|
| Scott et al. | **Super Mini Map** — four $72\times96$ binary matrices (home, away, ball, active player), stacked over four frames |
| Nakahara et al. | **92-dim vector** — 23 entities × position and velocity |
| Fujii et al. | Positions and velocities, vector form |

Scott et al.'s is an **image**, consumed by a CNN with [[residual-connections|residual blocks]]. The other two are vectors consumed by a [[gated-recurrent-unit|GRU]] or MLP.

The consequences are real and unremarked. A binary occupancy grid **discards velocity** (recovered only by stacking four frames) and **discards player identity entirely** — the map knows a home player is at a cell, not which one. That is fine for controlling one active player and fatal for per-player valuation, which may be a second reason Scott et al. produce no player metric alongside `policy-gradient-forecloses-action-valuation` on [[proximal-policy-optimization]].

## What Each Design Buys and Costs

**Continuous surface.** Full spatial resolution, but defined over *destinations* rather than over what a player does — it can say where a pass should have gone and nothing about whether to pass. See [[observed-versus-optimal-decisions]].

**Radical coarsening.** Makes [[game-theory|equilibrium]] computable and the answer explainable. The cost is that the equilibrium concerns a coarsened game.

**Middle discretisation.** The only design that can value **a player who neither has the ball nor will receive it.**

## The Cost of the Middle

Eight directions at 45° means **a run's direction is rounded to the nearest octant**, with three unstated consequences: two runs 40° apart get the same label and the same counterfactual value; the label is derived from **realised velocity rather than intent**, relocating the [[intent-vs-outcome-valuation|intent/outcome]] problem from outcomes to actions; and **no dynamics constraints are encoded**, so a player at full sprint is treated as able to turn 135° at will.

Nakahara et al. add gates (24 km/h, 0.1 m/s) that **determine label assignment** rather than value magnitude. See [[free-parameters-load-bearing]].

## Size Is Not the Only Axis

| | Size | Off-ball | Action *choice* | Pass types | Per-player |
|---|---|---|---|---|---|
| Fernández et al. | Huge | No | No | n/a | Yes |
| Yeung & Fujii | Tiny | No | **Yes** | No | Yes |
| Scott et al. | **Largest** | n/a — one agent | **Yes** | **Yes** | **No** |
| Nakahara et al. | Middling | **Yes** | **Yes** | No | **Yes** |
| Fujii et al. | Middling | **Yes** | **Yes** | **Yes** | **Yes** |

**The largest action space belongs to the framework that values nothing.** Scott et al. have the richest vocabulary and no per-player output — because the constraint on valuation is the algorithm family and the state representation, not the action set. That is the clearest available demonstration that action-space size and analytic power are not the same axis.

## The Simulator Constraint

A forward-approach action space is bounded below by **what the environment can execute**, where an inverse one is bounded only by what the data labels. That explains why [[nfootball|NFootball]] distinguishes pass types (a simulator must decide a ball's trajectory) and drops sprint states (representing acceleration dynamics is work).

**The action space of a forward framework is a statement about the simulator as much as about football** — one more reason results across the forward/inverse divide are not directly comparable. See [[domain-adaptation]].

## Consequence for Cross-Framework Comparison

C-OBSO's implicit action space is *the continuous trajectory a player ran*, valued against a predicted trajectory; Nakahara's is 14 discrete labels. Two metrics of "off-ball contribution" over different action spaces need not agree, and at $\rho = 0.182$ do not. See [[construct-validity]].

## Beyond Sport

Any system valuing decisions from logs faces the same choice, usually silently: which clinical interventions count as distinct, which trades are one action, which UI events are decisions. **The discretisation determines what "a better decision" could have meant**, and it is almost never reported as a modelling assumption. The genealogy point generalises too — borrowed action vocabularies carry their origin environment's assumptions.

## See Also

- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[action-supervision]] · [[temporal-difference-learning]] · [[deep-q-network]] · [[proximal-policy-optimization]]
- [[game-theory]] · [[probability-surface]] · [[counterfactual-baseline]] · [[counterfactual-simulation]] · [[policy-modelling]] · [[domain-adaptation]] · [[agent-based-simulation]]
- [[action-valuation]] · [[off-ball-value]] · [[c-obso]] · [[spadl]] · [[intent-vs-outcome-valuation]] · [[construct-validity]] · [[representation-learning]]
- [[google-research-football]] · [[nfootball]] · [[data-stadium]] · [[gated-recurrent-unit]] · [[residual-connections]] · [[free-parameters-load-bearing]] · [[observed-versus-optimal-decisions]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] · [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] · [[expected-value-possession-framework|EPV]]
