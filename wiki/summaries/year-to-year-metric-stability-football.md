---
title: "Year-to-Year Metric Stability in Elite Football: A Positional Analysis Across the Big Five European Leagues"
type: summary
tags: [reliability, evaluation, statistics, player-evaluation, recruitment, model-selection, sports-analytics, event-stream-data, selection-bias]
sources: [raw/papers/metric-stability-elite-football.md]
confidence: 0.8
provenance:
  extracted: 66%
  inferred: 12%
  generated: 20%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Year-to-Year Metric Stability in Elite Football

**Mohammad Arshan Shaikh**, sole author. Khoury College of Computer Science, Northeastern University. SportRxiv preprint, submitted to *Journal of Sports Analytics*. 8,207 player-season pairs, 2,866 players, Big Five leagues, 2017/18–2024/25.

> ### The vault's first football stability figures — and they falsify a claim made one ingest ago
>
> [[metric-stability]] stated flatly that **"no football metric in this vault has a stability figure at all."** That is now false for 24 position-metric combinations.
>
> ⚠️ **It remains true for every metric the vault actually cares about.** These are FBref counting statistics — goals, assists, tackles, progressive passes. Not [[vaep|VAEP]], not [[expected-threat|xT]], and not one of the six tracking-derived mechanisms on [[off-ball-value]]. `no-reliability-for-off-ball-metrics` **survives its thirteenth ingest.**

## The Headline Numbers

Pearson $r$ between season $t$ and $t+1$, pooled over seven season pairs.

| Tier | Metrics | $r$ |
|---|---|---|
| **Very high** | Pass completion % (MID and DEF) | **0.824** |
| **High** | Progressive carries, progressive passes, passes into box — **forwards only** | 0.707–0.733 |
| **Moderate** | xG (0.664), npxG (0.635), SCA, key passes, **goals (0.593)**, GCA | 0.52–0.68 |
| **Low** | **Assists (0.428)**, interceptions (0.476), **tackles won (0.414)**, **blocks (0.399)** | < 0.50 |

Two orderings matter more than the absolute values.

## 1. Process Beats Outcome — the First Evidence for an Untested Vault Claim

Progressive carries and passes (0.71–0.73) are more stable than xG (0.664), which is more stable than goals (0.593), which is more stable than assists (0.428).

**The gradient runs from what a player *did* to what *resulted*.**

> [[split-half-reliability]] lists three routes to a more reliable metric. Route 2 — **withhold outcome information**, value the decision rather than the result — has been flagged **untested** since it was written, and [[intent-vs-outcome-valuation]] carries it as an open proposal.
>
> **This is the first empirical support for it in the vault.** Not a direct test: these are different metrics, not one metric computed with and without outcome information. But the predicted ordering is exactly what appears, across three separate comparisons, on $n = 1{,}925$ forwards.
> ^[generated: no source connects this ordering to the intent-vs-outcome proposal. rests-on: source:shaikh-table2-fwd-ordering, claim:withholding-outcome-should-raise-reliability]

Assists at 0.428 is the sharpest case, and the author's explanation is the vault's: the metric credits one player for an outcome jointly produced by the finisher's conversion.

## 2. Defensive Metrics Are Worst — Converging With a Different Layer Entirely

Tackles won (0.414), blocks (0.399) and interceptions (0.476) are the three least stable combinations in the dataset.

> ### `defensive-actions-degrade-at-every-measurement-layer`
> **Defensive event counts are simultaneously the hardest to code consistently and the least stable year to year. Two independent studies, measuring different properties at different layers with different statistics on different data, both find defensive actions worst. The difficulties are not independent — they compound in the metrics built on them.**
> ^[generated: no source connects the two results; the convergence is observed here. rests-on: source:gong-table5-defensive-cell, source:shaikh-defensive-volume-low-stability]

The two results are genuinely independent:

| | [[champdas-validity-reliability\|Gong et al. (2019)]] | Shaikh (2026) |
|---|---|---|
| Layer | **Coding** — do two analysts agree? | **Metric** — does it repeat next season? |
| Statistic | ICC, typical error | Pearson $r$ |
| Defensive result | **ICC 0.93–0.95, TE 0.24–0.29** — worst cell | **$r$ 0.40–0.48** — worst cell |
| Passing result | **ICC 1.00, TE 0.01** — best cell | **$r$ 0.824** — best cell |

**Both find passing best and defending worst, by the same ordering.** See [[reliability-layers]] and [[operator-reliability]].

⚠️ **This is the second leg of `proxy-substitution-trades-statistical-power-for-measurement-noise`.** [[rare-event-proxy-targets]] records that [[vdep|VDEP]] escapes goal sparsity by moving to ball recoveries — events 90× more frequent and, per Gong et al., harder to code. **They are also the least stable events in football.** The trade buys positives and pays twice.

The author's explanation is team context: a defender's tackle count depends on how often the team defends, how high the line sits, and whether teammates win the ball first. **That is not measurement error — it is the metric measuring the team.**

## 3. Age: Veterans Are More Stable, Except in Defence

Veterans (30+) are more stable than U23 players in **18 of 24** combinations at $p < 0.05$, 15 at $p < 0.001$. For forwards the effect is large — progressive passes 0.621 → 0.825, xG 0.643 → 0.809.

⚠️ **Defenders are the exception**: only pass completion reaches significance; tackles, blocks, interceptions and clearances show no age effect at all.

The author's reading is consistent and worth keeping: **if team context dominates individual signal, there is nothing for age-related convergence to act on.** A metric that mostly measures the team does not stabilise as the player matures.^[extracted: the author states this]

> **This bears directly on [[empirical-bayes-shrinkage]].** Shrinking a player's estimate toward his career average assumes a stable underlying level. **U23 players are precisely where that assumption is weakest** — and precisely where a scouting operation most wants a projection. Shrinkage would improve their reliability scores by suppressing genuine developmental change. See [[player-development-curve]].
> ^[generated: no source connects the age result to shrinkage. rests-on: source:shaikh-fisher-z-age, claim:shrinkage-buys-reliability-by-assuming-the-answer]

## 4. Stability Decays, Universally

All 24 combinations decline from $t \to t+1$ to $t \to t+2$; median $\Delta r = -0.097$. Defensive volume counts fall below 0.30. See [[stability-decay]].

## ⚠️ Two Verifiable Mis-Citations

The vault holds one of the papers being cited, so this is checkable rather than suspected.

**Franks et al. (2016)** is described as having *"investigated the repeatability of various metrics in ice hockey"*. [[meta-analytics-sports-metrics|The vault holds it]]: it covers **NBA and NHL**, and the basketball analysis is the more extensive of the two — 70 NBA metrics against 40 NHL. Describing it as an ice-hockey paper drops the larger half.

**Brooks et al. (2016)** is twice described as a **baseball pitch-type reliability** study. The reference list gives *"Developing a data-driven player ranking in soccer using predictive model weights"*, SIGKDD — **a soccer paper, not baseball**. The claim attributed to it in §4.1, about outcome metrics involving other players' contributions, is not what that paper is about.

> **Fourth instance in five ingests**, and the first the vault can verify from both ends: [[network-theory-football-strategies|a citing paper garbling a name and manufacturing a finding]], [[data-driven-team-sports-behaviors|a survey describing works second-hand]], [[champdas-validity-reliability|an abstract misreporting its own table]], and now a preprint mis-describing two sources it builds on.
>
> `a-paper's-abstract-is-a-secondary-source-for-its-own-results` generalises: **any characterisation of another work, anywhere in a paper, has the epistemic status of a secondary description.** Holding the primary is the only check, and the vault now demonstrates that twice.
> ^[generated: the generalisation is drawn across four instances. rests-on: source:shaikh-franks-icehockey-miscitation, source:shaikh-brooks-baseball-miscitation]

## ⚠️ A Methodological Concern About the Headline Result

Pass completion percentage is the paper's central finding, and its handling is odd.

§2.2 states that PassPct **"was converted to a seasonal total by multiplying by 90-minute equivalents (90s) prior to analysis."** A completion *percentage* multiplied by minutes played is not a meaningful quantity — it is dominated by playing time, and playing time is strongly autocorrelated between seasons.

**[[meta-analytics-sports-metrics|Franks et al.]] warn about exactly this**: win shares and VORP outscore rate metrics *"primarily because MP significantly increases their reliability, not because there is stronger signal about player ability."* Minutes played scores 0.96 on discrimination in their data.

The paper anticipates the objection — ICC was computed on per-90 rates specifically to strip playing time. But the two estimates then agree to three decimal places across essentially every row (PassPct 0.824 / 0.823; PrgP 0.709 / 0.709). **If one statistic is computed on a playing-time-inflated variable and the other on a clean rate, near-identity across all 24 combinations is unexplained.**

^[generated: the paper reports both columns and the transformation but does not reconcile them; the concern is raised here and is not a demonstrated error. rests-on: source:shaikh-passpct-conversion, source:shaikh-table2-pearson-icc-agreement]

Either the transformation is inert, or both statistics are computed on the same scaling, or something else is going on. **It is not resolvable from the text**, and it affects the paper's headline claim. The analysis code is public, which makes it checkable by anyone who wants to.

## Limitations

- **Not the metrics the vault cares about.** FBref counting statistics, no tracking data, no value model.
- **Within-league design excludes transfer years** — the author states this. So these are *within-context* stability estimates, and the recruitment use case is largely cross-context.
- **Three coarse position buckets.** Central defenders and full-backs are one group; wingers and central midfielders another. The author names this.
- **No modelling of temporal trends** in stability across the eight seasons.
- **Sole-authored preprint**, submitted but not yet peer-reviewed. Given the two mis-citations and the transformation question, that matters.
- **Unmeasured confounders acknowledged**: mid-season injury, managerial change, squad-role transitions — all of which would depress within-player consistency independently of the metric.

## See Also

- [[metric-stability]] — the property, and the page this ingest corrects · [[stability-decay]] · [[metric-discrimination]] · [[metric-independence]] · [[metric-variance-components]]
- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the framework this paper applies · [[reliability-layers]] · [[split-half-reliability]] · [[operator-reliability]]
- [[champdas-validity-reliability|Gong et al.]] — the coding-layer convergence · [[gps-deceleration-reliability]]
- [[intent-vs-outcome-valuation]] · [[empirical-bayes-shrinkage]] · [[player-development-curve]] · [[performance-volatility]] · [[within-season-variation-noise-or-signal]]
- [[rare-event-proxy-targets]] · [[vdep]] · [[gvdep]] · [[defensive-valuation]] · [[off-ball-value]]
- [[recruitment]] · [[capability-profiling]] · [[expected-goals]] · [[vaep]] · [[expected-threat]] · [[event-stream-data]]
- [[mohammad-arshan-shaikh]] · [[construct-validity]] · [[predictive-validity]] · [[selection-bias]]
