---
title: "Message Passing"
type: concept
tags: [message-passing, inference, probabilistic-graphical-model, graph-neural-network, deep-learning, trajectory-prediction, sports-analytics]
sources: [raw/papers/bayesian-true-skill-rating.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md]
confidence: 0.8
provenance:
  extracted: 40%
  inferred: 53%
  generated: 5%
  imported: 0%
  ambiguous: 2%
lifecycle: draft
created: 2026-07-27
updated: 2026-08-14
---

# Message Passing

Computing something about a graph by having nodes and edges exchange local information iteratively, until the local computations collectively answer a global question.

The vault holds two traditions that share this name and are rarely discussed together. They turn out to be the same computational pattern applied to different objects.

## Tradition 1: Inference on Graphical Models

Given a distribution factorised over a graph, compute marginals by passing messages between variable and factor nodes. Each message summarises everything one part of the graph knows about a variable; on a tree the algorithm is exact.

Used in [[trueskill]], where the graph encodes players' skills, per-game performances, team sums and outcome comparisons. Because the comparison factors are non-Gaussian, exact messages are intractable and [[expectation-propagation]] approximates them by moment matching.

**Passed:** probability distributions. **Computed:** marginals of a known distribution. **Learned:** nothing — structure and factors are specified.

## Tradition 2: Representation Learning on Graphs

Given a graph of entities, compute node representations by passing *learned* messages along edges. In a [[graph-neural-network|GNN]]:

$$e_{(k,j)} = f_e([v_k, v_j]), \qquad o_k = f_v\Big(\sum_{j \in N(k)} e_{(k,j)}\Big)$$

Used in the GVRNN of [[trajectory-prediction]], where nodes are players and each agent's latent distribution is conditioned on all others — the predictor underneath [[c-obso|C-OBSO]].

**Passed:** learned feature vectors. **Computed:** representations for a downstream task. **Learned:** the message functions themselves.

## What They Share

**Locality.** Only neighbours communicate directly, so global structure emerges from repeated local operations. This is what makes both tractable on large graphs.

**Permutation invariance.** Messages aggregate by a symmetric operation, so relabelling nodes relabels outputs identically and changes nothing else. In the GNN case this is the architectural reason the approach suits multi-agent problems: **twenty-two players have no canonical ordering.** See [[graph-neural-network]].

**Approximation on loops.** Both are exact or well-behaved on trees and approximate on graphs with cycles — loopy belief propagation may not converge; a deep GNN over-smooths as repeated aggregation collapses node representations toward each other.

## The Ordering Problem, Three Ways

Permutation invariance is not an abstraction here — it is the central design problem of modelling a football team, and this vault holds three distinct answers to it:

| Approach | Mechanism | Example |
|---|---|---|
| **Graph message passing** | Symmetric aggregation | GVRNN in [[trajectory-prediction]], underpinning [[c-obso]] |
| **Attention over a set** | Learned order-free weighting | [[transformer]], via [[nmstpp]] |
| **Sorting by a meaningful key** | Impose a canonical order | [[vdep]] — players sorted by distance to ball |

Self-attention over a set with no positional encoding **is** message passing on a fully-connected graph, with attention-weighted rather than summed aggregation. That places the [[transformer]] in the same family as GNNs, and explains why the two literatures keep arriving at similar constructions from opposite directions.

**The third is the cheap version** — viable for tree ensembles that cannot express equivariance at all, and lossier, since the sort key must be meaningful and near-ties swap slots under noise. [[gvdep|GVDEP]] later measured how much it costs: ball-gain prediction saturates at three or four sorted players, and the other targets gain nothing from player positions at all.

> **Scope note.** The graphical-model inference machinery — factor graphs, approximate message passing, density filtering — lives in the general vault. This page keeps what the football frameworks depend on.

## See Also

- [[graph-neural-network]] · [[trajectory-prediction]] · [[c-obso]] · [[transformer]] · [[nmstpp]]
- [[trueskill]] · [[expectation-propagation]] · [[bayesian-inference]] · [[uncertainty-quantification]]
- [[vdep]] · [[gvdep]] · [[action-space-design]] · [[off-ball-value]]
- [[bayesian-true-skill-rating|TrueSkill Summary]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO Summary]]
