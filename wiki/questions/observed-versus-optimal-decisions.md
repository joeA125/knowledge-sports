---
title: "Do players decide suboptimally, or do the models only think so?"
type: question
tags: [policy-modelling, game-theory, action-valuation, evaluation, predictive-validity, counterfactual, sports-analytics, reinforcement-learning, action-space, auxiliary-loss, imitation-learning, domain-adaptation, simulator, needs-review]
sources: [raw/papers/expected_value_possession_framework.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.7
provenance:
  extracted: 40%
  inferred: 50%
  generated: 9%
  imported: 0%
  ambiguous: 1%
lifecycle: draft
created: 2026-07-27
updated: 2026-08-08
---

# Do players decide suboptimally, or do the models only think so?

**Status:** Open. The fourth objection — that the gap is partly tunable — is now supported by measurement. And a fifth source has arrived that **points the opposite way on the one substantive finding.**

| Source | Method | Observed | Best available | Gap |
|---|---|---|---|---|
| [[expected-value-possession-framework\|Fernández et al.]] | Pass-value [[probability-surface\|surface]] | 0.032 | 0.112 | 0.080 |
| [[xsot\|Yeung & Fujii]] | [[game-theory\|Game-theoretic]] payoffs | 0.0866 (shoot) | 0.2456 (pass) | 0.159 |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | Learned $Q$ over 14 actions | **Tunable via $\lambda_1$** | **Tunable** | Not reported |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | DQAAS in a simulator | — | — | Trade-off measured |
| **[[ai-football-reinforcement-learning\|Scott et al.]]** | **PPO agents ranked by TrueSkill** | — | — | **Better agents shoot *more*** |

## First: They Are Not the Same Claim

**Fernández et al. measure suboptimal *targeting within* an action.** Given a pass is made, it often does not go to the highest-value destination.

**Yeung & Fujii measure suboptimal *selection between* actions.** Given a shot-taking situation, the shooter shoots when the equilibrium says pass.

**Nakahara et al. could measure either** and measure neither.

So "players shoot too much" is Yeung & Fujii's claim alone. What the first two jointly support is weaker and still substantial: **a large observed-versus-optimal gap appears under two unrelated methods, on different leagues, data types and decision granularities.**

## The Simulator Points the Other Way

> **Added 2026-08-08, and it is the most direct challenge this page has received.**

[[ai-football-reinforcement-learning|Scott et al.]] rank fifteen PPO agents in [[google-research-football|GFootball]] by [[trueskill|TrueSkill]] and correlate play-style statistics against competitiveness:

- **Total shots: $r = 0.77$, $p = 0.001$** — the second-strongest positive correlate.
- **Total passes: $r = -0.50$, $p = 0.061$.**
- Successful shot %: $r = 0.68$.

**Agents that win shoot more and pass less.** Yeung & Fujii, on real football, find the reverse prescription: against a blocking defender, passing is worth 0.2456 and shooting 0.0866.

Three readings, and no held source distinguishes them:

| Reading | Implication |
|---|---|
| **1. GFootball's shot model is too generous** | Shooting genuinely *is* optimal in the simulator and not in football — a fidelity finding, consistent with [[domain-adaptation]] |
| **2. Both hold in their domains** | The divergence measures the domain gap on a dimension nobody chose to measure |
| **3. Neither axis means what it says** | Scott et al.'s TrueSkill ordering carries an unresolved anomaly; Yeung & Fujii's equilibrium assumes a best-responding defender |

**Reading 1 is the most useful if true, because it is checkable**: compare simulated and real shot conversion by pitch location. Nobody has, and it is the sharpest cheap test the vault has identified for GFootball's fidelity. See [[reinforcement-learning]].

⚠️ Reading 3 deserves weight. Scott et al. report that agents trained against the **easy** bot rank 1, 2 and 3, above agents trained four times longer against the hard bot. They call this counter-intuitive and defer it — and **every correlation above is measured against that axis.**

**Two Fujii-co-authored papers, pointing opposite ways on the vault's only prescriptive finding, with no cross-citation.**

## The Fourth Objection, Measured

[[action-supervision]] adds an [[imitation-learning|imitation]] loss weighted by $\lambda_1$. Nakahara et al. state both failure modes: too little and counterfactual values go unlearned; too much and the model **overfits to actions actually taken and stops distinguishing counterfactuals**.

> ### `optimality-gap-is-tunable`
> **In any framework that regularises a value function toward observed behaviour, the measured gap between observed and optimal action is partly a function of the regularisation weight, not of the players.**
> ^[generated: **strengthened 2026-08-07** — Fujii et al. measure the reward/imitation trade-off directly. Declared on [[action-supervision]]. rests-on: source:nakahara-lambda-tradeoff, source:fujii-reward-dtw-tradeoff]

**And the dial has a second knob.** Fujii et al. observe the model "first learned the ability to maximize a reward and then learned the reproducibility at the expense of the reward" — so position on the frontier moves with **training time** too. Even a paper reporting and sweeping $\lambda_1$ would not have pinned the gap down. See [[imitation-reward-tradeoff]] and [[free-parameters-load-bearing]].

**Every method here needs some assumption to fill the counterfactual arm.** Fernández et al. use a learned surface, Yeung & Fujii defender-removal simulations, Nakahara et al. an imitation prior, Scott et al. a simulator's physics. The gap's size depends on that assumption in all four; only the third makes the dependence numerical.

## The Objections That Have To Be Answered First

**1. Average-player models applied to specific players.** [[expected-goals|xG]], [[xsot|xSOT]] and EPV are player-agnostic. An elite finisher *should* shoot more than an average-player equilibrium prescribes, and would be scored as over-shooting for doing the right thing.

Nakahara et al. are a partial exception — ten agents, one per attacking slot, so role-conditional rather than identity-conditional.

**2. Execution difficulty is unmodelled.** All value the *intent* of the best option, not the difficulty of executing it. This is the [[intent-vs-outcome-valuation|intent/outcome distinction]] — the gap may be the price of reliability. Nakahara et al. inherit it cleanly: their [[action-space-design|8-direction discretisation]] encodes no dynamics constraints, so some "available" counterfactual actions are not available.

**3. Equilibrium assumes a rational opponent.** If defenders systematically fail to block, the shooter's true best response shifts toward shooting. The mirror problem afflicts Nakahara et al.: [[multi-agent-reinforcement-learning|independent]] agents mean a counterfactual run is evaluated with 21 players frozen.

> **Partial answer.** Fujii et al. test whether *centralised* MARL fixes anything (CDS, Li et al. 2021) and find it does not — evidence that independence is not the binding constraint on the forward approach, though silent on whether frozen-world counterfactuals distort valuation.

## Proposed Test

1. **Does the gap predict anything?** Correlate per-player or per-team gap with goals, points, or [[predictive-validity|next-match]] outcomes. Decisive and cheapest.
2. **Sweep $\lambda_1$ and plot the gap.** One retrain per point. **Still unrun** — the paper expected to run it did not.
3. **Checkpoint across training and plot the same thing.** Nearly free; the checkpoints exist.
4. **Compare simulated against real shot conversion by location.** New with the Scott ingest, and the way to settle reading 1 above.
5. **Condition on player skill.** If the over-shooting gap shrinks for elite finishers, objection 1 is answered.
6. **Test the execution confound** via the [[intent-vs-outcome-valuation|I-VAEP/O-VAEP]] split applied to decisions.
7. **Check the defender's side**, and recompute Nakahara-style counterfactuals with a [[trajectory-prediction|predicted]] rather than frozen defensive response.
8. **Reconcile the granularities** — do players who target badly also select badly?

Steps 2, 3, 4 and 7 need no new data.

## What Would Change

**If the gap predicts nothing** — the clearest case in the vault of a metric measuring its own consistency.

**If it predicts outcomes** — football has a measurable coaching inefficiency, and the literature's first actionable output.

**If only after conditioning on skill** — most likely, and most useful.

**If the gap moves with $\lambda_1$ or training steps** — the RL family cannot address this question without an external criterion.

**If simulated and real shot conversion differ sharply by location** — Scott et al.'s finding is about GFootball rather than about football, and the contradiction dissolves into a fidelity result. This is now the branch worth resolving first, because it is the cheapest.

## Why Nobody Has Done It

The papers treat the gap as an *illustration* rather than a result. Nakahara et al. do not report it at all, despite their framework producing it more directly than either — **a model outputting the Q-value of every action for every player at every timestep has the gap sitting in its output tensor, one subtraction away.**

The convergence, and now the contradiction, are only visible from outside — from holding these papers together. That is what a synthesis can produce and a single paper cannot, and it is also why nobody has tested it: **no one author owns the claim.**

## See Also

- [[policy-modelling]] · [[game-theory]] · [[xsot]] · [[probability-surface]] · [[action-space-design]] · [[proximal-policy-optimization]]
- [[action-supervision]] · [[imitation-reward-tradeoff]] · [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[temporal-difference-learning]] · [[imitation-learning]] · [[domain-adaptation]] · [[agent-based-simulation]]
- [[intent-vs-outcome-valuation]] · [[predictive-validity]] · [[construct-validity]] · [[counterfactual-baseline]] · [[counterfactual-simulation]] · [[trajectory-prediction]] · [[trueskill]] · [[google-research-football]]
- [[free-parameters-load-bearing]] · [[action-valuation]] · [[action-valuation-frameworks-compared]]
- [[expected-value-possession-framework|Fernández et al.]] · [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] · [[ai-football-reinforcement-learning|Scott et al.]]
