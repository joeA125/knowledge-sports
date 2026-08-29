---
title: "A network theory analysis of football strategies"
type: summary
tags: [network-analysis, sports-analytics, tactical-analysis, event-stream-data, player-evaluation, interpretability, clustering, evaluation, model-selection]
sources: [raw/papers/football_strategy_network_theory_analysis.md]
confidence: 0.85
provenance:
  extracted: 66%
  inferred: 18%
  generated: 13%
  imported: 2%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# A network theory analysis of football strategies

**Javier López Peña & Hugo Touchette.** 2010 FIFA World Cup passing data. Presented at the Euromech Physics of Sports Conference.

> **This closes the vault's #1 acquisition priority.** [[social-network-analysis]] previously rested *entirely* on citations inside [[ai-football-reinforcement-learning|Scott et al.]] — an entire task in the seven-task taxonomy held second-hand. It is now primary-sourced, and acquiring the primary **corrected three things the second-hand version had wrong** — see *What Acquiring the Primary Corrected*, below.

## The Citation Was Wrong, and the Vault Inherited It

Four wiki pages cited this work as **"Peña & Hugo (2012)"**. The authors are **Javier López Peña** and **Hugo Touchette** — "Hugo" is Touchette's *first name*, promoted to a surname.

The error did not originate here. [[ai-football-reinforcement-learning|Scott et al.]] write "(Peña and Hugo, 2012)" in their own reference list, and the vault extracted it faithfully.

> ### `an-extracted-claim-inherits-its-source's-errors`
> **Provenance marking tracks whether a claim came from a held source, not whether the held source was right. A citation extracted correctly from a paper that mis-cites is marked as cleanly as one extracted from a paper that does not — and nothing in the vault can distinguish them until the primary arrives.**
> ^[generated: no source states this; drawn from the propagation path of this specific error. rests-on: source:scott-pena-hugo-miscitation]

This is a gap in the provenance scheme rather than a failure of it. The conventions warn that `imported:` claims are the hardest to catch because nothing here contradicts them. **A wrong `extracted:` claim is harder still**, because it carries a real citation to a held file — and the file genuinely does say it.

The practical remedy is narrow and worth stating: *a bibliographic detail read out of a secondary source is not extracted from the primary*, and might reasonably be marked `imported:` even when the secondary is held.

## Method

| Element | Choice |
|---|---|
| Object | **Weighted directed graph** — nodes are players, arcs are completed passes |
| Adjacency | $A_{ij}$ = passes from player $i$ to $j$ |
| Edge length | $l_{ij} = 1/A_{ij}$ for $i \neq j$, $0$ for $i = j$; **infinite** if the pair never connect |
| Distance | Geodesic (shortest-path) $d_{ij}$ — **asymmetric**, and uncorrelated with physical distance |
| Layout | Nodes fixed at tactical formation positions — for visual reading only, not part of any metric |
| Software | FIFA site scraped with a custom Python script; Sage + NetworkX; Mathematica graphics |

Note the inversion in $l_{ij} = 1/A_{ij}$: **frequently-used passing lanes are short**. Every distance-based metric below inherits it, so "close" means *heavily connected*, not *near on the pitch*. The authors flag this explicitly.

### The metrics, with the normalisations the vault was missing

| Metric | Definition | Normaliser |
|---|---|---|
| **Closeness** | $C_i = 20 / \left( \sum_{j \neq i} d_{ij} + \sum_{j \neq i} d_{ji} \right)$ | **20** — ten teammates, both directions |
| **Betweenness** | $C_B(i) = \frac{1}{90} \sum_{j \neq k \neq i} n^i_{jk} / g_{jk}$ | **1/90** = $1/(10 \times 9)$, forcing $0 \leq C_B \leq 1$ |
| **PageRank** | $x_i = p \sum_{j \neq i} \frac{A_{ji}}{L_j^{out}} x_j + q$ | $p = 0.85$, $q = 1$ |
| **Clustering** | $c_i^w = \frac{1}{u_i(u_i-1)} \sum_{j,k} \frac{\sqrt[3]{A_{ij}A_{kj}A_{ki}}}{\max(A)}$ | $u_i$ = out-degree |

Closeness is offered with an optional asymmetric variant $C_i' = 10/(w\sum_j d_{ij} + (1-w)\sum_j d_{ji})$, weighting outgoing against incoming passes. **The authors do not use it** — $w$ is introduced and immediately set aside, which is a free parameter declared and then avoided rather than asserted.

## What It Found

### Team level, round of 16

Spain — the eventual winner — tops **passes (417), clustering (30.0) and largest clique (9)** while holding **betweenness at 1.9**, among the lowest. The authors read this as the quantitative signature of *tiki-taka*: well-connected players circulating the ball, with no single player load-bearing. The Netherlands and Brazil score similarly; Paraguay sits at the opposite corner — **edge connectivity 0, betweenness 7.5** — which the authors describe as a disconnected team leaning on a few individuals.

The normative claim is explicit and it is about **distribution, not level**:

> a team should seek betweenness scores that are evenly distributed among players

High concentrated betweenness indicates dependence on too few players. This is the sentence [[ai-football-reinforcement-learning|Scott et al.]] build their convergence result on.

### Player level

| Player | Reading |
|---|---|
| **Xavi** | Closeness 18.28, PageRank 14.66, **clustering 46.47** — the highest clustering in either finalist by 11 points |
| **Busquets** | Highest closeness (18.55), PageRank 12.99 |
| **Capdevila** | Highest Spanish betweenness (6.12) — a full-back, not a midfielder |
| **Schweinsteiger** | Betweenness 13.17, PageRank 17.32 — Germany's dependence concentrated |
| **Pedro / Klose** | Closeness 3.42 / 0.92 — the forward signature |

**Forwards are identifiable from topology alone.** The authors note that forwards almost always show the lowest closeness, betweenness and PageRank, being "isolated players waiting to receive passes" — and, confoundingly, the players most often substituted.

That last clause matters more than the paper lets on. Because the data is averaged over games played, **a player who is routinely substituted has his edge weights diluted by the minutes he was not on the pitch**.^[inferred: the authors state both the substitution pattern and the game-averaging separately; the interaction between them is drawn here] The "forward signature" is therefore partly a *playing-time* artefact rather than a purely tactical one, and Pedro is explicitly the example the authors use for both.

## What Acquiring the Primary Corrected

Three things, of ascending seriousness.

### 1. The PageRank damping parameter was inverted

The vault's [[social-network-analysis]] page read $p$ as **"the probability a player does *not* pass"**. The paper says the opposite:

> $p$ is a heuristic parameter representing the probability that a player will decide to give the ball away rather than keep it and go for a shot himself

$p$ is the probability the player **does** pass. Corrected on that page.

### 2. The parameter is imported from the web, and is probably wrong for football

$p = 0.85$ is Brin & Page's damping factor, transplanted from web surfers to footballers **with the value unchanged and the interpretation entirely replaced.** The authors are candid that it does not come from the data — the value "does not come from the network alone", "should be determined by heuristics", and could in principle be player-specific $p_i$.

But under their *own* gloss, $p$ is an empirically observable quantity. Spain averaged **417 passes per match**; shots run to roughly fifteen or twenty. A pass-versus-shoot ratio of 0.85 is far below what event data shows.

> ### `borrowed-constants-keep-their-value-and-lose-their-meaning`
> **A parameter transplanted between domains carries its numeric value across unchanged while its interpretation is replaced wholesale — so the value is justified by a derivation that no longer applies, and the new interpretation is testable in a way nobody tests.**
> ^[generated: the paper states the value, the source, and the football interpretation, but never connects them. The pass/shot arithmetic is drawn here. rests-on: source:lopez-pena-pagerank-heuristic]

This is a **seventh kind of free parameter** for [[free-parameters-load-bearing]], distinct from the six already catalogued: not asserted from domain intuition, but *inherited from another field's default*. And unusually among the vault's eighteen, **the test is nearly free** — compute the actual pass/(pass + shot) ratio from any held event dataset and compare.

### 3. "Winning teams presented lower betweenness" is not supported by this paper

The vault carried this as one of two SNA findings "with real content", on [[social-network-analysis]] and in the overview's acquisition priorities. It comes from Scott et al.'s one-line gloss.

**The paper never states it.** It observes that Spain kept a low betweenness score, and argues normatively that betweenness *should* be evenly distributed. There is no test, no correlation against tournament finish, and no claim about winning teams as a class.

Run against the paper's own Table 1, the claim does not hold:

| | $\rho$ vs finishing position | $p$ |
|---|---|---|
| **Betweenness** | **+0.17** | **0.54** |
| Largest clique | −0.19 | 0.49 |
| Clustering | −0.27 | 0.32 |
| Average passes | −0.33 | 0.22 |
| **Edge connectivity** | **−0.41** | **0.12** |

^[generated: Spearman rank correlation computed here over all 16 knockout teams, finishing position tie-averaged within elimination rounds. No such test appears in the paper. rests-on: source:lopez-pena-table-1]

The direction is right and the magnitude is negligible. The two finalists did have the joint-lowest betweenness — but **Germany (4.6) and Uruguay (4.8), the semi-finalists, were among the four highest**, and **Mexico tied Spain at the very bottom and went out in the round of 16.** The pattern is consistent with the finalists happening to play that way, not with betweenness tracking success.

**Edge connectivity is the strongest correlate on the table**, and nobody — including this paper — reports it as a finding. It is also non-significant at $n = 16$.

> **Narrowed rather than deleted.** The surviving claim is: *the 2010 finalists showed low and evenly distributed betweenness, and the paper reads this as the signature of a well-connected passing style.* That is what the primary supports. The generalisation to winning teams is Scott et al.'s, and it is the version four vault pages were carrying.

## The Reported Negative Result

Community detection **failed**, and the authors say so:

> Our initial attempt at studying communities within football teams has not provided any useful information

Eleven nodes at near-complete connectivity resolve into a single community containing everybody. They defer it to future work rather than dropping it silently.

**This is rare enough in the vault's corpus to be worth marking.** The negative result is also *structural rather than incidental*: it follows from the graph having eleven nodes and high density, so no amount of better data fixes it. Any community-detection approach to pass networks has to change the object — multilayer graphs, per-phase subgraphs, or opponent nodes — rather than the algorithm. See [[network-cohesion]].

## The Averaging Problem

The single most important limitation, and the authors state it plainly:

> As FIFA only provides the aggregate data over all the games, the passing networks were computed by dividing the number of passes by the total number of games played by each team. This introduces artifacts in some cases.

Every number in the paper is a **per-game average over a squad that rotated, across opponents of wildly varying strength, at different scorelines.** A team's network is not a network of any match it played.

This compounds with the substitution effect above, and it bounds what the individual player tables can mean. The authors wanted a per-game analysis and could not get the data.

⚠️ **This propagates into [[ai-football-reinforcement-learning|Scott et al.]]**, whose real-world baseline is five J-League matches analysed with these metrics. Their baseline is per-match and this paper's is not, so the two are **not the same measurement** despite sharing metric names.^[inferred: neither paper compares its aggregation choice to the other's]

## Limitations

- **Aggregate data averaged over games**, above — the dominant limitation.
- **Static networks.** The authors name this first in their own "further work". A pass network has no time axis, so a team that changed shape at half-time is one graph.
- **Successful passes only.** Interceptions, misplaced passes and turnovers are invisible; the authors propose weighting by pass accuracy as future work.
- **No defensive information whatsoever.** Proposed as future work via interceptions and recoveries.
- **No opponent.** Every network is a team in isolation. Adding a goal node for shots is proposed, and had already been done by Duch et al. (2010).
- **$n = 16$ teams**, one tournament, and no statistical testing anywhere in the paper.
- **Node connectivity discarded as uninformative** — pass networks are near-complete, so it is uniformly high. Edge connectivity is used instead.
- **PageRank's $p$ and $q$** asserted, not swept. See above.

## What This Adds to the Vault

1. **A primary source for an entire task.** The tactical row of [[action-valuation-frameworks-compared|the seven-task table]] is no longer second-hand.
2. **Three corrections** to [[social-network-analysis]] — the damping parameter's direction, the citation, and the betweenness finding.
3. **[[network-cohesion]]** — clustering coefficient, edge connectivity and cliques, the team-level half of the method the vault had no page for.
4. **A seventh kind of free parameter** for [[free-parameters-load-bearing]], with the cheapest test on that page.
5. **A reported negative result** on community detection, with a structural rather than incidental cause.
6. **The ancestor of the metric definitions in [[ai-football-reinforcement-learning|Scott et al.]]**, including the normalisation constants their summary omitted.
7. **A confound in Scott et al.'s real-world baseline** — per-match against per-tournament aggregation.

## See Also

- [[social-network-analysis]] · [[network-cohesion]] · [[tactical-analysis]] · [[event-stream-data]] · [[interpretability]]
- [[javier-lopez-pena]] · [[hugo-touchette]]
- [[free-parameters-load-bearing]] · [[model-selection]] · [[action-valuation-frameworks-compared]]
- [[counterfactual-baseline]] · [[player-rating-time-series]] · [[recruitment]] · [[graph-neural-network]] · [[message-passing]]
- [[ai-football-reinforcement-learning|Scott et al. Summary]] · [[atom-scott]] · [[google-research-football]] · [[domain-adaptation]]
