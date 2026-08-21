---
title: "Neural Machine Translation by Jointly Learning to Align and Translate — Source Summary"
type: summary
tags: [deep-learning, attention, sequence-modelling, rnn, encoder-decoder-bottleneck, alignment]
sources: [raw/papers/neural-machine-translation.md]
confidence: 0.9
provenance:
  extracted: 82%
  inferred: 13%
  generated: 3%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Neural Machine Translation by Jointly Learning to Align and Translate

**Bahdanau, Cho & Bengio**, ICLR 2015; arXiv:1409.0473.

> **Held here because [[gated-recurrent-unit|the GRU]] and the [[attention-mechanism|attention mechanism]] both originate in it**, and both are load-bearing for football models — Nakahara et al.'s value network is a GRU, and NMSTPP's encoder is self-attention. The machine-translation results and the authors live in the general vault.

## The Contribution

The first [[attention-mechanism|attention mechanism]]. It solves the [[encoder-decoder-bottleneck]] — compressing an entire source sentence into a single fixed-length vector — by letting the decoder attend to different parts of the source at each generation step:

$$c_i = \sum_j \alpha_{ij} h_j, \qquad \alpha_{ij} = \text{softmax}(e_{ij})$$

where $e_{ij}$ comes from a learned alignment network scoring how well source position $j$ matches decoder state $s_{i-1}$. A **soft alignment** — fully differentiable and trained end-to-end, unlike traditional hard alignments.

The decoder is a [[gated-recurrent-unit|GRU]], which is where that architecture enters this vault.

## The Result That Matters

| Model | BLEU (No UNK) |
|---|---|
| RNNencdec-50 (fixed vector) | 26.71 |
| **RNNsearch-50 (attention)** | **34.16** |

More importantly: **RNNsearch shows no degradation on sentences of 50+ words, while the fixed-vector encoder collapses after roughly 30.** That length-robustness is the paper's actual finding — attention removed a bottleneck rather than merely adding capacity.

## Two Things Football Inherited

**The GRU.** [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] use a single 64-unit GRU as the value network for ten per-player RL agents. At 1,669 training sequences, the GRU's smaller parameter count against an [[lstm|LSTM]] is the reason for the choice, not a footnote. See [[gated-recurrent-unit]].

**Attention — and then its inversion.** Bahdanau introduced attention to *remove* a fixed-length bottleneck. [[nmstpp|NMSTPP]] uses attention to make a bottleneck **lossy in the right places**, deliberately compressing 40 events into 31 dimensions because only one next event is being predicted.

> The same mechanism, serving the opposite architectural purpose. Recorded as `attention-can-improve-compression-not-only-replace-it` on [[encoder-decoder-bottleneck]].

## See Also

- [[gated-recurrent-unit]] · [[lstm]] · [[attention-mechanism]] · [[encoder-decoder-bottleneck]] · [[transformer]]
- [[nmstpp]] · [[neural-temporal-point-process]] · [[event-prediction]] · [[trajectory-prediction]]
- [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al. Summary]] · [[transformer-point-process-football-event-modelling|NMSTPP Summary]]
