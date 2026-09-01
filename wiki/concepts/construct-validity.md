---
title: "Construct Validity"
type: concept
tags: [construct-validity, evaluation, predictive-validity, reliability, player-evaluation, sports-analytics, action-valuation, off-ball, statistics, needs-review]
sources: [raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/beyond_expected_goals.md, raw/papers/football-performance-time-series.md, raw/papers/stats_reliability_football_champdas.md, raw/papers/wide_open_spaces_creation_football.md, raw/papers/understanding-sports-metric-statistical-properties.md]
confidence: 0.7
provenance:
  extracted: 42%
  inferred: 33%
  generated: 20%
  imported: 5%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-07
updated: 2026-08-29
---

# Construct Validity

Whether a metric measures the thing it claims to. Judged not by accuracy against ground truth — there usually is none — but by the **pattern** of its agreement and disagreement with other metrics.

Two halves, and the second is the one this literature keeps stumbling into:

- **Convergent** — the metric agrees with other measures of the *same* construct.
- **Discriminant** — the metric *diverges* from measures of *different* constructs.

## Why It Matters Here Specifically

Every off-ball metric in this vault has the same problem: **there is no ground truth for off-ball contribution.** Nobody observes the true value of a run. [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] state it flatly — since there is no true Q-value, model performance cannot be directly validated.

So the field validates by correlation with whatever else exists. That is construct validation whether or not anyone calls it that, and it is worth having the vocabulary, because **the same correlation can be reported as a success or a failure depending on which half you invoke**, and papers here choose without arguing for the choice.

## The Move That Prompted This Page

Nakahara et al. report $\rho = -0.761$ between their metric and season goals, and present this as evidence the method works.

The argument is discriminant: goals measure finishing, the metric measures off-ball and distributive contribution, so a *negative* relationship confirms it is picking up what goal-counting misses. Their ranking is topped by two defenders with zero goals between them, one of whom was second in the league for passes.

That argument is legitimate. It is also **unfalsifiable as deployed**, which is the problem.

> ### `discriminant-claims-need-a-convergent-anchor`
> **A metric validated only by divergence from existing measures cannot be distinguished from a metric measuring nothing. Noise also diverges from goals.**
> ^[generated: no source states this; drawn here from Nakahara et al.'s validation strategy. **Independently corroborated 2026-08-29** — see below. rests-on: source:nakahara-negative-goal-correlation, source:nakahara-no-ground-truth]

### ✅ Corroborated From the Other Side

> **Added 2026-08-29** on ingest of [[meta-analytics-sports-metrics|Franks et al. (2016)]], which reaches the same wall from the *internal* direction.

Franks et al. define three measurable properties of a metric — discrimination, stability and [[metric-independence|independence]] — and then note the limit of all three:

> an athlete's birthplace zip code would be perfectly discriminative, stable and independent from all other metrics, but is clearly irrelevant

They propose **relevance** as a needed fourth criterion, and do not build it.

**Two routes, one conclusion.** The vault argued that divergence cannot establish value because noise diverges too; Franks et al. argue that internal statistical quality cannot establish value because structured noise passes all three tests. The claim above was generated here from a single football paper's validation strategy — **it is now supported by an independent methodological source that never mentions football.**

That is a stronger position than the claim had, and it reframes what the vault already holds: [[obso|OBSO]]'s next-match goal prediction (0.26, beating shots and goals) is a **relevance** result, and remains the only one in this vault against an external criterion.

A metric needs at least one thing it is *supposed* to agree with, specified in advance. Nakahara et al. offer none: they diverge from goals (−0.761), from expert ratings (−0.218), from [[obso|OBSO]] (−0.305), and are unrelated to [[c-obso|C-OBSO]] (0.182). **Every comparison they report is a divergence, and all four are read as favourable.**

Their strongest available convergent evidence is not a correlation at all but a face-validity check: Thiago and Hatanaka, both highly ranked, were both among the league's top passers, and Thiago won a league best-XI award. That is real evidence. It is also two players.

## The $\rho = 0.182$ Problem

The C-OBSO comparison is the sharpest case in the vault, because here the discriminant reading is much harder to sustain.

[[c-obso|C-OBSO]] and Nakahara's Q-values are both from the Fujii group, both computed on **Yokohama F. Marinos, J1 2019, [[data-stadium|Data Stadium]] tracking**, both explicitly presented as measures of **off-ball contribution**. They correlate at 0.182.

| Reading | Claim | Status |
|---|---|---|
| **Benign** (the paper's) | They measure different *aspects* — C-OBSO the creator, Q-values the distributor and mover | Plausible; supported by which players each favours |
| **Harsh** (not considered) | "Off-ball contribution" is not one construct, so at least one metric is named more broadly than it measures | Equally consistent with the evidence |

The paper's own explanation concedes most of the harsh reading. If C-OBSO ranks forwards and Q-values rank midfielders and defenders, then neither measures off-ball contribution *as such* — each measures a positional slice of it. **The honest report would rename both**, or state the construct as position-conditional.

### ✅ There Is a Test, and It Was Available All Along

> **Added 2026-08-29** on ingest of [[meta-analytics-sports-metrics|Franks et al. (2016)]].

The vault had treated this as undecidable from the evidence. It is not. Franks et al. observe that blocks and rebounds score highly on metric-quality measures **largely because they indicate player position rather than ability** — and that meta-metrics must therefore be recomputed conditional on player type.

Applied here:

| Result | Reading |
|---|---|
| Both metrics remain **discriminative within position groups** | They measure position-conditional skill, and should be reported that way. The benign reading holds |
| Discrimination **collapses within groups** | They were measuring position all along. The harsh reading holds |

**One computation separates two readings the vault had recorded as equally consistent with the evidence.**^[generated: no source applies the conditional-meta-metric argument to this case. rests-on: source:franks-conditional-metametrics, source:nakahara-cobso-correlation]

Add [[metric-independence|independence]] over the full set — C-OBSO, the Q-values, [[obso|OBSO]] and [[space-occupation-gain|SOG]] on shared players — and the third explanation below becomes testable too.

**A third explanation exists and is more mundane.** The four held sources on this dataset each **subset it differently** — C-OBSO uses shot-ending sequences only, Nakahara et al. attacking-third possessions regardless of outcome. Metrics computed over different populations of moments need not agree, and no comparison controls for this. See [[data-stadium]].

This is the vault's first head-to-head between two off-ball metrics, and it is not reassuring. See [[off-ball-value]].

## The Three Validity Criteria, Compared

| | [[split-half-reliability\|Reliability]] | [[predictive-validity]] | **Construct validity** | **Content validity** |
|---|---|---|---|---|
| Asks | Is it stable? | Does it forecast? | Is it *the thing*? | Is it **defined** right? |
| Needs | Repeated samples | A future outcome | Other metrics | **An expert panel** |
| Can be gamed by | A degenerate constant | A proxy for the outcome | **Divergence from everything** | A friendly panel |
| Reported by | [[player-rating-time-series\|Mendes-Neves et al.]], [[on-ball-actions-football-xt-vs-vaep\|Van Roy et al.]] | [[obso\|OBSO]] (0.26 next-match goals) | Nakahara et al., [[c-obso\|C-OBSO]] | [[champdas-validity-reliability\|Gong et al.]] |

⚠️ **Reliability is itself three things** — coding, instrument, metric — measured by different statistics and failing for different reasons. The column above is the metric layer. See [[reliability-layers]].

These are not substitutes and none is sufficient alone. The strongest result in the vault — OBSO predicting next-match goals at 0.26, beating shots and goals themselves — is *predictive*, against an external outcome, and that is why it is the strongest.

**No held source reports all three for one metric.** Nakahara et al. report construct evidence only; [[c-obso|C-OBSO]] reports construct evidence (salary, expert ratings) only; neither reports reliability, consistent with the standing absence claim on [[off-ball-value]].

## The Salary and Expert-Rating Route

Both papers reach for external quantities as stand-ins for ground truth.

- **Annual salary** ([[c-obso|C-OBSO]], $\rho = 0.45$) — reflects the market's judgement, confounded by age, position, nationality and contract timing.
- **Expert match ratings** (both papers) — reflect journalists' judgement, and are known from C-OBSO's own tables to be **heavily goal-driven**.

Neither is ground truth; both are *other raters*. Agreement is convergent evidence against a noisy human criterion, and disagreement is ambiguous between "the metric sees what humans miss" and "the metric is wrong". C-OBSO's salary result is the more useful mainly because it was the only positive result among three tested on one sample, which makes it harder to attribute to fishing.

## Expert Assent Is a Recurring Mode, Not a One-Off Compromise

> **Added 2026-08-29** on ingest of [[champdas-validity-reliability|Gong et al. (2019)]]. The vault had recorded three instances separately without noticing they were the same move.

| Instance | Panel | What was judged |
|---|---|---|
| [[champdas-validity-reliability\|Gong et al.]] | **20 licensed coaches**, 13.3 ± 7.1 yrs experience | Whether each of 31 event **definitions** is correct and pertinent — Aiken's V 0.84–0.85 against a 0.52 critical value |
| [[wide-open-spaces-space-creation\|Fernández & Bornn]] | Two FC Barcelona analysts | Whether the **space-value surface itself** looks right, by video review |
| Tactical description | Implicit — the reader | Whether [[social-network-analysis\|pass-network]] numbers **recognisably match a known style** (Spain, *tiki-taka*) |

Three appearances across unrelated research lines makes this a **standard instrument** wherever the target quantity has no observable ground truth — which, per the top of this page, is most of what this vault holds.

**But the three are not equally sound, and the difference is what gets judged.**

> ### `expert-panels-validate-definitions-well-and-quantities-poorly`
> **Asking experts whether a definition is correct is the right use of a panel: definitional adequacy has no ground truth even in principle, so expert consensus is not a substitute for a measurement but the actual criterion. Asking experts whether a computed quantity is right is a fallback, because there the panel stands in for a measurement that could exist and has not been taken.**
> ^[generated: no source distinguishes these uses; drawn across the three instances. rests-on: source:gong-aiken-v, source:fb-expert-review]

Gong et al. is the good case, and it is instructive **because it does both jobs separately**: coaches rate the definitions, and then agreement on applying those definitions is measured with Kappa, ICC and typical error. Expert judgement establishes *what should be counted*; hard statistics establish *whether it was counted consistently*. Fernández & Bornn have no such second step available, because there is no ground truth for space value — and they say so explicitly.

**The tactical case is the weakest**, because the assent is never elicited. A reader recognising *tiki-taka* in a table of clustering coefficients is doing uncontrolled, unblinded, post-hoc validation with a sample of one. See [[action-valuation-frameworks-compared]], where the tactical task is the only one of seven validated this way.

⚠️ **Practical consequence for reading this vault:** where a metric's only validation is expert assent to *the quantity*, the honest status is "not yet validated" rather than "validated differently". Where the assent is to a *definition*, it is doing legitimate work and should not be discounted.

## What Would Settle It

1. **Specify a convergent target in advance.** For a metric claiming to value distribution and movement, pass volume, progressive distance carried, or [[space-occupation-gain|SOG]] are all candidates. Nakahara et al. *observe* their top players are top passers — pre-registering that as the test would convert an anecdote into evidence.
2. **Report reliability alongside.** A metric that diverges from everything *and* is unstable across splits is noise. This distinguishes the two readings above and nothing else does.
3. **Compare on identical samples** — and on identical *subsets*. Already possible: C-OBSO, the Q-values, [[obso|OBSO]] and [[space-occupation-gain|SOG]] could be computed on the same 14 players over the same possessions, and the correlation matrix would say whether "off-ball value" has one factor or several.

The third is the closest thing to a decisive, cheap test the vault has identified for off-ball valuation, and no source has run it.

## The Same Problem Elsewhere

Wherever a construct is unobservable, validation reduces to a correlation matrix among instruments. See [[capability-profiling]] for the version of this inside the vault — reporting a decomposition rather than a composite is one response to it.

## See Also

- [[predictive-validity]] · [[split-half-reliability]] · [[capability-profiling]] · [[uncertainty-quantification]] · [[probability-calibration]]
- [[off-ball-value]] · [[c-obso]] · [[obso]] · [[space-occupation-gain]] · [[action-valuation]] · [[action-space-design]] · [[data-stadium]]
- [[player-rating-time-series]] · [[performance-volatility]] · [[selection-bias]] · [[recruitment]] · [[dynamic-time-warping]]
- [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO]] · [[beyond-expected-goals|OBSO]]
