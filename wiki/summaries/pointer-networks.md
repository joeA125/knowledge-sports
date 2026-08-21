---
title: "Pointer Networks — Source Summary"
type: summary
tags: [deep-learning, attention, sequence-modelling, combinatorial-optimisation, pointer-mechanism, pass-modelling]
sources: [raw/papers/pointer-networks.md]
confidence: 0.85
provenance:
  extracted: 78%
  inferred: 12%
  generated: 8%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Pointer Networks

**Vinyals, Fortunato & Jaitly**, [[google-brain]] and UC Berkeley, NeurIPS 2015.

> **Held on forward-looking grounds (D3).** No football source cites this paper. It is retained because **pass selection is structurally a pointer problem**, and no held framework has yet treated it as one. The architecture's combinatorial-optimisation results live in the general vault.

## The Contribution

Standard sequence-to-sequence models softmax over a **fixed** output vocabulary. Many problems need an output dictionary whose size depends on the input — selecting a subset of the inputs themselves.

Pointer networks solve this by using [[attention-mechanism|attention scores directly as the output distribution]] rather than as weights for blending encoder states:

$$u_j^i = v^T \tanh(W_1 e_j + W_2 d_i), \qquad p(C_i \mid C_{<i}, \mathcal{P}) = \text{softmax}(u^i)$$

The softmax has dictionary size equal to the input length, varying per instance. **Attention becomes the answer rather than a routing mechanism.**

Results: 72.6% exact accuracy on convex hull at $n=50$ against 38.9% for standard attention; near-optimal TSP tours for $n \leq 20$. Notably, when trained on the *worst* available heuristic it outperformed that heuristic — learning past its training signal.

## Why It Is Held

**A pass is a selection over a variable-size candidate set.** Ten teammates, no canonical order, exactly one chosen. That is the pointer formulation.

The vault's existing frameworks handle it differently and less naturally:

| Framework | How pass destination is modelled |
|---|---|
| [[expected-value-possession-framework\|Fernández et al.]] | A [[probability-surface\|surface]] over pitch *locations* — not over players |
| [[pass-probability-model\|Spearman et al.]] | Physical intercept and control times, per receiver |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | **One** undifferentiated "pass" action among 14 |

Fernández et al.'s surface is the closest, and it answers *where* rather than *to whom* — which is why it can say a pass should have gone elsewhere but nothing about whether to pass at all. See [[action-space-design]].

> ### `pass-selection-is-a-pointer-problem`
> **Selecting a receiver from a variable-size set of teammates has the exact structure pointer networks were built for, and no held framework models it that way. Location surfaces and per-action discretisations are both workarounds for an output-space problem that has a known solution.**
> ^[generated: no source connects these. Speculative — recorded as an acquisition-and-modelling direction rather than a finding. rests-on: source:vinyals-pointer-mechanism, source:fernandez-pass-surface, source:nakahara-14-actions]

⚠️ **This claim is untested and no held source supports the football half.** It is the reason the paper was retained, and it should be either acted on or retired rather than left standing indefinitely.

## See Also

- [[attention-mechanism]] · [[lstm]] · [[transformer]] · [[action-space-design]]
- [[pass-probability-model]] · [[probability-surface]] · [[expected-possession-value]] · [[receiving-efficiency]]
- [[google-brain]] · [[neural-machine-translation|Bahdanau Summary]] · [[expected-value-possession-framework|EPV Summary]]
