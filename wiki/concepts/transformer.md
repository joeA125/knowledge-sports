---
title: "Transformer"
type: concept
tags: [transformer, architecture, deep-learning, sequence-modelling, attention, event-prediction, sports-analytics]
sources: [raw/papers/attention-is-all-you-need.md]
confidence: 0.85
provenance:
  extracted: 55%
  inferred: 32%
  generated: 10%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-05-07
updated: 2026-08-14
---

# Transformer

A sequence architecture built entirely on [[attention-mechanism|attention]], dispensing with recurrence and convolution ([[attention-is-all-you-need|Vaswani et al., 2017]]).

Stacked encoder and decoder layers, each combining multi-head self-attention with a position-wise feed-forward network, wrapped in residual connections and layer normalisation. Order is supplied by positional encoding, since attention itself is permutation-invariant. The base model runs $d_{\text{model}} = 512$, 8 heads, ~65M parameters.

Three properties matter: it **parallelises** across sequence positions where recurrence cannot, any two positions connect in $O(1)$ operations, and performance scales with size and data.

> **Scope note — this page is deliberately lean.** The architecture's components, variants, and its role in language modelling are covered in the general vault. What follows is what football event modelling depends on.

## Why This Page Is Football-Side

[[nmstpp|NMSTPP]] — the vault's event-forecasting model — uses a Transformer encoder to compress a 40-event match history into a 31-dimensional vector, from which it predicts the next event's inter-event time, zone and action type. [[hpus|HPUS]], the possession-value metric derived downstream, rests on those forecasts.

So the dependency chain is short and real: **remove the Transformer and NMSTPP has no history encoder.** That is the whole reason this page stays.

## The Choice Was Closer Than the Field Suggests

Worth recording, because it cuts against the architecture's reputation. [[transformer-point-process-football-event-modelling|Yeung et al.]] tested a Transformer encoder against a uni-directional [[lstm|LSTM]] on the same football event data:

| Encoder | Total loss | Training time | Parameters |
|---|---|---|---|
| Uni-LSTM | **4.51** | 129 min | **4K** |
| Transformer | 4.57 | **47 min** | 13K |

**The Transformer lost on loss and was chosen anyway**, for a 2.7× training speedup. At football data scale — 40-event windows, not 4,000-token documents — its central advantage buys less than it does in language. See [[lstm]] and `recurrence-persists-where-sequences-are-short`.

## Permutation Invariance Is the Deeper Connection

Attention's order-independence is usually described as a limitation, patched by positional encoding. In multi-agent sport it is the **feature**.

Self-attention over a set with no positional encoding is [[message-passing]] on a fully-connected graph, with attention-weighted rather than summed aggregation. That places the Transformer in the same family as the [[graph-neural-network|GNN]] underpinning [[c-obso|C-OBSO]] — two answers to the same problem, that **22 players have no canonical order.** The third answer is [[vdep|VDEP's]]: sort them by distance to the ball and accept the loss. See [[action-space-design]].

## Where It Appears Here

| Framework | Use |
|---|---|
| [[nmstpp]] | History encoder over match events |
| [[scoutgpt]] · [[eventgpt]] | Generative event-sequence models |
| [[large-event-model]] | The general framing of match-as-language |

## See Also

- [[attention-mechanism]] · [[encoder-decoder-bottleneck]] · [[lstm]] · [[gated-recurrent-unit]] · [[neural-temporal-point-process]]
- [[nmstpp]] · [[hpus]] · [[scoutgpt]] · [[eventgpt]] · [[large-event-model]] · [[seq2event]]
- [[message-passing]] · [[graph-neural-network]] · [[action-space-design]] · [[tokenization]] · [[player-embedding]]
- [[google-brain]] · [[google-research]] · [[calvin-yeung]]
- [[attention-is-all-you-need|Source Summary]] · [[transformer-point-process-football-event-modelling|NMSTPP Summary]]
