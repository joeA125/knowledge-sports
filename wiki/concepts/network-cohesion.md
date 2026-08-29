---
title: "Network Cohesion (Clustering, Connectivity, Cliques)"
type: concept
tags: [network-analysis, sports-analytics, tactical-analysis, interpretability, counterfactual, event-stream-data, evaluation, clustering]
sources: [raw/papers/football_strategy_network_theory_analysis.md]
confidence: 0.8
provenance:
  extracted: 62%
  inferred: 21%
  generated: 15%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Network Cohesion (Clustering, Connectivity, Cliques)

The **team-level** half of [[social-network-analysis]]. Where centrality measures ask *which player matters*, cohesion measures ask *how tightly the eleven hang together as a structure* — and they return one number for the whole team rather than eleven.

Split from [[social-network-analysis]] on ingest of [[network-theory-football-strategies|López Peña & Touchette]], which supplies all four measures below. The parent page had carried centrality only, because the source it rested on second-hand ([[ai-football-reinforcement-learning|Scott et al.]]) used centrality only.

## The Four Measures

| Measure | Definition | Football reading |
|---|---|---|
| **Weighted clustering** $c_i^w$ | $\frac{1}{u_i(u_i-1)} \sum_{j,k} \frac{\sqrt[3]{A_{ij}A_{kj}A_{ki}}}{\max(A)}$ | How readily player $i$ works as a **middle-man** in $j \to i \to k$ |
| **Edge connectivity** $k$ | Minimum arcs to remove to disconnect the graph | **Passes an opponent must intercept** to sever the team |
| **Node connectivity** | Minimum players to remove to disconnect the graph | **Discarded — uninformative** |
| **Maximal clique** $C_q$ | Largest all-pairs-connected subset | How many players **all** interchange directly |

Clustering is a per-player quantity averaged to a team figure, so it straddles the two halves; the other three are irreducibly team-level.

## Why Node Connectivity Fails and Edge Connectivity Does Not

A pass network over eleven nodes is nearly **complete** — most players pass to most others at least once over a tournament. Node connectivity is therefore uniformly high and discriminates nothing, and the authors drop it.

Edge connectivity survives the same density because removing *arcs* is a finer operation than removing *nodes*. It ranged from **0 to 5** across the 2010 round of 16 — Paraguay, Chile and Mexico at zero, Brazil and the Netherlands at five.

**A connectivity of zero does not mean the team was disconnected.** It means the *averaged* network had at least one player with no recorded pass in one direction — which, given the aggregation over games discussed on the source page, may reflect a substitute's thin sample rather than a tactical fact.^[inferred: the authors report both the zero values and the averaging artefact but do not connect them]

## Edge Connectivity Is a Counterfactual Without a Model

This is the interesting property and it is not in the paper.

Every other counterfactual construct in this vault — [[c-obso]], [[drso]], [[counterfactual-baseline|baseline-differencing]] generally — needs a **model of what would have happened**: a predicted trajectory, an optimal position, a fitted value surface. Each is expensive and each introduces the modelling assumptions that make [[observed-versus-optimal-decisions|observed-versus-optimal]] comparisons contestable.

Edge connectivity answers *"what if this passing lane were taken away?"* by **deleting edges from a graph and re-running a shortest-path computation.** No model, no assumption, no training.

> ### `graph-counterfactuals-are-cheap-and-shallow`
> **Removing an edge from a pass network answers a counterfactual question with no model of the world — but it answers only the question of what the remaining graph looks like, not what the team would actually do. The cost of the counterfactual and the depth of it fall together.**
> ^[generated: no source frames edge connectivity as a counterfactual construct; the contrast against the vault's model-based counterfactuals is drawn here. rests-on: source:lopez-pena-edge-connectivity-robustness]

The shallowness is the whole of it. A real team whose best passing lane is cut **finds another one** — that adaptation is exactly what a static graph cannot represent. Edge connectivity measures the fragility of the *observed* structure, and silently assumes the structure would not reorganise. See [[social-network-analysis]] on why network metrics cannot be transferred between clubs for the same reason.

This is worth holding against [[drso|DRSO]], which spends a trajectory-prediction model to ask a comparable question about defensive positioning and gets an answer that survives the team moving.

## The Community Detection Failure

[[network-theory-football-strategies|López Peña & Touchette]] attempted community detection — finding sub-groups within a team — and report that it **did not work**:

> Our initial attempt at studying communities within football teams has not provided any useful information

Eleven nodes at near-complete density collapse into one community containing everyone.

**The cause is structural, not incidental.** Community detection needs a graph sparse enough that a partition is meaningful, and a pass network is neither large nor sparse. No better algorithm and no cleaner data changes this — the object has to change:

| Route | What it changes |
|---|---|
| **Multilayer graphs** | Separate layers per pass type or phase (Buldú et al., 2018 — cited by Scott et al., not held) |
| **Per-phase subgraphs** | Build a graph per possession phase, not per tournament — restores sparsity |
| **Opponent nodes** | Add the opposing eleven, doubling node count and introducing genuine structure |
| **Temporal slicing** | Windowed networks, which also fixes the static-graph limitation |

Every one of these is a route the paper's *own* future-work section gestures at. **The negative result is more useful than a positive one would have been**, because it identifies which dimension is binding: node count and density, not method.

## Clustering: Cohesion, Not Cluster Analysis

⚠️ **Name collision.** This is the **graph-theoretic clustering coefficient** — transitivity, the fraction of closed triangles through a node. It is unrelated to [[clustering]], which in this vault means unsupervised partitioning of data, and to the [[football-event-sequences-point-process-mixture|mixture-model sequence clustering]] under the *Clustering* task of [[action-valuation-frameworks-compared|the seven-task table]].

Two pages, one word, no relationship. Recorded because the collision is exactly the kind that propagates through a summary written from tag names.

The football content: a high $c_i^w$ means player $i$ is frequently the middle of a completed triangle — the outlet a teammate uses when the direct lane is defended. **Xavi at 46.47** is the paper's cleanest illustration, eleven points clear of any other finalist.

## What the Numbers Did

Across the 2010 round of 16, Spain led on clustering (30.0) and maximal clique (9); the Netherlands followed at 29.7 and 8. Uruguay and Portugal sat lowest at 14.3 and 14.6.

⚠️ **The apparent relationship with success does not survive testing.** Rank-correlated against finishing position over all sixteen teams, clustering returns $\rho = -0.27$ ($p = 0.32$) and maximal clique $\rho = -0.19$ ($p = 0.49$).^[generated: computed here; the paper reports no statistical test. rests-on: source:lopez-pena-table-1] Edge connectivity is the strongest of the four at $\rho = -0.41$, and still not significant at $n = 16$.

The paper's argument is descriptive and it is fair on its own terms — these numbers *characterise* tiki-taka. **The inferential step to "cohesive teams win" is one nobody in the held corpus has earned**, and the second-hand version of this literature the vault carried had taken it. See [[network-theory-football-strategies]].

## Aggregation, Again

Every figure above is averaged over a team's tournament, because FIFA released only aggregate passing data. A clustering coefficient computed this way describes **no match the team played**.

This is the same aggregation problem that makes the individual player tables hard to read, and it is the reason edge connectivity of zero appears for three teams that plainly were not disconnected.

## See Also

- [[social-network-analysis]] — the parent method, and the player-level centrality half
- [[network-theory-football-strategies|López Peña & Touchette]] — the source
- [[tactical-analysis]] · [[event-stream-data]] · [[interpretability]] · [[clustering]]
- [[counterfactual-baseline]] · [[counterfactual-simulation]] · [[c-obso]] · [[drso]] · [[observed-versus-optimal-decisions]]
- [[graph-neural-network]] · [[message-passing]] · [[action-valuation-frameworks-compared]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[google-research-football]]
