---
title: "Domain Adaptation (Sim-to-Real and Real-to-Sim)"
type: concept
tags: [domain-adaptation, transfer-learning, reinforcement-learning, simulator, agent-based-simulation, multi-agent, imitation-learning, machine-learning, sports-analytics, evaluation, network-analysis, animal-behaviour]
sources: [raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 62%
  inferred: 28%
  generated: 9%
  imported: 1%
  ambiguous: 0%
lifecycle: reviewed
created: 2026-08-07
updated: 2026-08-08
---

# Domain Adaptation (Sim-to-Real and Real-to-Sim)

Transferring a model across a shift between the environment it learned in (**source**) and the environment it must work in (**target**). [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] supply the vocabulary; [[ai-football-reinforcement-learning|Scott et al.]] supply the measurement.

## The Two Directions Are Not Symmetric

| | **Sim-to-Real** | **Real-to-Sim** |
|---|---|---|
| Source | A simulator | **Real-world data** |
| Target | A physical system | A simulator |
| Source dynamics | **Known** — someone wrote them | **Unknown** — that is the whole problem |
| Typical domain | Robotics | Biological multi-agents; team sports |
| Correctable by | Randomising or tuning source parameters | **Not correctable that way** |
| Literature | Large and mature | Sparse |

Sim-to-Real is well studied because the asymmetry favours it: you control the source, so you can perturb it, randomise it, or measure the discrepancy. Real-to-Sim inverts that. **You cannot adjust the source dynamics to close the gap, because you do not know what they are** — the transition function of 22 interacting footballers is not written down anywhere.

On the real-world side the transition model $\mathcal{T}^E(s'^E|s^E, \vec{a}^E)$ is *not modelled at all*, because the next state can be read from the data. On the simulator side a transition model $\mathcal{T}$ must be assumed. The gap between them is unmeasurable in principle.

## The Claim, Four Revisions Later

> **This page exists partly to hold the history of a claim the vault has had to revise repeatedly. The sequence is worth keeping, because each revision was prompted by a different kind of evidence.**

| | Claim | Basis |
|---|---|---|
| **v1** | No simulator is faithful enough, so the forward approach is closed off in football | Inference, no held RL source |
| **v2** | The forward approach is *available*; what is unavailable is **evidence of transfer** | Inference from [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] borrowing GFootball's action vocabulary and discarding its dynamics |
| **v3** | The bottleneck is **simulator fidelity, not algorithm choice** | [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] fail to reproduce demonstrated movement and rule out the algorithm |
| **v4** | ⚠️ **Evidence of transfer exists, on pass-network topology, and the dimension was chosen for its insensitivity to the gap** | [[ai-football-reinforcement-learning\|Scott et al.]], now held |

**v2 was wrong as stated and had propagated to four pages.** Transfer evidence existed, in a paper the vault knew about and had flagged, since before any of the RL sources were ingested.

## What Scott et al. Actually Measured

Fifteen PPO agents of varying competitiveness in [[google-research-football|GFootball]], compared against three J-League teams on [[social-network-analysis|pass-network]] metrics.

| Metric | Converges toward real football as agents improve? |
|---|---|
| Betweenness (mean) | **Yes** |
| Betweenness (std) | **Yes** |
| Closeness (std) | **Yes** |
| Closeness (mean) | No |
| PageRank (std) | No |
| PageRank (min) | No |

Three of six. The betweenness result is the interpretable one — high betweenness deviation indicates balanced passing with less single-player dependence, and agents develop it without instruction.

## The Qualification That Makes It Interpretable

The paper's justification for choosing SNA is the crux:

> an analysis framework that is not influenced by physical differences between simulations and the real-world is necessary

Passes, they argue, do not depend on individual physical ability.

> ### `transfer-evidence-is-conditional-on-the-dimension-chosen`
> **Measuring domain transfer requires choosing a comparison dimension, and choosing one insensitive to the domain gap is both the methodologically defensible move and the one that limits what the result can establish. A transfer finding is a finding about the chosen dimension, not about the domains.**
> ^[generated: no source states this; drawn from Scott et al.'s explicit justification read against Fujii et al.'s failure. rests-on: source:scott-sna-justification, source:fujii-football-reproducibility-failure]

That resolves what looks like a contradiction between the two held forward-approach papers into a boundary:

| | [[ai-football-reinforcement-learning\|Scott et al. (2022)]] | [[adaptive-action-supervision-multi-agent-rl\|Fujii et al. (2023)]] |
|---|---|---|
| Compared on | **Pass-network topology** | **Movement trajectories** ([[dynamic-time-warping\|DTW]]) |
| Physical dynamics | **Deliberately factored out** | **Central to the comparison** |
| Result | **Partial convergence** | **Failure to reproduce** |

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

Consistent, not contradictory — and considerably more useful than either paper alone supports. The two share an author ([[atom-scott]]) and neither draws the comparison.

**A caveat on the positive half.** In Scott et al. only one player per team is agent-controlled; the other ten run a rule-based in-game system. The converging pass network is therefore partly scripted, and how much of the convergence is learned is not separable from the reported results.

## Two Conditions That Make Learning From Demonstration the Better Option

Fujii et al. state these plainly and they generalise well beyond sport:

1. **Transition functions are difficult to design explicitly.**
2. **Expert demonstrations are available.**

Where both hold, learning from demonstration beats building an environment and doing pure RL inside it. Football satisfies both emphatically — which is why the field's overwhelming preference for the **inverse** approach is a rational response to its conditions rather than a failure of ambition. See [[multi-agent-reinforcement-learning]].

## The Mechanism: Adapting the Supervision, Not the Environment

Since source dynamics cannot be corrected, Fujii et al. correct the *supervision signal*.

Under a domain gap, an agent rolling out in the simulator drifts **out of phase** with the demonstration — by timestep $t$ it is somewhere the expert reached at some other timestep. Supervising against $a_t^E$ supervises against the right action at the wrong moment.

The fix is [[dynamic-time-warping|DTW]]: supervise against the expert action at the **most similar state**, $t' = \arg\min_j W(s,s^E)_{t,j}$. See [[action-supervision]].

**Wherever a model is supervised against a reference trajectory that may run at a different rate, alignment should precede supervision** — and assuming index correspondence is an unstated assumption of matched dynamics.

## What the Gap Costs, Measured

Fujii et al. induce a *controlled* gap in their chase-and-escape task: predator mobility 120% of prey in the source, **110%** in the target. A 10-point shift, nothing else changed, and enough to make the task materially harder.

**A domain gap does not need to be exotic to matter.** If a 10% velocity difference degrades transfer in a two-predator toy problem, the gap between real football and any simulator is not a detail to be tuned away.

DTW helps on chase-and-escape and **not at all** on football. One reading: DTW corrects *phase* mismatch, and the football gap is a difference in what is physically possible. Alignment cannot fix a target environment in which the demonstrated behaviour is unavailable.

## Relation to Neighbouring Ideas

| | Adapts | Assumes |
|---|---|---|
| **Domain adaptation** | A model across an environment shift | Task is the same, distribution differs |
| Transfer learning | A representation across tasks | Some shared structure |
| [[imitation-learning]] | Nothing — mimics within one domain | Source and target coincide |
| [[agent-based-simulation]] | Nothing — the simulator *is* the object | That emergence is informative |
| [[counterfactual-simulation]] | An entity within a fixed model | The model captures the right dependencies |

The distinction from imitation learning is the one that matters. **RoboCup imitation work has source and target environments that are basically the same**, which Fujii et al. note explicitly — why robot-soccer imitation results do not transfer to this problem.

## Why This Generalises

Real-to-Sim is the shape of any attempt to build a simulator *from* observational data rather than from first principles. In all such cases the source dynamics are unknown, the target dynamics are assumed, and **the discrepancy cannot be measured because measuring it would require the thing you lack.**

`transfer-evidence-is-conditional-on-the-dimension-chosen` is the portable warning: every such field validates its simulator on *some* dimension, and the dimension is usually chosen because it is measurable across both — which is close to choosing it because the gap does not affect it.

## See Also

- [[dynamic-time-warping]] · [[action-supervision]] · [[imitation-reward-tradeoff]] · [[deep-q-network]] · [[proximal-policy-optimization]]
- [[agent-based-simulation]] · [[social-network-analysis]] · [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[imitation-learning]] · [[action-space-design]]
- [[nfootball]] · [[google-research-football]] · [[counterfactual-simulation]] · [[trajectory-prediction]] · [[construct-validity]] · [[representation-learning]]
- [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]] · [[aist]]
- [[ai-football-reinforcement-learning|Scott et al. Summary]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al. Summary]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al. Summary]]
