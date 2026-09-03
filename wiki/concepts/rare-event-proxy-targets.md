---
title: "Rare-Event Proxy Targets"
type: concept
tags: [proxy-target, class-imbalance, machine-learning, statistics, evaluation, sports-analytics, defensive-valuation, predictive-validity, game-theory, reliability, event-stream-data]
sources: [raw/papers/football_defence_evaluation.md, raw/papers/transformer-point-process-football-event-modelling.md, raw/papers/epv_control_and_duel_skills_football.md, raw/papers/optimal_football_decisions_shot_taking_situations.md, raw/papers/stats_reliability_football_champdas.md, raw/papers/metric-stability-elite-football.md]
confidence: 0.85
provenance:
  extracted: 50%
  inferred: 45%
  ambiguous: 5%
lifecycle: reviewed
created: 2026-07-27
updated: 2026-08-29
---

# Rare-Event Proxy Targets

When the outcome you care about is too rare to learn from, train on a **frequent event on its causal path** instead. A modelling move that appears independently in several vault sources and deserves naming, because it is usually presented as a domain trick rather than the general method it is.

## The Problem

Supervised learning needs positives. When the positive class is a fraction of a percent, three things go wrong at once:

- **Too little signal.** A few hundred positives cannot support a rich feature space.
- **Degenerate optima.** Predicting "no" always is nearly always right, and gradient descent finds that.
- **Misleading metrics.** Accuracy, Brier and AUC are all inflated by true negatives. See [[class-imbalance-evaluation]].

Football is a severe case. In the [[football-defence-evaluation-vdep|VDEP dataset]], 45 matches produced **106 goals** across 97,335 events. The consequence is measured, not asserted: [[vaep]]'s conceding classifier achieves **F1 = 0.000** — it identifies no true positives whatsoever.

## The Move

Replace the target with a frequent correlate:

| Framework | Rare target abandoned | Frequent proxy adopted | Gain |
|---|---|---|---|
| [[vdep]] | Goals conceded | Ball recovery, effective attack | ~90×, ~35× more frequent |
| [[xsot\|Yeung & Fujii]] | Goal | **Shot on target** | ~1 in 3 shots, not ~1 in 10 |
| [[pass-carry-reward\|Shelopugin]] | "Possession ended in a goal" (binary) | Accumulated future xG | Real-valued at every shot |
| [[hpus]] | Goals | Possession utilisation from event dynamics | **No goal data at all** |
| [[obso\|Spearman]] | Goal, as a *rating* | Positional opportunity | Every frame, not every goal |

[[epv-control-duel-skills-football|Shelopugin]] states the reasoning explicitly, rejecting a binary goal target as too sparse and overfitting-prone. [[optimal-decisions-shot-taking-situations|Yeung & Fujii]] frame theirs as **the minimum requirement of a good shot** — if it is not on target it cannot score.

## What a Proxy Change Does to the Model

An underrated consequence, and Yeung & Fujii supply the cleanest illustration: **changing the target changes which parts of the world matter.**

Because their payoff is shot-*on-target* rather than goal, a save still counts as success — so **the goalkeeper drops out of the shot-block model entirely** and is filtered from the defender set. In [[c-obso|C-OBSO]], whose target is a goal, the goalkeeper is included and weighted double.

Same sport, same geometry, opposite treatment of one player, entirely because the target moved. A proxy is not a neutral substitution for the outcome; it reorganises the model around itself.

## What Makes a Proxy Good

Three conditions, and the second is the one that gets skipped.

**1. Frequent enough to learn from.** The point of the exercise.

**2. Causally upstream of the real outcome, not merely correlated.** A proxy that co-occurs with goals for incidental reasons will be optimised in ways that do not produce goals. Ball recovery, territorial penetration and shot-on-target all sit on the causal path; "passes completed" correlates with winning largely through confounding.

**3. Not so far upstream that it measures something else.** Push far enough and the proxy becomes its own construct with its own validity question.

### 4. Reliably observable — the condition nobody states

> **Added 2026-08-29** on ingest of [[champdas-validity-reliability|Gong et al. (2019)]]. See [[operator-reliability]].

The trade above is always presented as **statistical**: swap a rare target for a frequent one and buy positives. It is also a trade in **measurement quality**, and that half has gone unremarked.

Operator agreement on manual event coding is not uniform across event types:

| Event group | Inter-operator ICC | Typical error |
|---|---|---|
| Passing | **1.00** | **0.01** |
| Attacking | 0.99–1.00 | 0.06–0.08 |
| **Defending and goalkeeper** | **0.93, 0.95** | **0.24, 0.29** |

**[[vdep|VDEP]]'s proxies live in the worst cell.** Ball recovery is built from tackles, interceptions and clearances — precisely the actions two trained analysts agree on least.

> ### `proxy-substitution-trades-statistical-power-for-measurement-noise`
> **A frequent proxy is not merely more numerous than the rare outcome; it may also be harder to observe consistently. Goals are unambiguous and near-perfectly coded — a goal is the one football event nobody misclassifies. The events substituted for them are defined by judgement, and coding agreement is measurably worse on exactly those. The move buys positives and pays in per-observation noise, and no source prices the second half.**
> ^[generated: no source connects coding-agreement figures to proxy selection. rests-on: source:gong-table5-defensive-cell, source:vdep-proxy-frequencies]

⚠️ **Not a refutation, and the direction of the net effect is unknown.** A typical error of 0.29 is still "small" on Hopkins' scale, and 90× more observations is a large gain against a modest per-observation cost. The point is that **the arithmetic has never been done**, and the trade has been presented as free.

### ⚠️ The Second Leg: Those Events Are Also the Least Stable

> **Added 2026-08-29** on ingest of [[year-to-year-metric-stability-football|Shaikh (2026)]], which measures a **different property at a different layer** and finds the same ordering.

| Event group | Coding agreement (Gong) | Year-to-year stability (Shaikh) |
|---|---|---|
| Passing | **ICC 1.00, TE 0.01** | **$r = 0.824$** |
| Attacking | ICC 0.99–1.00 | $r$ 0.52–0.73 |
| **Defending** | **ICC 0.93–0.95, TE 0.24–0.29** | **$r$ 0.399–0.476** |

Tackles won (0.414), blocks (0.399) and interceptions (0.476) are the three **least stable** combinations in a dataset of 8,207 player-season pairs — and, separately, the worst-coded cell in a reliability study that never looked at stability.

> ### `defensive-actions-degrade-at-every-measurement-layer`
> **Defensive event counts are simultaneously the hardest to code consistently and the least stable year to year. The difficulties are not independent — they compound in the metrics built on them.**
> ^[generated: declared on [[year-to-year-metric-stability-football]]. rests-on: source:gong-table5-defensive-cell, source:shaikh-defensive-volume-low-stability]

**So proxy substitution pays twice.** [[vdep|VDEP]] escapes goal sparsity by moving to ball recoveries — events 90× more frequent, harder to code, *and* far less repeatable across seasons.

⚠️ **The third cost is the one that matters most, and it is not noise at all.** Shaikh's explanation for defensive instability is **team context**: a defender's tackle count depends on how often the team defends, how high the line sits, and whether teammates win the ball first. That is not error to be averaged away — **it is the metric measuring the team rather than the player**, which is a [[construct-validity]] problem rather than a reliability one.

A proxy chosen for frequency can therefore be *systematically* contaminated rather than merely noisy, and no amount of additional data fixes it.^[generated: the distinction between noise and contamination in proxy selection is drawn here. rests-on: source:shaikh-defensive-team-context]

It also predicts an ordering: among the proxies tabled above, **shot on target should be the cleanest** — a discrete, refereeable outcome — and **ball recovery the noisiest.** [[hpus|HPUS]] sits oddly, being built from possession dynamics rather than from any single coded event type.

⚠️ Note the coding figures come from **one system on one match**, and from a provider none of the vault's sources use. They bound nothing; they raise a question. See [[reliability-layers]].

## The Evidence That It Works

Three independent results, all pointing the same way:

- [[hpus]] uses **no goal or shot-outcome data at any stage** yet correlates 0.92 with season xG and −0.78 with league position, against xG's −0.81.
- [[xsot|xSOT]] correlates **0.58** with average goals against [[expected-goals|xG]]'s **0.46** across World Cup 2022 teams.
- [[obso|OBSO]] predicts a player's next-match goals (0.26) better than his shots (0.17) or goals (0.12) do.

The pattern is consistent: **a metric built on a denser proxy can outperform one built on the outcome itself**, because the outcome is a noisy realisation of the process both are trying to measure. See [[predictive-validity]].

## The Cost

**The proxy becomes the definition.** [[vdep]] does not measure defensive quality; it measures recovery-and-penetration performance, which is a *hypothesis about* defensive quality. If the hypothesis is wrong, the metric is confidently wrong.

**Weighting is arbitrary.** Combining proxies needs relative weights, and there is rarely a principled source. VDEP's $C \approx 3.9$ comes from the observed frequency ratio — which encodes *how often* each event happens, not *how much each matters*. Frequency is available; importance is not.

**Validation gets harder.** You can no longer check against the real outcome without reintroducing the sparsity you escaped. [[predictive-validity]] against downstream results becomes the main available test.

**Goodhart risk.** A proxy optimised as a target drifts from the thing it proxied. Acute in sport, where a team told to maximise ball recoveries can do so by conceding territory cheaply.

**Noisier per observation.** See condition 4 above — the events substituted in are the ones coders agree on least.

## Elsewhere

Standard wherever the outcome of interest is rare and costly: surrogate endpoints in medicine (tumour shrinkage for survival), near-misses in reliability engineering, leading indicators in safety, clicks instead of purchases in advertising.

The medical literature is the cautionary one: surrogate endpoints have repeatedly been validated, adopted, and later found not to predict the outcome that mattered. Condition 2 above is where those failures live.

## See Also

- [[class-imbalance-evaluation]] · [[vdep]] · [[xsot]] · [[defensive-valuation]]
- [[expected-goals]] · [[hpus]] · [[obso]] · [[vaep]] · [[pass-carry-reward]]
- [[predictive-validity]] · [[probability-calibration]] · [[game-theory]]
- [[football-defence-evaluation-vdep|VDEP Summary]] · [[optimal-decisions-shot-taking-situations|Yeung & Fujii Summary]]
