---
title: "Agent-Based Simulation"
type: concept
tags: [agent-based-simulation, simulator, reinforcement-learning, multi-agent, domain-adaptation, counterfactual, sports-analytics, evaluation, animal-behaviour, tactical-analysis]
sources: [raw/papers/ai_football_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md]
confidence: 0.75
provenance:
  extracted: 52%
  inferred: 32%
  generated: 14%
  imported: 2%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-08
updated: 2026-08-08
---

# Agent-Based Simulation

Simulating a system by giving individual agents rules or policies and letting collective behaviour **emerge**, rather than modelling the aggregate directly.

## The Motivation, Stated Plainly

[[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] give the clearest statement of why anyone bothers, and it is an argument about **experimental control**, not about compute:

> it will remain nearly impossible to collect real-world sport data in a scientific manner where variables can be controlled … top level sports are highly competitive in nature and leave very little room for experimentation

That is the whole case. Football generates enormous observational data and permits **no experiments** — nobody will run a season with a randomised formation to see what happens. Simulation is the only route to a controlled manipulation.

This is worth separating from the reasons the vault usually gives for simulation. [[counterfactual-simulation]] simulates to answer *what if this player joined*; [[domain-adaptation]] concerns the gap between simulated and real. **ABS is the prior question — why simulate at all — and the answer is that the alternative is not a worse experiment but no experiment.**

> ### `observational-abundance-does-not-substitute-for-control`
> **A field can have exponentially growing data and remain unable to answer causal questions, because volume addresses estimation error and not confounding. Football is a clean instance: more tracking data does not make formation choice randomised.**
> ^[generated: no source states this in general form; drawn from the paper's motivating argument. rests-on: source:scott-abs-motivation]

That claim connects to the [[selection-bias]] material running through this vault, and to [[counterfactual-simulation]]'s causal caveats: a generative model trained on observational data learns the observational distribution, and intervening on it gives the right causal answer only under assumptions nobody checks. **ABS's promise is that the intervention is real, inside the simulator.** Its problem is that the simulator may not be football.

## The Environments

Scott et al. tabulate the field, and the spread is instructive:

| Environment | Scale | Physics |
|---|---|---|
| **RoboCup Soccer** (1995) | 11v11 | Noisy virtual sensors; basic dash/turn/kick |
| **MuJoCo 2v2** (Liu et al., 2019) | 2v2 | Simulated physics, simple bodies, 3-D action space |
| **Unity 2v2** (Cao & Lin, 2020) | 2v2 | Two player types with differing action spaces |
| **[[google-research-football\|Google Research]]** (2019) | **11v11** | Full match: goals, fouls, corners, tiredness, misses |
| **Humanoid** (Liu et al., 2021) | 2v2 | Biomechanical motor control; ball and goal scaled to humanoid height |
| **[[nfootball\|NFootball]]** (2023) | 2v2, 4v8 | Bespoke; built because GFootball resisted customisation |

**There is a hard trade-off along this list between scale and physical fidelity**, and nothing occupies both corners. GFootball is the only 11v11 option and abstracts away biomechanics; Humanoid Football models motor control and manages 2v2.

That trade-off explains a good deal of what the vault has recorded elsewhere. [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] work at 2v2 and 4v8 and state that 11v11 is computationally out of reach; Scott et al. work at 11v11 and analyse only pass-network topology, which is precisely the aspect that survives physical abstraction. **Each paper chose a corner and then chose an analysis the corner could support.**

## Emergence Is the Point and the Problem

ABS is used to observe behaviour that was not programmed. Scott et al.'s finding is exactly this shape: agents were rewarded only for goals (plus a shaped checkpoint term) and nonetheless developed a **balanced passing distribution** resembling real teams, measured by betweenness deviation. Nothing instructed them to distribute.

The interpretive difficulty is symmetrical. When emergent behaviour resembles reality, it is evidence the simulation captures something. When it does not, it may indict the simulator, the algorithm, the reward, or the training budget — and Scott et al.'s own results supply an example of the ambiguity in their **TrueSkill anomaly**, where agents trained against easy bots outrank agents trained far longer against hard ones. The authors flag it as counter-intuitive and defer it.

⚠️ **A confound specific to this setup:** only one player per team is agent-controlled and the other ten run a rule-based in-game system. So the emergent pass network is partly *scripted*, and the paper cannot separate learned distribution from GFootball's built-in AI. See [[social-network-analysis]].

## What ABS Has and Has Not Delivered Here

| Claim | Status |
|---|---|
| Simulators exist at full scale | **Yes** — GFootball, RoboCup |
| Agents can be trained to competitive play | **Yes** — Scott et al., 200M timesteps |
| Agents develop humanlike **passing structure** | **Partially** — 3 of 6 SNA metrics converge |
| Agents reproduce demonstrated **movement** | **No** — [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]], attributed to simulator fidelity |
| ABS has produced a football insight not otherwise obtainable | **Not yet in this vault** |

The last row is the honest summary. Both held ABS papers are about **whether the method works**, not about football. Scott et al. explicitly frame their contribution as verifying simulations as a practical approach for football analysis — a methodological claim. The controlled experiments that motivated the whole enterprise (what does this formation do? what happens under this press?) remain unrun.

## The Validation Trap

**The technique is adopted where experiments are impossible, which is exactly where the simulator cannot be validated against experiment.** The validation problem is not incidental to ABS; it is the same condition that made ABS necessary.

The partial answer both held papers reach for is the same: compare simulated behaviour against real observational data on some dimension, and argue about whether the dimension is the right one. See [[domain-adaptation]] and [[construct-validity]].

## See Also

- [[domain-adaptation]] · [[google-research-football]] · [[nfootball]] · [[social-network-analysis]] · [[proximal-policy-optimization]]
- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[counterfactual-simulation]] · [[imitation-learning]] · [[trajectory-prediction]]
- [[selection-bias]] · [[construct-validity]] · [[tactical-analysis]] · [[capability-profiling]] · [[predictive-validity]]
- [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]] · [[aist]]
- [[ai-football-reinforcement-learning|Scott et al. Summary]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al. Summary]]
