---
title: "Encoder-Decoder Bottleneck"
type: concept
tags: [deep-learning, sequence-modelling, architecture, point-process, event-prediction, attention, sports-analytics]
sources: [raw/papers/neural-machine-translation.md, raw/papers/transformer-point-process-football-event-modelling.md]
confidence: 0.85
provenance:
  extracted: 62%
  inferred: 30%
  generated: 5%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Encoder-Decoder Bottleneck

The problem that arises when an encoder must compress a variable-length input sequence into a **single fixed-length vector**. As sequences lengthen, the vector cannot retain everything, and performance degrades.

[[neural-machine-translation|Bahdanau et al. (2014)]] removed it by replacing the single context vector with a position-specific one computed at each decoding step, $c_i = \sum_j \alpha_{ij} h_j$ — the origin of the [[attention-mechanism|attention mechanism]]. The [[transformer]] later eliminated recurrence entirely, leaving no fixed-length bottleneck at all.

> **Scope note.** The machine-translation history and the attention variants that resolved it live in the general vault. This page keeps the part that matters here — which is that football event modelling **puts the bottleneck back on purpose.**

## Where the Bottleneck Returns by Design

The bottleneck is a problem in *sequence-to-sequence* settings, where each output position may need different information from the input. It is **not** intrinsically a problem when there is a single thing to predict.

The [[neural-temporal-point-process|NTPP]] framework makes this explicit: its second step is to encode the history $(\vec{y}_1, \dots, \vec{y}_{i-1})$ into a fixed-size vector $\vec{h}_i$ — deliberately reintroducing exactly the compression Bahdanau removed. Since only one next event is predicted, one context vector suffices in principle.

[[nmstpp]] compresses **40 events into a 31-dimensional history vector** this way, using a Transformer encoder to do it. Attention here serves to *weight the history well while compressing it*, rather than to avoid compressing at all.

> ### `attention-can-improve-compression-not-only-replace-it`
> **Attention was introduced to remove a fixed-length bottleneck. In single-prediction settings it is instead used to make that bottleneck lossy in the right places — the same mechanism serving the opposite architectural purpose.**
> ^[generated: neither source frames it this way; drawn from reading Bahdanau's motivation against NMSTPP's construction. rests-on: source:bahdanau-context-vector, source:nmstpp-history-encoding]

## The Cost, and How NMSTPP Pays It

All three predicted components — inter-event time, zone, and action type — must share **one** history representation. Whether a per-component context vector would help is untested.

The model instead recovers dependence through its [[autoregressive-model|chain-rule factorisation]], letting later components condition on earlier *predictions* rather than on separately attended history. That is a cheaper fix and a weaker one: a component's access to history is mediated entirely through what the previous component decided.

## Is the Bottleneck Binding?

Yeung et al. supply the diagnostic: inspect self-attention weights across the history window. Concentration at either end suggests a mis-sized window. Over 40 events theirs lay between 0.01 and 0.06 with no systematic trend, read as evidence the window suits the compression.

That check is the vault's only instance of a horizon parameter tested against evidence rather than asserted. See [[attention-mechanism]] and [[free-parameters-load-bearing]].

## See Also

- [[attention-mechanism]] · [[transformer]] · [[nmstpp]] · [[neural-temporal-point-process]] · [[point-process]]
- [[autoregressive-model]] · [[event-prediction]] · [[lstm]] · [[gated-recurrent-unit]]
- [[free-parameters-load-bearing]] · [[model-selection]] · [[hpus]]
- [[transformer-point-process-football-event-modelling|NMSTPP Summary]] · [[neural-machine-translation|Bahdanau Summary]]
