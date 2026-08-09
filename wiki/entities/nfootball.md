---
title: "NFootball"
type: entity
tags: [organisation, benchmark-environment, simulator, ai-research, reinforcement-learning, multi-agent, action-space, sports-analytics, evaluation, domain-adaptation, agent-based-simulation, single-source]
sources: [raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md]
confidence: 0.7
provenance:
  extracted: 60%
  inferred: 25%
  generated: 14%
  imported: 1%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-07
updated: 2026-08-08
---

# NFootball

A football RL environment written by the [[nagoya-university|Nagoya]] group for [[adaptive-action-supervision-multi-agent-rl|Fujii et al. (2023)]], and released with their code. The vault's second simulator after [[google-research-football|GFootball]].

## Why It Exists

The authors state the reason directly: in GFootball, the **transition algorithms are difficult to customise** and some commands — passing is the example given — **did not work at their intended timings**.

Two complaints worth separating:

- **Customisability** is a research-tooling complaint. They needed to induce a controlled domain gap, which means editing the dynamics.
- **Pass timing** is a fidelity complaint, and the serious one. A simulator in which passes do not execute when intended cannot serve as a target for imitating football, because passing *is* the demonstrated behaviour.

## Specification

| | |
|---|---|
| Pitch | $x \in [-1, 1]$, $y \in [-0.42, 0.42]$ — **copied from GFootball** |
| Goal | $y \in [-0.044, 0.044]$ |
| Timestep | 0.1 s; episode limit 8.5 s |
| Actions | **12** — 8 directions at 45°, idle, high pass, short pass, shot — "partially based on GFootball" |
| Tasks held | 2v2, 4v8 |
| Implementation | Python throughout, "and then transparent" |

The action set is a **deletion from GFootball's 19**: sliding, dribble, stop-dribble, sprint, stop-sprint, stop-moving and long pass all go. See [[action-space-design]], where the genealogy across three papers is set out.

## The Group Validated GFootball, Then Replaced It

> **Added 2026-08-08 on acquisition of [[ai-football-reinforcement-learning|Scott et al. (2022)]].**

A year before NFootball was built, the same group published a paper whose stated contribution was **verifying simulation as a practical approach for football analysis** — using GFootball, at 11v11, with 200 million training timesteps, concluding that agent play style comes to resemble real footballers as competitiveness increases.

[[atom-scott]] is an author on both papers. **Neither acknowledges the tension.**

It is not a contradiction — the 2022 paper needed a fixed environment to analyse and the 2023 paper needed an editable one to perturb, which are genuinely different requirements. But it sharpens what the replacement cost:

| | [[google-research-football\|GFootball]] (Scott et al.) | NFootball (Fujii et al.) |
|---|---|---|
| Scale | **11v11** | 2v2, 4v8 |
| Shared | **Yes** — Kaggle competition, published baselines | **No** — one paper |
| Comparison possible against | Any other GFootball work | Nothing |
| Result obtained | **Partial transfer** demonstrated | Failure to reproduce |

**The environment that produced the vault's only positive transfer result is the one that got abandoned**, and the bespoke replacement operates at a quarter the scale and has no external reference point.

> ### `bespoke-environments-foreclose-comparison`
> **A research group that builds its own simulator to fix a shared one's shortcomings trades external comparability for internal control, and the trade is rarely acknowledged as a cost.**
> ^[generated: no source raises this. **Strengthened 2026-08-08** — Scott et al. demonstrate concretely what the shared environment made possible, which is the counterfactual the claim previously lacked. rests-on: source:fujii-nfootball-motivation, source:scott-gfootball-comparison-study, absence:no-held-source-benchmarks-across-frameworks]

This is the [[action-valuation-frameworks-compared|`no-cross-framework-benchmarking`]] problem in a new place. Where the vault has recorded that groups do not benchmark against competitors' *metrics*, here a group moved the *environment* out from under any possible comparison — for defensible reasons, and with the same effect.

The mitigation the authors do apply is **releasing the code**, which makes the environment reproducible even if it is not yet a reference point. That is more than most.

## What Its Failure Establishes

The most valuable thing in the vault about NFootball is that **it did not work well enough.**

Neither DQAAS nor the baselines reproduced demonstrated football behaviour inside it. The authors test whether the cause is algorithmic — decentralised against centralised MARL, classic against recent — conclude it is not, and attribute the failure to "the domain-specific modeling and reality of the simulator".

**A purpose-built simulator, built by domain experts specifically to fix the incumbent's problems, still could not support imitation of real football.** Stronger evidence for the fidelity gap than any argument from GFootball's shortcomings, because it removes the objection that a better-suited environment would have worked.

**And it does not contradict Scott et al.** The two papers compared different things: pass-network topology, deliberately chosen to be insensitive to physical difference, against movement trajectories, where physical difference is the whole content. See [[domain-adaptation]].

## Where It Sits on the Scale/Fidelity Trade-off

Per [[agent-based-simulation]], football simulators trade scale against physical fidelity and nothing occupies both corners. NFootball sits with the small-scale group by necessity — Fujii et al. state 11v11 is computationally out of reach — while inheriting GFootball's *abstraction level* rather than a biomechanical one like Humanoid Football.

That is an awkward position: **small enough to lose team-scale phenomena, abstract enough to lose physical realism.** The paper's failure to reproduce movement may partly reflect it.

## Caution

Everything here comes from one paper's methods section. Whether NFootball has been used since, by this group or others, is not established.

## See Also

- [[google-research-football]] · [[domain-adaptation]] · [[agent-based-simulation]] · [[action-space-design]] · [[imitation-reward-tradeoff]]
- [[multi-agent-reinforcement-learning]] · [[reinforcement-learning]] · [[deep-q-network]] · [[dynamic-time-warping]] · [[counterfactual-simulation]] · [[proximal-policy-optimization]]
- [[spadl]] · [[camera-calibration-benchmarking]] · [[soccernet-game-state-reconstruction]] · [[action-valuation-frameworks-compared]]
- [[keisuke-fujii]] · [[atom-scott]] · [[masaki-onishi]] · [[nagoya-university]] · [[aist]] · [[data-stadium]]
- [[adaptive-action-supervision-multi-agent-rl|Source Summary]] · [[ai-football-reinforcement-learning|Scott et al. Summary]]
