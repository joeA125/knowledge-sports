---
title: "Off-Ball Value"
type: concept
tags: [off-ball, space-creation, sports-analytics, action-valuation, defensive-valuation, player-evaluation, optical-tracking-data, probability-surface, pitch-control, counterfactual, reinforcement-learning, multi-agent, construct-validity, evaluation, reliability]
sources: [raw/papers/wide_open_spaces_creation_football.md, raw/papers/expected_value_possession_framework.md, raw/papers/football_defence_evaluation.md, raw/papers/defensive_player_location_analysis.md, raw/papers/team_defense_positioning_statsbomb.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/beyond_expected_goals.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/test_retest_reliability_soccer_positioning_and_movement.md, raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.85
provenance:
  extracted: 60%
  inferred: 25%
  generated: 10%
  imported: 0%
  ambiguous: 5%
lifecycle: reviewed
created: 2026-07-27
updated: 2026-08-29
---

# Off-Ball Value

Quantifying the contribution of players who do not have the ball — the runs that stretch a defence, the positioning that opens a passing lane, the shape a defence holds to deny space.

The framing statistic, quoted by three origin papers: **a player has the ball for roughly 3 of 90 minutes.** [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] give the complement — approximately 87 of 90 minutes off the ball.

Long the largest acknowledged gap here. **Six held mechanisms now address it.**

## Why Event Data Cannot See It

An [[event-stream-data|event stream]] records actions. Off-ball contribution is by definition the absence of an action — a player who makes a decisive run and never receives the ball generates **no event at all**. Fixable only with [[optical-tracking-data|tracking]] — though [[gvdep|GVDEP]] and [[drso|DRSO]] both show partial observation goes further than expected.

## Six Mechanisms

### 1. Read a value surface at player positions — the receiver

[[expected-value-possession-framework|Fernández, Bornn & Cervone]] obtain off-ball value as a by-product of the [[probability-surface|pass EPV surface]]. [[obso|Spearman's OBSO]] is the same move with a physically-grounded surface.

### 2. Multiply control by value, and difference it over time — the occupier

[[space-occupation-gain|Fernández & Bornn (2018)]] define quality of owned space as $Q_i(t) = PC_i(t)\,V(t)$ — [[pitch-control]] times [[pitch-value-model|pitch value]] — and take **Space Occupation Gain** as the thresholded change in $Q$ over a three-second window.

The distinguishing feature is that it is a **rate rather than a level**. Routes 1 and 5 ask what a player's position is worth *now*; SOG asks what he *gained*, which credits movement rather than standing.

### 3. Put all 22 positions in the model state — the defence

[[vdep]] appends all 22 coordinates sorted by proximity to the ball; [[shap]] confirms these dominate. [[gvdep|GVDEP]] then measured how much was needed: **ball-gain prediction saturates at three or four players**, and the other three targets gain nothing from player positions at all.

That suggests off-ball value for *defensive* purposes is a **local** quantity, carried by the few players nearest the ball rather than by the configuration as a whole.

### 4. Compare against a predicted reference — the creator

[[c-obso]] credits the improvement in *someone else's* [[obso|OBSO]] attributable to deviating from a predicted trajectory. See [[space-creation]].

### 5. Compare against an optimal position — the defender

[[drso|DRSO]] moves the nearest defender to each vertex of his grid cell and takes the position minimising danger. Same [[counterfactual-baseline|counterfactual machinery]] as route 4, opposite reference: deviation from **optimal** rather than from **normal**.

### 6. Learn an action-value over every player and timestep — the mover

> **Added 2026-08-07** on ingest of [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]].

Instantiate **one [[multi-agent-reinforcement-learning|RL agent per player]] and learn $Q(s,a)$ over a 14-action space** by [[temporal-difference-learning|temporal difference]]. Off-ball value is the Q-value of a movement direction; a player's contribution is the aggregate over the possession.

**This is the only route that requires no reference to difference against.** Routes 4 and 5 need a counterfactual baseline; routes 1 and 2 need a value surface; route 3 needs a classifier target. Here the counterfactual comes free, because a learned $Q$ is *defined* over actions nobody took.

The cost is that the unchosen actions' values are constrained only by [[action-supervision]] and network smoothness — SARSA never targets them. So "free" means "not separately estimated", not "well estimated".

### The mechanisms compared

| | Surface at position | Control × value, differenced | 22 positions in state | Predicted reference | Optimal position | **Learned $Q$** |
|---|---|---|---|---|---|---|
| Values | The **receiver** | The **occupier** | The **defence** | The **creator** | The **defender** | **The mover** |
| Measures | A level | **A rate** | A level | A rate | A level | **A rate** |
| Needs a baseline | Yes (surface) | Yes (surface) | No | **Yes** | **Yes** | **No** |
| Reported unit | Player | Player | **Team only** | Player | **Team** | **Player** |
| Example | EPV surface, [[obso]] | [[space-occupation-gain\|SOG]] | [[vdep]], [[gvdep]] | [[c-obso]] | [[drso]] | Nakahara et al. |

**`counterfactual-individuates`** — the individuating ingredient is the counterfactual, not the data.^[generated: declared on [[counterfactual-baseline]]. Supported by DRSO; **weakened 2026-08-07** — route 6 individuates via per-agent value functions rather than by intervening, which is a second non-counterfactual individuator alongside SGG. rests-on: claim:counterfactual-individuates]

Routes 2 and 4 both produce per-player *creation* credit without a counterfactual — [[space-occupation-gain|SGG]] does it by spatial predicate. Route 6 does it by **agent decomposition**. Neither falsifies the claim outright, since SGG attributes by co-occurrence and route 6's per-agent split is itself a modelling choice rather than a measurement, but two independent non-counterfactual routes is materially more pressure than one.

## The Two Off-Ball Metrics That Have Been Compared Disagree

> **Added 2026-08-07 — the first head-to-head in this vault, and it is not reassuring.**

[[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] correlate their Q-values against the other off-ball metrics available on **the same club, season and data provider**:

| Against | $\rho$ | Reading |
|---|---|---|
| [[obso\|OBSO]] | −0.305 | Low negative |
| [[c-obso\|C-OBSO]] | **0.182** | **No relationship** |
| Season goals | −0.761 | Strong negative |
| Expert ratings | −0.218 | Low negative |

C-OBSO and the Q-values are both from the [[keisuke-fujii|Fujii group]], both on Yokohama F. Marinos in J1 2019 with [[data-stadium|Data Stadium]] tracking, both presented as measures of off-ball contribution. They are unrelated.

The paper's explanation — C-OBSO ranks forwards, Q-values rank midfielders and defenders — is plausible and partly self-defeating. **If each metric ranks a different position group, neither measures off-ball contribution as such.** The honest report would name the construct more narrowly, or report it as position-conditional.

$N = 14$, so this is weak evidence for any conclusion. But it is the only evidence the field has produced, and it points the wrong way. See [[construct-validity]].

## What Is Now Covered

| Capability | Status |
|---|---|
| Positional value if the ball arrives | **Covered** — EPV surface, [[obso\|OBSO]] |
| Space gained by movement | **Covered** — [[space-occupation-gain\|SOG]] |
| Team defensive contribution | **Covered** — [[vdep]], [[gvdep]] |
| Credit for creating space for a teammate | **Covered twice** — [[space-occupation-gain\|SGG]] and [[c-obso]], by different mechanisms |
| Per-defender positioning quality | **Computed but not reported** — [[drso\|DRSO]] |
| **Value of a movement not made** | **Covered** — route 6, via $Q$ over unchosen actions |
| Movement over time | Partial — C-OBSO 4 s, SOG 3 s windows; **route 6 covers whole possessions to 30 s** |
| Errors of omission | **Partially covered** — route 6 assigns a value to every available action, so failing to make a valuable run is now visible in principle. Not reported as such by any source |
| **Agreement between the metrics** | **Now measured, and poor** |

## What Remains Open

- **The metrics do not agree.** See above. This displaces "not enough mechanisms" as the field's main problem.
- **Per-player defensive results are unpublished.** [[drso|DRSO]] computes $Diff_{opt-obs}$ per named defender; every published result averages to teams.
- **The two space-creation methods have never been compared**^[generated: the observation that SGG and C-OBSO are directly comparable and never compared is drawn here. **Re-checked 2026-08-07** on ingest of Nakahara et al.: still holds. That paper compares Q-values against C-OBSO and OBSO but not against SGG. rests-on: absence:no-held-source-compares-sgg-and-cobso — ⚠️ re-check on any space-creation ingest], despite being directly comparable on a single match.
- **Errors of omission** — route 6 addresses this in principle; no source reports it.
- **Scale.** C-OBSO predicts 3 of 22; SOG/SGG analyse one match; Nakahara et al. use 14 players from one club, attacking third only.
- **No reliability figure for any off-ball metric here.**^[generated: an instance of `no-reliability-for-off-ball-metrics`, declared on the synthesis. **Re-checked 2026-08-07** on the Nakahara ingest: still holds — that paper reports no reliability figure either, and its authors state no ground truth exists. Six mechanisms, zero reliability estimates. rests-on: claim:no-reliability-for-off-ball-metrics] Since [[split-half-reliability|reliability]] is the criterion that matters most for [[recruitment]], the metrics best suited to finding undervalued players are the ones whose stability is least known.

**The reliability gap and the disagreement gap are the same gap.** If two metrics disagree, either they measure different things or at least one is unstable — and without a reliability figure those cannot be told apart. That makes reliability the cheapest next measurement in this whole area.

### ⚠️ And the Missing Numbers Are Probably Worse Than Assumed

> **Added 2026-08-29** on ingest of [[gps-deceleration-reliability|Jones et al. (2024)]]. See [[reliability-layers]].

The only empirical handle the vault has on how reliability behaves under transformation comes from outside football valuation entirely: **reliability falls with derivation depth.** Measuring maximal deceleration, directly sampled velocity reproduced at ICC 0.81 and CV 1.4%; quantities two transformations away — an integral, a detected segment duration — fell to ICC 0.48–0.57.

Ranked by how far each mechanism sits from a sampled position:

| Mechanism | Chain from tracking data | Depth |
|---|---|---|
| [[vdep]], [[gvdep]] | positions → classifier | **Shallowest** |
| Route 6 ($Q$) | positions, velocities → GRU → $Q$ | Shallow |
| [[obso\|OBSO]] | positions → pitch control → scoring surface | Medium |
| [[space-occupation-gain\|SOG]] | positions → control × value → **differenced over 3 s** | Deep |
| [[drso\|DRSO]] | positions → control → danger → **optimal-position search** | Deep |
| [[c-obso\|C-OBSO]] | positions → **trajectory prediction** → control → surface → **counterfactual difference** | **Deepest** |

^[generated: no source ranks these by derivation depth; constructed here from each mechanism's stated pipeline. rests-on: claim:each-layer-of-derivation-costs-reliability]

**Two of the deepest three are the ones that differencing makes deeper still.** A difference of two noisy quantities is noisier than either — the standard variance result — and routes 2, 4 and 5 all end in a subtraction.

This is an extrapolation across domains and explicitly not a measurement.^[generated: flagged as extrapolation on [[reliability-layers]]] But it predicts something checkable: **if the six mechanisms were ranked by split-half reliability, the counterfactual-differenced ones should come last.** That is a concrete, falsifiable ordering where the vault previously had only "nobody has measured it".

### ✅ Confirmed on Football Tracking Data, 2026-08-29

> The extrapolation is no longer an extrapolation. [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]] measured robustness for ~32 tracking-derived off-ball defensive metrics and report, in their own words, that **removing pass value** raises stability — allowing "lower variance on the event level" while "retaining the ability to distinguish players".

Their *raw* metrics — proximity without [[expected-threat|xT]] weighting — beat their *valued* metrics on robustness almost everywhere. **Removing the value model is removing a derivation layer**, and it does exactly what the claim predicted, on the right class of construct, measured by people not testing the claim.

⚠️ **With a nuance the vault's version lacked.** *Valued fault per 90* has the **highest validity of any metric they tested** and below-average robustness. **Derivation costs reliability and buys something.** The vault had treated the cost as pure loss; it is a trade, and the ranking above is therefore a prediction about *reliability only*, not about which mechanism is best.

The second stabiliser they identify — adding a **role-conditioned expected involvement** — is [[empirical-bayes-shrinkage|shrinkage]] under another name. See [[role-conditioned-baseline]].

⚠️ **And their headline scores are z-scored composites**, so the absolute reliability of any off-ball metric remains unmeasured. The prediction above is confirmed in *direction* and still untested in *level*.

### ✅ The Levels, Recovered 2026-08-29

Computed in-vault from their committed World Cup data — **not figures they published.**

| | Unadjusted ICC(1,1) | Role-adjusted |
|---|---|---|
| Best of twelve off-ball variants | **0.656** | **0.304** |
| Worst | **0.342** | **0.077** |

^[generated: computed in-vault; full table and caveats on [[off-ball-defensive-performance-blame]]. rests-on: source:bischofberger-committed-player-level-csv]

**Two things follow for this page.**

**The derivation-depth ordering holds with absolute numbers.** Raw fault 0.656 against valued fault 0.479 on the same denominator, and the raw-beats-valued gap holds across all six metric pairs. Removing the xT weighting — one derivation layer — is worth roughly 0.15 of ICC.

⚠️ **But the levels are low enough to change how the ρ = 0.182 disagreement should be read.** At an unadjusted ICC of 0.34–0.66, and **0.08–0.30 once position is removed**, these metrics do not have the reliability required to support a construct-level interpretation of a correlation that size. **Two measures this noisy would disagree even if they were measuring the same thing.**

That does not settle the disagreement — [[c-obso|C-OBSO]] and the RL Q-values are different constructs from these, on different data — but it makes the **instability reading substantially more plausible than the vault has been treating it**, and it is the first evidence on that question rather than an inference.^[generated: the transfer from these metrics to C-OBSO is an inference across constructs and datasets, not a measurement of either]

⚠️ **Caveats apply**: World Cup only, median three matches per unit, and a blunter role correction than the authors use. See the source page.

**It also supplies a candidate reading of the ρ = 0.182 disagreement above.** C-OBSO is the deepest mechanism here and the Q-values among the shallowest. The paper's explanation — different position groups — and an instability explanation are not exclusive, and **the reliability measurement would separate them.** One number, and it settles which.^[generated: neither reading is proposed by any source]

> **And the measurement now has a specification.** [[meta-analytics-sports-metrics|Franks et al. (2016)]] require meta-metrics to be computed **conditional on player type**, precisely because a metric can score well by tracking position rather than ability. Computing discrimination for these six within position groups distinguishes "different positional slices of one construct" from "position dressed as skill". See [[construct-validity]].
>
> Add [[metric-independence|independence]] across all six on shared players and the redundancy question — how much of this proliferation is one signal renamed — becomes a reported statistic rather than a complaint. ⚠️ Franks et al.'s NBA result suggests **defensive and off-ball metrics are the *least* redundant class**, which would make the proliferation here more defensible than the vault has assumed.

> ⚠️ **And role-dependence may be worse than a confound — it may be the construct.** [[off-ball-defensive-performance-blame|Bischofberger et al.]] rank Kylian Mbappé **third best** defending winger at the 2022 World Cup by raw fault and **third weakest** by raw fault responsibility. Two closely related metrics, one player, opposite conclusions — because responsibility charges him for passes his role implied he should have covered and he was nowhere near.
>
> They note this contrasts with offensive metrics like pass completion and xT, where one metric serves across roles. **If defensive duty is assigned by shape while attacking contribution is largely taken, a single off-ball defensive number may not be a coherent object** — and ρ = 0.182 is what that incoherence looks like when two such numbers are correlated. See [[role-conditioned-baseline]].
> ^[generated: the paper states the role-dependence but does not connect it to cross-metric disagreement]

## What the Evidence Shows

**[[obso|OBSO]] predicts next-match goals better than shots or goals do** — 0.26 against 0.17 and 0.12, at player level against an independent outcome. The strongest [[predictive-validity]] result in the vault, and still the only one against an external criterion.

**[[c-obso|C-OBSO]] correlates 0.45 with salary** where own-opportunity (−0.28) and goals (−0.23) do not.

**Occupation and generation are distinct skills**, found independently by [[space-occupation-gain|SOG/SGG]] in La Liga and [[c-obso|C-OBSO]] in the J-League. Two methods, two leagues, same structural finding.

**Off-ball and on-ball ability correlate moderately within a player** — Nakahara et al. report $\rho = 0.618$ between a player's mean on-ball and off-ball Q-values, suggesting a consistent style across both states rather than two separable skills. Mild tension with the occupation/generation split above, and unresolved.

**The gain/attacked trade-off replicates** across [[vdep|VDEP]] and [[gvdep|GVDEP]] — J-League and European tournament, men's and women's football.

## Applications

**Pressing analysis.** Off-ball advantage heatmaps per opponent formation — see [[tactical-analysis]].

**Pairwise relationships.** [[expected-value-possession-framework|EPV]] between David Silva and each teammate; [[space-occupation-gain|SGG]]'s generator–receiver matrix does the same for space, showing a reciprocal Suárez↔Messi pair and Busquets receiving from nearly everyone.

**Coaching defensive positioning.** [[drso|DRSO]] outputs a specific alternative position — advice rather than a score.

**Valuing the run not made.** Route 6's per-action Q-values are, in principle, prescriptive at every timestep for every player. No source exploits this; it is the most obvious unclaimed application in this area.

**Broadcast-only analysis.** [[gvdep|GVDEP]], [[drso|DRSO]] and [[obso|OBSO]] all deliberately minimise data requirements. Route 6 does not — it needs full tracking.

## Relation to Pitch Control

Control asks *who would win the ball here*; off-ball value asks *what would this possession be worth if the ball arrived here*. [[obso|OBSO]] and [[space-occupation-gain|SOG]] both make the relationship explicit by multiplying a control surface by a value surface — with **different control models and different value models**, and no comparison between them. See [[pitch-control-traditions-compared]].

Route 6 is the first mechanism here that uses **no pitch-control model at all** — raw positions and velocities go into a [[gated-recurrent-unit|GRU]] and value comes out. Whether that is an advantage (fewer assumptions) or a loss (no [[theory-based-modelling|physical structure]]) is exactly the question [[handcrafted-features-rule]] asks.

## See Also

- [[space-occupation-gain]] · [[c-obso]] · [[obso]] · [[drso]] · [[space-creation]] · [[counterfactual-baseline]]
- [[multi-agent-reinforcement-learning]] · [[action-supervision]] · [[action-space-design]] · [[temporal-difference-learning]] · [[reinforcement-learning]]
- [[pitch-control]] · [[pitch-value-model]] · [[probability-surface]] · [[vdep]] · [[gvdep]] · [[defensive-valuation]]
- [[action-valuation]] · [[shap]] · [[trajectory-prediction]] · [[tactical-analysis]] · [[recruitment]] · [[construct-validity]] · [[split-half-reliability]]
- [[william-spearman]] · [[javier-fernandez]] · [[luke-bornn]] · [[keisuke-fujii]] · [[rikuhei-umemoto]] · [[hiroshi-nakahara]]
- [[wide-open-spaces-space-creation|Wide Open Spaces]] · [[beyond-expected-goals|Spearman 2018]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO]] · [[team-defense-positioning-counterfactuals|DRSO]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]]
