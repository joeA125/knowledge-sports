---
title: "Social Network Analysis (Pass Networks)"
type: concept
tags: [network-analysis, sports-analytics, tactical-analysis, pass-modelling, player-evaluation, evaluation, event-stream-data, simulator, domain-adaptation, interpretability, model-selection]
sources: [raw/papers/ai_football_reinforcement_learning.md, raw/papers/football_strategy_network_theory_analysis.md, raw/papers/team_sports_data_analysis.md]
confidence: 0.85
provenance:
  extracted: 55%
  inferred: 24%
  generated: 10%
  imported: 11%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-08
updated: 2026-08-29
---

# Social Network Analysis (Pass Networks)

Describing a team as a **weighted directed graph** whose nodes are players and whose edges are passes, then characterising the team by graph-theoretic properties rather than by counting actions.

Given adjacency matrix $A$ where $A_{ij}$ is the number of passes from player $i$ to $j$, and weights $W_{ij} = 1/A_{ij}$ for $i \neq j$ (so frequently-used edges are *short*), the standard metrics are:

| Metric | Formula | Reads as |
|---|---|---|
| **Closeness** | $C_i = 20 / \left( \sum_{j \neq i} d_{ij} + \sum_{j \neq i} d_{ji} \right)$ | How easily a player connects to teammates |
| **Betweenness** | $C_B(i) = \frac{1}{90} \sum_{j \neq k \neq i} n^i_{jk} / g_{jk}$ | How much a player bridges passing plays |
| **PageRank** | $x_i = p \sum_{j \neq i} \frac{A_{ji}}{L_j^{out}} x_j + q$ | How popular a player is as a pass target |

**The normalisers matter and the vault did not have them.** Closeness divides by **20** — ten teammates counted in both directions — and betweenness by **90** $= 10 \times 9$, which forces $0 \leq C_B \leq 1$. Both were absent while this page rested on [[ai-football-reinforcement-learning|Scott et al.]]'s summary description, so the metrics were unreproducible from it. Supplied by [[network-theory-football-strategies|López Peña & Touchette]].

PageRank is run with $p = 0.85$, $q = 1$, following [[network-theory-football-strategies|López Peña & Touchette (2012)]], with $q$ read as "free popularity".

> ⚠️ **Corrected 2026-08-29 on acquiring the primary source.** This page previously read $p$ as **"the probability a player does *not* pass"**. That is backwards. The primary defines $p$ as the probability that a player **"will decide to give the ball away rather than keep it and go for a shot himself"** — the probability he **does** pass.
>
> The error was invisible for as long as the page rested on a secondary description, because [[ai-football-reinforcement-learning|Scott et al.]] use the parameter without glossing it.

**And the value is borrowed rather than derived.** $p = 0.85$ is Brin & Page's web-surfer damping factor carried over unchanged, while its interpretation was replaced entirely. Under the football reading, $p$ is empirically measurable — and Spain's 417 passes per match against fifteen-odd shots puts the real ratio far above 0.85. See `borrowed-constants-keep-their-value-and-lose-their-meaning` on [[network-theory-football-strategies]], and [[free-parameters-load-bearing]], where this is the **seventh kind** of free parameter and carries the cheapest available test.

## An Established Tradition, Now Partly Held

This is not a fringe method. [[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] cite a substantial literature: Clemente et al. (2016) for the framework, Buldú et al. (2018) on the multilayer nature of passing networks, and two results with real content —

- **López Peña & Touchette (2012):** **now held.** See [[network-theory-football-strategies]] — and see below, because the claim as transmitted is *not what the paper says*.
- **Gonçalves et al. (2017):** lower passing dependency on any one player, and higher intra-team connectedness, may optimise team performance. **Not held.**

Clemente, Buldú and Gonçalves remain unheld. The vault's football coverage has been almost entirely **valuation-first** — assign a number to an action, aggregate to a player — and this is a genuinely different tradition that describes **structure** instead. It belongs alongside [[tactical-analysis]] rather than [[action-valuation]].

### ⚠️ The Group's Own Survey Gives This One Clause

> **Added 2026-08-29** on ingest of [[data-driven-team-sports-behaviors|Fujii's 2021 survey]].

Pass-network analysis appears in that survey **once** — a single citation of Yamamoto & Yokoyama (2011), listed among "more mathematically sophisticated" rule-based approaches, alongside self-similarity in team front position and group theory in a 3-vs-1 possession task.

One year later, [[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] make pass-network topology **the central instrument** of their sim-to-real transfer study.

**The method was peripheral in the group's own map of the field twelve months before it became load-bearing in the group's own paper.**

This is not an inconsistency. It is confirmation of the reason Scott et al. gave: SNA was selected for a **property** — insensitivity to physical differences between simulator and reality — rather than because the group considered it a central tool. The survey is the group's own evidence that it was not otherwise a priority.

> Strengthens `transfer-evidence-is-conditional-on-the-dimension-chosen` on [[domain-adaptation]]. A dimension chosen instrumentally is a weaker basis for a transfer claim than one chosen because it matters, and here the instrumental choice is documented from two directions.
> ^[generated: the juxtaposition is drawn here; neither source remarks on it. rests-on: source:fujii-survey-sna-single-clause, source:scott-sna-justification]

It also bears on the [[network-theory-football-strategies|citation error]] found in the previous ingest. A tradition the citing group treated as peripheral is exactly the kind whose bibliography receives least scrutiny — which is consistent with, though not proof of, how "Peña & Hugo" survived review.^[inferred: no source connects the two; offered as a mechanism, not a finding]

### ⚠️ The Betweenness Finding Did Not Survive Acquisition

**Corrected 2026-08-29.** This page, the overview and [[action-valuation-frameworks-compared]] all carried *"winning teams presented lower betweenness scores"*, attributed to "Peña & Hugo (2012)". Three problems, found on reading the primary:

1. **The citation was garbled.** The authors are [[javier-lopez-pena|Javier López Peña]] and [[hugo-touchette|Hugo Touchette]]; "Hugo" is a forename. The error is [[ai-football-reinforcement-learning|Scott et al.]]'s and the vault extracted it faithfully.
2. **The paper never makes the claim.** It observes that Spain kept a low betweenness score, and argues *normatively* that betweenness **should** be evenly distributed across a team. No test, no correlation against finishing position, no claim about winning teams as a class.
3. **The claim fails against the paper's own table.** Rank-correlated over all sixteen knockout teams, betweenness against finishing position gives $\rho = +0.17$, $p = 0.54$.^[generated: computed on ingest; no such test appears in any source. rests-on: source:lopez-pena-table-1] The two finalists did hold the joint-lowest betweenness — but **Germany and Uruguay, the semi-finalists, were among the four highest**, and **Mexico tied Spain at the bottom and went out in the round of 16.**

> **Narrowed rather than deleted**, per the conventions. The surviving claim is: *the 2010 finalists showed low, evenly distributed betweenness, and the paper reads this as the signature of a well-connected passing style.* The generalisation to winning teams belongs to Scott et al., not to the primary.

**What the vault lost here was not a finding but a false one**, and the loss is instructive about where the tradition's real content sits. The paper's substantive contribution is **descriptive** — a vocabulary for team style — and the inferential claim about winning was manufactured downstream by a one-line gloss. See [[network-theory-football-strategies]].

## The Team-Level Half

Centrality is only one side of the method. The team-level structural measures — **weighted clustering, edge connectivity, maximal clique** — and the paper's reported failure at community detection are on [[network-cohesion]], split out on ingest because this page had grown past the length the conventions allow for two distinct ideas.

The short version: edge connectivity reads as *how many passes an opponent must intercept to sever the team*, and is a **counterfactual computed with no model of the world** — cheap, and shallow in exactly proportion.

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

**SNA is the instrument you reach for when the two things you want to compare share a logical structure but not a physical one.**

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

## Origin

Centrality metrics come from sociology, where they measure interaction structure rather than interaction volume. The football application is one of the more literal — passes are unambiguous directed edges in a way that "influence" or "collaboration" rarely is.

The vault's nearest neighbour is [[message-passing]], which shares the graph substrate and nothing else: message passing computes *inference* on a graph, SNA computes *description* of one.

## See Also

- [[tactical-analysis]] · [[pass-probability-model]] · [[event-stream-data]] · [[spadl]] · [[receiving-efficiency]]
- [[domain-adaptation]] · [[agent-based-simulation]] · [[google-research-football]] · [[trueskill]] · [[proximal-policy-optimization]]
- [[message-passing]] · [[graph-neural-network]] · [[player-rating-time-series]] · [[performance-volatility]] · [[recruitment]]
- [[action-valuation]] · [[counterfactual-simulation]] · [[interpretability]] · [[action-valuation-frameworks-compared]]
- [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]]
- [[ai-football-reinforcement-learning|Source Summary]]
