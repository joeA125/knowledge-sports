---
title: "The Validity and Reliability of Live Football Match Statistics From Champdas Master Match Analysis System"
type: summary
tags: [reliability, evaluation, statistics, sports-analytics, event-stream-data, player-evaluation, tactical-analysis, model-selection]
sources: [raw/papers/stats_reliability_football_champdas.md]
confidence: 0.85
provenance:
  extracted: 68%
  inferred: 16%
  generated: 13%
  imported: 2%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# The Validity and Reliability of Live Football Match Statistics From Champdas Master System

**Bingnan Gong, Yixiong Cui, Yang Gai, Qing Yi & Miguel-Ángel Gómez.** *Frontiers in Psychology* 10:1339, 11 June 2019. Beijing Sport University and Universidad Politécnica de Madrid.

> **The vault's first source on measurement reliability — and it is not the one the overview has been asking for.**
>
> `no-reliability-for-off-ball-metrics` has topped the acquisition list for nine ingests. This paper reports reliability, in depth, with proper psychometrics. **It measures a different layer**, and separating the two is the main thing this ingest contributes. See [[operator-reliability]].

## Two Layers of Reliability, Previously Conflated

| | **Coding layer** | **Metric layer** |
|---|---|---|
| Question | Does the same match yield the same event log? | Does the same player yield the same rating across samples? |
| Threatened by | Operator error, definition ambiguity, camera angle | Rare events, small samples, genuine inconsistency |
| Statistic | Kappa, ICC, typical error | Split-half $\rho$, test–retest |
| **This paper** | ✅ **Measured** | ✗ |
| [[on-ball-actions-football-xt-vs-vaep\|Van Roy et al.]] | ✗ | ✅ Measured for xT and VAEP |
| [[obso\|OBSO]], [[c-obso\|C-OBSO]], [[drso\|DRSO]], [[space-occupation-gain\|SOG]] | ✗ | ✗ — **the standing gap** |

**The absence claim survives**, and is now sharper: the vault can state precisely which layer is unmeasured for the off-ball metrics, rather than treating "reliability" as one undifferentiated hole.

## What Was Measured

Two studies in one paper.

### Content validity — 20 coaches, 31 variables

Twenty professional coaches from China, Spain, Portugal, Germany and Ireland, averaging **13.3 ± 7.1 years** of experience and holding UEFA-Pro, UEFA-A, AFC-A or AFC-B licences, rated each variable on two 1–10 scales: is it **correctly defined**, and is it **pertinent** to match performance.

Scored with **Aiken's V**, against an exact critical value of **0.52** computed for 20 judges at $p < 0.05$:

| | Aiken's V |
|---|---|
| Pertinence | **0.84 ± 0.03** |
| Definition | **0.85 ± 0.03** |

Comfortably above threshold across all 31 variables.

### Operator reliability — one match, four operators, two passes

Four operators from Champion Technology (experience 1.5–2 years) coded the **La Liga match Real Madrid vs Villarreal, 13 January 2018**, from conventional TV coverage, **twice, two weeks apart**. They worked in two independent pairs; within each pair one coded and one checked, so the pair reports as a single operator.

| | Kappa | ICC | Standardised TE |
|---|---|---|---|
| **Intra-operator** (1st vs 2nd pass) | 0.87–0.93 | **0.98–1.00** | **0.01–0.15** |
| **Inter-operator** (Op1 vs Op2) | 0.89, 0.97 | **0.93–1.00** | **0.01–0.29** |

Mean event-coding time differences were **under one second** in all three comparisons. Roughly 5,430 events agreed for Real Madrid and 4,065 for Villarreal.

**Defending-and-goalkeeper actions are the weakest cell throughout** — inter-operator ICC 0.93 and 0.95, TE 0.29 and 0.24, against 1.00 and 0.01 for passing. Attacking sits between.

## ⚠️ The Abstract Misreports the Paper's Own Reliability Coefficients

Checked against **Table 5 directly**, not against the results prose.

| Quantity | Abstract says | Table 5 says |
|---|---|---|
| Intra-operator ICC | 0.93–1.00 | **0.98–1.00** |
| Intra-operator TE | 0.01–**0.34** | **0.01–0.15** |
| Inter-operator ICC | **0.90**–1.00 | **0.93–1.00** |
| Inter-operator TE | 0.01–0.24 | **0.01–0.29** |

**The values 0.34 and 0.90 appear nowhere in Table 5.** The abstract also appears to have imported the inter-operator ICC floor (0.93) into the intra-operator row. The Kappa figures disagree too: the abstract gives Operator 1 as 0.92 and 0.90, the results text as 0.91 and 0.93.

The direction of error is not uniform — intra-operator reliability is understated, inter-operator TE is overstated — so this reads as transcription failure rather than favourable rounding.

> ### `a-paper's-abstract-is-a-secondary-source-for-its-own-results`
> **An abstract is a summary written by hand and checked less carefully than the tables it summarises, so it fails in the same ways any secondary description fails. A claim read from an abstract has the epistemic status of a claim read from a citing paper, even though it carries the primary's own name.**
> ^[generated: no source states this; drawn from this discrepancy against the citation errors found in the two preceding ingests. rests-on: source:gong-abstract-table5-mismatch]

This is the **third instance in three ingests** of a summary misreporting a primary: [[network-theory-football-strategies|Scott et al. garbling a citation and manufacturing a finding]], [[data-driven-team-sports-behaviors|a survey describing works second-hand]], and now a paper's own abstract misreporting its own table. The pattern is not about dishonesty. **It is about which text gets checked.**

Practical consequence for this vault: **anything extracted from an abstract should be verified against the body**, and where only an abstract is available the claim is closer to `imported:` than to `extracted:`.

## Cross-System Comparison: The Genuinely Awkward Result

The authors compare their operators' output against **OPTA Sports** for the same match. Agreement is described as acceptable across variables **except for a discrepancy in short passes**, and they report disagreement on **passing directions and pass lengths** in both the OPTA comparison *and* their own inter-operator analysis.

The diagnosis is definitional and mechanical:

- **Short/long pass disagreement with OPTA comes from a different length threshold.** Total pass counts were similar across systems — the passes are the same, the labels differ.
- **Direction errors come from plotting on a miniaturised on-screen pitch.** The system auto-classifies a pass as lateral when the angle to the sideline parallel is under 15°, so a small marking error flips the category. The authors note the same problem in ProZone MatchViewer and Trakperformance.

> **High within-system reliability and cross-system disagreement on the same quantity are compatible**, and both were observed here. Two operators using one definition agree; two systems using different definitions do not.
> ^[generated: the paper reports both but does not frame them as compatible. rests-on: source:gong-opta-short-pass-discrepancy]

⚠️ **This bears directly on [[spadl|SPADL]] and on every held source that pools providers.** SPADL exists to unify event vocabularies across providers, and the unification is at the level of *action type*. A pass is a pass in both systems. **The disagreement here is in the attributes** — length band, direction category — which is exactly where a metric like [[expected-threat|xT]] or [[vaep|VAEP]] draws its features.

## What This Settles for the Vault

### It removes a candidate explanation for VAEP's low reliability

[[split-half-reliability]] records VAEP at $\rho = 0.25$ against xT's 0.89, and decomposes it as $\rho = \sigma^2_\theta / (\sigma^2_\theta + 2\sigma^2_\varepsilon/n)$. **Event-coding noise would sit inside $\sigma^2_\varepsilon$.**

Two arguments now rule it out as the cause, one of which does not depend on this paper at all:

1. **Internal.** xT and VAEP were computed on the *same event stream*. Coding noise would depress both. xT returns 0.89.
2. **External, from this paper.** Coding noise is small in absolute terms in the one system where it has been measured — standardised TE at or below 0.29, ICC at or above 0.93.

^[inferred: the transfer to Van Roy et al.'s data is an inference. Gong et al. measure Champdas; Van Roy used a different provider. The bound is suggestive, not a measurement on their data]

So the low $\rho$ is **either genuine game-to-game variation in what players did, or instability in the metric's construction** — which is exactly the disjunction [[within-season-variation-noise-or-signal]] is built to resolve. One branch of that question is now closed.

### It supplies a validation mode the vault keeps meeting

Aiken's V over an expert panel is **content validity by expert assent** — the same mode as [[wide-open-spaces-space-creation|Fernández & Bornn's]] expert video review, and the same as the tactical task's style-recognition validation noted on [[action-valuation-frameworks-compared]].

Three appearances across unrelated lines makes this a **recurring validation strategy rather than a one-off compromise**, used wherever the target quantity has no observable ground truth. It deserves recording as such on [[construct-validity]].^[generated: the vault had recorded the three instances separately. rests-on: source:gong-aiken-v, source:fb-expert-review]

**Note what makes this instance different and better:** Gong et al. validate *definitions*, where ground truth genuinely cannot exist, and then measure *agreement* separately with hard statistics. Fernández & Bornn use expert assent for the quantity itself. Expert panels are the right tool for the first job and a fallback for the second.

## Limitations

- **One match.** The authors name this first: more matches are needed to test generalisation. Every reliability figure rests on Real Madrid vs Villarreal, 13 January 2018.
- **One system, and not one this vault's sources use.** Champdas serves the Chinese Super League, China League, Chinese youth divisions and the Korean K-League. The held football corpus runs on StatsBomb, Wyscout, Opta and [[data-stadium]]. **The reliability figures do not transfer by right.**
- **TV coverage, not stadium feed.** The authors cite camera angle, image size and blur as degrading location accuracy — a limitation absent from tracking-based pipelines.
- **Trained operators from the vendor**, coding a system their employer sells, with 1.5–2 years' experience. Not blinded to the system, though blinded to the study purpose.
- **Two operators, reported as two, from four people.** Each "operator" is a coder plus a checker, so the inter-operator figure compares two *pairs* — a more favourable configuration than two individuals.
- **Kappa on event-type agreement** rewards the many easy, frequent categories. Passing dominates the counts and drives the aggregate; the sparse defensive categories are where agreement falls, and they are averaged away.
- **The abstract's numbers are wrong**, above.

## See Also

- [[operator-reliability]] — the concept this ingest created, and the layer distinction
- [[split-half-reliability]] · [[within-season-variation-noise-or-signal]] · [[predictive-validity]] · [[construct-validity]] · [[performance-volatility]]
- [[champdas-master-system]] · [[data-stadium]] · [[stats-perform]]
- [[event-stream-data]] · [[spadl]] · [[expected-threat]] · [[vaep]] · [[action-valuation]]
- [[selection-bias]] · [[probability-calibration]] · [[model-selection]] · [[action-valuation-frameworks-compared]]
- [[on-ball-actions-football-xt-vs-vaep|Van Roy et al. Summary]] · [[network-theory-football-strategies]] · [[data-driven-team-sports-behaviors]]
