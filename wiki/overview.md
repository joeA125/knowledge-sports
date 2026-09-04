---
title: "Knowledge Base Overview"
type: synthesis
tags: [sports-analytics, action-valuation, off-ball, evaluation, reinforcement-learning, computer-vision, player-evaluation, network-analysis, tactical-analysis]
sources: []
confidence: 0.8
provenance:
  extracted: 5%
  inferred: 55%
  generated: 35%
  imported: 0%
  ambiguous: 5%
lifecycle: reviewed
created: 2026-04-22
updated: 2026-09-04
---

# Knowledge Base Overview

A narrative map of what this vault holds and how it hangs together. The index is the catalog; this page is the argument for the catalog's shape.

**Scope: football and sports analytics.** General ML and LLM material is being migrated to a sibling vault — see *Pending Migration* below.

> **Rewritten 2026-08-12.** The previous version dated from 2026-07-07 and described a general-ML vault with football computer vision as one cluster of seven. That inverted some time ago. It also reported ~29 summaries and ~90 concept/entity pages against actual figures of 56 and 298.

## Composition

| | Count |
|---|---|
| Total pages | **305** |
| Concepts | 167 |
| Entities | 77 |
| Source summaries | **48** |
| Open questions | 8 |
| Syntheses | 2 (plus this page) |
| Dashboards | 3 |

**All 48 raw sources are ingested.** Mean page confidence 0.823; 163 pages `reviewed`, 139 `draft`, 3 `evergreen` (the dashboards).

> **Updated across an extended reliability-focused stretch, late Aug – 4 Sep 2026.** Eight sources ingested over this run, six of them on measurement reliability: [[network-theory-football-strategies|López Peña & Touchette]], [[data-driven-team-sports-behaviors|Fujii]], [[champdas-validity-reliability|Gong et al.]], [[gps-deceleration-reliability|Jones et al.]], [[meta-analytics-sports-metrics|Franks et al.]], [[year-to-year-metric-stability-football|Shaikh]], [[off-ball-defensive-performance-blame|Bischofberger et al.]] and [[match-to-match-variability-high-speed|Gregson et al.]].
>
> ⚠️ **Date-stamp caveat.** Many pages touched in this stretch carry `updated: 2026-08-29` because the run was mistakenly treated as a single day; it in fact spanned ~1 week. Frontmatter dates from that window are approximate to within that span. Corrected from 2026-09-04 onward. Counts re-read from `vault_stats`.
>
> ✅ **Cross-page propagation caught up** in a dedicated run, 2026-08-29. The derivation-depth argument reached [[off-ball-value]]; the coding-noise finding reached [[vdep]] and [[rare-event-proxy-targets]] as a **fourth proxy criterion**; the expert-assent mode reached [[construct-validity]]; the attribute-level provider caution reached [[spadl]]; and the shifted prior reached [[within-season-variation-noise-or-signal]]. Residual backlog is itemised in the final `log.md` entry.

## What This Vault Is About

The corpus is dominated by one problem — **assigning value to what happens in a football match** — approached from many directions by groups that mostly do not cite each other. Nearly every framework here reduces to one equation:

$$V(a_i) = Q(S_i) - Q(S_{i-1})$$

and differs only in how it represents the state and computes the value. That common form is what makes the vault's comparisons possible, and it is why [[action-valuation]] and [[vaep]] are its two densest hubs.

The material divides into **seven tasks**, set out in full on [[action-valuation-frameworks-compared]]: valuation, forecasting, clustering, counterfactual/transfer, tactical, prescription, and simulation. They are easily mistaken for one problem and are not.

## The Main Lines

**On-ball valuation.** [[expected-threat|xT]], [[vaep|VAEP]], [[expected-possession-value|EPV]] and [[martingale-epv]] — the tradition running from Markov zone models through supervised action classification to full possession-value surfaces. [[expected-value-possession-framework|Fernández, Bornn & Cervone]] is the most elaborate instance held.

**Off-ball valuation.** The vault's most developed argument, on [[off-ball-value]]: **six distinct mechanisms** for valuing players who do not have the ball, from reading a value surface at a player's position ([[obso|OBSO]]) through [[space-occupation-gain|control × value differencing]], [[vdep|classifier state]], [[c-obso|predicted-trajectory counterfactuals]], [[drso|optimal-position counterfactuals]], to learned per-player action values. **Where two of them have been compared, they disagree** — ρ = 0.182 on the same club, season and provider. See [[construct-validity]].

**Defensive valuation.** [[vdep]] and [[gvdep]] change the prediction target rather than the model, on the finding that goals conceded are too rare to classify. See [[rare-event-proxy-targets]] and the open question at [[vaep-conceding-classifier]].

**Physical and geometric models.** [[pitch-control]], [[probability-surface|value surfaces]], [[space-creation]] — the [[william-spearman|Spearman]] and [[javier-fernandez|Fernández]] traditions, which turn out to be siblings framed against a common ancestor rather than rivals ([[pitch-control-traditions-compared]]).

**Reinforcement learning and simulation.** Three held sources, split forward/inverse: [[ai-football-reinforcement-learning|Scott et al.]] and [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] generate behaviour in simulators; [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] estimates from logged data. Supporting concepts at [[multi-agent-reinforcement-learning]], [[temporal-difference-learning]], [[deep-q-network]], [[proximal-policy-optimization]], [[action-supervision]], [[action-space-design]], [[domain-adaptation]], [[agent-based-simulation]].

**Sequence and event modelling.** [[nmstpp]], [[seq2event]], [[sig-model]], [[scoutgpt]], [[eventgpt]] — forecasting the next event, and the metrics ([[hpus]], [[lpv]]) derived downstream from those forecasts.

**Tactical description.** [[social-network-analysis]] and [[network-cohesion]] — pass networks as weighted directed graphs, characterising a team by graph structure rather than by valuing its actions. **The one line here that entered football from outside sports analytics**, from graph theory and statistical physics, and the only task validated by expert recognition of a style rather than against a criterion. Primary-sourced since 2026-08 by [[network-theory-football-strategies|López Peña & Touchette]].

**Computer vision for sport.** [[game-state-reconstruction]], [[camera-calibration]], [[soccernet-game-state-reconstruction]], [[camera-calibration-benchmarking]]. Notably the **only area here with functioning cross-method benchmarking**, which makes it a useful control on the valuation literature's failure to do the same.

**Rating systems.** [[elo-rating-system]], [[glicko-rating-system]], [[trueskill]], [[bradley-terry-model]], applied through [[league-strength-rating]] and [[duel-skill-rating]].

## The Hubs

By inbound wikilink count, not by memory:

| Page | Inbound |
|---|---|
| [[vaep]] | 84 |
| [[c-obso]] | 82 |
| [[action-valuation-frameworks-compared]] | 71 |
| [[keisuke-fujii]] | 66 |
| [[off-ball-value]] | 65 |
| [[action-valuation]] | 65 |
| [[expected-threat]] | 64 |
| [[split-half-reliability]] | 64 |
| [[nmstpp]] | 58 |
| [[vdep]] | 55 |

Three things worth remarking on. **[[keisuke-fujii]] at 66** reflects a single research group authoring ten held sources — the largest concentration in the vault, and the only one with enough overlapping work on one dataset to check itself. **[[split-half-reliability]] at 62** has climbed from 43 in three weeks, because five reliability sources arrived in one session and the absence of reliability figures was load-bearing across the off-ball argument before any of them.

And **[[transformer]] has left the top fifteen entirely.** Before the migration it sat third at 60 inbound. Reducing it to a lean page — what the architecture is, and why [[nmstpp|NMSTPP]] uses one — moved those links onto the football pages that actually depend on them. NMSTPP itself gained ten.

## Recurring Findings

- **Metrics beat outcomes at predicting outcomes.** [[obso|OBSO]] predicts next-match goals better than shots or goals do. Goals are the worst predictor of future goals at both player and team level. See [[predictive-validity]].
- **Where metrics have been compared, they disagree** — and the comparisons are rare because comparison requires two methods on one dataset, which licensing usually prevents. See [[data-stadium]].
- **The literature does not benchmark across frameworks.** Weakened twice and still standing; contrast the CV cluster, where [[camera-calibration-benchmarking|ProCC]] exists specifically to enable it.
- **Sixteen asserted free parameters carry no sensitivity analysis**, across six distinct kinds — **now eighteen across eight**, the seventh being a constant borrowed from another field ([[social-network-analysis|PageRank's]] $p = 0.85$) and the eighth the [[aggregation-denominator|aggregation denominator]], which is the only kind anyone has actually swept — and it reversed metric rankings. See [[free-parameters-load-bearing]].
- ⚠️ **But assertion is not always the defect.** Where no ground truth exists, fitting a parameter against a proxy launders arbitrariness rather than removing it. The surviving standard is narrower: sweep for *sensitivity* even where you cannot fit for *value*. See [[defensive-pressure-area]].
- **Offensive bias has four causes with four different remedies** — definitional, data, modelling choice, and statistical. See [[action-valuation]].
- **A claim held only through citation is unaudited, not merely thin.** Acquiring [[network-theory-football-strategies|one long-cited primary]] falsified three things the vault believed about it, including a finding its citing source had manufactured. See [[social-network-analysis]].
- **The field's own survey neither benchmarks nor discusses reliability.** [[data-driven-team-sports-behaviors|Fujii (2021)]] catalogues dozens of frameworks and compares none, with no licensing constraint to excuse it — which moves both absences from *structural* to *disciplinary*. See [[action-valuation-frameworks-compared]].
- **Twenty-two players have no canonical order**, and every framework here resolves that silently. Four routes exist, they trade order against interpretability, and none has been compared. See [[permutation-problem]].
- **Reliability is at least two things.** Whether two analysts produce the same event log, and whether a player's rating is stable across samples, fail for unrelated reasons. **Revised 2026-08-29: it is three layers, and the metric layer is itself three properties** — discrimination, stability, independence. See [[reliability-layers]].
- **The framework and the metrics share authors and never meet.** [[daniel-cervone|Cervone]] and [[luke-bornn|Bornn]] co-wrote the standard for testing metric quality and two of this vault's football value surfaces, reporting none of those properties for the latter. The gap is a norm about what a paper contains, not a missing tool. See [[meta-analytics-sports-metrics]].
- **Summaries misreport primaries, and the pattern is about which text gets checked.** Four consecutive ingests found it: a citing paper garbling a citation and manufacturing a finding, a survey describing works second-hand, a paper's own abstract misreporting its own table, and a preprint mis-describing two sources it builds on. See [[champdas-validity-reliability]].
- ⚠️ **The most robust metric may be measuring the team.** *Passes against per 90* — essentially how much your side defends — scores near the top on both robustness and validity in [[off-ball-defensive-performance-blame|the one study that ranked them]], because good players play for good teams. This is Franks' zip-code warning appearing in real data, and internal properties cannot detect it.
- **Off-ball defensive metrics are poor once position is removed.** Recovered ICCs run 0.34–0.66 unadjusted and **0.08–0.30 role-adjusted** — roughly half the apparent reliability is positional. The vault's longest-running question has an answer, and the answer is that these metrics are noisier than the literature's use of them assumes. See [[metric-discrimination]].

## Open Questions

Eight, grouped by who could answer them.

*Component-level gaps* — [[pitch-control-traditions-compared]], [[shot-value-formulations-compared]], [[tracking-error-propagation]].

*Untested assumptions in held work* — [[free-parameters-load-bearing]], [[vaep-conceding-classifier]].

*Claims this vault generated* — [[within-season-variation-noise-or-signal]], [[observed-versus-optimal-decisions]], [[handcrafted-features-rule]].

The third group is the distinctive one: these are questions no single paper poses, visible only from holding several at once.

## Source Acquisition Priorities

The RL and simulation line is fully held, and **the SNA gap that topped this list has been closed** — see below for what remains of it. Five areas, highest need first.

### 1. Reliability of tracking-derived metrics — ✅ **substantially closed 2026-08-29**
> ✅ [[off-ball-defensive-performance-blame|Bischofberger et al. (2026)]] reports $ICC_{Match}$, a half-season repeat correlation and a bootstrapped $ICC_{Season}$ for **~32 tracking-derived off-ball defensive metrics** across three competitions, citing [[meta-analytics-sports-metrics|Franks et al.]] for the framing. **Thirteen ingests of asking, answered.**
>
> ⚠️ They report the three as a **z-scored composite**, not as raw values.
>
> ✅ **The absolute figures were then recovered in-vault** from their committed World Cup data. Unadjusted ICC(1,1) runs **0.34–0.66** across twelve off-ball variants — **no metric reaches "good"** — and **0.08–0.30 once role-group means are removed**, which is uniformly "poor". Roughly half the apparent reliability is positional. See [[off-ball-defensive-performance-blame]] and [[metric-discrimination]].
>
> ⚠️ **Vault-side recomputation, not published figures.** World Cup only, median three matches per unit, and a blunter role correction than the authors' covariate model. Direction and magnitude solid; levels indicative.

| Property | Held for football? |
|---|---|
| **[[metric-discrimination\|Discrimination]]** | ✅ xT (0.89), VAEP (0.25); off-ball defensive **0.34–0.66**, or **0.08–0.30 role-adjusted** |
| **[[metric-stability\|Stability]]** | ⚠️ 24 FBref counting stats ([[year-to-year-metric-stability-football\|Shaikh 2026]]); no value model |
| **[[metric-independence\|Independence]]** | ❌ nothing |

**What remains open is now specific:** $r_{Repeat}$ — the between-season repeat correlation — **cannot be recovered from public data.** The authors excluded it for the World Cup (too few matches per team when halved), and the Frauen-Bundesliga and 3. Liga datasets are proprietary. **That requires a DFB data request, not a computation.**

This list originally said *"split-half or test–retest reliability"*, treating those as two ways of measuring one property. **They are two different properties**, and stability is not one number either — it decays with lag. See [[stability-decay]] and [[reliability-layers]].

✅ **The prediction from the previous ingest held within one ingest.** [[mohammad-arshan-shaikh|Shaikh]] showed the measurement was blocked by **multi-season tracking access**, not method, and that the list should filter by institutional access rather than topic. Bischofberger et al. has a **VfB Stuttgart** author and two at the **DFB**, on 516 matches of proprietary data. See [[jonas-bischofberger]].

✅ **And it confirmed two vault claims.** `each-layer-of-derivation-costs-reliability` — generated from a GPS study and flagged as an extrapolation — is now measured on football off-ball metrics: **removing pass value raises robustness.** And role-conditioned responsibility, the other stabiliser, is [[empirical-bayes-shrinkage|shrinkage]] under another name. ⚠️ With a nuance: *valued fault per 90* has the **highest validity** of any metric tested and below-average robustness. **Derivation costs reliability and buys something.**

⚠️ **The "outside the field" reading remains wrong.** [[daniel-cervone|Cervone]] and [[luke-bornn|Bornn]] co-wrote the framework *and* two of the vault's football value surfaces, reporting none of these properties for either.

### 1a. What is still worth acquiring here
- ⚠️ **Frauen-Bundesliga and 3. Liga data from the DFB** — the only route to $r_{Repeat}$ and to reproducing Bischofberger et al.'s published composite. An acquisition, not a computation.
- ⚠️ **A validation study for a semi-automatic optical tracking system** — **newly surfaced 2026-09-04.** [[reliability-layers|The instrument layer]] is held for GPS wearables (Jones et al.) and for manual event coding (Gong et al.) but **not for optical tracking**, which is what C-OBSO, DRSO and the Barcelona line actually run on. Lead: **Di Salvo, Collins, McNeill & Cardinale (2006)**, *Validation of Prozone*, cited in [[match-to-match-variability-high-speed|Gregson et al.]]. See [[valter-di-salvo]].
- **Di Salvo, Gregson, Atkinson, Tordoff & Drust (2009)**, *Analysis of high intensity activity in Premier League soccer* — the companion holding the activity *levels* to Gregson (2010)'s *variability*; together they give both halves of the physical-output picture
- **Gregson et al. (2010)** is now held — **the raw-signal noise floor.** Not a closer for priority 1 but the baseline that makes the derivation-depth argument quantitative: high-speed running varies 16–31% match to match before any modelling
- **Bush et al. (2015)**, extending that design to **technical** as well as physical variability
- **Swartz et al.**, *Evaluation of Off-the-Ball Actions in Soccer* — even/odd week split on a tracking-derived defensive anticipation metric
- **Franks, Miller, Bornn & Goldsberry (2015)**, *Counterpoints* — a tracking-derived defensive metric by the author who then wrote the framework
- **Renkin, Bischofberger, Schikuta & Baca (2022)**, motion-model validation — the tool Bischofberger **declined to use**, arguing physically reachable areas overestimate involvement for tactically constrained players. A critique of the [[pitch-control]] tradition from outside it

⚠️ **Vocabulary note, now three registers deep.** Psychometrics says "split-half reliability", [[meta-analytics-sports-metrics|Franks]] says "stability", and the sports-science tracking literature says "match-to-match variability" and "coefficient of variation" — largely non-intersecting citation graphs for adjacent properties. **This has cost the vault twice. Treat it as a standing search heuristic, not a per-ingest observation.**

### 1b. The validation literature — a whole discipline the vault does not hold
> **Added 2026-08-29.** Three reliability papers arrived at three different layers and none closed priority 1. The reason is disciplinary: [[data-driven-team-sports-behaviors|Fujii's survey]] shows the ML literature does not treat reliability as a topic, and both new sources came from psychology and sports-physiology journals instead.

[[champdas-validity-reliability|Gong et al.]] cite prior reliability validations of **AMISCO, PROZONE, SportsCode, OPTA, SICS, Dartfish and Nacsports.** The vault holds none.

- **Liu, Hopkins, Gómez & Molinuevo (2013)**, *Inter-operator reliability of live football match statistics from OPTA Sportsdata* — **the highest-value item.** Opta is a provider the vault's own sources use, unlike Champdas
- **Bradley et al. (2007)** on ProZone MatchViewer; **Beato et al. (2018)** on Data.Stadium — the latter shares a name with [[data-stadium|the tracking provider behind four held sources]]

See [[miguel-angel-gomez]], the vault's only representative of this literature.

### 1c. The Koopman line — new, and by the vault's most central author
> **Added 2026-08-29.** [[koopman-mode-decomposition]] was created this ingest from a survey description alone, and carries a generated claim resting on an `imported:` dependency — the shape the vault has just been burned by twice.

- **Fujii, Takeishi, Hojo, Inaba & Kawahara (2020)**, *Physically-interpretable classification of network dynamics*, Scientific Reports — the fullest application
- **Fujii, Inaba & Kawahara (2017)**, Koopman spectral kernels — the construct itself
- **Kawahara (2016)**, DMD with reproducing kernels — the methodological primary

Cheap to justify: [[keisuke-fujii]] already accounts for ten held sources, and this is the one line of his work the vault describes without holding.

### 2. Social network analysis — *narrowed, not closed*
> **Closed as the #1 gap, 2026-08-29.** [[network-theory-football-strategies|López Peña & Touchette (2012)]] is held. The tactical task is primary-sourced and **every one of the seven tasks now has at least one primary source.**

Acquiring it was more productive than a confirming source would have been, because it **falsified** what the vault held second-hand: a garbled citation on four pages, an inverted PageRank parameter, and a headline finding that is not in the paper and does not survive testing against its own table. See [[social-network-analysis]].

What remains, and why it still ranks second:

- **Gonçalves et al. (2017)** — lower single-player passing dependency may optimise team performance. **The last unheld SNA claim carrying an inferential conclusion**, and therefore the one most likely to be a downstream gloss like the one just corrected
- **Buldú et al. (2018)** — multilayer passing networks; the route past the [[network-cohesion|community-detection failure]], which is structural rather than fixable by method
- **Clemente et al. (2016)** — the framework text; lowest priority, since the framework is now held in instance form

### 3. Team-as-one-agent RL
Cited across [[multi-agent-reinforcement-learning]] as the tradition both held MARL papers define themselves against, entirely second-hand.

- **Liu & Schulte (2018)**; **Liu et al. (2020)**; **Routley & Schulte (2015)**
- **Luo, Schulte & Poupart (2020)** on inverse RL for sports

⚠️ **[[data-driven-team-sports-behaviors|Fujii's survey]] describes Liu & Schulte and does not close this.** It reports Q-function estimation with an RNN, made interpretable via a linear model tree. That is a second-hand description in a survey — precisely the status of the López Peña gloss that turned out to have manufactured a finding. Use it to locate the papers, not to characterise them.

### 4. Primary sources for borrowed parameters
Two citation problems that only primaries can fix.

- **Spearman (2018) primary** — [[c-obso]] and [[drso]] set PPCF parameters σ = 0.45, λ = 4.3 citing a paper that fits s = 0.54, λ = 3.99. A citation error propagating through the line; see [[obso]]
- **Kurach et al. (2020)** — everything on [[google-research-football]] comes from three papers that *used* the environment

⚠️ **This category has been upgraded by the SNA ingest.** It was framed as parameter hygiene; it is now the vault's second confirmed instance of **a numeric detail propagating wrongly through a citation chain**, alongside the "Peña & Hugo" name and the $p = 0.85$ damping factor borrowed from web search. Three instances is a pattern, and the Spearman discrepancy is the one still unresolved.

### 5. Scale versus fidelity in simulation
- **Liu et al. (2021), Humanoid Football** — the only environment occupying the biomechanical corner of the trade-off tabled on [[agent-based-simulation]], and the natural counterpart to GFootball's 11v11 abstraction

---

**Priority 1 is substantially closed**, and what replaces it at the top is narrower: the raw robustness coefficients exist in Bischofberger et al.'s public code and were never printed. That is a computation, not an acquisition, and it would finally let the vault say whether *any* off-ball metric is reliable in absolute terms rather than relative to its peers.

**The SNA ingest taught something about this list itself.** The gap was ranked first on the assumption that a whole task held second-hand was *under-supported*. It was worse than that: it was **wrongly supported**, and three of the vault's claims about it were false. A tradition described only through citations is not a thin part of the vault but an unaudited one.^[generated: drawn from the outcome of this ingest against the reasoning that prioritised it. rests-on: source:lopez-pena-betweenness-not-stated]

**And the reliability sequence taught something else.** Five sources arrived; four looked like hits and were not, because the claim was worded too coarsely to be checked. Each near-miss forced a refinement — layer, then property, then denominator — and the fifth closed a gap that by then was precisely enough stated to recognise. **The list's value was in how it failed, repeatedly, at a finer resolution each time.**^[generated: drawn across the five reliability ingests of 2026-08-29]

## Method

The vault runs a claim-dependency system beyond ordinary sourcing. Generated claims carry IDs and `rests-on:` clauses recording what they stand on, with four dependency kinds — `source:`, `claim:`, `imported:`, and `absence:`. Absence claims are the dangerous kind: they expire when a source is acquired, not when anything is re-read, and several here have been narrowed or retired exactly that way.

Full rules in `_schema/conventions.md`.

## Migration Completed

> **2026-08-14.** The general ML and LLM tier moved to a sibling general vault. 96 pages left — 34 concepts, 37 entities, 16 source summaries and their raw papers.

**The retention rule was dependency, not topic.** A page stayed if a football page's argument breaks without it. Applied to sources, that became a provenance test: a raw paper had to stay if any staying page cited it in `sources:`.

That produced three outcomes:

| | Count | Examples |
|---|---|---|
| **Moved** | 96 pages, 16 sources | The Transformer substructure, the LLM tier, the Vinyals and Graves lines, ResNet |
| **Forked** — copied, kept in both | 22 pages, 9 sources | [[gated-recurrent-unit]], [[variational-autoencoder]], [[trueskill]], [[attention-mechanism]], [[lstm]], [[proximal-policy-optimization]] |
| **Trimmed** — kept, general content stripped | 14 pages | [[transformer]], and the cross-domain sections on [[domain-adaptation]], [[construct-validity]] and others |

**The cut was not clean, and the interesting part is where it wasn't.** Nine general papers had to be retained because football pages cite them — Bahdanau for the GRU, VLAE for C-OBSO's GVRNN, TrueSkill for ranking RL agents, InstructGPT for PPO. Three more were kept on forward-looking grounds: pointer networks (pass selection is structurally a pointer problem), Order Matters (22 players are an unordered set), and Zaremba (small-data overfitting). **Those three carry speculative claims that should be acted on or retired rather than left standing.**

Every retained general page carries a scope note pointing at the general vault, so it reads as deliberately lean rather than damaged.

### What the migration revealed

- **Inference beat intuition, badly.** Classifying pages by subject matter rather than by reading their `sources:` ran at a **25% hit rate** on forks. Three genuine forks — [[lstm]], [[encoder-decoder-bottleneck]], [[autoregressive-model]] — were found only by verification, all citing [[nmstpp]], which turned out to be the football paper with the widest reach into general-ML territory.
- **A dual-sourced page is a fork by definition**, and that mechanical test proved far more reliable than "a football page links it".
- **The football hubs became more central.** [[c-obso]] gained nine inbound links, [[nmstpp]] ten, [[vaep]] two — not from new content but from general pages being rewritten to state their football dependency explicitly.

## Dashboards

- [[health|Wiki Health]] — stale, low-confidence, draft and orphan-risk pages
- [[reinforcement|Reinforcement]] — ageing, single-source and confidence-decay watch
- [[sources|Source Tracking]] — raw sources and reference counts

## Maintenance Notes

- Created 2026-04-22; rewritten 2026-08-12; migration recorded 2026-08-14
- **154 tags declared, 152 in use.** The 23 that described departed material were pruned; `contradicted` and `stale-risk` are retained unused, as ready vocabulary for conditions that have not yet arisen
- Page-type tags were removed from the taxonomy — `type:` in frontmatter is the single source of truth. Two namespaces recording one fact had already drifted (`source_summary` against `summary`)
- **No page sits below 0.5 confidence.** The lowest was this one, at 0.4, before the August rewrite
- `find_mentioned_but_missing` returns empty — every wikilink resolves, including seven pre-existing breaks that predated the migration
- This page should be revisited whenever a source opens a new area, and its claims re-checked against `vault_stats` rather than recalled
