---
title: "Knowledge Base Overview"
type: synthesis
tags: [sports-analytics, action-valuation, off-ball, evaluation, reinforcement-learning, computer-vision, player-evaluation]
sources: []
confidence: 0.75
provenance:
  extracted: 5%
  inferred: 55%
  generated: 35%
  imported: 0%
  ambiguous: 5%
lifecycle: reviewed
created: 2026-04-22
updated: 2026-08-14
---

# Knowledge Base Overview

A narrative map of what this vault holds and how it hangs together. The index is the catalog; this page is the argument for the catalog's shape.

**Scope: football and sports analytics.** General ML and LLM material is being migrated to a sibling vault — see *Pending Migration* below.

> **Rewritten 2026-08-12.** The previous version dated from 2026-07-07 and described a general-ML vault with football computer vision as one cluster of seven. That inverted some time ago. It also reported ~29 summaries and ~90 concept/entity pages against actual figures of 56 and 298.

## Composition

| | Count |
|---|---|
| Total pages | **367** |
| Concepts | 193 |
| Entities | 105 |
| Source summaries | **56** |
| Open questions | 8 |
| Syntheses | 1 (plus this page) |
| Dashboards | 3 |

**All 56 raw sources are ingested.** Mean page confidence 0.846; 206 pages `reviewed`, 157 `draft`, 1 `archived`.

## What This Vault Is About

The corpus is dominated by one problem — **assigning value to what happens in a football match** — approached from many directions by groups that mostly do not cite each other. Nearly every framework here reduces to one equation:

$$V(a_i) = Q(S_i) - Q(S_{i-1})$$

and differs only in how it represents the state and computes the value. That common form is what makes the vault's comparisons possible, and it is why [[action-valuation]] and [[vaep]] are its two densest hubs.

The material divides into **seven tasks**, set out in full on [[action-valuation-frameworks-compared]]: valuation, forecasting, clustering, counterfactual/transfer, tactical, prescription, and simulation. They are easily mistaken for one problem and are not.

## The Main Lines

**On-ball valuation.** [[expected-threat|xT]], [[vaep|VAEP]], [[expected-possession-value|EPV]] and [[martingale-epv]] — the tradition running from Markov zone models through supervised action classification to full possession-value surfaces. [[expected-value-possession-framework|Fernández, Bornn & Cervone]] is the most elaborate instance held.

**Off-ball valuation.** The vault's most developed argument, on [[off-ball-value]]: **six distinct mechanisms** for valuing players who do not have the ball, from reading a value surface at a player's position ([[obso|OBSO]]) through [[space-occupation-gain|control × value differencing]], [[vdep|classifier state]], [[c-obso|predicted-trajectory counterfactuals]], [[drso|optimal-position counterfactuals]], to learned per-player action values. **Where two of them have been compared, they disagree** — ρ = 0.182 on the same club, season and provider. See [[construct-validity]].

**Defensive valuation.** [[vdep]] and [[gvdep]] change the prediction target rather than the model, on the finding that goals conceded are too rare to classify. See [[rare-event-proxy-targets]] and the open question at [[vaep-conceding-classifier]].

**Physical and geometric models.** [[pitch-control]], [[probability-surface|value surfaces]], [[space-creation]] — the [[william-spearman|Spearman]] and [[javier-fernandez|Fernández]] traditions, which turn out to be siblings framed against a common ancestor rather than rivals ([[pitch-control-traditions-compared]]).

**Reinforcement learning and simulation.** Three held sources, split forward/inverse: [[ai-football-reinforcement-learning|Scott et al.]] and [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] generate behaviour in simulators; [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] estimates from logged data. Supporting concepts at [[multi-agent-reinforcement-learning]], [[temporal-difference-learning]], [[deep-q-network]], [[proximal-policy-optimization]], [[action-supervision]], [[action-space-design]], [[domain-adaptation]], [[agent-based-simulation]].

**Sequence and event modelling.** [[nmstpp]], [[seq2event]], [[sig-model]], [[scoutgpt]], [[eventgpt]] — forecasting the next event, and the metrics ([[hpus]], [[lpv]]) derived downstream from those forecasts.

**Computer vision for sport.** [[game-state-reconstruction]], [[camera-calibration]], [[soccernet-game-state-reconstruction]], [[camera-calibration-benchmarking]]. Notably the **only area here with functioning cross-method benchmarking**, which makes it a useful control on the valuation literature's failure to do the same.

**Rating systems.** [[elo-rating-system]], [[glicko-rating-system]], [[trueskill]], [[bradley-terry-model]], applied through [[league-strength-rating]] and [[duel-skill-rating]].

## The Hubs

By inbound wikilink count, not by memory:

| Page | Inbound |
|---|---|
| [[vaep]] | 71 |
| [[action-valuation]] | 61 |
| [[transformer]] | 60 |
| [[keisuke-fujii]] | 60 |
| [[action-valuation-frameworks-compared]] | 56 |
| [[c-obso]] | 54 |
| [[off-ball-value]] | 46 |
| [[expected-threat]] | 46 |
| [[nmstpp]] | 45 |
| [[expected-value-possession-framework]] | 45 |

Two of these are worth remarking on. **[[keisuke-fujii]] at 60** reflects a single research group authoring nine held sources — the largest concentration in the vault, and the only one with enough overlapping work on one dataset to check itself. **[[split-half-reliability]] at 42** is higher than its subject matter would suggest, because the absence of reliability figures is load-bearing across the off-ball argument.

## Recurring Findings

- **Metrics beat outcomes at predicting outcomes.** [[obso|OBSO]] predicts next-match goals better than shots or goals do. Goals are the worst predictor of future goals at both player and team level. See [[predictive-validity]].
- **Where metrics have been compared, they disagree** — and the comparisons are rare because comparison requires two methods on one dataset, which licensing usually prevents. See [[data-stadium]].
- **The literature does not benchmark across frameworks.** Weakened twice and still standing; contrast the CV cluster, where [[camera-calibration-benchmarking|ProCC]] exists specifically to enable it.
- **Sixteen asserted free parameters carry no sensitivity analysis**, across six distinct kinds. See [[free-parameters-load-bearing]].
- **Offensive bias has four causes with four different remedies** — definitional, data, modelling choice, and statistical. See [[action-valuation]].

## Open Questions

Eight, grouped by who could answer them.

*Component-level gaps* — [[pitch-control-traditions-compared]], [[shot-value-formulations-compared]], [[tracking-error-propagation]].

*Untested assumptions in held work* — [[free-parameters-load-bearing]], [[vaep-conceding-classifier]].

*Claims this vault generated* — [[within-season-variation-noise-or-signal]], [[observed-versus-optimal-decisions]], [[handcrafted-features-rule]].

The third group is the distinctive one: these are questions no single paper poses, visible only from holding several at once.

## Method

The vault runs a claim-dependency system beyond ordinary sourcing. Generated claims carry IDs and `rests-on:` clauses recording what they stand on, with four dependency kinds — `source:`, `claim:`, `imported:`, and `absence:`. Absence claims are the dangerous kind: they expire when a source is acquired, not when anything is re-read, and several here have been narrowed or retired exactly that way.

Full rules in `_schema/conventions.md`.

## Pending Migration

A general ML and LLM tier remains here and is scheduled to move to the sibling general vault: the transformer and attention line, recurrent architectures, generative models, LLM reasoning and retrieval, and the mathematical foundations pages.

**This is not a clean cut.** [[transformer]] is the vault's third-largest hub at 60 inbound links, and pages like [[representation-learning]], [[graph-neural-network]] and [[residual-connections]] are load-bearing for football pages that depend on them. Several concepts — [[reinforcement-learning]], [[imitation-learning]], [[trueskill]] — cite football and non-football sources together. The migration will need a bridge policy rather than a filter.

## Dashboards

- [[health|Wiki Health]] — stale, low-confidence, draft and orphan-risk pages
- [[reinforcement|Reinforcement]] — ageing, single-source and confidence-decay watch
- [[sources|Source Tracking]] — raw sources and reference counts

## Maintenance Notes

- Created 2026-04-22; this rewrite 2026-08-12
- 182 tags declared, 180 in use. Six declared-but-unused; separately, **four tags appear on pages without being declared** and will fire lint warnings until reconciled
- Page-type tags are applied inconsistently: `entity` and `summary` are used, `concept`, `synthesis` and `question` are not, despite being valid types
- This page previously sat at confidence 0.4 — the lowest in the vault — and was exempt from staleness checks by having no meaningful `updated` discipline. It now carries a real lifecycle and should be revisited whenever a source opens a new area
