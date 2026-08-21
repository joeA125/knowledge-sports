---
title: "Attention Mechanism"
type: concept
tags: [attention, deep-learning, sequence-modelling, interpretability, event-prediction, sports-analytics, model-selection]
sources: [raw/papers/attention-is-all-you-need.md, raw/papers/neural-machine-translation.md, raw/papers/pointer-networks.md, raw/papers/transformer-point-process-football-event-modelling.md]
confidence: 0.85
provenance:
  extracted: 62%
  inferred: 28%
  generated: 7%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-05-07
updated: 2026-08-14
---

# Attention Mechanism

Maps a query and a set of key–value pairs to an output: a weighted sum of the values, where the weights come from a compatibility function between the query and each key.

**Self-attention** relates positions within a single sequence, and in the [[transformer]] replaces recurrence entirely. Compatibility is computed as a scaled dot product, run in parallel across several projected subspaces.

> **Scope note.** The variant taxonomy — additive versus multiplicative, scaling, multi-head projection, pointer mechanisms — lives in the general vault. This page keeps what football event modelling depends on.

## Why This Page Is Football-Side

[[nmstpp|NMSTPP]] is a Transformer-based point process over match events. Its encoder is self-attention over a 40-event history window, which makes attention the mechanism deciding **how much each past event contributes to the prediction of the next one.**

That is a different use from language. In translation, attention aligns words. Here it weights *history*, and the weights are directly interpretable as a claim about how far back football events remain relevant.

## Attention Weights as a Hyperparameter Diagnostic

The most portable idea on this page, and it comes from the football source.

[[transformer-point-process-football-event-modelling|Yeung et al. (2023)]] read the last row of the self-attention matrix — the contribution of each historical event to the final history representation — to test whether their 40-event window is **the right size**. Weights lay between 0.01 and 0.06 with **no trend across the window**, which they take as evidence it is neither too short (weights would pile up at the recent end, implying more history would help) nor too long (distant events would receive negligible weight).

> ### `attention-weights-diagnose-window-length`
> **Where a fixed-size context window feeds a self-attention encoder, the attention distribution over that window is a cheap test of whether the window is correctly sized — a trend toward either end indicates a misspecified horizon.**
> ^[generated: the source applies this to its own model; the general form is drawn here. rests-on: source:yeung-attention-window-diagnostic]

This deserves attention because of what surrounds it. [[free-parameters-load-bearing]] catalogues **sixteen asserted parameters with no sensitivity analysis**, and horizon parameters are the largest category — VDEP's $k = 5$, C-OBSO's 4 s, SOG's 3 s window, Nakahara's 300-frame cap. **Yeung et al. are the only case in the vault where a horizon parameter is checked against evidence at all**, and the check costs nothing beyond reading a matrix the model already computes.

A caveat worth keeping: attention weights are widely used as [[interpretability|interpretability]] evidence and the wider literature is divided on whether they constitute explanation. The diagnostic use here is more defensible than causal claims, since it asks only whether the model *distributes* attention across the window, not what any individual weight means.

## The Tokens Are Arbitrary

Attention is agnostic about what a "sequence" contains. In this vault it runs over football match events ([[nmstpp]]), image patches for retrieval ([[siamese-network]] in calibration), and — through the same mathematics — over graphs.

Self-attention over a set with no positional encoding **is** [[message-passing]] on a fully-connected graph, with attention-weighted rather than summed aggregation. That places attention alongside the [[graph-neural-network|GNN]] used in [[c-obso|C-OBSO]] as one of three answers to the 22-players-have-no-canonical-order problem. See [[action-space-design]].

## See Also

- [[transformer]] · [[nmstpp]] · [[neural-temporal-point-process]] · [[encoder-decoder-bottleneck]] · [[event-prediction]]
- [[message-passing]] · [[graph-neural-network]] · [[trajectory-prediction]] · [[c-obso]]
- [[interpretability]] · [[free-parameters-load-bearing]] · [[model-selection]] · [[siamese-network]]
- [[calvin-yeung]] · [[keisuke-fujii]]
- [[transformer-point-process-football-event-modelling|NMSTPP Summary]] · [[attention-is-all-you-need|Transformer Summary]]
