---
title: "Metric Independence"
type: concept
tags: [evaluation, statistics, reliability, construct-validity, player-evaluation, model-selection, dimensionality-reduction, sports-analytics]
sources: [raw/papers/understanding-sports-metric-statistical-properties.md]
confidence: 0.8
provenance:
  extracted: 52%
  inferred: 18%
  generated: 28%
  imported: 2%
  ambiguous: 0%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Metric Independence

**Does a metric tell you anything the metrics you already have do not?**

Formally, one minus the $R^2$ of regressing one metric on a set of others:

$$\mathcal{I}_{m\mathcal{M}} = C_{m,m} - C_{m,\mathcal{M}} C_{\mathcal{M},\mathcal{M}}^{-1} C_{\mathcal{M},m}$$

where $C$ is the latent correlation matrix of a Gaussian copula fitted over all metrics. $\mathcal{I} = 1$ means fully independent; $\mathcal{I} \approx 0$ means redundant. From [[meta-analytics-sports-metrics|Franks et al. (2016)]].

> **The vault had no word for this.** [[split-half-reliability]] asks whether a metric is stable, [[predictive-validity]] whether it forecasts, [[construct-validity]] whether it measures the named thing. **None asks whether it is worth adding to a set you already have** — which is the question a decision-maker facing six off-ball mechanisms actually has.

## Why It Is the Missing Third Question

The copula construction matters and is not incidental. Football metrics have wildly different supports — probabilities on the unit interval, counts, real-valued differences, per-90 rates. A raw correlation matrix across them is not well defined. Fitting dependence on **latent normal scores** via semiparametric rank likelihood sidesteps estimating each marginal, and handles missing values, which is exactly the situation when different metrics cover different players.

**So the computational obstacle the vault might have expected is already solved.** What is missing is the data: several metrics computed over a shared set of players.

## It Is the Quantitative Form of Discriminant Validity

[[construct-validity]] distinguishes convergent from discriminant evidence, and records that papers here invoke whichever half flatters them without arguing for the choice. `discriminant-claims-need-a-convergent-anchor` is the vault's response.

**Independence makes the discriminant half measurable.** Instead of "our metric diverges from goals, therefore it sees something goals miss", one computes how much variance in the new metric survives conditioning on every existing metric — and reports a number.

⚠️ **It does not rescue the discriminant argument.** A metric of pure noise scores $\mathcal{I} = 1$. Franks et al. name this directly: birthplace zip code would be perfectly discriminative, stable and independent, and useless. Independence establishes *novelty*, never *value*. See the `relevance` discussion on [[meta-analytics-sports-metrics]].

## The Result Most Likely to Transfer

Independence curves — independence as a function of how many other metrics you condition on — have different **shapes** for different metric types:

| Type | Curve | First PC of five such metrics |
|---|---|---|
| **Omnibus / composite** (WS, VORP, PER, BPM) | Roughly **linear** — partially correlated with nearly everything | **73%** |
| **Defensive** (DBPM, STL, BLK, DWS, DRtg) | **Concave** — correlated with a small set, independent of the rest | **51%** |

The reading: composite metrics are functions of the others, so they shed independence gradually as the conditioning set grows. Defensive metrics are correlated with a few close relatives and nothing else — so they drop sharply once those relatives are included, and hold their independence otherwise.

> ### `defensive-metrics-carry-more-independent-information-than-composite-ones`
> **A composite metric is redundant by construction: it aggregates the things it would be conditioned on. Defensive and off-ball measures sit in a sparser region of the metric space, so each new one is more likely to add information — and correspondingly less likely to be checkable against anything.**
> ^[inferred: the paper's result is NBA and NHL; the transfer to football's off-ball corpus is drawn here. rests-on: source:franks-independence-curves]

If that transfers, it cuts both ways for this vault. [[off-ball-value]]'s six mechanisms are in the sparse region, so **the field's proliferation there is more defensible than the vault's "too many metrics, no comparison" framing suggests.** But sparseness is also why [[construct-validity|nothing anchors them]] — there is little to converge with.

## The Redundancy Result

Across 70 NBA metrics, **15 principal components explain over 75%** of the variation. Across 40 NHL metrics, 15 explain over 90%.

Applied to football, this is the quantitative version of a complaint the vault makes qualitatively on [[action-valuation-frameworks-compared]]: seven tasks, dozens of frameworks, and no account of how much of it is the same information renamed. **Nobody has computed the number.**

⚠️ **No held football source reports independence, or any measure of redundancy across metrics.**^[generated: an absence claim, newly declared. rests-on: source:franks-pca-redundancy] It is the cheapest of the three meta-metrics to run — it needs no bootstrap and no repeated seasons, only several metrics over one shared set of players.

## The Test the Vault Can Almost Run

[[construct-validity]] already proposes computing [[c-obso|C-OBSO]], the RL Q-values, [[obso|OBSO]] and [[space-occupation-gain|SOG]] on the same players over the same possessions and inspecting the correlation matrix. **Independence is what to compute from that matrix**, and it converts the proposal from "see what it looks like" into a reported statistic.

The existing evidence is one cell of it: C-OBSO and the Q-values at $\rho = 0.182$ on the same club and season. On its own that reads as either different constructs or instability. **With an independence score over the full set, plus discrimination computed within position groups, the two readings separate.**^[generated: the combination is proposed here]

## Principal-Component Metrics

Franks et al. also use the eigendecomposition of $C$ constructively: the principal components of the latent correlation matrix are, by construction, **mutually independent metrics** with $\mathcal{I} = 1$ against each other.

Their NBA components come out interpretable — "efficient shooters", "shooters and assisters", "high usage" — and their NHL first component is offensive, which they read as evidence that **a disproportionate share of available hockey metrics measure offence.**

That diagnostic is directly portable. Running it over football's metric set would say, in one number, how much of the corpus is measuring attacking contribution — a claim the vault currently supports only by enumeration on [[action-valuation]].

⚠️ The cost is interpretability: the authors concede the meaning of these constructed metrics is harder to determine. See [[capability-profiling]], which argues the opposite direction — report a decomposition rather than a composite.

## See Also

- [[meta-analytics-sports-metrics|Franks et al. (2016)]] — the source
- [[reliability-layers]] · [[split-half-reliability]] · [[construct-validity]] · [[predictive-validity]] · [[capability-profiling]]
- [[off-ball-value]] · [[defensive-valuation]] · [[c-obso]] · [[obso]] · [[space-occupation-gain]] · [[vaep]] · [[expected-threat]]
- [[action-valuation]] · [[action-valuation-frameworks-compared]] · [[model-selection]] · [[recruitment]]
- [[luke-bornn]] · [[daniel-cervone]] · [[alexander-franks]]
