---
title: "Is within-season variation noise or signal?"
type: question
tags: [reliability, volatility, evaluation, predictive-validity, player-evaluation, statistics, time-series, sports-analytics, needs-review]
sources: [raw/papers/on-ball-actions-football-xt-vs-vaep.md, raw/papers/football-performance-time-series.md, raw/papers/test_retest_reliability_soccer_positioning_and_movement.md, raw/papers/stats_reliability_football_champdas.md]
confidence: 0.75
provenance:
  extracted: 30%
  inferred: 65%
  ambiguous: 5%
lifecycle: draft
created: 2026-07-27
updated: 2026-08-29
---

# Is within-season variation noise or signal?

**Status:** Open, but **analytically sharper than the vault has recorded** — the two positions turn out to be measuring the same quantity, which makes the question decidable by a single test.

Two held pages take opposite views of the same numbers.

**[[split-half-reliability]]** splits a season's matches at random, computes player ratings on each half, and correlates. [[vaep]] scores $\rho = 0.25$ against [[expected-threat|xT]]'s $0.89$, and VAEP is marked down accordingly. **Within-season variation is treated as measurement error.**

**[[performance-volatility]]** takes the same match-to-match variation and treats it as a **property of the player** — consistency against streakiness — proposing downside deviation, residualised against the player's own trend, as a squad-building input.

Both cannot be wholly right about the same variance.

## They Are Measuring the Same Quantity

Model a player's per-match value as $X_i = \mu_p + \varepsilon_i$, with $\mu_p$ the player's underlying level and $\varepsilon_i$ match-to-match departure from it. Each random half of $n$ matches gives a mean of roughly $\mu_p + \varepsilon/\sqrt{n/2}$, so the across-player correlation is approximately

$$\rho \approx \frac{\sigma^2_{\text{between}}}{\sigma^2_{\text{between}} + 2\sigma^2_{\varepsilon}/n}$$

**Split-half reliability is low precisely when $\sigma_\varepsilon$ is high.** And $\sigma_\varepsilon$ — the spread of departures from a player's own level — is what volatility metrics measure.

Two consequences follow, and neither is stated in the vault.

**The two metrics are near-deterministic functions of each other**, given between-player spread and sample size. Reporting both as independent findings overstates the evidence.

**Random splitting matters.** Because each half samples across the whole season, a *slow* form trend affects both halves alike and does not depress $\rho$. What depresses $\rho$ is high-frequency, match-to-match variation. So split-half reliability specifically penalises **erratic** variation, not development or form cycles — which is exactly the component volatility metrics isolate by residualising against trend.

So this is not a dispute about different quantities. It is a dispute about **whether $\varepsilon$ is exchangeable noise or a stable player property.**

## ⚠️ A Third Possibility Has Been Eliminated

> **Added 2026-08-29** on ingest of three reliability sources across three layers. See [[reliability-layers]].

The framing above quietly folded a third option into "noise": that $\varepsilon$ is **measurement error** — miscoded events, instrument drift, processing artefacts — rather than anything about the player at all. That option is now closed from two directions.

**Coding error is ruled out, decisively and without needing a new source.** xT and VAEP were computed on the **same event stream**. Coding noise would depress both. xT returns 0.89. [[champdas-validity-reliability|Gong et al.]] add the absolute bound: operator agreement runs at ICC ≥ 0.93.

**And where the instrument has been separated from the athlete, the athlete was the unstable part.** [[gps-deceleration-reliability|Jones et al. (2024)]] measured maximal deceleration with a 47 Hz radar *criterion* device and found test–retest ICC of only **0.62–0.78**, attributing the error to the player's *deceleration strategy* rather than to measurement technique. Under maximal effort, in a standardised protocol, on the same surface a week apart, an elite academy player does not reproduce his own braking.

**So both surviving options are about the player.** The question is cleaner than it was:

| | Before | Now |
|---|---|---|
| $\varepsilon$ is measurement error | Implicitly live | ❌ **Eliminated** |
| $\varepsilon$ is exchangeable within-player noise | Live | Live |
| $\varepsilon$ is a stable player property | Live | Live |

⚠️ **This does not decide the remaining dispute, and it is worth being precise about why.** Jones et al. show that $\varepsilon$ is *large* and *athlete-originated*. They do **not** show that a player's $\varepsilon$ *magnitude* is a stable trait — which is exactly what the test below asks. A player who brakes inconsistently may be inconsistently inconsistent.

The transfer is also an inference: a maximal physical capacity under laboratory-like control is not an in-match value metric.^[inferred: no source connects the deceleration result to metric-level reliability]

**What it does change is the prior.** The field, and this vault, have leaned on the noise reading — [[recruitment]] rests on it. The one place where instrument and athlete have been cleanly separated in football, the athlete moved more. That is weak evidence, but it points the opposite way from the default assumption.

## And the Missing Numbers Are Probably Worse Than Assumed

[[reliability-layers]] records a regularity from the same source: **reliability falls with derivation depth.** Directly sampled velocity reproduces at CV 1.4%; quantities two transformations away fall to ICC 0.48–0.57.

Every off-ball metric with no reported $\rho$ — [[obso|OBSO]], [[c-obso|C-OBSO]], [[drso|DRSO]], [[space-occupation-gain|SOG]] — sits **further from its sampled signal** than any variable in that comparison. C-OBSO runs tracking → trajectory prediction → pitch control → scoring surface → counterfactual difference.

That is an extrapolation across domains and is explicitly not a measurement.^[generated: the extrapolation is drawn here; declared on [[reliability-layers]]. rests-on: source:jones-table1-reliability-column] But it argues the standing gap is **more** consequential, not less — the metrics with no reliability figure are the ones most likely to have a poor one.

## The Test That Settles It

If $\varepsilon$ is noise, it is exchangeable, and a player's volatility in one half of a season tells you nothing about the other half. If $\varepsilon$ is a property, volatility persists.

**Compute the split-half reliability of the volatility metric itself.**

- **High** — volatility is a real, measurable player characteristic. The "noise" reading is wrong, and [[vaep]]'s low $\rho$ is partly measuring genuine player inconsistency rather than metric failure. VAEP would be partially exonerated.
- **Low** — volatility is not a stable property, the "signal" reading is wrong, and volatility metrics should not inform recruitment.

The test is cheap, needs no new data, and uses machinery both sides already have. That nobody has run it is the notable part: **each side would have to apply the other's tool to its own claim.**

## A Second, Complementary Test

The [[action-valuation-frameworks-compared|synthesis]] has carried a different formulation for several entries: does short-term deviation from a player's long-term level **predict next-match contribution** beyond the long-term average alone?

That asks whether $\varepsilon$ is *forecastable*, which is stronger than whether it is *stable*. A player could be reliably streaky without their streaks being predictable in advance.

Run in order, the two are informative jointly:

| Volatility replicates? | $\varepsilon$ forecasts? | Reading |
|---|---|---|
| No | No | Pure noise. Volatility metrics should be dropped |
| **Yes** | No | A real trait, not exploitable. Useful for squad *risk*, not selection |
| Yes | **Yes** | Form is real and predictable — the strongest case, and the most surprising |
| No | Yes | Incoherent; would indicate a bug |

The second row is the most likely and is the useful answer: it would justify [[performance-volatility]] for portfolio construction while denying it any role in match-by-match prediction.

## Why This Matters Beyond the Two Pages

The [[recruitment]] recommendation currently rests on the noise reading. xT is preferred over VAEP for season-long decisions *because* it replicates better. If VAEP's low $\rho$ is substantially genuine player inconsistency, that recommendation is measuring the wrong thing — VAEP would be reporting real variation and being penalised for it.

It also bears on every reliability figure in the vault. **No off-ball or defensive metric has a reported $\rho$**, and if $\rho$ conflates metric noise with player inconsistency, those missing numbers would be harder to interpret than their absence suggests.

There is now a **third test**, cheaper than either above and not requiring the volatility literature at all: **code $n$ matches twice and compute VAEP under each coding**, then correlate player ratings. That bounds how much of $\varepsilon$ is inherited from the coding layer. The internal argument above says the answer should be "almost none" — this would measure it. Proposed on [[operator-reliability]].

## Why Nobody Has Done It

The two positions come from different papers with different purposes — [[on-ball-actions-football-xt-vs-vaep|Van Roy et al.]] are critiquing a metric, [[football-performance-time-series|Mendes-Neves et al.]] are building one — and neither cites the other's framing of the variance.

The contradiction is only visible from holding both. Like [[observed-versus-optimal-decisions]], **no individual author owns the claim**, so no individual author has reason to resolve it.

## See Also

- [[split-half-reliability]] · [[performance-volatility]] · [[player-rating-time-series]] · [[predictive-validity]]
- [[vaep]] · [[expected-threat]] · [[recruitment]] · [[player-development-curve]]
- [[observed-versus-optimal-decisions]] · [[action-valuation-frameworks-compared]]
- [[on-ball-actions-football-xt-vs-vaep|xT/VAEP Summary]] · [[football-performance-time-series|Valuing Players Over Time Summary]]
