---
title: "Regularization"
type: concept
tags: [regularization, deep-learning, machine-learning, training-technique, model-selection, reinforcement-learning, auxiliary-loss]
sources: [raw/papers/rnn-regularisation.md, raw/papers/attention-is-all-you-need.md]
confidence: 0.8
provenance:
  extracted: 35%
  inferred: 50%
  generated: 12%
  imported: 0%
  ambiguous: 3%
lifecycle: draft
created: 2026-07-07
updated: 2026-08-14
---

# Regularization

Any technique that improves generalisation to unseen data, typically by discouraging a model from fitting noise. It trades a small increase in training error for a larger reduction in the train–test gap.

Weight penalties ($L_1$, $L_2$), stochastic methods, target softening, early stopping, data augmentation and noise injection are the standard catalogue.^[inferred: general catalogue beyond the held sources]

> **Scope note.** The general treatment — dropout and its variants, label smoothing, normalisation as implicit regularisation — lives in the general vault. This page keeps what the football frameworks actually do, which turns out to be a narrower and more interesting question.

## Why It Matters Disproportionately Here

Football datasets are **small**. Not small by the standards of this literature — small in absolute terms:

| Framework | Training set |
|---|---|
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 1,669 possession sequences |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | 1,121 sequences pre-training; **16 target-RL episodes** |
| [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | 412 shot-ending sequences |

At that scale, regularisation is not a refinement — it is the thing standing between the model and memorisation. Both RL papers say so explicitly, justifying their choice by the size of the demonstration set rather than by any property of the task.

## Two Papers, Same Group, Opposite Penalties

| | Penalty | Stated reason |
|---|---|---|
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | $L_1$ on weights and biases | Overfitting on "the relatively small demonstration dataset" |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | $L_2$ | Same data scale, same justification |

$L_1$ and $L_2$ are not interchangeable — $L_1$ drives weights to exactly zero and induces sparsity, $L_2$ shrinks smoothly and keeps everything. **Overlapping authors, the same J-League dataset, the same stated rationale, opposite choices, no discussion.**

Neither reports what the penalty weight does to results. Fujii et al. do not report $\lambda_2$ at all. See [[free-parameters-load-bearing]], where this sits alongside fourteen other asserted parameters.

## Regularisation That Is Not Really Regularisation

The most consequential penalty in the football RL corpus is not a weight penalty at all.

[[action-supervision]] adds a cross-entropy term pulling learned $Q$-values toward the actions humans actually took. Formally it regularises. Substantively it **imports an assumption about human competence**, and its weight $\lambda_1$ controls how much apparent suboptimality survives into the results.

> ### `not-all-penalties-are-regularisers`
> **A penalty that shrinks parameters constrains model complexity. A penalty that pulls a model toward observed behaviour constrains its conclusions. The two are written identically in the loss and mean entirely different things.**
> ^[generated: no source draws this distinction. Bears on `optimality-gap-is-tunable`, declared on [[action-supervision]]. rests-on: source:nakahara-l1-justification, source:nakahara-lambda-tradeoff]

That distinction is why $\lambda_2$ being unreported is a minor gap and $\lambda_1$ being unreported is a substantive one.

## Architecture as a Substitute

Worth noting because the football papers reach for it instead. [[gated-recurrent-unit|Nakahara et al.]] use a single 64-unit GRU — smaller than the task would suggest, precisely because a larger network at 1,669 sequences would overfit whatever the penalty. See [[handcrafted-features-rule]] and [[theory-based-modelling]]: **at football data scale, capacity limits and structural priors do work that regularisation alone cannot.**

## See Also

- [[action-supervision]] · [[imitation-reward-tradeoff]] · [[free-parameters-load-bearing]] · [[model-selection]]
- [[gated-recurrent-unit]] · [[lstm]] · [[deep-q-network]] · [[temporal-difference-learning]] · [[transformer]]
- [[theory-based-modelling]] · [[handcrafted-features-rule]] · [[representation-learning]]
- [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]]
