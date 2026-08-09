---
title: "Reinforcement Learning"
type: concept
tags: [reinforcement-learning, machine-learning, markov-model, dynamic-programming, temporal-difference, policy-gradient, multi-agent, action-space, simulator, domain-adaptation, agent-based-simulation, discounting, policy-modelling, imitation-learning, game-theory, action-valuation, experience-replay]
sources: [raw/papers/evaluating-football-player-actions.md, raw/papers/expected_value_possession_framework.md, raw/papers/training-lm-follow-instructions-with-human-feedback.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.9
provenance:
  extracted: 42%
  inferred: 53%
  generated: 4%
  imported: 0%
  ambiguous: 1%
lifecycle: reviewed
created: 2026-07-27
updated: 2026-08-08
---

# Reinforcement Learning

Learning to act by interacting with an environment and receiving reward. Formally, finding a policy $\pi(a|s)$ maximising expected cumulative discounted reward in a [[markov-game|Markov decision process]].

This page exists because RL vocabulary is used throughout the vault by work that is **mostly not doing reinforcement learning.**

> **Correction history.** This page's simulator claim has been revised **four times**. The full sequence, and what prompted each revision, is on [[domain-adaptation]]. The short version: v2 — "no evidence of transfer exists" — **was wrong**, and had propagated to four pages before [[ai-football-reinforcement-learning|Scott et al.]] was acquired.

## The Core Objects

| Object | Definition | Role |
|---|---|---|
| State value $V(s)$ | Expected return from $s$ under $\pi$ | "How good is this situation?" |
| Action value $Q(s,a)$ | Expected return from taking $a$ in $s$ | "How good is this move?" |
| **Advantage** $A(s,a) = Q(s,a) - V(s)$ | How much better than average | "Was that a good decision?" |
| Discount factor $\gamma$ | Geometric weight on future reward | Convergence and impatience |

Three solution families, all now represented: [[value-iteration|dynamic programming]] with known dynamics, [[temporal-difference-learning|temporal difference]] and [[deep-q-network|DQN]] without, and [[proximal-policy-optimization|policy gradient]] optimising $\pi$ directly.

## What Sports Analytics Borrows — and What It Does Not

Every [[action-valuation]] framework uses $V(a_i) = Q(S_i) - Q(S_{i-1})$, structurally an **advantage**; [[expected-possession-value|EPV]] is structurally a state-value function. The vocabulary is exact.

But **almost** none of this work is reinforcement learning, because nobody is learning a policy by interaction.

| | Reinforcement learning | Sports valuation |
|---|---|---|
| Goal | Find a better policy | **Measure** the observed one |
| Environment | Interacted with | Only observed |
| Reward | Designed to be optimised | A label to be predicted |

The reasons: **no usable simulator** (revised below), **sparse reward** (goals ~0.2% of events, see [[rare-event-proxy-targets]]), and **extrapolation risk** in evaluating actions nobody took.

[[expected-value-possession-framework|Fernández, Bornn & Cervone]] state the alternative explicitly: value under the **average** policy learned from history.

## Three Frameworks That Actually Do RL

All three are [[keisuke-fujii|Fujii-group]] or Fujii-co-authored, and they divide cleanly.

| | [[ai-football-reinforcement-learning\|Scott et al. (2022)]] | [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al. (2023)]] | [[adaptive-action-supervision-multi-agent-rl\|Fujii et al. (2023)]] |
|---|---|---|---|
| Direction | **Forward** | **Inverse** | **Forward** |
| Environment | [[google-research-football\|GFootball]] | None — never acts | [[nfootball\|NFootball]] |
| Algorithm | **[[proximal-policy-optimization\|PPO]]** (policy gradient) | SARSA (on-policy TD) | DDQN (off-policy, full stack) |
| Agents | **One**, central control | **Ten**, independent | Two to four, decentralised |
| Deliverable | A feasibility finding | **A valuation metric** | A method |
| Outcome | **Partial transfer** on pass structure | A metric that disagrees with C-OBSO | **Failed to reproduce movement** |

> ### `policy-gradient-forecloses-action-valuation`
> **Policy-gradient methods optimise the policy without materialising per-action values, so an RL framework built on them cannot be repurposed into an action-valuation metric.**
> ^[generated: declared on [[proximal-policy-optimization]]. rests-on: source:scott-ppo-setup, source:nakahara-q-per-action]

That is not incidental. Scott et al. produce **no player metric at all**, analysing [[social-network-analysis|pass-network structure]] instead; the two later value-based papers both produce or attempt one. **The turn from PPO to SARSA/DDQN across these three papers is also the turn from describing agents to valuing actions.**

Nakahara et al. clear the three objections in three ways — sidestepping the simulator by learning offline, deferring sparse reward via terminal [[expected-possession-value|EPV]], and regularising counterfactual extrapolation away with [[action-supervision]], which converts it into a hyperparameter rather than eliminating it.

## The Optimality Gap Is Tunable

> ### `optimality-gap-is-tunable`
> **In any framework that regularises a value function toward observed behaviour, the measured gap between observed and optimal action is partly a function of the regularisation weight, not of the players.**
> ^[generated: declared on [[action-supervision]]. rests-on: source:nakahara-lambda-tradeoff, source:fujii-reward-dtw-tradeoff]

Fujii et al. add that the position on that frontier **also moves with training time**. See [[imitation-reward-tradeoff]] and [[observed-versus-optimal-decisions]].

## Simulated Agents Shoot More; Real Players Already Shoot Too Much

> **Added 2026-08-08, and it is a genuine tension between two Fujii-co-authored papers.**

Scott et al. find that **more competitive agents shoot more** (total shots $r = 0.77$ with TrueSkill) **and pass less** ($r = -0.50$, $p = 0.061$). [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] find that real shooters **shoot too much** — passing is worth 0.2456 against shooting's 0.0866 against a blocking defender.

Three readings, and no held source distinguishes them:

1. **GFootball's shot model is too generous**, so shooting is genuinely optimal in the simulator and not in football — a fidelity finding, consistent with [[domain-adaptation]].
2. **Both are right in their domains**, and the divergence measures the domain gap on a dimension nobody chose to measure.
3. **Neither axis means what it says** — Scott et al.'s TrueSkill ordering carries an unresolved anomaly (agents trained against *easy* bots rank top three), and Yeung & Fujii's equilibrium assumes a best-responding defender.

Reading 1 is the most useful if true, because it is checkable: compare simulated and real shot conversion by location. **Nobody has, and this is the sharpest cheap test the vault has identified for GFootball's fidelity.**

## Where Optimal Policy *Is* Recovered

**Correction, 2026-07-27.** An earlier revision stated flatly that this literature does not solve for optimal policy. Too strong.

[[optimal-decisions-shot-taking-situations|Yeung & Fujii (2024)]] do — with [[game-theory]], by shrinking the strategy space to four enumerable profiles and estimating unobserved payoffs by re-running the models with the closest defender removed.

**The barrier to optimal-policy analysis is the size of the action space, not the observational nature of the data.** Sharpened since: size is not the only axis — granularity and coverage vary independently, and the vault now holds four action spaces spanning surface, 4 profiles, 12, 14 and 19 actions. See [[action-space-design]].

## Game Theory as the Alternative Route

| | Reinforcement learning | [[game-theory\|Game theory]] |
|---|---|---|
| Other agents | Folded into the environment | **Modelled as agents** |
| Solution concept | Optimal policy against a fixed world | **Equilibrium** against a reasoning opponent |
| Explains *why* | Poorly | By construction |

**The irony holds across four papers.** Scott et al. use one agent with ten scripted teammates; Nakahara et al.'s ten agents do not model each other; Fujii et al.'s decentralised agents do not either, and a *centralised* alternative (CDS) changed nothing. The vault's RL football papers model interaction less than its two-agent game-theory paper does.

## The Simulator Objection, Four Revisions

**v1** asserted the forward approach is closed off for lack of a faithful simulator. **v2** weakened this to "available, but no evidence of transfer". **v3** confirmed via Fujii et al. that the bottleneck is fidelity rather than algorithm — they build a bespoke simulator, fail to reproduce demonstrated movement, and rule out centralised/decentralised and classic/recent deep RL as causes.

**v4, on acquisition of Scott et al.:** transfer evidence exists. Fifteen PPO agents in GFootball, compared against three J-League teams on [[social-network-analysis|pass-network]] metrics, show **convergence on three of six** as agents improve.

The qualification is what makes it usable. Scott et al. chose SNA because it is *"not influenced by physical differences between simulations and the real-world"* — so the transfer was measured on the dimension **selected for insensitivity to the gap.**

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

Two papers, one shared author, neither drawing the comparison. See [[domain-adaptation]] and [[agent-based-simulation]].

## Discounting, Borrowed and Repurposed

$\gamma$ encodes impatience and guarantees convergence; [[temporal-discounting|Shelopugin]] uses the identical formula for **attribution decay**. Nakahara et al. set $\gamma = 1$, safe under terminal-only reward but spreading credit flat across thirty seconds; Fujii et al. never report the value used. See [[free-parameters-load-bearing]].

## RL Proper in This Vault

- **[[ai-football-reinforcement-learning|Scott, Fujii & Onishi (2022)]]** — PPO in GFootball; TrueSkill ranking; SNA comparison against real football. **Forward**, and the vault's only positive transfer result.
- **[[action-valuation-multi-agent-reinforcement-learning|Nakahara et al. (2023)]]** — ten SARSA agents on J-League tracking. **Inverse.**
- **[[adaptive-action-supervision-multi-agent-rl|Fujii et al. (2023)]]** — DQAAS in NFootball. **Forward**, and the clearest negative result on simulator fidelity.
- **[[rlhf|RLHF]]** ([[training-lm-follow-instructions-with-human-feedback|InstructGPT]]) — PPO against a learned reward model, with a [[kl-divergence|KL]] penalty. The best-behaved instance of the [[imitation-reward-tradeoff|imitation/reward trade-off]] here, because the coefficient is reported.
- **MDP-based decision optimisation** — Rahimian et al., Van Roy et al. Cited, not held.
- **Deep RL for team-level valuation** — Liu & Schulte (2018), Liu et al. (2020), Routley & Schulte (2015). The **team-as-one-agent** tradition. Cited, not held.
- **Inverse RL for sports** — Luo, Schulte & Poupart (2020); Rahimian & Toka (2022). Cited, not held.

## See Also

- [[markov-game]] · [[game-theory]] · [[value-iteration]] · [[temporal-difference-learning]] · [[deep-q-network]] · [[proximal-policy-optimization]] · [[temporal-discounting]] · [[policy-modelling]]
- [[multi-agent-reinforcement-learning]] · [[action-supervision]] · [[action-space-design]] · [[domain-adaptation]] · [[dynamic-time-warping]] · [[imitation-reward-tradeoff]] · [[agent-based-simulation]] · [[social-network-analysis]]
- [[google-research-football]] · [[nfootball]] · [[imitation-learning]] · [[counterfactual-baseline]] · [[counterfactual-simulation]] · [[action-valuation]]
- [[rlhf]] · [[rare-event-proxy-targets]] · [[xsot]] · [[off-ball-value]] · [[expected-possession-value]] · [[trueskill]]
- [[free-parameters-load-bearing]] · [[observed-versus-optimal-decisions]] · [[action-valuation-frameworks-compared]]
- [[training-lm-follow-instructions-with-human-feedback|InstructGPT]] · [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] · [[ai-football-reinforcement-learning|Scott et al.]]
