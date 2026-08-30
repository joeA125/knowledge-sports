---
title: "Football Modelling Tasks Compared"
type: synthesis
tags: [sports-analytics, action-valuation, defensive-valuation, off-ball, space-creation, player-evaluation, evaluation, counterfactual, game-theory, clustering, event-prediction, reliability, predictive-validity, construct-validity, time-series, recruitment, transfer-prediction, duel-analysis, discounting, selection-bias, probability-surface, tactical-analysis, model-decomposition, proxy-target, class-imbalance, trajectory-prediction, pitch-control, theory-based-modelling, reinforcement-learning, multi-agent, action-space, simulator, domain-adaptation, agent-based-simulation, network-analysis, policy-gradient]
sources: [raw/papers/on-ball-actions-football-xt-vs-vaep.md, raw/papers/evaluating-football-player-actions.md, raw/papers/multiresolution-stochastic-process-model-nba-possessions.md, raw/papers/transformer-point-process-football-event-modelling.md, raw/papers/understanding_football_posessions_using_path_signatures.md, raw/papers/football-event-sequences-spatiotemporal-point-process-mixture-model.md, raw/papers/scoutgpt-generative-transformer-football-player-valuation.md, raw/papers/football-performance-time-series.md, raw/papers/epv_control_and_duel_skills_football.md, raw/papers/expected_value_possession_framework.md, raw/papers/football_defence_evaluation.md, raw/papers/defensive_player_location_analysis.md, raw/papers/team_defense_positioning_statsbomb.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/physics_based_pass_probabilities.md, raw/papers/wide_open_spaces_creation_football.md, raw/papers/beyond_expected_goals.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md, raw/papers/football_strategy_network_theory_analysis.md, raw/papers/team_sports_data_analysis.md]
confidence: 0.9
provenance:
  extracted: 46%
  inferred: 34%
  generated: 15%
  imported: 2%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-07-23
updated: 2026-08-29
---

# Football Modelling Tasks Compared

The vault's football sources are easily mistaken for variations on one problem. They divide into **seven distinct tasks**, each with different data requirements and validation strategies.

> **On provenance.** A synthesis generates claims no single source states. Load-bearing ones are marked `^[generated]` at their point of use, with the fuller argument on the linked page. Claims marked `absence:` have a built-in expiry date.

## The Seven Tasks

| Task | Question | Examples |
|---|---|---|
| **Valuation** | How good was that action or position? | [[expected-goals\|xG]], [[expected-threat\|xT]], [[vaep]], [[vdep]], [[obso]], [[c-obso]], [[space-occupation-gain\|SOG]], [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] |
| **Forecasting** | What happens next? | [[seq2event]], [[nmstpp]], [[sig-model]], [[trajectory-prediction\|GVRNN]] |
| **Clustering** | What kind of sequence is this? | [[football-event-sequences-point-process-mixture\|Mixture model]] |
| **Counterfactual / transfer** | What if this player joined? | [[scoutgpt]], [[transfer-performance-prediction\|Shelopugin regression]] |
| **Tactical** | How does this team play? | [[tactical-analysis\|Pressing analysis]], **[[social-network-analysis\|pass networks]]**, [[network-cohesion\|network cohesion]] |
| **Prescription** | What *should* the player have done? | [[physics-based-pass-probabilities\|Spearman (2017)]], [[xsot\|Yeung & Fujii]], [[drso\|DRSO]] |
| **Simulation** | Can we reproduce real football synthetically? | [[ai-football-reinforcement-learning\|Scott et al.]], [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] |

Simulation is distinct from counterfactual/transfer though both generate: **counterfactual work generates to produce a number about a player; simulation work generates to produce a faithful environment.** [[scoutgpt|ScoutGPT]] would succeed with implausible rollouts; the simulation papers would not.

**The tactical row is now primary-sourced.** [[social-network-analysis]] describes team *structure* rather than valuing actions, and is an established tradition (López Peña & Touchette, Clemente, Buldú, Gonçalves) the vault had entirely missed until 2026-08.

> **Updated 2026-08-29.** [[network-theory-football-strategies|López Peña & Touchette (2012)]] is held, closing the last row of this table that rested wholly on citations inside another source. **Every one of the seven tasks now has at least one primary source.**
>
> ⚠️ Two corrections came with it. The work was cited throughout the vault as *"Peña & Hugo (2012)"* — a mis-citation inherited from [[ai-football-reinforcement-learning|Scott et al.]], in which Touchette's forename was read as a surname. And the finding attributed to it — *winning teams show lower betweenness* — **is not in the paper** and does not survive a rank correlation against its own table ($\rho = +0.17$, $p = 0.54$). See [[social-network-analysis]].

**The tactical task is also the only one validated differently from every other row.**The other six are judged against a predicted or observed outcome; pass-network analysis is judged by whether the description **recognisably matches a team's known style** — Spain's numbers looking like tiki-taka. That is [[construct-validity]] by expert assent, with no criterion behind it, and it is why the one inferential claim the tradition produced turned out to be manufactured downstream rather than measured.^[generated: no source contrasts the tactical task's validation mode against the other six. rests-on: source:lopez-pena-no-statistical-test]

### Prescription is the oldest task

| Instance | Year | Choice set |
|---|---|---|
| **[[physics-based-pass-probabilities\|Hypothetical passing]]** | **2017** | Continuous, searched by annealing |
| [[drso\|DRSO]] | 2023 | Four grid vertices |
| [[xsot\|SPC framework]] | 2024 | Two: shoot or pass |
| *([[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] — **could**, and does not)* | 2023 | 14, per player, per timestep |

The 2017 instance is the only one that **did not scale**; both successors coarsened the choice set rather than finding cheaper search. **You can only prescribe over a choice set you can enumerate — and a group also has to want to make the prescriptive claim.**

## Task 1: Valuation

$$V(a_i) = Q(S_i) - Q(S_{i-1})$$

| | [[expected-threat\|xT]] | [[vaep]] | [[expected-value-possession-framework\|Fernández]] | [[vdep]] | [[obso]] | [[space-occupation-gain\|SOG]] | [[c-obso]] | [[drso]] | [[xsot\|xSOT]] | [[action-valuation-multi-agent-reinforcement-learning\|Nakahara $Q$]] |
|---|---|---|---|---|---|---|---|---|---|---|
| **Perspective** | Attack | Attack | Attack | **Def** | Attack | Attack | Attack | **Def** | **Both** | Attack |
| **Whose value** | Actor | Actor | Actor | Team | Receiver | **Occupier** | **Teammate's** | **Defender** | Actor | **All ten** |
| **Measures** | Level | Level | Level | Level | Level | **Rate** | Rate | Level | Level | **Rate** |
| **Mechanism** | DP | Boosting | Neural ×9 | XGB | **Physical** | **Control × value** | **Counterfactual** | **Counterfactual** | **Hybrid + game** | **[[temporal-difference-learning\|TD/RL]]** |
| **[[interpretability]]** | **High** | Low | Moderate | Moderate | **High** | **High** | Moderate | **High** | Moderate | **Low** |
| **Cost** | Trivial | Modest | Modest | Modest | **Low** | Modest | High | **Low** | Low | High |

**The central trade-off** — richer state buys sensitivity and pays in stability, interpretability and cost — holds except for [[obso|OBSO]], [[space-occupation-gain|SOG]] and [[drso|DRSO]], which are **physical or geometric rather than learned**.

### Five estimation styles

Van Roy et al. name three; this vault added *counterfactual* as a fourth and **reinforcement-learning-based** as a fifth.^[generated: added on [[action-valuation]]. rests-on: source:vanroy-three-style-taxonomy, source:nakahara-sarsa-td]

⚠️ Reliability evidence is thinner than it looks. [[split-half-reliability]] and [[performance-volatility]] measure **the same variance component** with opposite interpretations. See [[within-season-variation-noise-or-signal]].

> **`no-reliability-for-off-ball-metrics`**^[generated: rests-on: absence:no-held-source-reports-off-ball-reliability — ⚠️ re-checked across seven ingests including **Scott et al.**: still holds. Six mechanisms, zero reliability estimates.]

### Axis 1: Perspective, and the proxy migration

VAEP's conceding classifier at **F1 = 0.000**. ⚠️ Near-guaranteed for any calibrated model at a 0.23% base rate, and VAEP never thresholds. See [[vaep-conceding-classifier]].

**The proxy-substitution response has now migrated into reward functions three times**, and nobody in the group notes the migration: Nakahara et al. put conceding in the reward at possession level; Fujii et al. add a shot reward "because the goal reward was sparse and limited"; and Scott et al. inherit GFootball's **checkpoint reward**, added for the same reason **by the environment's authors, outside the group entirely.** See [[rare-event-proxy-targets]].

> ### `offensive-bias-four-causes`
> **Four distinct causes with four different remedies:** definitional, data, modelling choice, statistical.
> ^[generated: rests-on: source:vandijk-rankings, source:mendes-neves-event-data-limits, source:shelopugin-duel-tables, source:vaep-f1-zero. Declared on [[action-valuation]].]

### Free parameters

**Eighteen asserted parameters carry no sensitivity analysis**, of **seven kinds** — horizons, shapes, geometric gates, detection thresholds, prior strengths, stopping points, and **constants borrowed from another field**.^[generated: rests-on: absence:no-sensitivity-analysis-on-horizon-parameters — re-checked at every ingest; seventh kind added 2026-08-29. Declared on [[model-selection]].]

> ⚠️ **Add an eighteenth-and-a-half: the player ordering.** [[permutation-problem|Imposing an order]] on an unordered set of 22 players is a free parameter with measurable effect, and it is **harder to see than any entry on that list because it is not written as a parameter at all** — it is written as an array index. [[data-driven-team-sports-behaviors|Fujii's survey]] catalogues four routes past the problem and compares none of them.

The seventh kind arrived with [[network-theory-football-strategies|López Peña & Touchette]]: PageRank's damping factor $p = 0.85$, taken from Brin & Page's web-surfer model, keeping its value while its football interpretation — the probability a player passes rather than shoots — replaced the derivation that justified it. **It carries the cheapest test on [[free-parameters-load-bearing]]**, needing a pass/shot ratio rather than a retrain.

### `no-cross-framework-benchmarking` Survives Its Hardest Test

> **Added 2026-08-29.** [[data-driven-team-sports-behaviors|Fujii's 2021 survey]] catalogues dozens of frameworks across §3 and §4 and **compares none of them.** It sorts by technique and by goal; it never asks which performs better.

This is the strongest evidence the claim has had, and it arrives from the most awkward direction. The vault has explained the absence **structurally** — comparison requires two methods run on one dataset, and licensing prevents anyone holding both.

**A survey faces no such constraint.** It could have tabulated published results at zero data cost. Fujii, a central author with nine other papers held here, wrote a field review and did not.

That shifts the explanation. The gap is not only a data-access problem; it is **a norm about what a contribution is**. Comparison is nobody's paper.^[generated: the survey's silence is observed here; the shift from structural to disciplinary explanation is drawn here. rests-on: absence:no-held-source-benchmarks-across-frameworks, source:fujii-survey-no-comparison]

⚠️ The same survey contains **no discussion of measurement reliability** — no split-half, no test–retest, nothing. `no-reliability-for-off-ball-metrics` survives its ninth ingest, and for the same reason: the field's own review does not treat reliability as a topic that exists. See [[split-half-reliability]].

⚠️ **The vault predicted where the $\lambda_1$ answer would be and was wrong.** The recorded lesson: **methods papers optimise for ablation questions ("is this component needed?"), not calibration questions ("how much of it?")**. See [[free-parameters-load-bearing]].

## Off-Ball Valuation: Six Mechanisms

| | Surface at position | Control × value | Positions in state | Predicted reference | Optimal position | **Learned $Q$** |
|---|---|---|---|---|---|---|
| Values | The **receiver** | The **occupier** | The **defence** | The **creator** | The **defender** | **The mover** |
| Needs a baseline | Yes | Yes | No | **Yes** | **Yes** | **No** |
| Reported unit | Player | Player | **Team only** | Player | **Team** | **Player** |

**`counterfactual-individuates`** — **weakened**: route 6 individuates by *agent decomposition*, a second non-counterfactual individuator alongside SGG.^[generated: declared on [[counterfactual-baseline]]. rests-on: claim:counterfactual-individuates]

## The Metrics Disagree — and Now So Do the Datasets

| Nakahara $Q$ against | $\rho$ |
|---|---|
| [[c-obso\|C-OBSO]] | **0.182** *(no relationship)* |
| [[obso\|OBSO]] | −0.305 |
| Season goals | −0.761 |

C-OBSO and the Q-values share group, club, season, [[data-stadium|provider]] and 14 players. Both claim off-ball contribution. They are unrelated.

**A mundane explanation has strengthened with each ingest.** The **five** held sources on this dataset each subset it differently: all events (VDEP), shot-ending sequences (C-OBSO), attacking-third possessions (Nakahara), last-pass sequences (Fujii), **passes and shots only (Scott)**. Metrics over different populations of moments need not agree, and the shared name conceals it. See [[data-stadium]].

> ### `shared-data-not-shared-method-enables-comparison`
> **Cross-framework comparison requires two methods on one dataset, which licensing usually prevents. Where one provider serves one group repeatedly, comparison becomes available almost by accident — and is still mostly not done.**
> ^[generated: declared on [[data-stadium]]. Five papers share this dataset; two comparisons have been drawn. rests-on: absence:no-held-source-benchmarks-across-frameworks, source:nakahara-obso-cobso-correlations, source:scott-jleague-comparison]

**`no-cross-framework-benchmarking` survives, weakened twice and undermined from a third direction.** Nakahara et al. compare against a Spearman metric; Scott et al. compare simulated agents against real teams; but Fujii et al. **replace the shared simulator with a bespoke one**.

> ### `bespoke-environments-foreclose-comparison`
> ^[generated: declared on [[nfootball]]. **Strengthened** — Scott et al. demonstrate what the shared environment made possible, the counterfactual the claim lacked. rests-on: source:fujii-nfootball-motivation, source:scott-gfootball-comparison-study]

**And the barrier has now been named from inside.** Scott et al. report that *"it was difficult to use state of the art football analysis methods due to different representations of the underlying data"*, convert everything to a simplified [[spadl|SPADL]], and ask the community to standardise. **A practitioner locating the obstacle as representational rather than social** — and per `interchange-formats-unify-within-a-modality-not-across-one`, SPADL reduces to a two-type subset once a simulator is on the other side.

## Task 7: Simulation, and Where Generation Breaks

The vault holds two forward-approach papers with opposite outcomes, and they resolve into a boundary rather than a contradiction:

| | [[ai-football-reinforcement-learning\|Scott et al. (2022)]] | [[adaptive-action-supervision-multi-agent-rl\|Fujii et al. (2023)]] |
|---|---|---|
| Environment | [[google-research-football\|GFootball]], 11v11 | [[nfootball\|NFootball]], 2v2–4v8 |
| Algorithm | **[[proximal-policy-optimization\|PPO]]** | **[[deep-q-network\|DDQN]]** |
| Compared on | **Pass-network topology** | **Movement trajectories** |
| Result | **Partial transfer** — 3 of 6 metrics converge | **Failure to reproduce** |

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

Scott et al. chose SNA because it is *"not influenced by physical differences between simulations and the real-world"* — so transfer was measured on the dimension **selected for insensitivity to the gap**.

> ### `transfer-evidence-is-conditional-on-the-dimension-chosen`
> ^[generated: declared on [[domain-adaptation]]. rests-on: source:scott-sna-justification, source:fujii-football-reproducibility-failure]

⚠️ **The vault's "no evidence of transfer" claim was wrong** and had propagated to four pages before Scott et al. was acquired. The four-revision history is on [[domain-adaptation]].

**A parallel question for task 4.** ScoutGPT re-generates *event tokens*; Fujii et al. re-generate *continuous movement*.

> ### `regeneration-fidelity-scales-with-representation-coarseness`
> ^[generated: declared on [[counterfactual-simulation]]. ScoutGPT's counterfactuals may work partly because event tokens hide the physics. rests-on: source:scoutgpt-transfer-mae, source:fujii-football-reproducibility-failure]

## Task 2: Forecasting

**`handcrafted-features-rule`** — encode structure the representation cannot recover *and* the data cannot support learning.^[generated: declared on [[representation-learning]]. **Checked three times**: Nakahara et al., Fujii et al. and Scott et al. are all candidate cases and none reports an accuracy metric against which the rule's prediction could be observed. Three uninformative tests suggests **the rule may be unfalsifiable within this literature.** rests-on: claim:handcrafted-features-rule]

## Metrics Beat Outcomes at Predicting Outcomes

**Player level:** OBSO 0.26 against next-match goals, shots 0.17, goals 0.12. **Team level:** LPV 0.28, HPUS 0.26, xG 0.17, goals 0.11. **Goals are the worst predictor of future goals in both.**

Six validation modes, ascending: self-prediction → cross-horizon consistency → [[construct-validity|agreement or divergence against other metrics]] → external outcome → external criterion outside the pipeline → **validation of a component against a directly observable quantity** ([[pass-probability-model|PPCF]] against 5,471 held-out receivers).

> ### `discriminant-claims-need-a-convergent-anchor`
> **Noise also diverges from goals.**
> ^[generated: declared on [[construct-validity]]. rests-on: source:nakahara-negative-goal-correlation, source:nakahara-no-ground-truth]

Fujii et al. supply the honest version of mode 1 — [[dynamic-time-warping|DTW]] distance to a held-out demonstration, reported as a headline metric that **does not improve**. Compare [[eventgpt|EventGPT]], whose simulated value for Saka *exceeds* ground truth and which then uses the simulated value as the baseline.

## Limitations Shared Across Tasks

1. **Offensive bias** — four causes, four remedies.
2. **On-ball bias narrowed** by six off-ball mechanisms; **errors of omission** addressed in principle, unreported.
3. **Individual defensive credit computed but not reported.** Same for prescription in Nakahara et al.
4. **No ground truth** for most quantities.
5. **[[selection-bias]] throughout.**
6. **Scale limits.** C-OBSO predicts 3 of 22; Nakahara et al. use 14 players; Fujii et al. **16 training and 5 test episodes**; Scott et al. **15 agents and 5 real matches**.
7. **Component-level divergence** — two [[pitch-control-traditions-compared|pitch-control traditions]], four [[shot-value-formulations-compared|shot-value formulations]], PPCF parameters attributed to the wrong paper, and **three action spaces (19, 14, 12) descending from one ancestor by successive deletion, none citing another.** See [[action-space-design]].
8. **Strategy-space coarsening** is the price of prescription.
9. **Price is absent everywhere.**
10. **Cross-framework benchmarking almost entirely absent** — weakened twice, undermined by bespoke environments, and now named from inside as a representation problem.
11. ⚠️ **Where metrics have been compared, they disagree.**
12. ⚠️ **Where the forward approach has been attempted on physical behaviour, it failed** — and the cause is the environment, not the algorithm.
13. ⚠️ **Simulated agents shoot *more* as they improve**, against the vault's only prescriptive finding. Three readings on [[observed-versus-optimal-decisions]]; the cheapest test is comparing simulated and real shot conversion by location.

## Practical Guidance

- **Season-long recruitment** → xT for stability; [[transfer-performance-prediction|regression]] to shortlist; [[scoutgpt|simulation]] for fit, noting the coarseness caveat.
- **An attacker whose output understates him** → [[obso|OBSO]] and [[c-obso]], the two with external validation. ⚠️ They disagree with the RL Q-values.
- **A defender or deep midfielder** → the RL Q-values, on 14 players and no reliability figure.
- **Describing team *style* rather than valuing players** → [[social-network-analysis|pass networks]] for players, [[network-cohesion]] for the team as a structure. Note these metrics **cannot be summed or transferred between clubs**, and that the tradition's one inferential claim about winning did not survive acquisition of its primary source.
- **Asking what breaks if a passing lane is cut** → [[network-cohesion|edge connectivity]], which is a counterfactual with no model behind it. Cheap, and shallow in proportion: it assumes the team would not reorganise, which is exactly what a team does.
- **Coaching a decision** → [[xsot|SPC]]. **A position** → [[drso|DRSO]]. **A pass** → [[physics-based-pass-probabilities|hypothetical passing]].
- **Simulating football** → agents can be trained and their *passing structure* partly resembles reality; movement does not. Nothing here supports a tactical conclusion drawn from a simulator.
- **Assessing a defence** → [[gvdep|GVDEP]] over [[vdep|VDEP]]. Team level.
- **Broadcast video only** → [[gvdep|GVDEP]], [[drso|DRSO]], [[obso|OBSO]].
- **Small-data modelling** → [[theory-based-modelling|theory-based features]]; avoid tree ensembles.
- **Any thresholded classifier** → tune the cutoff. See [[class-imbalance-evaluation]].

## Open Questions

- [[pitch-control-traditions-compared]] · [[shot-value-formulations-compared]] · [[tracking-error-propagation]] — component-level gaps
- [[free-parameters-load-bearing]] · [[vaep-conceding-classifier]] — untested assumptions in held work
- [[within-season-variation-noise-or-signal]] · [[observed-versus-optimal-decisions]] · [[handcrafted-features-rule]] — claims this vault generated

## See Also

- [[action-valuation]] · [[defensive-valuation]] · [[off-ball-value]] · [[space-creation]] · [[expected-possession-value]] · [[tactical-analysis]] · [[social-network-analysis]]
- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[temporal-difference-learning]] · [[deep-q-network]] · [[proximal-policy-optimization]] · [[action-supervision]] · [[action-space-design]] · [[construct-validity]]
- [[domain-adaptation]] · [[agent-based-simulation]] · [[dynamic-time-warping]] · [[imitation-reward-tradeoff]] · [[nfootball]] · [[google-research-football]] · [[counterfactual-simulation]] · [[trueskill]]
- [[game-theory]] · [[xsot]] · [[drso]] · [[theory-based-modelling]] · [[policy-modelling]] · [[imitation-learning]] · [[spadl]] · [[data-stadium]]
- [[obso]] · [[c-obso]] · [[space-occupation-gain]] · [[counterfactual-baseline]] · [[trajectory-prediction]] · [[pitch-control]] · [[voronoi-tessellation]]
- [[expected-threat]] · [[vaep]] · [[vdep]] · [[gvdep]] · [[martingale-epv]] · [[expected-goals]] · [[pass-carry-reward]] · [[pass-probability-model]]
- [[rare-event-proxy-targets]] · [[class-imbalance-evaluation]] · [[probability-calibration]] · [[model-selection]] · [[gradient-boosting]] · [[representation-learning]]
- [[hpus]] · [[lpv]] · [[sig-model]] · [[nmstpp]] · [[seq2event]] · [[scoutgpt]] · [[eventgpt]] · [[event-prediction]]
- [[split-half-reliability]] · [[predictive-validity]] · [[selection-bias]] · [[performance-volatility]] · [[recruitment]]
- [[william-spearman]] · [[javier-fernandez]] · [[luke-bornn]] · [[keisuke-fujii]] · [[atom-scott]] · [[hiroshi-nakahara]] · [[calvin-yeung]] · [[rikuhei-umemoto]]
