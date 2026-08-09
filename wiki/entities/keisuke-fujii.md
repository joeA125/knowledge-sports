---
title: "Keisuke Fujii"
type: entity
tags: [person, researcher, ai-research, university, sports-analytics, defensive-valuation, off-ball, event-prediction, game-theory, proxy-target, counterfactual, reinforcement-learning, multi-agent, domain-adaptation, agent-based-simulation, optical-tracking-data, animal-behaviour]
sources: [raw/papers/transformer-point-process-football-event-modelling.md, raw/papers/football_defence_evaluation.md, raw/papers/defensive_player_location_analysis.md, raw/papers/team_defense_positioning_statsbomb.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.9
provenance:
  extracted: 76%
  inferred: 19%
  generated: 3%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-07-23
updated: 2026-08-08
---

# Keisuke Fujii

Researcher at the Graduate School of Informatics, [[nagoya-university]], with affiliations at the RIKEN Center for Advanced Intelligence Project and JST PRESTO. **Author on nine held sources** — nearly three times anyone else in this vault.

## Nine Primary Sources

| Year | Work | Lead author | Contribution |
|---|---|---|---|
| **2022** | **[[ai-football-reinforcement-learning\|How does AI play football?]]** | **[[atom-scott\|Scott]]** | **[[agent-based-simulation\|ABS]] feasibility; [[proximal-policy-optimization\|PPO]] in GFootball; [[social-network-analysis\|SNA]] comparison** |
| 2022 | [[football-defence-evaluation-vdep\|VDEP]] | [[kosuke-toda\|Toda]] | [[vdep]] — team defensive value from frequent proxies |
| 2022 | [[generalized-vdep-euro-location-analysis\|GVDEP]] | [[rikuhei-umemoto\|Umemoto]] | [[gvdep]] — score-scaled weights |
| 2022/23 | [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | [[masakiyo-teranishi\|Teranishi]] | [[c-obso]] — credit for [[space-creation\|space created]] |
| 2023 | [[transformer-point-process-football-event-modelling\|NMSTPP]] | [[calvin-yeung\|Yeung]] | [[nmstpp]] and [[hpus]] |
| 2023 | [[team-defense-positioning-counterfactuals\|DRSO]] | Umemoto | [[drso]] — per-defender counterfactual positioning |
| 2023 | [[action-valuation-multi-agent-reinforcement-learning\|Multi-agent deep RL valuation]] | [[hiroshi-nakahara\|Nakahara]] | [[multi-agent-reinforcement-learning\|Per-player RL agents]]; [[action-supervision]] |
| 2023 | [[adaptive-action-supervision-multi-agent-rl\|Adaptive action supervision]] | **Fujii — *first author*** | [[domain-adaptation\|Real-to-Sim]]; [[dynamic-time-warping\|DTW]] supervision; [[nfootball]] |
| 2024 | [[optimal-decisions-shot-taking-situations\|SPC framework]] | Yeung | [[game-theory\|Game-theoretic]] shot decisions; [[xsot\|xSOT]] |

> **Corrected three times on 2026-08-07/08.** Six → seven (Nakahara et al. moved from cited-not-held) → eight (arXiv:2305.13030 acquired) → **nine** (Scott et al. 2022 acquired). The 2022 Scott paper is now the **earliest** of the nine and was the vault's standing acquisition target across four ingests.

## Two Registers, and the Earliest Paper Belongs to the Second

Seven of nine have Fujii as **senior** author with Nagoya students leading. Two do not, and both sit outside the applied metric line.

| | Applied line | Methodological line |
|---|---|---|
| Papers | VDEP, GVDEP, C-OBSO, NMSTPP, DRSO, SPC, Nakahara et al. | **Scott et al. (2022)**, **Fujii et al. (2023)** |
| Fujii's role | Senior author | Middle author; first author |
| Co-authors | Nagoya students | [[aist\|AIST]]; [[university-of-tokyo\|Tokyo]] + [[osaka-university\|Osaka]] + RIKEN |
| Subject | Football | **Simulation; biological multi-agents** |
| Football is | The object of study | **A test case** |
| Deliverable | A metric | A feasibility finding; a method |

**Adding Scott et al. shows the methodological line is not a late departure — it is the earliest thing here.** The 2022 ABS paper predates every applied metric paper except VDEP and GVDEP, and it asks whether simulation is viable at all. The programme runs in both registers throughout, and the non-university affiliations (AIST, RIKEN) attach to the methodological one.

That matters for reading these papers. **Their football results are demonstrations, not claims about football**, and should not be read the way [[c-obso|C-OBSO]] or [[vdep|VDEP]] can be.

## Three Signatures, and What Breaks Them

### Change the target, not the model

Visible in four of nine: **goals are too rare to model, so model something else on the causal path.** [[vdep]] and [[gvdep]] use ball recovery and effective attack; [[hpus]] uses possession dynamics with no goal data at all; [[xsot]] uses shot on target.

Empirically justified from inside the group's own work — VDEP measures [[vaep|VAEP's]] conceding classifier at F1 = 0.000. See [[rare-event-proxy-targets]].

**The signature now appears three times inside reward functions**, which is a migration nobody in the group comments on: Fujii et al. add a shot reward "because the goal reward was sparse and limited"; Scott et al. inherit GFootball's **checkpoint reward**, added for the same reason by the environment's authors. The move is portable enough that it arrives from outside the group too.

### Counterfactual on one named agent

[[c-obso]] against a predicted trajectory, [[drso]] against an optimal grid vertex. Both build on [[obso|OBSO]] rather than event classification. See [[counterfactual-baseline]].

### Forward simulation

**Newly visible as a signature rather than a one-off.** Scott et al. (2022) and Fujii et al. (2023) both build or use a simulator and generate behaviour inside it — the *forward* approach, against the inverse approach of everything else. Two of nine, five years apart in conception, with [[atom-scott]] on both.

### The outliers

**Nakahara et al.** declines the target substitution (the reward *is* the goal) and is counterfactual *without a reference*. **Scott et al.** produces no metric at all — and per `policy-gradient-forecloses-action-valuation` on [[proximal-policy-optimization]], its PPO choice makes one unavailable.

## An Internal Contradiction on Shooting

> **Added 2026-08-08, and it is between two papers he co-authored.**

[[ai-football-reinforcement-learning|Scott et al.]]: more competitive RL agents **shoot more** (total shots $r = 0.77$ with TrueSkill) and pass less.
[[optimal-decisions-shot-taking-situations|Yeung & Fujii]]: real shooters **shoot too much** — passing is worth 0.2456 against shooting's 0.0866.

Either GFootball's shot model is too generous, or both hold in their domains and the divergence measures the gap, or neither axis means what it says. **Fujii is on both papers and neither cites the other.** See [[reinforcement-learning]].

## Iteration Within the Group, and Where It Stops

**Each paper fixes a limitation the previous one named.** VDEP states three; GVDEP addresses all three. NMSTPP's gaps are addressed by VDEP and C-OBSO. VDEP's inability to individuate is addressed by DRSO. C-OBSO's inability to value players who never receive the ball is addressed by Nakahara et al. **Scott et al. declares multi-agent RL out of scope; Nakahara et al. and Fujii et al. are the follow-ups.**

That chain is rare in this literature.

**But it does not extend to parameters, action spaces, or cross-citation.** Fujii et al. propose [[action-supervision]] and report no value for its weight; Nakahara et al. borrow it and report $\lambda_1 = 0.01$. The three RL papers use action spaces of 19, 14 and 12 on overlapping data without mentioning each other's choice. And the interaction blind spot persists across four papers — Scott et al. use one agent with ten scripted teammates, both MARL papers use independent agents, a centralised alternative (CDS) changed nothing, while [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] model a best-responding opponent explicitly.

## Methodological Range

| Approach | Work |
|---|---|
| Supervised classification on engineered state | [[vdep]], [[gvdep]] |
| [[transformer]] [[point-process\|point process]] | [[nmstpp]] |
| [[graph-neural-network\|Graph]] [[variational-autoencoder\|VAE]] trajectory prediction | [[c-obso]] |
| [[theory-based-modelling\|Theory-based]] geometry + game theory | [[xsot]] |
| **[[proximal-policy-optimization\|PPO]] + [[social-network-analysis\|SNA]] + [[trueskill]]** | **Scott et al.** |
| [[temporal-difference-learning\|TD]] RL with [[gated-recurrent-unit\|GRU]] | Nakahara et al. |
| [[deep-q-network\|DDQN]] + [[dynamic-time-warping\|DTW]] supervision | Fujii et al. |
| Physical surface + counterfactual search, **no ML** | [[drso]] |

No single architecture recurs. What recurs is **prediction or physics first, metric derived downstream** — broken by Nakahara et al. (value learned directly) and by Scott et al. (no metric at all).

## A Caution

Two papers ([[c-obso]], [[drso]]) set PPCF parameters $\sigma = 0.45$, $\lambda = 4.3$ citing [[beyond-expected-goals|Spearman (2018)]], which fits $s = 0.54$, $\lambda = 3.99$. A citation error propagating through the line — see [[obso]].

## Cited, Not Held

- **Fujii (2021)**, *Data-driven analysis for understanding team sports behaviors*, arXiv:2102.07545 — the group's own survey.
- **Fujii, Takeishi, Kawahara & Takeda (2020)**, *Policy learning with partial observation and mechanical constraints*, arXiv:2007.03155.
- **Fujii, Takeishi, Tsutsui et al. (2021)**, *Learning interaction rules from multi-animal trajectories*, NeurIPS 34.
- **Fujii, Takeuchi, Kuribayashi et al. (2022)**, *Estimating counterfactual treatment outcomes in complex multi-agent scenarios*, arXiv:2206.01900.
- **Tsutsui, Takeda & Fujii (2023)**, *Synergizing deep RL and biological pursuit behavioral rule*, ICML workshop.
- **Ding, Takeda & Fujii (2022)**, *Deep RL in a racket sport for player evaluation* — the only non-football RL valuation work.
- **Teranishi, Fujii & Takeda (2020)**, IEEE GCCE pp. 124–125.

## See Also

- [[vdep]] · [[gvdep]] · [[drso]] · [[c-obso]] · [[nmstpp]] · [[xsot]] · [[hpus]] · [[obso]]
- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[proximal-policy-optimization]] · [[deep-q-network]] · [[temporal-difference-learning]] · [[action-supervision]] · [[action-space-design]]
- [[agent-based-simulation]] · [[social-network-analysis]] · [[domain-adaptation]] · [[dynamic-time-warping]] · [[imitation-reward-tradeoff]] · [[nfootball]] · [[google-research-football]] · [[trueskill]]
- [[rare-event-proxy-targets]] · [[counterfactual-baseline]] · [[defensive-valuation]] · [[off-ball-value]] · [[space-creation]] · [[construct-validity]]
- [[atom-scott]] · [[masaki-onishi]] · [[calvin-yeung]] · [[kosuke-toda]] · [[rikuhei-umemoto]] · [[masakiyo-teranishi]] · [[hiroshi-nakahara]] · [[kazushi-tsutsui]] · [[kazuya-takeda]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]]
- [[nagoya-university]] · [[aist]] · [[kyoto-university]] · [[university-of-tokyo]] · [[osaka-university]] · [[data-stadium]] · [[william-spearman]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[football-defence-evaluation-vdep|VDEP]] · [[generalized-vdep-euro-location-analysis|GVDEP]] · [[team-defense-positioning-counterfactuals|DRSO]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO]] · [[transformer-point-process-football-event-modelling|NMSTPP]] · [[optimal-decisions-shot-taking-situations|SPC]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]]
