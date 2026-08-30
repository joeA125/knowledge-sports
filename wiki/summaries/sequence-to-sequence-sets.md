---
title: "Order Matters: Sequence to Sequence for Sets — Source Summary"
type: summary
tags: [deep-learning, sequence-modelling, attention, set-modelling, ordering, lstm, model-selection, multi-agent, sports-analytics]
sources: [raw/papers/sequence-to-sequence-sets.md]
confidence: 0.85
provenance:
  extracted: 76%
  inferred: 14%
  generated: 8%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-29
---

# Order Matters: Sequence to Sequence for Sets

**Vinyals, Bengio & Kudlur**, [[google-brain]], ICLR 2016; arXiv:1511.06391.

> **Held on forward-looking grounds (D3).** No football source cites it. It is retained because **a football game state is an unordered set of 22 players**, and every framework in this vault has to solve that problem — mostly without acknowledging it is a known one. The read-process-write architecture and the combinatorial results live in the general vault.

## The Central Claim

**Order matters — for inputs and outputs — even when the underlying object is unordered.** With unlimited capacity and data it would not. In practice:

| Change | Effect |
|---|---|
| Reversing English input sentences | +5.0 BLEU (Sutskever et al.) |
| Sorting input points by angle (convex hull) | Up to +10% accuracy |
| Depth-first vs breadth-first parse traversal | 89.5% vs 81.5% F1 |
| 3-word scramble in language modelling | Perplexity 86 → 96 |

The paper also proposes searching over orderings during training, maximising $\max_{\pi} \log p(Y_{\pi(X)} \mid X)$, approximated by ancestral sampling since exact search over $n!$ is intractable.

## Why This Bears on Football

Twenty-two players have no canonical ordering, and the vault holds **three distinct responses** to that — none of which cites this paper:

| Response | Mechanism | Where |
|---|---|---|
| **Symmetric aggregation** | Permutation-equivariant by construction | [[graph-neural-network\|GNN]] in [[trajectory-prediction]], behind [[c-obso]] |
| **Attention over a set** | Learned order-free weighting | [[transformer]] via [[nmstpp]]; see [[message-passing]] |
| **Sort by a meaningful key** | Impose a canonical order | [[vdep]] — players sorted by distance to ball |

**The third is exactly what this paper warns about**, and exactly what it recommends. VDEP's proximity sort is an ordering choice that was never searched or ablated — and Vinyals et al.'s convex-hull result shows a *well-chosen* sort can help substantially, while a poor one costs.

[[gvdep|GVDEP]] later measured part of the cost: ball-gain prediction saturates at three or four sorted players, and the other three targets gain nothing from player positions at all. That is evidence the sort is lossy, but not evidence about *which* sort would be best.

> ### `player-ordering-is-an-unsearched-choice`
> **Where a model imposes an ordering on an unordered set of players, that ordering is a free parameter with measurable effect. VDEP sorts by ball proximity; no held source has compared it against alternatives, and the one paper addressing the problem directly proposes searching over orderings.**
> ^[generated: connects a general-ML source to a football design choice neither addresses. **Acted on 2026-08-29** — see below. rests-on: source:vinyals-ordering-effects, source:vdep-proximity-sort, source:gvdep-nearest-sweep]

## ✅ Acted On, 2026-08-29 — Retained and Narrowed

> **This page was held on forward-looking grounds with a speculative claim flagged to be acted on or retired.** The claim has now been tested against a held football source: [[data-driven-team-sports-behaviors|Fujii's 2021 survey]], which names the permutation problem explicitly and catalogues the responses to it.
>
> **The retention was justified.** The claim survives, but one of its premises was wrong.

| Component | Status |
|---|---|
| The ordering is a free parameter with measurable effect | **Survives** — untested, unchanged |
| No held source compares VDEP's sort against alternatives | **Survives** — the survey lists alternatives and compares none |
| Football frameworks mostly do not acknowledge this is a known problem | ⚠️ **Falsified at field level** |

The third premise was this page's framing — that football sources solve the problem "mostly without acknowledging it is a known one". **Fujii's survey acknowledges it as a known one by name**, and catalogues four routes past it rather than the three recorded here. The missing route is **role assignment via the Hungarian algorithm**, which has a longer track record in team-sport analysis than any of the three.

**Narrowing rather than deletion**, per the conventions — and the narrowed claim is *stronger*:

> Individual football method papers mostly do not acknowledge the permutation problem, though the field's own survey does. The choice is therefore made in a field that has catalogued the alternatives, which makes an unexamined sort **harder** to excuse rather than easier.

An unmarked arbitrary choice is a blind spot. The same choice, in a field that has written down the alternatives, is a decision nobody recorded.

**The material has moved to [[permutation-problem]]**, which is now the home page for this claim and carries the four routes, the trade-off against interpretability, and a four-arm test including a shirt-number null.

This page reverts to what it is: **a general-ML source about ordering effects**, retained because the football connection it supports has now been made explicit elsewhere.

## See Also

- [[message-passing]] · [[graph-neural-network]] · [[transformer]] · [[attention-mechanism]] · [[lstm]] · [[action-space-design]]
- [[permutation-problem]] — the football-side home for this page's claim, since 2026-08-29
- [[data-driven-team-sports-behaviors|Fujii survey]] — the held football source that names the problem
- [[vdep]] · [[gvdep]] · [[trajectory-prediction]] · [[c-obso]] · [[nmstpp]] · [[autoregressive-model]]
- [[free-parameters-load-bearing]] · [[model-selection]] · [[google-brain]]
- [[football-defence-evaluation-vdep|VDEP Summary]] · [[generalized-vdep-euro-location-analysis|GVDEP Summary]]
