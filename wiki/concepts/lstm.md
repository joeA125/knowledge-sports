---
title: "Long Short-Term Memory"
type: concept
tags: [deep-learning, rnn, lstm, architecture, sequence-modelling, event-prediction, sports-analytics, model-selection]
sources: [raw/papers/rnn-regularisation.md, raw/papers/neural-machine-translation.md, raw/papers/transformer-point-process-football-event-modelling.md]
confidence: 0.85
provenance:
  extracted: 58%
  inferred: 32%
  generated: 6%
  imported: 0%
  ambiguous: 4%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Long Short-Term Memory (LSTM)

A recurrent architecture (Hochreiter & Schmidhuber, 1997) that learns long-range dependencies by maintaining an explicit **cell state** and gating what enters, leaves, and is read from it.

$$c_t = f \odot c_{t-1} + i \odot g, \qquad h_t = o \odot \tanh(c_t)$$

with input, forget, output and modulation gates computed from the current input and previous hidden state. The cell state gives gradients a path across many timesteps with minimal decay, which is what addresses the vanishing-gradient problem.

> **Scope note.** Gate-level detail, variants, and the recurrent-dropout literature live in the general vault. This page keeps the encoder-choice question, which is live in football event modelling.

## Why This Page Is Football-Side

Every sequence model in this vault has to choose a history encoder, and the choice is not settled. The [[transformer]] displaced recurrent encoders for most long-sequence tasks, **but the trade-off is narrower than the displacement suggests.**

[[transformer-point-process-football-event-modelling|Yeung et al. (2023)]] give the cleanest measurement available, on football event sequences with everything else held fixed:

| Encoder | Total loss | Training time | Parameters |
|---|---|---|---|
| **Uni-LSTM** | **4.51** | 129 min | **4K** |
| Transformer | 4.57 | **47 min** | 13K |

**The LSTM wins on loss and loses on speed** — 2.7× slower to train, with a third of the parameters. [[seq2event|Simpson et al. (2022)]] report the same pattern, and it recurs across the [[neural-temporal-point-process|NTPP]] literature.

> ### `recurrence-persists-where-sequences-are-short`
> **The Transformer's advantage is parallelisation across long sequences. Where sequences are short, training budget is tight relative to inference budget, or parameter count is constrained by data scale, recurrent encoders remain competitive or better — which describes most football event modelling.**
> ^[generated: the sources report the measurements; the generalisation is drawn here. rests-on: source:yeung-encoder-comparison, source:simpson-seq2event-encoder]

That claim connects to [[regularization]] and [[handcrafted-features-rule]]: at football data scale, **capacity is rarely the binding constraint**, so an architecture that parallelises better buys less than it does at language scale. A 40-event window is not a 4,000-token document.

## Where Recurrence Actually Runs Here

| Framework | Encoder | Note |
|---|---|---|
| [[nmstpp]] | Transformer, LSTM tested | Transformer chosen for speed despite worse loss |
| [[seq2event]] | LSTM | Same finding independently |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | [[gated-recurrent-unit\|GRU]], 64 units | Carries state across the TD bootstrap |
| [[trajectory-prediction\|GVRNN]] | Recurrent, per-agent | Underpins [[c-obso]] |

The Nakahara case is the interesting one: there the recurrence is not only encoding a sequence but **substituting for [[deep-q-network|DQN's]] stabiliser stack**, carrying information across the bootstrap where a target network would otherwise hold it steady. See [[gated-recurrent-unit]].

## Relation to GRU

The [[gated-recurrent-unit]] merges the forget and input gates into one update gate and combines the cell and hidden states — fewer parameters for broadly comparable performance. At football data scale that difference is not incidental; it is why Nakahara et al. use a GRU rather than an LSTM.

## See Also

- [[gated-recurrent-unit]] · [[transformer]] · [[attention-mechanism]] · [[encoder-decoder-bottleneck]] · [[neural-temporal-point-process]]
- [[nmstpp]] · [[seq2event]] · [[sig-model]] · [[event-prediction]] · [[trajectory-prediction]]
- [[regularization]] · [[handcrafted-features-rule]] · [[deep-q-network]] · [[model-selection]]
- [[transformer-point-process-football-event-modelling|NMSTPP Summary]] · [[neural-machine-translation|Bahdanau Summary]]
