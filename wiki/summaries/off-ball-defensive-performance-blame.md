---
title: "Blame is easier than praise: Measuring off-ball defensive performance in football"
type: summary
tags: [reliability, evaluation, statistics, off-ball, defensive-valuation, optical-tracking-data, player-evaluation, model-selection, counterfactual, construct-validity, tactical-analysis, event-stream-data, sports-analytics]
sources: [raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.85
provenance:
  extracted: 68%
  inferred: 11%
  generated: 19%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Blame is Easier Than Praise: Measuring Off-Ball Defensive Performance

**Jonas Bischofberger\*, Runqing Ma\*, [[pascal-bauer|Pascal Bauer]], Kilian Arnsmeyer & Arnold Baca** (\* equal contribution). University of Vienna, Universität des Saarlandes, DFB, VfB Stuttgart. arXiv:2606.19931, June 2026.

> ## ✅ This substantially closes the vault's top acquisition priority
>
> `no-reliability-for-off-ball-metrics` has topped the list for **thirteen ingests**. This is the first held source to compute [[metric-discrimination|discrimination]]- and [[metric-stability|stability]]-type statistics **for tracking-derived off-ball metrics**, and it cites [[meta-analytics-sports-metrics|Franks et al. (2016)]] as the source of the framing.
>
> Three robustness measures, across ~32 metrics and three competitions:
>
> | | What it measures | Franks equivalent |
> |---|---|---|
> | $ICC_{Match}$ | Match-by-match repeatability and discriminatory power | **Discrimination** |
> | $r_{Repeat}$ | First half of season against second half | Split-half at season scale |
> | $ICC_{Season}$ | Bootstrapped robustness of season aggregates | Sampling variance, Franks' own estimator |
>
> ⚠️ **It does not close it completely, for one specific reason.** See *The Numbers Are Standardised Away*, below.

## The Model

The perspective shift is the contribution. Rather than measuring defence from **recorded defensive actions** — tackles, interceptions, pressures — the paper evaluates defenders through the **attacking team's passing outcomes**, distributing blame and credit for every opponent pass.

That inverts the standard framing's blind spot. Action-anchored metrics cannot see a defender who is out of position, fails to cover, or leaves a lane open, **because no action is recorded.** The paper's motivating example is Virgil van Dijk, second in the 2019 Ballon d'Or, sitting in the 11th, 19th and 11th percentiles for tackles, interceptions and blocks.

| Stage | Mechanism |
|---|---|
| Pass value | [[expected-threat\|xT]] on a 16×12 grid, fitted independently on StatsBomb open World Cup data |
| Defensive value | $xDT = -\Delta xT$ for successful passes; $xT_{start}$ for unsuccessful ones |
| Attribution | [[defensive-pressure-area\|Defensive Pressure Areas]] — geometric, 5 m radius, four shapes by pass outcome |
| Expectation | [[role-conditioned-baseline\|Responsibility]] — average involvement per (passer role, receiver role, defender role) triplet |
| Aggregation | Per 90 minutes **and** per opponent pass, then fused |

Six base metrics fall out — raw and valued **contribution**, **fault**, and **involvement** — each computable in involvement or responsibility form, each aggregated two ways.

## ✅ The Derivation-Depth Claim Is Confirmed on Football Tracking Data

The vault generated `each-layer-of-derivation-costs-reliability` from a **GPS deceleration study**, and flagged it explicitly as an extrapolation across domains that was not a measurement.

**This paper measures it, in football, on off-ball tracking metrics**, and states the result directly:

> We find two properties of our metrics that regulate them towards higher stability: Adding a location-independent responsibility and removing pass value. Both approaches allow for lower variance on the event level and thus lead to more easily repeatable metrics while retaining the ability to distinguish players.

**"Removing pass value" is removing a derivation layer.** Raw metrics — proximity without xT weighting — are consistently more robust than valued metrics, which multiply proximity by a modelled quantity. In the summary table, *raw* beats *valued* on robustness almost everywhere.

> The claim is no longer an extrapolation. It holds for the exact class of construct the vault most cares about, measured by people who were not testing it.
> ^[generated: the paper does not connect its finding to any general principle about derivation depth. rests-on: source:bischofberger-raw-vs-valued-robustness, claim:each-layer-of-derivation-costs-reliability]

⚠️ **And it comes with the nuance the vault's version lacked.** *Valued fault per 90* has the **highest validity of any metric tested** and **below-average robustness**. Derivation costs reliability *and buys something* — the vault's version treated the cost as pure loss.

**The trade is real and it is a trade.** See [[metric-independence]] on why internal properties cannot settle this alone.

## ✅ And Shrinkage-Like Smoothing Is Confirmed Too

The second stabilising property — *adding a location-independent responsibility* — is [[empirical-bayes-shrinkage|shrinkage]] in all but name. Responsibility replaces a player's actual measured involvement with **the average involvement of players in his role**, in that passer-receiver-role configuration, across the competition.

That is pooling toward a group mean, and it does what pooling does: **raises robustness while retaining the ability to distinguish players.**

⚠️ **It is also precisely the prior the vault flagged as dangerous.** [[empirical-bayes-shrinkage]] warns that a **position-group mean** bakes positional structure into the metric. Here that is not a bug — the paper *wants* role-conditioning, because defensive responsibility is genuinely role-determined. **The same operation is contamination in one framing and the entire point in another**, and which it is depends on whether role is a confounder or the construct.^[generated: no source frames responsibility as shrinkage. rests-on: source:bischofberger-responsibility-stability, claim:shrinkage-buys-reliability-by-assuming-the-answer]

## ⚠️ The Zip-Code Problem, Appearing in Real Data

[[meta-analytics-sports-metrics|Franks et al.]] warned that birthplace zip code would score perfectly on all internal properties and be useless, and proposed *relevance* as a missing fourth criterion.

**Look at what tops the robustness column here:** *Passes Against per 90* — a "traditional" metric that counts how many passes the opponent plays against your team — scores **1.0 robustness and 0.9 validity**, beating nearly every sophisticated metric in the paper.

It is close to a pure measure of **how much your team defends**. It is stable, discriminative, and correlates with market value — because good players play for good teams.

> ### `the-most-robust-metric-may-be-measuring-the-team`
> **A team-level quantity attached to a player's name will outscore genuine individual metrics on every internal property, because team identity is more stable and more discriminative than individual skill. Robustness and validity scores computed against team-correlated external anchors cannot detect this, and will actively reward it.**
> ^[generated: the paper reports the figure and separately notes the possession confound in limitations, but does not connect them. rests-on: source:bischofberger-passes-against-score, source:franks-zip-code-caveat]

The authors half-see it — limitations note that *passes against per 90* correlates strongly negatively with market value and that per-90 fault metrics are therefore inflated for teams that defend a lot. **But it sits in their results table as a high scorer rather than as a warning.**

This is the sharpest instance yet of `discrimination-rewards-measuring-position` — here at team rather than positional level. See [[construct-validity]].

## The Numbers Are Standardised Away

⚠️ **The single most frustrating thing about this paper for the vault's purposes.**

$ICC_{Match}$, $r_{Repeat}$ and $ICC_{Season}$ are all computed. They are then combined into a **z-scored composite** before being reported:

$$Robustness = \frac{z(ICC_{Match}) + z(r_{Repeat}) + z(ICC_{Season})}{3}$$

**Figures 7 and 8 report the composite, not the components.** So the vault learns that *raw fused responsibility* is 0.6 standard deviations above the mean of the metrics considered — and **not whether its ICC is 0.3 or 0.9.**

> A z-score is a ranking, not a measurement. If every metric in the set is unreliable, the best one still scores +1. **The absolute question — is any off-ball defensive metric reliable enough to make a decision on — is computed in this paper and not reported.**
> ^[generated: the observation is drawn here; the paper does not remark on the loss. rests-on: source:bischofberger-equation-8]

So the absence claim **narrows sharply rather than closing**: the vault now holds relative robustness rankings for tracking-derived off-ball metrics, and still no absolute ICC or correlation for any of them. The code is public, which makes recovering them possible.

⚠️ Note also that $r_{Repeat}$ is **excluded for the World Cup** — too few matches per team when halved — so the stability-like evidence rests on the Frauen-Bundesliga and 3. Liga.

## ✅ It Also Benchmarks Across Frameworks

`no-held-source-benchmarks-across-frameworks` has survived every ingest by the same argument: comparison needs two methods on one dataset, and licensing prevents anyone holding both.

**This paper compares tackles won, tackles won ratio and interceptions against its own ~29 metric variants, on three shared datasets, with a common evaluation protocol.** That is a real cross-framework benchmark.

⚠️ **The claim survives, narrowly.** The comparison is *new metrics against traditional counting stats*, not [[obso|OBSO]] against [[c-obso|C-OBSO]] against [[space-occupation-gain|SOG]]. None of the vault's six off-ball mechanisms appears. But the structural excuse is now weaker — **it was done, on proprietary multi-league data, by a group with club and federation access.**

The result is worth recording regardless: *interceptions per 90* scores **−0.3 on validity**, and *tackles won per 90* **−0.6**. The two most widely used defensive metrics in football are, on this evidence, close to uninformative about defensive positioning.

## Other Findings

**Fault beats contribution, hence the title.** The authors read this as intrinsic to defending: a defender can perform well for 89 minutes and one mistake concedes. Blame is more measurable than praise.

⚠️ **Several contribution metrics correlate *negatively* with external ratings** — higher contribution, lower rating. The authors do not explain this. A plausible reading is that high contribution counts indicate a player *repeatedly placed in defensive situations*, which is a team-quality signal inverted.^[inferred: offered as a mechanism; the paper reports the reversal without diagnosis]

**Role-dependence is severe, and they have a clean example.** Kylian Mbappé at the 2022 World Cup ranks **third best defending winger** by raw fault and **third weakest** by raw fault responsibility. Same player, two closely related metrics, opposite conclusions — because responsibility charges him for passes he *should* have defended given his role and did not go near.

The authors note this contrasts with offensive metrics like pass completion and xT, where one metric works across roles. **Defensive metrics may be irreducibly role-specific**, which bears directly on the ρ = 0.182 problem on [[off-ball-value]].

**Discrimination depends on the league, not just the metric.** The novel metrics beat traditional ones in the World Cup and Frauen-Bundesliga but only marginally in the 3. Liga. The authors attribute this to skill spread — 2.03 vs 0.79 points per match between best and worst in the 3. Liga, against 2.73 vs 0.18 in the Frauen-Bundesliga.

> **A metric's discrimination score is a property of the metric *and* the population.** Franks' $\mathcal{D}$ is a ratio in which between-player variance sits in the numerator, so a homogeneous league mechanically depresses it. **Discrimination figures are not comparable across competitions**, and the vault should not treat a single number as a property of a metric.
> ^[generated: the paper offers the skill-gap explanation but does not draw the consequence for cross-competition comparison. rests-on: source:bischofberger-3liga-result, source:franks-discrimination-definition]

## Limitations

- **Robustness components not reported**, above.
- **No ground truth**, stated explicitly, citing Davis et al. (2024). Validity rests on **market values and FIFA "defensive awareness" ratings** — both crowd- or scout-sourced, both confounded by age, popularity and halo effects. The authors name the halo problem and restrict correlation analysis to central defenders to mitigate it.
- **Team strength confounds everything.** Per-90 metrics punish teams that defend a lot; per-pass metrics favour high-possession teams. The authors say comparisons between the two aggregations "require caution".
- **The 5 m DPA radius is handcrafted**, deliberately: they argue that fitting it against unreliable benchmarks would overfit and reproduce the benchmark's biases. **An unusually well-argued free parameter** — see [[free-parameters-load-bearing]].
- **xT's cross-gender and cross-level generalisation is unknown**, and they say so. The women's and 3. Liga metrics are built on a value model fitted to men's World Cup data.
- **Event data is manually coded** by provider operators, and **no validation study exists for the tracking systems used.** The authors flag this. See [[operator-reliability]] and [[reliability-layers]].
- **~12% of matches missing** from both league datasets due to data quality issues.
- **Sample after filtering is small**: 60 central defenders in the World Cup, 47 in the Frauen-Bundesliga, 81 in the 3. Liga.

## See Also

- [[defensive-pressure-area]] · [[role-conditioned-baseline]] · [[aggregation-denominator]] — the three concepts this ingest created
- [[metric-discrimination]] · [[metric-stability]] · [[metric-independence]] · [[metric-variance-components]] · [[reliability-layers]] · [[meta-analytics-sports-metrics]]
- [[off-ball-value]] · [[defensive-valuation]] · [[c-obso]] · [[drso]] · [[space-occupation-gain]] · [[vdep]] · [[gvdep]]
- [[expected-threat]] · [[karun-singh]] · [[counterfactual-baseline]] · [[construct-validity]] · [[predictive-validity]]
- [[jonas-bischofberger]] · [[pascal-bauer]] · [[arnold-baca]] · [[optical-tracking-data]] · [[operator-reliability]]
- [[free-parameters-load-bearing]] · [[empirical-bayes-shrinkage]] · [[action-valuation-frameworks-compared]] · [[rare-event-proxy-targets]]
