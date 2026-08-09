---
title: "How does AI play football? An analysis of RL and real-world football strategies"
type: source_summary
tags: [summary, reinforcement-learning, policy-gradient, simulator, agent-based-simulation, domain-adaptation, network-analysis, ranking-system, action-space, sports-analytics, event-stream-data, evaluation, gaming]
sources: [raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 74%
  inferred: 20%
  generated: 5%
  imported: 0%
  ambiguous: 1%
lifecycle: reviewed
created: 2026-08-08
updated: 2026-08-08
---

# How does AI play football?

**Scott, Fujii & Onishi** — [[atom-scott]] (**lead author**), [[keisuke-fujii]], [[masaki-onishi]]. [[aist|AIST Tsukuba]] and [[nagoya-university]]. ICAART 2022.

> **This was the vault's standing acquisition target**, flagged across four ingests on [[reinforcement-learning]], [[google-research-football]], [[multi-agent-reinforcement-learning]] and [[atom-scott]] as the one cited work bearing directly on the **simulator transfer question**. It has now been acquired, and unlike the previous target it **does** contain what the vault expected — with an important qualification about *what* was shown to transfer.

## The Question

Can behaviour learned in a football simulator tell you anything about real football? The paper frames this as measuring the **domain gap between RL agents and real-world players**, and answers it by comparing play-style characteristics rather than performance.

## Method

| Stage | Choice |
|---|---|
| Environment | [[google-research-football\|GFootball]] (Kurach et al., 2019), 11v11 |
| Algorithm | **[[proximal-policy-optimization\|PPO]]** with an IMPALA policy — [[residual-connections\|residual blocks]] on a CNN |
| State | **Super Mini Map** — four $72 \times 96$ binary matrices (home, away, ball, active player), stacked over four frames |
| Actions | **19** — 8 directions, shot, short/high/long pass, idle, sliding, dribble, stop-dribble, sprint, stop-moving, stop-sprint |
| Control | **Single agent controlling one active player**, switchable; others on a rule-based system |
| Training | 50M steps vs easy/medium/hard bots; extended to 100M (easy, medium) and 200M (hard) |
| Ranking | **[[trueskill\|TrueSkill]]** over 15 agents, 50 round-robin tournaments, 5,250 matches |
| Analysis | [[social-network-analysis\|SNA]] of pass networks + descriptive statistics |

**The 19-action figure confirms** what [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] reported second-hand and the vault had recorded on inference. See [[action-space-design]].

### Real-world comparison data

Event-stream data from the 2019 J1 League, purchased from [[data-stadium|Data Stadium]] — **the fifth held source on this provider**, and again involving **Yokohama F. Marinos**, alongside Kashima Antlers and FC Tokyo, chosen as the league's top three at the time.

⚠️ **Internal inconsistency.** The text states "event-stream data for three matches"; Table 5 lists **five**. The three *teams* are three; the matches are five. Later analysis compares five agents/teams, consistent with five matches. Recorded because it is the kind of discrepancy that propagates if summarised carelessly.

## Why Social Network Analysis — and Why That Choice Is the Crux

The paper's justification is explicit and it is the single most important sentence in it for the vault's purposes:

> an analysis framework that is not influenced by physical differences between simulations and the real-world is necessary

Passes, they argue, do not depend on individual physical ability, so pass-network topology is comparable across domains where movement is not.

**That is methodologically honest and it bounds the result severely.** The paper measures transfer on **the one dimension deliberately selected for its insensitivity to the domain gap.** See [[domain-adaptation]], where this is worked through.

Three metrics, all on the weighted directed pass graph: **closeness** (how easily a player connects to teammates), **betweenness** (how much a player bridges passing plays), and **PageRank** (how popular a player is as a pass target).

## Result 1: What Correlates With Competitiveness

Pearson correlations against TrueSkill rating, $N = 15$ agents:

| Metric | $r$ | $p$ |
|---|---|---|
| **PageRank (min)** | **−0.91** | 0.001 |
| **Total shots** | **0.77** | 0.001 |
| **Betweenness (mean)** | **0.72** | 0.002 |
| Successful shot % | 0.68 | 0.005 |
| Closeness (min) | −0.66 | 0.007 |
| Betweenness (std) | 0.65 | 0.009 |
| Closeness (mean) | −0.64 | 0.010 |
| Successful pass % | 0.62 | 0.014 |
| PageRank (std) | 0.58 | 0.022 |
| **Total passes** | **−0.50** | 0.061 |
| PageRank (mean) | −0.05 | 0.848 |

**Better agents shoot more and pass less.** Total shots is the second-strongest positive correlate; total passes is negative (though $p = 0.061$).

That sits directly against [[optimal-decisions-shot-taking-situations|Yeung & Fujii's]] game-theoretic finding that real shooters **shoot too much** and should pass. Two papers sharing an author, pointing opposite ways. See [[observed-versus-optimal-decisions]].

The **PageRank (min)** result at $r = -0.91$ is the paper's headline and rests on an unverified identification: the authors reason that the minimum-PageRank node "is likely to be the goalkeeper", so a strong agent learns to keep the ball away from him. **Nearly-perfect correlation, plausible story, no check that the node is in fact the goalkeeper.**

### The anomaly the paper does not resolve

Agents trained against the **easy** bot rank 1, 2 and 3 on TrueSkill. Agents trained 200M steps against the hard bot rank lower. The authors call this counter-intuitive, suggest better training strategies exist, and defer it.

**Every correlation above is against this axis.** If TrueSkill ordering is distorted by a curriculum artefact, "competitiveness" is not straightforwardly what the correlations measure. The paper is candid about the anomaly and does not carry the caveat into its conclusions.

## Result 2: Do Agents Play Like Humans?

Distributions built by bootstrapping $N = 500$ passes, 50 times, for the best agent, the worst agent, and the three J-League teams.

| Metric | Best agent vs real teams |
|---|---|
| **Betweenness (mean)** | **Converges** — worst agent distant, best agent close |
| **Betweenness (std)** | **Converges** |
| **Closeness (std)** | **Converges** |
| Closeness (mean) | No convergence |
| PageRank (std) | No convergence |
| PageRank (min) | No convergence |

**Three of six metrics converge toward real football as the agent improves.** The betweenness result is the interpretable one: high betweenness deviation indicates a well-balanced passing strategy with less dependence on any single player, so agents appear to *learn* balanced distribution without being told to.

The authors also note something they find surprising and which cuts the other way: **even the worst agent overlaps with real footballers** on PageRank and Closeness. Their explanation is that those strategies are easy to learn. An alternative they do not consider is that **those metrics may not discriminate** — if the worst agent already matches, the metric is weak evidence of anything.

## What This Settles, and What It Does Not

**Settles:** there is *some* measurable convergence between simulator-trained agents and real footballers, on pass-network topology, increasing with agent quality. The vault's repeated claim that "no evidence of transfer exists" is **wrong as stated** and must be revised. See [[reinforcement-learning]].

**Does not settle:** whether anything *physical* transfers. The metrics were chosen precisely because they factor out physical difference. Read alongside [[adaptive-action-supervision-multi-agent-rl|Fujii et al. (2023)]], which fails to reproduce demonstrated **movement** in a simulator and attributes the failure to simulator fidelity, the two papers are consistent rather than contradictory:

> **Where physical dynamics are factored out, partial transfer appears. Where they are central, transfer fails.**

That is a considerably more useful statement than either paper alone supports, and it is the main synthesis product of this ingest.

## Limitations

- **$N = 15$ agents** for every correlation, and 5 real matches for every comparison.
- **Single-agent central control**, explicitly: the agent controls one active player and multi-agent RL is declared out of scope. The eventual [[multi-agent-reinforcement-learning|MARL]] follow-ups are [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] and [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]].
- **Non-active players are rule-based**, so the "team" whose pass network is analysed is partly scripted. The pass network is therefore not purely a learned artefact.
- **The TrueSkill anomaly** above.
- **PageRank (min) interpretation unverified.**
- **Match lengths differ**, so raw pass and shot counts cannot be compared across domains — acknowledged, and why the comparison is restricted to SNA.
- **Distributions compared by visual inspection** after a normality test; no formal test of distributional distance is reported.

## The Representation Plea

The paper closes with a specific complaint worth recording, because it comes from inside:

> we noticed that it was difficult to use state of the art football analysis methods due to different representations of the underlying data

They convert both simulated and real data to a **simplified [[spadl|SPADL]]** (passes and shots only) and ask other researchers to build on such representations so the community can reuse methods.

That is a direct, author-side statement of the problem the vault records as `no-cross-framework-benchmarking`. **The barrier is named by practitioners as representational, not merely social.** See [[action-valuation-frameworks-compared]].

## What This Adds to the Vault

1. **The transfer evidence**, and the qualification that makes it interpretable.
2. **[[social-network-analysis]]** — an entire analytic tradition (Peña & Hugo, Clemente et al., Buldú et al., Gonçalves et al.) the vault had no page for.
3. **[[proximal-policy-optimization|PPO]]** — the vault's first policy-gradient method.
4. **[[agent-based-simulation]]** — the motivation layer for simulating at all.
5. **A novel use of [[trueskill|TrueSkill]]**: ranking RL checkpoints rather than human players.
6. **Confirmation of GFootball's 19 actions** from a paper that used the environment directly.
7. **A finding that contradicts the vault's shooting result** — better agents shoot *more*.

## See Also

- [[social-network-analysis]] · [[proximal-policy-optimization]] · [[agent-based-simulation]] · [[domain-adaptation]] · [[google-research-football]]
- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[action-space-design]] · [[trueskill]] · [[nfootball]]
- [[spadl]] · [[observed-versus-optimal-decisions]] · [[action-valuation-frameworks-compared]] · [[residual-connections]]
- [[atom-scott]] · [[keisuke-fujii]] · [[masaki-onishi]] · [[hiroshi-nakahara]] · [[ralf-herbrich]] · [[tom-minka]]
- [[aist]] · [[nagoya-university]] · [[data-stadium]]
- [[adaptive-action-supervision-multi-agent-rl|Fujii et al. Summary]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al. Summary]] · [[bayesian-true-skill-rating|TrueSkill Summary]]
