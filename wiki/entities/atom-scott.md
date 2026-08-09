---
title: "Atom Scott"
type: entity
tags: [person, researcher, research-institute, university, ai-research, sports-analytics, reinforcement-learning, multi-agent, simulator, agent-based-simulation, domain-adaptation, network-analysis]
sources: [raw/papers/ai_football_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md]
confidence: 0.8
provenance:
  extracted: 66%
  inferred: 26%
  generated: 6%
  imported: 2%
  ambiguous: 0%
lifecycle: reviewed
created: 2026-08-07
updated: 2026-08-08
---

# Atom Scott

Researcher working on football simulation and reinforcement learning. **Lead author of one held source and co-author of a second.**

| Year | Work | Role | Affiliation given |
|---|---|---|---|
| 2022 | [[ai-football-reinforcement-learning\|How does AI play football?]] (ICAART) | **Lead** | [[aist\|AIST]], Tsukuba |
| 2023 | [[adaptive-action-supervision-multi-agent-rl\|Adaptive action supervision in RL]] | Co-author | [[nagoya-university\|Nagoya]] (collective list) |

> ⚠️ **Corrections, 2026-08-08.** This page previously described Scott as "Researcher at the Graduate School of Informatics, Nagoya University" and his 2022 paper as **cited, not held**. Both were wrong.
>
> **The 2022 paper is now held**, and he is its lead author, not a middle author.
>
> **The affiliation was an over-confident inference.** The 2023 paper lists institutions collectively rather than per-author, and the vault assigned Scott to Nagoya on that basis. The 2022 paper places him at [[aist|AIST]] with per-author affiliation markers. The chronology suggests a move between 2022 and 2023, but no held source states it, so this is recorded as **ambiguous rather than resolved.**

## The Standing Acquisition Target, Resolved

Scott's 2022 paper was flagged across four ingests — on [[reinforcement-learning]], [[google-research-football]], [[multi-agent-reinforcement-learning]] and this page — as the one cited work bearing directly on the **simulator transfer question**. It was the vault's highest-value acquisition target after arXiv:2305.13030 was acquired and disappointed.

**This one delivered.** It measures transfer, finds partial convergence between simulator-trained agents and real J-League teams on pass-network topology, and does so with an explicit statement of why that dimension was chosen. See [[ai-football-reinforcement-learning]] and [[domain-adaptation]].

## The Two Papers Bracket the Question

Read together, Scott's two held works make a statement neither makes alone:

| | 2022 (Scott lead) | 2023 (Fujii lead) |
|---|---|---|
| Direction | Forward | Forward |
| Environment | **[[google-research-football\|GFootball]]** | **[[nfootball\|NFootball]]** — built to replace it |
| Agents | Single, central control | Decentralised multi-agent |
| Algorithm | **[[proximal-policy-optimization\|PPO]]** — policy gradient | **[[deep-q-network\|DDQN]]** — value-based |
| Compared on | **Pass-network topology** | **Movement trajectories** ([[dynamic-time-warping\|DTW]]) |
| Result | **Partial convergence** | **Failure to reproduce** |

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

That is the synthesis product of holding both, and Scott is on both sides of it. It resolves what looked like a contradiction into a coherent boundary — and it depends on his 2022 paper's methodological candour about *why* SNA was chosen: an analysis framework not influenced by physical differences between simulation and reality.

**He also participated in abandoning the environment his own paper validated.** Having used GFootball to argue simulation is a practical approach for football analysis, he appears a year later on the paper that builds a bespoke replacement because GFootball's transitions resist customisation and its passes fire at the wrong timings. Neither paper acknowledges the tension. See [[nfootball]].

## The Representation Complaint

His 2022 paper closes with a specific, author-side statement of a problem the vault records as `no-cross-framework-benchmarking`:

> we noticed that it was difficult to use state of the art football analysis methods due to different representations of the underlying data

He converts both simulated and real data to a simplified [[spadl|SPADL]] and asks the community to build on such representations. **A practitioner naming the barrier as representational rather than social**, which is more specific than the vault's own framing. See [[action-valuation-frameworks-compared]].

## See Also

- [[ai-football-reinforcement-learning|Scott et al. 2022]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al. 2023]]
- [[agent-based-simulation]] · [[social-network-analysis]] · [[proximal-policy-optimization]] · [[domain-adaptation]] · [[google-research-football]] · [[nfootball]]
- [[multi-agent-reinforcement-learning]] · [[reinforcement-learning]] · [[imitation-reward-tradeoff]] · [[action-space-design]] · [[dynamic-time-warping]] · [[spadl]]
- [[masaki-onishi]] · [[keisuke-fujii]] · [[kazushi-tsutsui]] · [[hiroshi-nakahara]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]]
- [[aist]] · [[nagoya-university]] · [[data-stadium]] · [[trueskill]]
