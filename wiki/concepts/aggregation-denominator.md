---
title: "Aggregation Denominator"
type: concept
tags: [reliability, evaluation, statistics, player-evaluation, model-selection, sports-analytics, selection-bias, construct-validity]
sources: [raw/papers/off-ball-defensive-performance-football.md, raw/papers/understanding-sports-metric-statistical-properties.md, raw/papers/metric-stability-elite-football.md]
confidence: 0.8
provenance:
  extracted: 48%
  inferred: 16%
  generated: 35%
  imported: 0%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Aggregation Denominator

**What you divide a season total by is a modelling choice, and it is load-bearing.**

Per 90 minutes. Per opponent pass. Per possession. Per touch. Raw total. Each answers a different question, each carries a different confound, and three separate held sources now show that **the choice can change a metric's rank order more than the metric's design does.**

This page exists because the vault treated the denominator as bookkeeping until 2026-08.

## Three Sources, One Finding

| Source | Finding |
|---|---|
| [[meta-analytics-sports-metrics\|Franks et al. (2016)]] | Win shares and VORP outscore rate metrics **"primarily because MP significantly increases their reliability, *not* because there is stronger signal about player ability."** Minutes played alone scores **0.96** on discrimination |
| [[year-to-year-metric-stability-football\|Shaikh (2026)]] | Computes Pearson on season **totals** and ICC on **per-90 rates**, and gets near-identical answers across all 24 combinations — an agreement his own transformation should have broken. See that page |
| [[off-ball-defensive-performance-blame\|Bischofberger et al. (2026)]] | Fault metrics score well **per 90** and badly **per pass**; contribution metrics the reverse. The naïve combined metrics "consequently perform weakly", and a hand-built fusion is required to recover performance |

> ### `the-denominator-is-a-free-parameter-that-nobody-lists`
> **Normalisation choices sit alongside horizons, gates and priors as asserted, consequential and unswept — but they are less visible than any of them, because a denominator reads as arithmetic rather than as a modelling decision. A metric reported per 90 and the same metric reported per possession are different constructs with different confounds, and the literature reports whichever was chosen without argument.**
> ^[generated: constructed across three sources, none of which frames the denominator as a free parameter. rests-on: source:franks-mp-inflation, source:bischofberger-aggregation-reversal, source:shaikh-passpct-conversion]

⚠️ This is an **eighth kind** for [[free-parameters-load-bearing]], and it differs from the seven already there in an important way: **Bischofberger et al. actually tested it**, and found it decisive. Most entries on that page are unswept. This one is swept, and the sweep says it matters enormously.

## Why the Reversal Happens

The two denominators encode opposite assumptions about **what a player had the opportunity to do**.

| | Per 90 minutes | Per opponent pass |
|---|---|---|
| Asks | How much did you accumulate in the time you played? | How well did you handle the situations you faced? |
| Rewards | Playing more | Efficiency per opportunity |
| Confounded by | **Team possession share** — teams that defend a lot rack up more of everything | **Team possession share, inverted** — high-possession teams face few passes, so each contribution counts more |

For **fault**, per-90 is right because mistakes accumulate: three errors in a match is worse than one, regardless of how many passes you faced. For **contribution**, per-pass is right because otherwise a team that defends constantly scores well by volume alone.

The paper's fusion is explicit about combining them:

$$I_{fused} = \frac{C_{total}}{\text{passes faced}} \times 250 - \frac{F_{total}}{\text{minutes played}} \times 90$$

⚠️ **Note the 250.** It is "a rough estimate of the typical number of passes faced in a match combined with an additional weighting factor between fault and contribution" — so a scaling constant and a relative weight, folded into one asserted number and not swept. **A fix for one free parameter that introduces another**, which is the pattern [[free-parameters-load-bearing]] documents throughout.

## The Confound Is the Same One in Both Directions

Both denominators are contaminated by **team possession share**, which is why neither escapes it.

Bischofberger et al.'s own data makes this vivid: *passes against per 90* — essentially a measure of how much your team defends — scores **1.0 on robustness and 0.9 on validity**, near the top of their table. It is a team statistic wearing a player's name, and it beats most of the sophisticated metrics.

> **A denominator cannot remove a confound it is made of.** Dividing by minutes or by passes faced normalises by quantities that are themselves determined by team quality, so both aggregations inherit the confound rather than correcting it. Removing it requires conditioning on team strength, which none of the three sources does.
> ^[generated: drawn from the paper's limitations against its results table. rests-on: source:bischofberger-passes-against-score, source:bischofberger-team-strength-confound]

See `the-most-robust-metric-may-be-measuring-the-team` on [[off-ball-defensive-performance-blame]].

## What the Vault Should Do With This

**Every reliability figure it holds is denominator-specific**, and mostly the denominator is not stated prominently:

- [[split-half-reliability]]'s xT 0.89 and VAEP 0.25 — computed on what basis, per match or per season total? The vault does not record it.
- [[year-to-year-metric-stability-football|Shaikh's]] figures are on **season totals**, which per Franks means they partly measure appearance count.
- [[hpus|HPUS]] is a possession-normalised construct and [[vaep|VAEP]] is usually summed, so **the two are not comparable on any reliability score.**

> ⚠️ Comparing metrics on reliability requires holding the denominator fixed, and no held source does this across frameworks. **A cross-framework reliability comparison that ignores aggregation is measuring aggregation.**
> ^[generated: an absence claim, newly declared. rests-on: source:franks-mp-inflation, source:bischofberger-aggregation-reversal]

## The Cheap Test

Recompute any held metric under **per-90, per-possession, per-touch and raw total**, and report the rank correlation between the four resulting player orderings.

If they agree, the denominator is inert and this page can be demoted. If they disagree — which Bischofberger et al.'s reversal suggests they will — then every single-denominator reliability figure in the literature is an artefact of an unreported choice.

**This is cheaper than any other test on [[free-parameters-load-bearing]]**: no retraining, no new data, four divisions of numbers already computed.

## See Also

- [[off-ball-defensive-performance-blame|Bischofberger et al.]] · [[meta-analytics-sports-metrics|Franks et al.]] · [[year-to-year-metric-stability-football|Shaikh]] — the three sources
- [[free-parameters-load-bearing]] — the eighth kind · [[model-selection]] · [[metric-variance-components]]
- [[metric-discrimination]] · [[metric-stability]] · [[split-half-reliability]] · [[reliability-layers]]
- [[defensive-pressure-area]] · [[role-conditioned-baseline]] · [[off-ball-value]] · [[hpus]] · [[vaep]] · [[expected-threat]]
- [[construct-validity]] · [[selection-bias]] · [[recruitment]]
