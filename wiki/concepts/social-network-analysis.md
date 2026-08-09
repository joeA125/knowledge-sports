---
title: "Social Network Analysis (Pass Networks)"
type: concept
tags: [network-analysis, sports-analytics, tactical-analysis, pass-modelling, player-evaluation, evaluation, event-stream-data, simulator, domain-adaptation, interpretability]
sources: [raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.75
provenance:
  extracted: 55%
  inferred: 24%
  generated: 10%
  imported: 11%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-08
updated: 2026-08-08
---

# Social Network Analysis (Pass Networks)

Describing a team as a **weighted directed graph** whose nodes are players and whose edges are passes, then characterising the team by graph-theoretic properties rather than by counting actions.

Given adjacency matrix $A$ where $A_{ij}$ is the number of passes from player $i$ to $j$, and weights $W_{ij} = 1/A_{ij}$ for $i \neq j$ (so frequently-used edges are *short*), the standard metrics are:

| Metric | Formula | Reads as |
|---|---|---|
| **Closeness** | $1 / \sum_{w \in V} \sigma_{vw}$ | How easily a player connects to teammates |
| **Betweenness** | $\sum_{s \neq v}\sum_{t \neq v} \sigma_{st}(v)/\sigma_{st}$ | How much a player bridges passing plays |
| **PageRank** | $p \sum_{v \neq w} \frac{A_{vw}}{L_w^{out}} \text{PR}(w) + q$ | How popular a player is as a pass target |

PageRank is run with $p = 0.85$, $q = 1$ following Peña & Hugo (2012) — $p$ read as the probability a player does *not* pass, $q$ as "free popularity".

## An Established Tradition the Vault Had Missed

This is not a fringe method. [[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] cite a substantial literature: Clemente et al. (2016) for the framework, Buldú et al. (2018) on the multilayer nature of passing networks, and two results with real content —

- **Peña & Hugo (2012):** winning teams presented *lower* betweenness scores.
- **Gonçalves et al. (2017):** lower passing dependency on any one player, and higher intra-team connectedness, may optimise team performance.

None of these is held. The vault's football coverage has been almost entirely **valuation-first** — assign a number to an action, aggregate to a player — and this is a genuinely different tradition that describes **structure** instead. It belongs alongside [[tactical-analysis]] rather than [[action-valuation]].

## What Makes It Different From Everything Else Here

> ### `network-metrics-are-relational-not-additive`
> **Network metrics describe a player's position in a structure and cannot be summed to a team total or decomposed to individual credit. That makes them immune to the aggregation problems that afflict action valuation, and useless for the questions action valuation answers.**
> ^[generated: no source states this contrast; drawn here against the vault's valuation corpus. rests-on: source:scott-sna-metric-definitions]

Almost every metric in this vault ends by summing per-action values into a per-90 rating — where, per [[player-rating-time-series]], a season of variation is discarded. **A betweenness score cannot be summed.** It is defined only relative to the other 21 nodes and changes if a teammate is substituted.

That is a real advantage for describing *style* and a real limitation for [[recruitment]]: you cannot ask what a player's betweenness would be at another club without rebuilding the graph, which is the [[counterfactual-simulation|counterfactual]] problem in a form nobody here has attempted.

## Why This Paper Chose It

The reason is unusually explicit and it is what makes the method load-bearing rather than decorative:

> an analysis framework that is not influenced by physical differences between simulations and the real-world is necessary

Passes do not depend on individual physical ability, so pass-network topology is comparable between a simulator and a real match where movement, speed and body mechanics are not.

**SNA is the instrument you reach for when the two things you want to compare share a logical structure but not a physical one.** That generalises well past sport — comparing organisational communication across companies of different sizes, or trade networks across economies of different scales.

It also bounds what the paper's transfer result can mean. See [[domain-adaptation]].

## What It Found

Correlating against agent competitiveness ([[trueskill|TrueSkill]], $N = 15$):

- **PageRank (min), $r = -0.91$** — the strongest correlation in the paper. The authors reason the minimum-PageRank node is probably the goalkeeper, so strong agents learn to keep the ball away from him. **Plausible, and the identification is never verified.**
- **Betweenness (mean), $r = 0.72$**; **Betweenness (std), $r = 0.65$** — high betweenness deviation indicates a balanced passing strategy with less single-player dependence, so agents appear to learn distribution without instruction.

Comparing agents against J-League teams, **three of six metrics converge** toward real football as the agent improves (betweenness mean and std, closeness std) and three do not.

⚠️ **The convergence result has a confound the paper does not name.** Only one player is agent-controlled; the other ten run [[google-research-football|GFootball's]] **rule-based in-game system**. So the pass network being analysed is partly scripted, and how much of the convergence is learned behaviour rather than the built-in AI is not separable from these results.

## Aggregation Choices Are Doing Work

Each metric appears four ways — mean, std, min, max — and the paper's two headline findings use **min** and **std** rather than mean. PageRank (mean) correlates at $-0.05$; PageRank (min) at $-0.91$.

**The aggregation is not a presentational detail; it is where the signal is.** A metric that says nothing on average can say a great deal at its extreme, which is the same lesson [[performance-volatility]] draws about variance being signal rather than noise, arriving from an unrelated direction.

It also raises a multiple-comparisons question the paper does not address: sixteen metric–aggregation combinations tested at $p < 0.05$, with no correction.

## Beyond Sport

Centrality metrics originate in sociology and are used wherever interaction structure matters more than interaction volume: organisational communication, citation networks, supply chains, protein interaction, and infrastructure resilience. The football application is one of the more literal — passes are unambiguous directed edges in a way that "influence" or "collaboration" rarely is.

The vault's nearest neighbour is [[message-passing]], which shares the graph substrate and nothing else: message passing computes *inference* on a graph, SNA computes *description* of one.

## See Also

- [[tactical-analysis]] · [[pass-probability-model]] · [[event-stream-data]] · [[spadl]] · [[receiving-efficiency]]
- [[domain-adaptation]] · [[agent-based-simulation]] · [[google-research-football]] · [[trueskill]] · [[proximal-policy-optimization]]
- [[message-passing]] · [[graph-neural-network]] · [[player-rating-time-series]] · [[performance-volatility]] · [[recruitment]]
- [[action-valuation]] · [[counterfactual-simulation]] · [[interpretability]] · [[action-valuation-frameworks-compared]]
- [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]]
- [[ai-football-reinforcement-learning|Source Summary]]
