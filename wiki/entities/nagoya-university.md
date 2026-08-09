---
title: "Nagoya University"
type: entity
tags: [entity, organisation, university, ai-research, sports-analytics, off-ball, defensive-valuation, event-prediction, reinforcement-learning, counterfactual, domain-adaptation, agent-based-simulation, animal-behaviour]
sources: [raw/papers/football_defence_evaluation.md, raw/papers/transformer-point-process-football-event-modelling.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/defensive_player_location_analysis.md, raw/papers/team_defense_positioning_statsbomb.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 52%
  inferred: 40%
  generated: 5%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-07-27
updated: 2026-08-08
---

# Nagoya University

Japanese research university. Its Graduate School of Informatics is the base of [[keisuke-fujii]], and through him the institutional home of the vault's Japanese sports-analytics line — **the largest single-institution cluster here by a wide margin.**

> **Corrected 2026-08-07, then extended twice.** This page once stated two vault sources originate here. **Nine** now do.

## The Nine

| Source | Year | Approach | Target |
|---|---|---|---|
| [[football-defence-evaluation-vdep\|VDEP]] | 2022 | Event classification with off-ball state | **Defensive** value from frequent proxies |
| [[generalized-vdep-euro-location-analysis\|GVDEP]] | 2022 | Score-scaled reweighting; input sweep | Defensive value, partial observation |
| **[[ai-football-reinforcement-learning\|How does AI play football?]]** | **2022** | **[[proximal-policy-optimization\|PPO]] in a simulator; [[social-network-analysis\|SNA]]** | **Whether simulation is viable at all** |
| [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | 2022/23 | GVRNN [[trajectory-prediction]] + counterfactual | [[space-creation\|Space created]] for a teammate |
| [[transformer-point-process-football-event-modelling\|NMSTPP]] | 2023 | [[transformer]] [[point-process\|point process]] | Forecasting event time, zone, action |
| [[team-defense-positioning-counterfactuals\|DRSO]] | 2023 | Geometric counterfactual, **no ML** | Per-defender positioning |
| [[action-valuation-multi-agent-reinforcement-learning\|Multi-agent deep RL]] | 2023 | [[temporal-difference-learning\|TD]] RL, ten agents, **inverse** | On- and off-ball value at every timestep |
| [[adaptive-action-supervision-multi-agent-rl\|Adaptive action supervision]] | 2023 | [[deep-q-network\|DDQN]] + [[dynamic-time-warping\|DTW]], **forward** | A method, not a metric |
| [[optimal-decisions-shot-taking-situations\|SPC framework]] | 2024 | [[game-theory\|Game theory]] + [[theory-based-modelling\|theory-based]] geometry | Optimal shot-or-pass decision |

## Two Registers, and the Second One Came First

Seven papers have Fujii as **senior** author with Nagoya students leading. Two do not — and adding the 2022 [[ai-football-reinforcement-learning|Scott et al.]] paper changes how that division reads.

| | Applied line | Methodological line |
|---|---|---|
| Papers | VDEP, GVDEP, C-OBSO, NMSTPP, DRSO, SPC, Nakahara et al. | **Scott et al. (2022)**, Fujii et al. (2023) |
| Non-university partner | — | **[[aist\|AIST]]**; RIKEN AIP, [[university-of-tokyo\|Tokyo]], [[osaka-university\|Osaka]] |
| Subject | Football | Simulation; biological multi-agents |
| Deliverable | A metric | A feasibility finding; a method |

**The methodological register is not a late departure from an applied programme — it is the earliest work here**, predating every applied metric paper but VDEP and GVDEP. The programme has run in both registers throughout, and the **non-university affiliations attach to the methodological one**. See [[aist]].

## The Signature, and Where It Breaks

Six of the nine come at football through **prediction or physics rather than valuation**, deriving metrics downstream from a model built for something else. That distinguishes the group from the Leuven ([[vaep]]) and Barcelona ([[expected-value-possession-framework|EPV]]) lines.

Three break it, each differently:

- **Nakahara et al.** — the value function is not derived from a predictive model; it *is* the model, trained against reward.
- **Fujii et al. (2023)** — no metric at all; a method, and the only source going **forward** with a bespoke environment.
- **Scott et al. (2022)** — no metric either, and per `policy-gradient-forecloses-action-valuation` on [[proximal-policy-optimization]], its algorithm choice makes one structurally unavailable.

## The Cluster Can Check Itself, and Mostly Does Not

This is the only institution in the vault with enough overlapping work on one dataset to **compare its own outputs**. The comparison has been run once, and it was unflattering: [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] correlate their Q-values against [[c-obso|C-OBSO]] on the same club, season and [[data-stadium|provider]] at $\rho = 0.182$. See [[construct-validity]].

**Three further internal inconsistencies have surfaced since**, none acknowledged by the group:

| Inconsistency | Papers |
|---|---|
| Action spaces of **19, 14 and 12** on overlapping data | Scott et al., Nakahara et al., Fujii et al. |
| $L_1$ against $L_2$ regularisation, same small-data justification | Nakahara et al., Fujii et al. |
| Agents **shoot more** as they improve, against a finding that real shooters shoot too much | Scott et al., [[optimal-decisions-shot-taking-situations\|Yeung & Fujii]] |

Concentration makes comparison possible and apparently does not make it happen. The group's papers repair each other's **stated** limitations reliably — see [[keisuke-fujii]] — and do not reconcile their **unstated** design choices.

## Funding Structure

Fujii holds joint appointments at Nagoya, RIKEN AIP, and JST PRESTO — typical of Japanese research funding. The 2023 RL papers acknowledge JSPS KAKENHI 20H04075, 21H04892, 21H05300, 23H03282 and JST PRESTO JPMJPR20CA.

## People

[[keisuke-fujii]] (author on all nine) · [[hiroshi-nakahara]] · [[masakiyo-teranishi]] · [[rikuhei-umemoto]] · [[calvin-yeung]] · [[kazushi-tsutsui]] · [[kazuya-takeda]] · [[atom-scott]] (Nagoya on the 2023 paper; [[aist|AIST]] in 2022 — see his page for the ambiguity). [[kosuke-toda]] leads VDEP from [[kyoto-university]]; [[masaki-onishi]] is the AIST senior author; [[naoya-takeishi]] and [[yoshinobu-kawahara]] join from Tokyo and Osaka via RIKEN.

## See Also

- [[keisuke-fujii]] · [[atom-scott]] · [[masaki-onishi]] · [[masakiyo-teranishi]] · [[calvin-yeung]] · [[hiroshi-nakahara]] · [[rikuhei-umemoto]] · [[kazushi-tsutsui]] · [[kazuya-takeda]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]]
- [[vdep]] · [[gvdep]] · [[c-obso]] · [[drso]] · [[nmstpp]] · [[hpus]] · [[xsot]] · [[multi-agent-reinforcement-learning]] · [[domain-adaptation]] · [[agent-based-simulation]] · [[proximal-policy-optimization]]
- [[off-ball-value]] · [[construct-validity]] · [[action-space-design]] · [[action-valuation-frameworks-compared]]
- [[aist]] · [[kyoto-university]] · [[university-of-tokyo]] · [[osaka-university]] · [[data-stadium]] · [[google-research-football]] · [[nfootball]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[football-defence-evaluation-vdep|VDEP]] · [[transformer-point-process-football-event-modelling|NMSTPP]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]]
