---
title: "Koopman Operator and Dynamic Mode Decomposition"
type: concept
tags: [dynamical-systems, dimensionality-reduction, interpretability, theory-based-modelling, clustering, multi-agent, sports-analytics, trajectory-prediction, metric-learning, single-source]
sources: [raw/papers/team_sports_data_analysis.md]
confidence: 0.7
provenance:
  extracted: 40%
  inferred: 14%
  generated: 12%
  imported: 33%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Koopman Operator and Dynamic Mode Decomposition

Describing a multi-agent system by the **operator that governs how it evolves**, rather than by a function fitted from states to outcomes. **Dynamic mode decomposition (DMD)** extracts from trajectory data a small set of time dynamics — each with a frequency and a growth rate — plus the coefficients that weight them.

> ⚠️ **Held second-hand.** Everything here comes from [[data-driven-team-sports-behaviors|Fujii's 2021 survey]], which describes this line rather than presenting it. **No primary source on DMD or Koopman methods is held.** The vault learned one ingest ago what a second-hand description can conceal — see [[network-theory-football-strategies]] — so claims below are marked `^[imported]` and the technical detail should be treated as a map, not as verified content.

## The Idea

DMD rests on the **spectral theory of the Koopman operator**, which represents a non-linear dynamical system as a *linear* operator acting on functions of the state rather than on the state itself.^[imported: standard framing, described in the survey; no primary held] The trade is dimensionality for linearity — the operator is infinite-dimensional, and DMD approximates its leading spectrum from data.

The practical output is a decomposition into a **small number of dynamic modes**, each carrying a frequency and a growth rate. Where PCA gives directions of variance, DMD gives *modes of motion*.

**Why it entered team sports:** the survey's complaint about PCA, t-SNE, NMF and topic modelling is that several assume independence of sampling, so **the extracted information does not reflect dynamical properties.** A football possession is not a bag of frames. DMD is the survey's answer.

## Variants the Survey Names

| Variant | Purpose |
|---|---|
| **DMD in a reproducing kernel Hilbert space** | Handles the case where raw data dimension is too small to approximate the Koopman eigenfunctions |
| **Multi-task DMD** | Supervised formulation across related systems |
| **Neural-network DMD** | Learning Koopman-invariant subspaces |
| **Tensor-train decomposition** | Applied to the adjacency-matrix series $R_K$ of a multi-agent system |

The RKHS variant is the one applied to team sports, on **sequences of inter-agent relations** — the $R_K$ matrices where $R_{i,k,l} = h(p_{i,k}, p_{i,l})$ for a distance function or Gaussian kernel $h$.

## The Koopman Spectral Kernel

The construct with the most consequence for this vault. It is a **similarity measure between multivariate non-linear dynamical systems**, which makes conventional clustering methods applicable to whole plays rather than to static feature vectors.^[imported: described in the survey; the primary works are not held]

Two properties the survey emphasises:

1. **It is permutation-invariant.** Because DMD is a dimensionality reduction like PCA, the extracted dynamical property does not depend on which player is indexed first. See [[permutation-problem]] — this is the third of four routes past that problem.
2. **It reflects dynamics rather than position.** Similarity is computed over how the configuration *moves*, not over where it *is*.

## Why This Is a Genuinely Different Object

Every other method in this vault that compares two passages of play does so by comparing **their contents**: [[dynamic-time-warping|DTW]] aligns trajectories point-to-point, Fréchet distance bounds the worst-case separation of two curves, [[sig-model|path signatures]] compute iterated integrals along a path, and the [[football-event-sequences-point-process-mixture|mixture model]] compares event sequences by fitted intensity.

**A Koopman spectral kernel compares the systems that generated them.** Two possessions with no player in the same position can be near-identical under it, if the underlying dynamics match.

> ### `dynamics-similarity-and-content-similarity-are-different-objects`
> **Comparing two passages of play by their trajectories and comparing them by their generating dynamics are distinct operations that can disagree arbitrarily, and no held source computes both on the same data. The vault's clustering and retrieval material has been implicitly assuming the first.**
> ^[generated: no source contrasts these; drawn across the survey's description of Koopman kernels and the vault's held trajectory-comparison methods. rests-on: imported:koopman-kernel-permutation-invariance, source:fujii-survey-similarity-taxonomy]

⚠️ **This claim rests partly on `imported:`,** which per the conventions can fail at any time and nothing here can check it. It should be treated as a hypothesis to test on acquisition, not a finding.

## Where It Sits Against the Held Corpus

| | Compares | Permutation-safe | Held here |
|---|---|---|---|
| **Koopman spectral kernel** | **Generating dynamics** | **Invariant** | ✗ second-hand |
| [[dynamic-time-warping\|DTW]] | Trajectories, time-warped | No | ✓ via Fujii et al. |
| Fréchet distance | Trajectories, worst-case | No | ✗ |
| [[path-signature\|Path signature]] | Path, via iterated integrals | No | ✓ via Sig-Model |
| [[graph-neural-network\|GNN]] features | Relational structure | **Equivariant** | ✓ via C-OBSO |

The survey also notes that Fréchet distance and DTW have **high computational cost and do not scale to large sports datasets**, which motivated a trie-based scalable Fréchet search — and that neural trajectory-similarity methods exist but **have not been applied to team-sport multi-agent datasets.**^[imported: the survey's assessment as of 2021]

## What It Was Used For

Two applications, both by Fujii and colleagues:^[imported: described in the survey; primaries unheld]

- **Prediction and classification of scoring probability** from collective motion dynamics
- **Classifying defensive tactics** (defending the area against defending players) and **offensive tactics** (with or without cooperation), via physically-interpretable classification of network dynamics

The second is the survey's cleanest example of its own thesis: a non-linear method whose output is a **label a coach could act on**, rather than a probability.

## Acquisition Priority

**This should be ranked.** The vault now has a named methodological line by its most central author, described only through his own survey, with a generated claim resting on an `imported:` dependency.

Highest value first:
- **Fujii, Takeishi, Hojo, Inaba & Kawahara (2020)**, *Physically-interpretable classification of network dynamics* — the fullest application, and the one whose output is directly comparable to the vault's tactical material
- **Fujii, Inaba & Kawahara (2017)**, Koopman spectral kernels — the kernel construct itself
- **Kawahara (2016)**, DMD with reproducing kernels — the methodological primary

## See Also

- [[data-driven-team-sports-behaviors|Fujii's survey]] — the only source
- [[permutation-problem]] — the invariance property, in context
- [[keisuke-fujii]] · [[naoya-takeishi]] · [[yoshinobu-kawahara]] — the line's authors
- [[dynamic-time-warping]] · [[path-signature]] · [[clustering]] · [[sig-model]] · [[representation-learning]]
- [[interpretability]] · [[theory-based-modelling]] · [[trajectory-prediction]]
- [[graph-neural-network]] · [[message-passing]] · [[tactical-analysis]] · [[action-valuation-frameworks-compared]]
