---
title: "Meta-Analytics: Tools for Understanding the Statistical Properties of Sports Metrics"
type: summary
tags: [reliability, evaluation, statistics, player-evaluation, model-selection, sports-analytics, construct-validity, predictive-validity, uncertainty-quantification, selection-bias, dimensionality-reduction]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md]
confidence: 0.9
provenance:
  extracted: 74%
  inferred: 11%
  generated: 14%
  imported: 1%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Meta-Analytics: Tools for Understanding the Statistical Properties of Sports Metrics

**[[alexander-franks|Alexander Franks]], [[alex-damour|Alexander D'Amour]], [[daniel-cervone|Daniel Cervone]] & [[luke-bornn|Luke Bornn]].** *Journal of Quantitative Analysis in Sports* 12(4):151–165, 2016. NBA and NHL. **No football.**

> **This is the framework the vault's top acquisition priority needs**, and the first source held here that treats metric quality as a measurable property rather than a rhetorical claim. It supplies definitions, estimators and a worked demonstration — but no football numbers, so `no-reliability-for-off-ball-metrics` **survives its twelfth ingest.**

## ⚠️ Two of the Authors Are Already in This Vault

[[daniel-cervone|Cervone]] and [[luke-bornn|Bornn]] are authors on [[expected-value-possession-framework|the EPV framework]]; Bornn is also on [[wide-open-spaces-space-creation|Wide Open Spaces]], the origin of the [[space-occupation-gain|SOG]] line and one of the vault's two pitch-control traditions.

**They co-wrote the standard for testing whether a metric is stable, and neither of their football papers applies it.**

> ### `the-framework-and-the-metrics-share-authors-and-never-meet`
> **The absence of reliability figures in football valuation is not explained by the field lacking the tools. Two of the four authors who defined how to measure metric quality went on to build football value surfaces and reported none of these properties for them. The gap is a norm about what a football paper contains, not a methodological deficit.**
> ^[generated: no source observes the overlap; drawn from the author lists against the held football papers. rests-on: source:franks-author-list, source:fernandez-bornn-wide-open-spaces, source:cervone-bornn-epv]

This sharpens what the vault had recorded as a disciplinary gap on [[reliability-layers]]. It is not that football analytics is unaware of psychometrics — **the same people work in both registers and do not carry one into the other.**

## The Three Meta-Metrics

Each now has its own page: **[[metric-discrimination|discrimination]]**, **[[metric-stability|stability]]**, **[[metric-independence|independence]]** — all three ratios of the four variances on [[metric-variance-components]].

All three are R-squared–style statistics, functions of variance components in a players × seasons × metrics array.

| | Question | What it partitions |
|---|---|---|
| **Discrimination** $\mathcal{D}_m$ | Does the metric separate players *within* a season? | Between-player variance, into true differences and chance |
| **Stability** $\mathcal{S}_m$ | Does it measure the same thing *over* time? | Total variance (chance removed), into within-player and between-player |
| **Independence** $\mathcal{I}_{m\mathcal{M}}$ | Does it say anything new? | Variance in one metric, conditional on a set of others |

$$\mathcal{D}_{sm} = 1 - \frac{E_{sm}[V_{spm}[X]]}{V_{sm}[X]} \qquad \mathcal{S}_m = 1 - \frac{E_m[V_{pm}[X] - V_{spm}[X]]}{V_m[X] - E_m[V_{spm}[X]]}$$

Under a linear mixed model these reduce to readable ratios: $\mathcal{D}_m = \frac{\sigma^2_{PM} + \sigma^2_{SPM}}{\sigma^2_{PM} + \sigma^2_{SPM} + \tau^2_M}$ and $\mathcal{S}_m = \frac{\sigma^2_{PM}}{\sigma^2_{PM} + \sigma^2_{SM} + \sigma^2_{SPM}}$.

**Estimation is nonparametric and cheap.** Sampling variance $V_{spm}$ comes from a bootstrap — resample a team's games with replacement, recompute end-of-season metrics, take the sample variance. Everything else is sample moments. Independence uses a Gaussian copula fitted by semiparametric rank likelihood, so it handles the mixed discrete/continuous/bounded supports that real metrics have.

## ⚠️ The Correction This Forces on the Vault

**Discrimination and stability are different quantities, and the vault has been treating them as one.**

[[split-half-reliability]] records VAEP at $\rho = 0.25$ against xT's $0.89$, from splitting *one season* into two random halves. Under this framework that is an estimate of **discrimination** — within-season, between-player, chance removed. It is **not** stability, which is explicitly between-season.

The absence claim reads *"no held source reports split-half or test–retest reliability"* — phrasing that treats those as two ways of measuring one property. They are two different properties:

| Vault phrasing | Franks equivalent | Held for football? |
|---|---|---|
| Split-half reliability | **Discrimination** | ✅ xT and VAEP only |
| Test–retest reliability | **Stability** | ❌ **nothing, for any football metric** |
| — | **Independence** | ❌ **nothing, and the vault had no word for it** |

**So the gap is larger than recorded, not smaller.** The vault has one of three properties, for two of its metrics, and no vocabulary for the third. See [[reliability-layers]], which now carries both axes — three *layers* and, within the metric layer, three *properties*.

## Independence, and Why It Settles an Open Vault Problem

$\mathcal{I}_{m\mathcal{M}}$ is one minus the $R^2$ of regressing metric $m$ on a set of others, computed on latent normal scores. **It is the quantitative form of discriminant validity**, which [[construct-validity]] has been arguing about qualitatively.

Franks et al. find steals the most independent NBA metric ($\mathcal{I} \approx 0.40$ against 69 others) and takeaways the most independent in the NHL ($0.73$ against 39). Both are **defensive**.

More usefully for this vault, the *shape* of the curves differs by metric type:

- **Omnibus metrics** (WS, VORP, PER, BPM) have roughly **linear** independence curves — they are partially correlated with nearly everything, being functions of it. First principal component of those five explains **73%**.
- **Defensive metrics** have **concave** curves — correlated with a small set and independent of the rest. First PC of five defensive metrics explains only **51%**.

> **Defensive metrics carry more independent information than composite ones, and the vault's corpus is composite-heavy.** See [[defensive-valuation]] and [[off-ball-value]].
> ^[inferred: the football application is drawn here; the paper's result is NBA]

⚠️ Across all 70 NBA metrics, 15 principal components explain over 75% of the variation. **The clutter is mostly redundant**, which is the paper's motivating complaint and applies directly to the seven-task proliferation on [[action-valuation-frameworks-compared]].

## The Fourth Criterion the Paper Names and Does Not Build

The discussion contains a caveat that matters more to this vault than the framework does:

> an athlete's birthplace zip code would be perfectly discriminative, stable and independent from all other metrics, but is clearly irrelevant for determining a player's value

Meta-metrics measure **internal quality only.** They say nothing about whether a metric measures anything worth measuring. Franks et al. propose **relevance** as a needed fourth criterion — qualitative, or a causal/predictive relationship to an outcome like wins — and do not develop it.

> **This independently corroborates a claim the vault generated by a different route.** [[construct-validity]] carries `discriminant-claims-need-a-convergent-anchor`: a metric validated only by divergence cannot be distinguished from noise. Franks et al. reach the same wall from the internal side — three properties that noise-with-structure could pass, and an admitted need for an external anchor.
> ^[generated: neither source cites the other; the convergence is observed here. rests-on: source:franks-zip-code-caveat, claim:discriminant-claims-need-a-convergent-anchor]

Two routes, one conclusion, and it strengthens the vault's version. It also reframes [[obso|OBSO]]'s next-match goal prediction (0.26, beating shots and goals): that is a **relevance** result, and it remains the only one in the vault against an external criterion.

## A Third Route to a More Reliable Metric

[[split-half-reliability]] lists two routes — restrict scope (tested, 0.25 → 0.59) and withhold outcome information (untested). Franks et al. supply a third, demonstrated:

**Empirical Bayes shrinkage toward a player's own career average improves discrimination *and* stability simultaneously.** Three-point percentage — the worst metric they examine, at $\mathcal{D} = 0.43$, $\mathcal{S} = 0.30$, with **over half of between-player variance being chance** — rises to $0.53$ and $0.64$ under a hierarchical Beta-Binomial model.

The estimator is **biased** for every player and better anyway, by the standard variance-reduction argument.

> **This route is post-hoc and requires no change to the metric.** The other two require redesigning what is measured. For a vault whose complaint is that six off-ball mechanisms have no reliability figures, a technique that improves reliability *without touching the mechanism* is unusually cheap.
> ^[generated: the contrast against the vault's two existing routes is drawn here. rests-on: source:franks-empirical-bayes-3pt]

## Two Warnings That Land Directly on Football

### Rate versus total, and playing time as a reliability inflator

Metrics incorporating total minutes (WS, VORP) score higher than rate metrics (WS/48, ORtg, BPM) — and the authors are blunt that this is **not** because they carry more signal about ability:

> WS and VORP are more reliable than the rate based statistics primarily because MP significantly increases their reliability, *not* because there is stronger signal about player ability

Minutes played is itself highly discriminative (0.96) and unstable (0.40). **Any football metric summed over a season inherits the reliability of appearance count.**

⚠️ This bears on [[recruitment]] and on the vault's habit of comparing season-aggregated ratings. It also means **rate and total metrics should not be compared on these scores at all** — a comparison the vault would otherwise have been tempted to make across, say, [[hpus|HPUS]] and season-summed [[vaep|VAEP]].

### Meta-metrics must be computed conditional on player type

Blocks and rebounds score highly on both properties largely because they **indicate position**, not ability. Computed within centres, they would score far lower. The authors are explicit that meta-metrics should be recomputed for the relevant subset.

> **This supplies the missing methodological response to the $\rho = 0.182$ problem.** [[off-ball-value]] records C-OBSO and the RL Q-values agreeing at 0.182 on the same club and season, with the paper's explanation being that one ranks forwards and the other midfielders and defenders. [[construct-validity]] notes the harsh reading — that neither measures off-ball contribution *as such*.
>
> Franks et al. say what to do about it: **compute the meta-metrics conditional on position.** If both metrics remain discriminative within position groups, they measure position-conditional skill and should be reported that way. If discrimination collapses within groups, they were measuring position all along.
> ^[generated: no source applies this to the football case. rests-on: source:franks-conditional-metametrics, source:nakahara-cobso-correlation]

That is a concrete, runnable test for a problem the vault had characterised as unresolvable from the available evidence.

## Limitations

- **No football, and no tracking data.** NBA and NHL box-score and model-derived metrics from 2000 onwards. Every number here is imported context, not evidence about football.
- **Stability requires multiple seasons per player**, which the vault's football sources mostly do not have — C-OBSO uses one season, Nakahara et al. 14 players from one club, SOG a single match. **The framework is cheap; the data to run its stability half is not held.**
- **The linear mixed model is illustrative, not assumed** — the authors note it is inappropriate for many metrics and use nonparametric estimation instead. Convenient, but it means the readable variance-ratio interpretations are heuristic.
- **Relevance is named and not built.** The one criterion that would connect these properties to whether a metric is worth having is left as future work.
- **Bootstrap over games** assumes games are exchangeable within a season — questionable where opponent strength, home advantage and scoreline effects are strong, all of which apply in football.
- **2016**, so it predates the tracking-derived football metrics the vault most wants it applied to.

## See Also

- [[reliability-layers]] — now carrying layers *and* properties · [[split-half-reliability]] · [[operator-reliability]]
- [[metric-discrimination]] · [[metric-stability]] · [[metric-independence]] · [[metric-variance-components]] · [[empirical-bayes-shrinkage]] — the five concepts this ingest created
- [[construct-validity]] · [[predictive-validity]] · [[within-season-variation-noise-or-signal]] · [[performance-volatility]] · [[capability-profiling]]
- [[alexander-franks]] · [[alex-damour]] · [[daniel-cervone]] · [[luke-bornn]] · [[kirk-goldsberry]]
- [[expected-value-possession-framework]] · [[wide-open-spaces-space-creation]] · [[space-occupation-gain]] · [[obso]] · [[c-obso]] · [[off-ball-value]] · [[defensive-valuation]]
- [[vaep]] · [[expected-threat]] · [[recruitment]] · [[model-selection]] · [[selection-bias]] · [[uncertainty-quantification]]
- [[action-valuation-frameworks-compared]] · [[on-ball-actions-football-xt-vs-vaep]] · [[champdas-validity-reliability]] · [[gps-deceleration-reliability]]
