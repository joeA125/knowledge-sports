---
title: "Attention Is All You Need — Source Summary"
type: summary
tags: [transformer, attention, deep-learning, architecture, sequence-modelling, event-prediction]
sources: [raw/papers/attention-is-all-you-need.md]
confidence: 0.9
provenance:
  extracted: 80%
  inferred: 14%
  generated: 4%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-07
updated: 2026-08-14
---

# Attention Is All You Need

**Vaswani et al.**, [[google-brain]] and [[google-research]], NeurIPS 2017.

> **Held here because [[nmstpp|NMSTPP]] depends on it.** This summary is deliberately lean — the full architectural treatment, the authors, and the paper's impact on language modelling live in the general vault. What follows is what football event modelling needs.

## The Contribution

The first sequence transduction model built entirely on [[attention-mechanism|attention]], eliminating recurrence and convolution. Stacked encoder and decoder layers, each combining multi-head self-attention with a position-wise feed-forward network, wrapped in residual connections and layer normalisation. Sequence order is injected by sinusoidal positional encoding, since attention itself is permutation-invariant.

Base model: $d_{\text{model}} = 512$, $d_{ff} = 2048$, 6 layers, 8 heads, ~65M parameters. Trained to 28.4 BLEU on WMT14 English–German in 3.5 days on 8 P100s.

## Why Self-Attention

The paper's argument, on three axes:

| | Self-attention | Recurrence |
|---|---|---|
| Complexity per layer | $O(n^2 \cdot d)$ | $O(n \cdot d^2)$ |
| Sequential operations | **$O(1)$** | $O(n)$ |
| Path length between positions | **$O(1)$** | $O(n)$ |

Self-attention wins when $n < d$ — sequences shorter than the model dimension, which holds for most NLP and, as it happens, for football event windows.

## What Football Took From It

**The encoder, and only the encoder.** [[nmstpp|NMSTPP]] uses a Transformer encoder to compress a 40-event match history into a 31-dimensional vector, from which it predicts the next event's time, zone and action type. No decoder, no generation — the architecture is used purely as a history compressor. See [[encoder-decoder-bottleneck]].

**And the choice was closer than the paper's reputation suggests.** On football event data, [[transformer-point-process-football-event-modelling|Yeung et al.]] measured a Transformer encoder against a uni-LSTM:

| Encoder | Total loss | Training time | Parameters |
|---|---|---|---|
| Uni-LSTM | **4.51** | 129 min | **4K** |
| Transformer | 4.57 | **47 min** | 13K |

The Transformer **lost on loss** and was chosen for a 2.7× speedup. At a 40-event window the parallelisation advantage is worth far less than at document scale. See [[lstm]].

**Permutation invariance turns out to be the feature, not the bug.** In multi-agent sport, the property positional encoding exists to patch is exactly what you want — 22 players have no canonical order. Self-attention with no positional encoding *is* [[message-passing]] on a fully-connected graph, which puts the Transformer in the same family as the [[graph-neural-network|GNN]] behind [[c-obso|C-OBSO]]. See [[action-space-design]].

## Ablation Findings Worth Retaining

- A single attention head costs 0.9 BLEU; 32 heads also degrades slightly.
- Reducing $d_k$ hurts, suggesting dot-product compatibility needs dimensionality.
- **Learned positional embeddings perform nearly identically to sinusoidal ones** — relevant because NMSTPP had to make the same choice for event positions.

## See Also

- [[transformer]] · [[attention-mechanism]] · [[encoder-decoder-bottleneck]] · [[lstm]] · [[gated-recurrent-unit]]
- [[nmstpp]] · [[hpus]] · [[neural-temporal-point-process]] · [[event-prediction]] · [[seq2event]]
- [[message-passing]] · [[graph-neural-network]] · [[action-space-design]] · [[large-event-model]]
- [[google-brain]] · [[google-research]] · [[calvin-yeung]]
- [[transformer-point-process-football-event-modelling|NMSTPP Summary]]
