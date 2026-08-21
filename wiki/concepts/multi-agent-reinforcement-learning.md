---
title: "Multi-Agent Reinforcement Learning"
type: concept
tags: [multi-agent, reinforcement-learning, simulator, domain-adaptation, agent-based-simulation, game-theory, markov-model, sports-analytics, action-valuation, off-ball, player-evaluation, trajectory-prediction, deep-learning, animal-behaviour]
sources: [raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 64%
  inferred: 28%
  generated: 7%
  imported: 1%
  ambiguous: 0%
lifecycle: reviewed
created: 2026-08-07
updated: 2026-08-08
---

# Multi-Agent Reinforcement Learning

[[reinforcement-learning|RL]] where several decision-makers act in a shared environment, each with its own policy and value function — as opposed to folding the others into the environment dynamics.

## Three Positions, Not Two

The vault now holds the full progression, and the middle position had been missing.

| | **Team as one agent** | **Single agent, central control** | **Per-player agents** |
|---|---|---|---|
| Example | Liu & Schulte (2018), Liu et al. (2020) — *cited* | **[[ai-football-reinforcement-learning\|Scott et al. (2022)]]** | [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]], [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] |
| Controls | The team's event stream | **One *active* player, switchable** | Every attacker |
| Others are | Absorbed into the team abstraction | **A rule-based in-game system** | Independent learners |
| Can value a player | No | **No** | **Yes** |

**Scott et al. is the immediate predecessor both MARL papers define themselves against**, and it states the limitation itself: *"we consider multi-agent RL to be out of scope and hope to pursue such a setup in the future."* Nakahara et al. and Fujii et al. are that future work.

The central-control position is worth naming because its cost is different from the team-as-one-agent cost. **Scott et al. can act at every timestep — but only one player at a time, and the other ten are scripted.** So the team behaviour it measures is a blend of learned and rule-based, which is a material confound on its pass-network results. See [[social-network-analysis]].

## What the Team-as-One-Agent Default Cost

Prior RL work in team sports treats **the team as a single agent**. Three consequences follow, all limitations rather than choices:

1. **Only the ball-holder can be valued**, since only he generates an event to serve as the action.
2. **Value exists only at discrete events**, so the ~87 of 90 minutes off the ball is invisible. See [[off-ball-value]].
3. **No per-player attribution** without a further assumption.

Nakahara et al.'s ten independent agents lift all three at once — the cleanest demonstration in the vault that **an analytic gap that looked like a data problem was a modelling-granularity problem.** The tracking data was there; the agent decomposition was not.

## "Independent" Is Doing a Lot of Work

Both MARL sources use *independent* learners. Nakahara et al. "omit the agent index"; Fujii et al. "basically considered decentralized multi-agent models, which do not communicate with each other".

| | Independent MARL | Interactive MARL |
|---|---|---|
| Each agent models | Others as **part of the environment** | Others as **agents with policies** |
| Non-stationarity | Unhandled | Addressed |
| Solution concept | Separate value functions | Equilibrium, or a joint policy |

Teammates *appear* in the state vector; no agent reasons about what a teammate will do. **The "multi-agent" claim is about the number of value functions, not about interaction.**

The cost for counterfactual valuation is concrete: "what if this player had run left" is answered **holding every other player's trajectory fixed**. Compare [[counterfactual-simulation]], where regeneration lets the world respond and pays in compounding error.

### The centralised alternative was tested, and changed nothing

Fujii et al. run **CDS** (Li et al., 2021), a centralised MARL method, on the football 4v8 task and report results "very similar to those in DQN-based RL models":

> We confirmed that the cause of the reproducibility issue may not be the centralized/decentralized or classic/recent deep RL.

So the natural objection — that these papers fail because their agents do not model each other — **has been tested and does not survive.** The binding constraint is simulator fidelity. See [[domain-adaptation]].

That does not make independence harmless for *valuation*: a frozen-world counterfactual is still a frozen-world counterfactual. It means independence is not what stopped the forward approach from working.

## The Game-Theoretic Alternative

| | Independent MARL | Game theory ([[optimal-decisions-shot-taking-situations\|Yeung & Fujii]]) |
|---|---|---|
| Agents | 10, or 2–4 | 1 shooter, 1 defender |
| Opponent | In the state vector | **Best-responding** |
| Solution | Separate $Q$-functions | **Nash equilibrium** |
| Scales to a team | Yes | No |
| Models interaction | **No** | Yes |

**The irony now spans four papers.** Scott et al. control one agent among ten scripted teammates; both MARL papers use independent learners; a centralised alternative changed nothing. The vault's RL football papers model interaction *less* than its two-agent game-theory paper does, and none cites another.

## Forward and Inverse

- **Inverse** — estimate policies, rewards or values *from observed data*. Nearly everything here.
- **Forward** — build an environment and *generate* behaviour inside it.

Fujii et al. give this precise shape: on the inverse side the transition model $\mathcal{T}^E$ is **not modelled at all**, since the next state is read from the data; on the forward side a transition model must be assumed. The gap is what [[domain-adaptation|Real-to-Sim adaptation]] exists to bridge.

The vault holds **two forward papers and one inverse**, and the forward ones bracket the question:

| | [[ai-football-reinforcement-learning\|Scott et al.]] | [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] |
|---|---|---|---|
| Direction | **Forward** | **Inverse** | **Forward** |
| Environment | [[google-research-football\|GFootball]] | None | [[nfootball\|NFootball]] |
| Compared on | **Pass-network topology** | — | **Movement trajectories** |
| Outcome | **Partial transfer** | A metric | **Failed to reproduce** |

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

## Where the Gains Actually Came From

| Ingredient | Enables |
|---|---|
| **Per-player agents** | Off-ball valuation at all |
| **Continuous state, discrete actions** | Valuation between events |
| **[[action-supervision]]** | Counterfactual actions having meaningful values |
| Independence assumption | Tractability — and costs interaction |

Only the first is properly "multi-agent". The second and third are orthogonal and could be applied to a single-agent formulation — which is roughly what Scott et al. is.

## The Wider Framing

Fujii et al.'s stated subject is **biological multi-agents**, with football placed beside a predator-prey chase task — so the football results there are a demonstration of a general method rather than a claim about football. See [[naoya-takeishi]] and [[kazushi-tsutsui]].

## See Also

- [[reinforcement-learning]] · [[action-supervision]] · [[temporal-difference-learning]] · [[deep-q-network]] · [[proximal-policy-optimization]] · [[action-space-design]]
- [[domain-adaptation]] · [[agent-based-simulation]] · [[social-network-analysis]] · [[dynamic-time-warping]] · [[imitation-reward-tradeoff]] · [[nfootball]] · [[google-research-football]]
- [[game-theory]] · [[markov-game]] · [[policy-modelling]] · [[imitation-learning]] · [[trajectory-prediction]]
- [[off-ball-value]] · [[action-valuation]] · [[counterfactual-simulation]] · [[counterfactual-baseline]]
- [[hiroshi-nakahara]] · [[keisuke-fujii]] · [[atom-scott]] · [[calvin-yeung]] · [[masaki-onishi]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] · [[optimal-decisions-shot-taking-situations|Yeung & Fujii]]
