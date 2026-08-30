---
title: "Data-driven Analysis for Understanding Team Sports Behaviors"
type: summary
tags: [sports-analytics, interpretability, dynamical-systems, trajectory-prediction, multi-agent, reinforcement-learning, imitation-learning, counterfactual, clustering, dimensionality-reduction, set-modelling, ordering, theory-based-modelling, evaluation, tactical-analysis]
sources: [raw/papers/team_sports_data_analysis.md]
confidence: 0.8
provenance:
  extracted: 44%
  inferred: 16%
  generated: 12%
  imported: 27%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Data-driven Analysis for Understanding Team Sports Behaviors

**[[keisuke-fujii]], sole author.** [[nagoya-university]]. arXiv:2102.07545v2, February 2021. Acknowledges [[atom-scott]], [[masaki-onishi]] and Rory Bunker.

> ⚠️ **This is a survey, and the vault should read it as one.** Almost everything in it is a description of work not held here. The [[network-theory-football-strategies|previous ingest]] established why that matters: a secondary description can be wrong, and nothing in the vault can tell until the primary arrives.
>
> **Provenance rule applied.** Claims sourced to works this survey describes but the vault does not hold are marked `^[imported]`, **not** `^[extracted]`, even though they come from a held file. That rule was proposed on [[network-theory-football-strategies]] one ingest ago; this page is its first real exercise, and it is why the frontmatter carries 27% imported.

## Why This Source Is Unusual Here

**It is the [[keisuke-fujii|Fujii]] group's own map of the field, written before most of the work the vault holds from them.**

| Held Fujii-group source | Year | Relative to this survey |
|---|---|---|
| **This survey** | **2021** | — |
| [[ai-football-reinforcement-learning\|Scott et al.]] | 2022 | After |
| [[football-defence-evaluation-vdep\|VDEP]] | 2022 | After |
| [[generalized-vdep-euro-location-analysis\|GVDEP]] | 2022 | After |
| [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | 2022/23 | After |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 2023 | After |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | 2023 | After |
| [[team-defense-positioning-counterfactuals\|DRSO]] | 2023 | After |
| [[optimal-decisions-shot-taking-situations\|Yeung & Fujii]] | 2024 | After |

**Nine held sources, and this survey predates eight of them.** It is therefore not a summary of the group's work but something closer to a **charter for it** — a statement of what Fujii thought the field needed, immediately before his group produced the papers this vault is built on.

> ### `the-group's-later-work-executes-its-earlier-survey`
> **The Fujii group's held output is legible as a programme rather than a sequence of separate papers, because its stated organising commitment — that non-linear models earn their place only by yielding interpretable structure, visualisable representations, or plausible generated behaviour — is set out in a 2021 survey that precedes almost all of it.**
> ^[generated: no source states this; drawn from the survey's stated aims against the chronology of the eight later held sources. rests-on: source:fujii-survey-interpretability-aim]

This explains something the vault had recorded as a pattern without a cause: why the group's later work is so heavily weighted toward **counterfactual and interpretable constructs** — [[c-obso]]'s predicted-trajectory reference, [[drso|DRSO]]'s optimal-position reference, [[vdep|VDEP]]'s readable classifier state — rather than toward raw predictive performance. The survey argues for exactly that, in advance.

## The Two-Approach Taxonomy

| Approach | Means | Survey section |
|---|---|---|
| **(1) Extract features and rules** | Visualise learned representations; extract the mathematical structure behind the motions | §3 |
| **(2) Generate and control behaviour** | Simulate future and counterfactual behaviours to test hypotheses | §4 |

Approach 2 splits again, via a distinction borrowed from a **human-trajectory-prediction survey in robotics** (Rudenko et al., 2020) rather than from sports:

- **Pattern-based** — approximate the dynamics from data
- **Planning-based** — model explicit long-term goals, split into **inverse** (estimate the reward or action model from real data) and **forward** (assume the reward, optimise in a virtual space)

⚠️ **The vault's own forward/inverse axis descends from here, and it is borrowed twice over.** The vault organises its RL and simulation material as a forward/inverse pair on [[reinforcement-learning]], [[hiroshi-nakahara]] and [[action-valuation-frameworks-compared]]. That framing came from the Fujii group; the group took it from a robotics survey on human motion prediction.^[inferred: the vault's usage matches the survey's; the survey attributes the categorisation to Rudenko et al.]

That is worth knowing rather than alarming. But it does mean the axis is **not a finding about football** — it is an imported organising device, and its fit to football is assumed rather than argued anywhere in the chain.

## The Central Argument

The survey's thesis is a trade-off, stated plainly: non-linear data-driven models buy expressiveness and predictive performance and pay in **interpretability**. Fujii's position is that such models earn their place in sports science only if they deliver one of three things:

1. Extraction of the **mathematical structure** behind the motions
2. **Visualisation** of the learned representations
3. **Modelling components and generating plausible motions**

The justification is borrowed from cognitive science — models have value if they predict *and* explain, serving as a starting point for new theory.

**This is a normative paper wearing a survey's clothes.** It does not merely catalogue; it argues that a large part of the field is doing the wrong thing by optimising prediction alone. See [[interpretability]].

## What It Contributes to the Vault

### 1. Koopman and dynamic mode decomposition — an absent methodological line

The survey's most technically distinctive content, and **entirely missing from the vault before now**: [[koopman-mode-decomposition|DMD]], its formulation in reproducing kernel Hilbert spaces, Koopman spectral kernels as a similarity between non-linear dynamical systems, and tensor-train decomposition of adjacency-matrix series.

This is **Fujii's own line** — his work with [[naoya-takeishi]] and [[yoshinobu-kawahara]], for whom the vault held entity pages describing "the group's methodological line" without holding the methodology. Now on [[koopman-mode-decomposition]].

### 2. The permutation problem, named as a known problem

The survey states explicitly that comparing multi-agent trajectories "encounters permutation problems among the players", and catalogues **four** responses. The vault had three. See [[permutation-problem]] — and see below, because this **acts on a claim the vault flagged for resolution.**

### 3. Voronoi as the acknowledged common ancestor

The survey describes space-value work — [[wide-open-spaces-space-creation|Fernández & Bornn]] and Cervone et al.'s NBA court realty — as **extending a Voronoi diagram** (Taki & Hasegawa, 2000), and separately introduces [[beyond-expected-goals|Spearman's]] physics-based model without connecting the two.

That **independently supports** the vault's generated claim on [[pitch-control-traditions-compared]] that the two traditions are siblings framed against a common ancestor rather than rivals.^[imported: the genealogy is the survey's, describing works partly unheld here]

⚠️ **And it strengthens the absence claim rather than expiring it.** `no-held-source-compares-ppcf-and-gaussian` survives: a survey by a central figure places both traditions in the same section and **still does not compare them**. An absence that survives a dedicated review of the field is a stronger absence than one that has merely not been checked.

## What It Does *Not* Do

### It does not benchmark

A field survey by a central author, cataloguing dozens of frameworks, and **not one comparison of two methods on one dataset.** It sorts work by technique and by goal; it never asks which performs better.

> `no-cross-framework-benchmarking` **is strengthened**, and from the most awkward possible direction. The vault had explained the absence structurally — comparison needs two methods on one dataset, which licensing prevents. A *survey* faces no licensing constraint whatsoever and could have tabulated reported results. It did not.^[generated: the survey's silence on comparison is observed here; the contrast against the licensing explanation is drawn here. rests-on: absence:no-held-source-benchmarks-across-frameworks, source:fujii-survey-no-comparison]

### It does not discuss reliability

**No split-half, no test–retest, no measurement stability, anywhere.** The survey discusses validation only as "the strength of supervised learning is that the results can be clearly evaluated" — i.e. predictive accuracy against a label.

`no-reliability-for-off-ball-metrics` survives its **ninth** ingest, and this is the most telling instance yet: the field's own review does not treat metric reliability as a topic that exists. That is evidence the gap is **disciplinary rather than incidental**, and it reinforces why reliability is the overview's top acquisition priority.^[generated: rests-on: absence:no-held-source-reports-off-ball-reliability, source:fujii-survey-no-reliability-section]

### It does not close acquisition priority 3

The survey describes **Liu & Schulte** — Q-function estimation with an RNN, made interpretable via a linear model tree — which is the vault's third-ranked acquisition target, held second-hand across [[multi-agent-reinforcement-learning]].

⚠️ **A second-hand description in a survey is still second-hand.** The previous ingest found that a one-line gloss inside a held source had manufactured a finding its primary never stated, and propagated it to four pages. **This survey's description of Liu & Schulte has exactly that status.** Priority 3 stands unchanged, and the survey's account of it should be used to locate the papers, not to characterise them.

The same applies to its one-sentence treatment of **Kurach et al.** ([[google-research-football|GFootball]]), priority 4.

## The Sentence That Recontextualises Scott et al.

Network analysis of passes gets **one clause** in this survey — a single citation of Yamamoto & Yokoyama (2011), listed among "more mathematically sophisticated" rule-based approaches alongside self-similarity and group theory.

One year later, [[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] make [[social-network-analysis|pass-network topology]] **the central instrument** of their transfer study.

> **The method was peripheral in the group's own map of the field twelve months before it became load-bearing in the group's own paper.**

This is not an inconsistency; it is confirmation of the reason Scott et al. gave. SNA was selected for a **property** — insensitivity to physical differences between simulator and reality — and not because the group regarded it as a central tool. It strengthens `transfer-evidence-is-conditional-on-the-dimension-chosen` on [[domain-adaptation]]: the dimension was chosen instrumentally, and here is the group's own evidence that it was not otherwise a priority.^[generated: the juxtaposition of the survey's one clause against Scott et al.'s central use is drawn here. rests-on: source:fujii-survey-sna-single-clause, source:scott-sna-justification]

## Practical Framing

Fujii's stated applications are modest and worth recording because they are more honest than the field's usual claims:

- The most direct application is **reducing the workload of people who classify plays by watching video** — automation of annotation, not tactical revelation
- A **recommendation system** for retrieving similar plays, framed by analogy to web search
- Long-term prediction of an opponent's likely game situations for tactical planning

He names three obstacles: the **cost of location data**, which confines the field to professional sport; the expectation of **near-perfect performance** before practical adoption; and a resolution gap between research tactics and coaching tactics — noting that **baseball has a smaller gap** because its actions are discrete.

On the second obstacle he makes an argument the vault should keep: the standard should be whether these methods are **better than humans — less expensive, with fewer mistakes** — rather than whether they are nearly perfect.

## Limitations

- **A survey, so nothing here is a primary finding.** Its value is as a map and as the group's self-declared programme.
- **Sole-authored and group-centred.** Fujii cites his own work heavily — by rough count, a substantial share of the technical detail in §3.2 and §4.2 refers to papers he authored or co-authored. That is normal for a survey by a specialist and it does mean the "field" described is partly one lab's view of it.
- **Basketball and football mixed throughout**, sometimes without flagging which sport a method was validated on.
- **No comparison, no reliability, no benchmarking** — see above.
- **2021, so it predates** the entire event-modelling and LLM line the vault holds ([[nmstpp]], [[seq2event]], [[sig-model]], [[eventgpt]], [[scoutgpt]]) and does not anticipate it.

## See Also

- [[koopman-mode-decomposition]] · [[permutation-problem]] — the two concepts this ingest created
- [[keisuke-fujii]] · [[nagoya-university]] · [[atom-scott]] · [[masaki-onishi]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]]
- [[interpretability]] · [[theory-based-modelling]] · [[trajectory-prediction]] · [[imitation-learning]] · [[counterfactual-simulation]]
- [[reinforcement-learning]] · [[multi-agent-reinforcement-learning]] · [[domain-adaptation]] · [[agent-based-simulation]] · [[google-research-football]]
- [[social-network-analysis]] · [[network-theory-football-strategies]] · [[voronoi-tessellation]] · [[pitch-control-traditions-compared]] · [[pitch-control]]
- [[sequence-to-sequence-sets]] · [[graph-neural-network]] · [[message-passing]] · [[dynamic-time-warping]] · [[clustering]]
- [[action-valuation-frameworks-compared]] · [[free-parameters-load-bearing]] · [[split-half-reliability]]
