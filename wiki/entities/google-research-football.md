---
title: "Google Research Football (GFootball)"
type: entity
tags: [organisation, benchmark-environment, simulator, google, ai-research, reinforcement-learning, multi-agent, action-space, sports-analytics, evaluation, domain-adaptation, agent-based-simulation, policy-gradient]
sources: [raw/papers/ai_football_reinforcement_learning.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md]
confidence: 0.85
provenance:
  extracted: 66%
  inferred: 18%
  generated: 7%
  imported: 9%
  ambiguous: 0%
lifecycle: reviewed
created: 2026-08-07
updated: 2026-08-08
---

# Google Research Football (GFootball)

A physics-based football simulation environment for [[reinforcement-learning]] research, released by [[google-brain|Google Brain]] (Kurach et al., AAAI 2020). It reproduces a full 11v11 match with goals, fouls, corners, player tiredness and misses.

**Held through three sources that use it, react to it, and ultimately replace it.** The environment paper itself is still not held, but [[ai-football-reinforcement-learning|Scott et al.]] use it directly and at length, so this page is no longer purely second-hand.

## Three Uses, Progressively Less Flattering

| Source | What it takes | What it does with it |
|---|---|---|
| [[ai-football-reinforcement-learning\|Scott, Fujii & Onishi (2022)]] | **The whole environment** | Trains 15 [[proximal-policy-optimization\|PPO]] agents; compares their play against real J-League teams |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al. (2023)]] | **19 actions → 14** | Applies the vocabulary to real tracking; discards the dynamics |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al. (2023)]] | Pitch dimensions; action set "partially based on" it | **Rejects it and builds [[nfootball\|NFootball]]** |

**The same author appears on the first and third.** [[atom-scott]] validates GFootball as a practical approach for football analysis in 2022 and appears a year later on the paper that replaces it. Neither acknowledges the tension.

## Specification, Now First-Hand

| | |
|---|---|
| Scale | **11v11**, full match |
| Actions | **19** — 8 directions, shot, short/high/long pass, idle, sliding, dribble, stop-dribble, sprint, stop-moving, stop-sprint |
| Action stickiness | Movement actions **persist** until an explicit stop action |
| Observation | **Super Mini Map** — four $72\times96$ binary matrices (home, away, ball, active player), stacked over four frames |
| Control | One **active player**, switchable; non-active players on a rule-based system |
| Reward | Goal scored / conceded, plus a small **checkpoint reward** for sparse early goals |

> **The 19-action figure is confirmed.** The vault had recorded it from Nakahara et al.'s second-hand mention; Scott et al. enumerate the set directly. See [[action-space-design]].

Two details the vault did not have and that matter:

**Sticky actions.** Movement persists until stopped, which is why "stop-moving", "stop-sprint" and "stop-dribble" exist as separate actions. That explains a design choice in [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] that looked arbitrary — their sprint start / sprint stop / direction release triple is inherited from an environment where actions latch, applied to tracking data where nothing latches.

**Central control with a scripted supporting cast.** Only one player is agent-controlled at a time. **The other ten run GFootball's built-in rule-based AI**, so any team-level behaviour measured in this environment is a blend of learned and scripted. That is a material caveat on Scott et al.'s pass-network results and is recorded on [[social-network-analysis]].

## Why It Was Rejected

Fujii et al.'s stated reasons are specific:

- **Transition algorithms are difficult to customise** — a tooling complaint. They needed a *controlled* domain gap, which requires editing the dynamics.
- **Some commands did not work at intended timings**, passing named. A **fidelity** complaint, and the serious one: a simulator in which passes do not execute when intended cannot serve as a target for imitating football.

## What This Environment Does and Does Not Support

> **Revised 2026-08-08** on acquisition of Scott et al., which is the only held source to actually train agents in it and compare them to reality.

| Question | Answer |
|---|---|
| Can competitive agents be trained here? | **Yes** — 200M timesteps, +2.81 goal difference against the hard bot |
| Does agent **passing structure** come to resemble real football? | **Partially** — 3 of 6 [[social-network-analysis\|SNA]] metrics converge |
| Does agent **movement** reproduce real football? | **Not established here**; [[nfootball\|NFootball]] says no |
| Is it a stable reference point for comparison? | **It was.** See below |

The competitiveness result carries an anomaly Scott et al. flag and do not resolve: **agents trained against the *easy* bot rank 1, 2 and 3 on [[trueskill|TrueSkill]]**, above agents trained four times longer against the hard bot. Every correlation in that paper is measured against this axis.

## The Value Was Never the Physics

> **Sharpened 2026-08-08.**

GFootball's contribution to this literature is that **everyone used it**. Scott et al. could ask whether simulated football resembles real football *because* GFootball was a fixed, shared, well-documented reference point — a Kaggle competition with over a thousand teams, an established baseline, published action semantics.

That is what [[nfootball|its bespoke replacement]] gives up:

> ### `bespoke-environments-foreclose-comparison`
> **A research group that builds its own simulator to fix a shared one's shortcomings trades external comparability for internal control, and the trade is rarely acknowledged as a cost.**
> ^[generated: declared on [[nfootball]]. **Strengthened 2026-08-08** — Scott et al. demonstrate what the shared environment made possible, which is the counterfactual the claim needed. rests-on: source:fujii-nfootball-motivation, source:scott-gfootball-comparison-study, absence:no-held-source-benchmarks-across-frameworks]

## Comparison to Other Shared Resources Here

| Resource | Supplies | Shared? |
|---|---|---|
| **GFootball** | A simulator and an action vocabulary | **Yes** |
| [[nfootball\|NFootball]] | A simulator | No — one group |
| [[soccernet-game-state-reconstruction\|SoccerNet]] | Annotated broadcast video and tasks | **Yes** |
| [[camera-calibration-benchmarking\|ProCC]] | A calibration protocol and metrics | **Yes** |
| [[spadl\|SPADL]] | A common action representation | **Yes** |

GFootball and SPADL do structurally similar work from opposite directions — both impose a **shared vocabulary of what an action is**. Scott et al. use both at once, converting simulated *and* real data into a simplified SPADL to make them comparable, and close by asking the community to standardise representations so methods can be reused. See [[action-valuation-frameworks-compared]].

## Caution

The Kurach et al. (2020) environment paper remains unheld. Everything here comes from three sources that used it.

## See Also

- [[nfootball]] · [[domain-adaptation]] · [[agent-based-simulation]] · [[action-space-design]] · [[social-network-analysis]] · [[proximal-policy-optimization]]
- [[multi-agent-reinforcement-learning]] · [[reinforcement-learning]] · [[action-supervision]] · [[dynamic-time-warping]] · [[deep-q-network]] · [[trueskill]]
- [[spadl]] · [[imitation-learning]] · [[counterfactual-simulation]] · [[action-valuation-frameworks-compared]] · [[camera-calibration-benchmarking]]
- [[google-brain]] · [[google-research]] · [[atom-scott]] · [[masaki-onishi]] · [[hiroshi-nakahara]] · [[keisuke-fujii]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]]
