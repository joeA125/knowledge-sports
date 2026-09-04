---
title: "Match-to-Match Variability of High-Speed Activities in Premier League Soccer"
type: summary
tags: [reliability, evaluation, statistics, optical-tracking-data, volatility, player-evaluation, sports-analytics, model-selection, single-source]
sources: [raw/papers/match-level-variability-high-speed-actions-football.md]
confidence: 0.85
provenance:
  extracted: 70%
  inferred: 12%
  generated: 16%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-09-04
updated: 2026-09-04
---

# Match-to-Match Variability of High-Speed Activities in Premier League Soccer

**Warren Gregson, Barry Drust, Greg Atkinson & Valter Di Salvo.** *Int J Sports Med*, accepted January 2010. Liverpool John Moores & Rome Foro Italico. 485 outfield players, 7,281 match observations, EPL 2003/04–2005/06, Prozone.

> **This is the missing zero point for the vault's derivation-depth argument.** Not a value metric and not a closer for the reliability gap — it measures how noisy a **directly-tracked physical quantity** is *before* any modelling. The vault flagged the need for exactly this when it recovered ICCs from [[off-ball-defensive-performance-blame|Bischofberger et al.]]: everything downstream inherits the variability of the raw signal, and until now the vault had no measurement of the raw signal itself.

## What Was Measured

Between-match coefficient of variation — the per-player SD across matches divided by that player's mean — for six high-speed running variables, aggregated over players and reported with 95% CIs.

| Variable | Overall CV | Short-term CV (8 weeks, n=37) |
|---|---|---|
| Total high-speed running (THSR) | **17.7%** | 23.5% |
| High-speed running (HSR) | **16.2%** | 22.0% |
| Total sprint distance (TSD) | **30.8%** | 38.9% |
| HSR in possession (HSRP) | **30.6%** | 38.7% |
| HSR out of possession (HSRWP) | **23.5%** | 27.9% |
| Total sprint number | 30.0% | 34.4% |

Only players completing a full 90 minutes were included, which removes the playing-time confound [[aggregation-denominator|that dogs season-aggregated metrics]] by construction.

## The Three Findings the Vault Needs

### 1. The raw signal is already noisy — 16% at best

Even THSR and HSR, the most stable quantities, sit at 16–18% match-to-match CV. **These are directly-sampled tracking outputs with no model on top**, and they are already this variable.

> ### `the-tracking-signal-is-noisy-before-any-model-touches-it`
> **A directly-tracked physical quantity varies 16–31% between matches for the same player. Every value metric the vault holds is computed downstream of quantities like these, so 16% is a floor on the noise those metrics inherit — not a number any of them can beat by better modelling, only add to.**
> ^[generated: the paper measures physical outputs, not value metrics; the framing as a noise floor for downstream metrics is drawn here. rests-on: source:gregson-cv-table]

This completes a progression the vault assembled piecemeal:

| Layer | Source | Reliability of the *raw* quantity |
|---|---|---|
| **Physical output, match play** | **Gregson et al. (2010)** | **CV 16–31%** |
| Physical capacity, lab-like control | [[gps-deceleration-reliability|Jones et al. (2024)]] | Peak velocity CV 1.4%; derived quantities worse |
| Off-ball value metric | [[off-ball-defensive-performance-blame|Bischofberger, recovered]] | ICC 0.34–0.66, 0.08–0.30 role-adjusted |

⚠️ **The Jones and Gregson figures are not contradictory despite looking it** — 1.4% against 16%. Jones measured a *maximal, standardised, single-action* capacity test; Gregson measured *self-paced, tactically-governed* output over 90 minutes. **The gap between them is the cost of leaving the laboratory**, and it is roughly a factor of ten. See [[reliability-layers]].

### 2. Variability is higher for central players

Central defenders and central midfielders are the most variable; wide midfielders and attackers the least. For HSR: central defender 18.8% against wide midfielder 13.1%.

⚠️ **This is the third independent appearance of the position confound**, from a measurement layer the other two do not touch:

- [[metric-discrimination|Franks / recovered Bischofberger]]: off-ball metric discrimination is half positional
- [[year-to-year-metric-stability-football|Shaikh]]: defensive-metric stability is lowest, and he blames team-defensive context
- **Gregson: the raw physical signal is itself most variable for central players**

The authors' explanation is tactical: central positions are most exposed to swings in game intensity and coaching instruction. **So position drives not just what a metric measures but how noisily the underlying signal is produced** — a distinct mechanism from the position-as-construct problem, and it stacks with it.^[generated: the cross-layer convergence is drawn here; none of the three sources cites the others on this point]

### 3. Variability is higher in possession than out

HSR in possession runs ~30.6% CV against ~23.5% out of possession.

⚠️ **This lands directly on every off-ball value metric the vault holds.** [[off-ball-value|OBSO, C-OBSO, SOG]] and the rest are computed during **possession phases** — the noisier regime. [[off-ball-defensive-performance-blame|Bischofberger's]] defensive metrics are the exception, built from out-of-possession play, which is the *less* variable regime.

> **The attacking-value metrics are computed on the noisier half of the game and the defensive ones on the quieter half** — which cuts against the intuition, visible across the vault, that defensive valuation is the harder and flimsier problem. On the raw-signal evidence, the off-ball *attacking* metrics sit on shakier ground.
> ^[generated: drawn across Gregson's possession split and the vault's off-ball corpus; no source makes this comparison. rests-on: source:gregson-possession-split]

## The Sample-Size Argument, Which Is the Paper's Real Point

The authors' motivating concern is not player evaluation but **research design**: how many observations are needed to detect a real change against this much noise.

Their headline: at a CV of 20%, detecting a 10% intervention effect needs **≈80 players.** And for an individual, a THSR change must exceed **~64%** before you can be 95% confident it is real rather than normal between-match swing.

> **This is a direct, quantified warning to [[recruitment]] and [[player-development-curve]].** If a single match's high-speed numbers cannot distinguish a real 60% individual improvement from noise, then single-match scouting observations of physical output are close to uninformative, and multi-match aggregation is not optional. The vault had argued this qualitatively; here is the number.
> ^[generated: the transfer to recruitment is drawn here; the paper's concern is research power, not scouting. rests-on: source:gregson-sample-size]

## ⚠️ The Abstract Is a Secondary Source, and This One Checks Out

Following the vault's now-standing practice after four mis-citation findings: the abstract's headline figures (HSR 16.2%, TSD 30.8%, possession ~30% vs ~23%) were checked against Tables 2 and 3 and **match.** No discrepancy this time. Recorded because verifying is now the default, not because it failed.

⚠️ **One genuine tension the abstract papers over.** It states variability was "lower for wide midfielders and attackers" as though uniform, but Table 3 shows attackers are the *most* variable position for out-of-possession running (HSRWP 29.2%). The generalisation holds for total and in-possession running and reverses for the defensive component — which is exactly the possession split of finding 3, and the abstract flattens it.^[extracted: Table 3 vs abstract]

## Limitations

- **Physical output, not value or skill.** Nothing here measures decision quality, only how much fast running happened. Its relevance to the vault is as a *noise floor*, not as a metric.
- **CV over players, not ICC.** The paper reports dispersion, not a signal-to-total-variance ratio, so it is not directly comparable to [[metric-stability|stability]] or [[metric-discrimination|discrimination]] figures — it answers "how much does this bounce around", not "how much of the spread is real".
- **2003–2006 Prozone**, an early semi-automatic system. Threshold speeds (19.8, 25.2 km/h) are fixed rather than individualised, which the authors flag as inflating apparent variability.
- **90-minute completers only.** Removes the playing-time confound but selects for a fitter, more established subpopulation — the players *most* likely to be consistent, so these CVs may **understate** the true figure across all players.
- **Season-stage confound acknowledged and partly controlled** — the 8-week subsample was *more* variable, not less, so time-of-season is a small contributor.
- **No player-value or outcome link.** High-speed running is assumed relevant to performance; the paper does not establish it against results.

## See Also

- [[match-to-match-variability]] — the concept this ingest created · [[reliability-layers]] · [[coefficient-of-variation]]
- [[gps-deceleration-reliability|Jones et al.]] — physical capacity under lab-like control · [[off-ball-defensive-performance-blame|Bischofberger]] — the value-metric layer
- [[metric-stability]] · [[metric-discrimination]] · [[year-to-year-metric-stability-football|Shaikh]] · [[performance-volatility]] · [[within-season-variation-noise-or-signal]]
- [[off-ball-value]] · [[aggregation-denominator]] · [[recruitment]] · [[player-development-curve]]
- [[warren-gregson]] · [[valter-di-salvo]] · [[optical-tracking-data]]
