---
title: "Jonas Bischofberger"
type: entity
tags: [person, researcher, university, off-ball, defensive-valuation, optical-tracking-data, reliability, evaluation, sports-analytics, single-source]
sources: [raw/papers/off-ball-defensive-performance-football.md]
confidence: 0.75
provenance:
  extracted: 64%
  inferred: 16%
  generated: 19%
  imported: 0%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Jonas Bischofberger

Joint first author (with Runqing Ma) of [[off-ball-defensive-performance-blame|Blame is easier than praise]], **the source that substantially closed the vault's longest-standing acquisition priority.**

Centre for Sport Science and University Sports, University of Vienna; Vienna Doctoral School (VDS-PhaNuSpo); and **VfB Stuttgart 1893 AG.**

## The Affiliation Is the Point

He holds a **club appointment alongside the academic one**, and his co-authors include two at the **Deutscher Fußball-Bund**. The proprietary Frauen-Bundesliga and 3. Liga tracking data was supplied through them.

> [[mohammad-arshan-shaikh|The previous ingest]] concluded that the measurement the vault wanted was blocked not by method, funding or expertise but by **multi-season tracking access** — and that the vault should stop looking for methodological sophistication and start looking for anyone holding the data.
>
> **This paper is that prediction coming true within one ingest.** A club employee and two federation staff, working with a university group, on 516 matches of proprietary positional data across three competitions.
> ^[generated: drawn from the author affiliations against the acquisition analysis recorded one ingest earlier. rests-on: source:bischofberger-affiliations, claim:stability-blocked-by-data-access]

The practical lesson for the acquisition list is narrow and useful: **filter by institutional access, not by topic.** Papers with a club or federation co-author are where multi-season tracking results will appear.

## The Methodological Line

Cited within the paper, all unheld:

- **Renkin, Bischofberger, Schikuta & Baca (2022)**, *Validation and Optimisation of Player Motion Models in Football* — the motion-model work he **explicitly declined to use** for defensive pressure areas, arguing that physically reachable areas overestimate involvement for players constrained by team tactics. ⚠️ A researcher rejecting his own prior tool on substantive grounds is worth acquiring for the argument alone.
- **Bischofberger & Baca (2025)**, *Dangerous Accessible Space* — cited elsewhere in the literature as a pitch-control-adjacent construct.
- **Eigenrauch, Bischofberger, Baca & Schikuta (2024)**, pass prediction from positional data.

## What He Did That the Vault Had Not Seen Before

Three things, and the first is the one that matters.

**Reported [[metric-discrimination|meta-metrics]] for tracking-derived off-ball metrics.** Thirteen ingests of asking. He cites [[meta-analytics-sports-metrics|Franks et al. (2016)]] directly for the framing, making this the vault's first framework-to-football-application chain on metric quality.

⚠️ **And then z-scored the results**, reporting composites rather than raw ICCs — so the absolute question stays open. See [[off-ball-defensive-performance-blame]].

**Benchmarked against existing metrics on shared data.** Tackles won, tackles won ratio and interceptions, evaluated by the same protocol as his own ~29 variants. `no-held-source-benchmarks-across-frameworks` survives only because none of the vault's six off-ball mechanisms is in the comparison — **the structural excuse about licensing is now much weaker.**

**Argued for an asserted parameter rather than apologising for it.** The 5 m DPA radius is handcrafted, and the reasoning — that fitting against an unreliable benchmark launders arbitrariness rather than removing it — is the best defence of an unfitted parameter in the vault's corpus. See `fitting-a-parameter-to-a-bad-benchmark-is-worse-than-asserting-it` on [[defensive-pressure-area]].

## Cross-Gender and Lower-League Coverage

The paper claims to be **the first study using a near-full season of women's event and positional data**, and includes the men's third tier alongside a World Cup.

That matters beyond representation: the 3. Liga result — where his metrics only marginally beat interceptions — is what generated the finding that **discrimination depends on the league's skill spread, not just the metric.** A single-competition study could not have produced it.

Compare [[gvdep|GVDEP]], the vault's other cross-gender source. Both find that a metric's behaviour is competition-dependent; neither treats a single figure as a property of the metric alone.

## See Also

- [[off-ball-defensive-performance-blame|Blame is easier than praise]] — the source
- [[defensive-pressure-area]] · [[role-conditioned-baseline]] · [[aggregation-denominator]]
- [[pascal-bauer]] · [[arnold-baca]] — co-authors
- [[metric-discrimination]] · [[metric-stability]] · [[reliability-layers]] · [[meta-analytics-sports-metrics]] · [[mohammad-arshan-shaikh]]
- [[off-ball-value]] · [[defensive-valuation]] · [[optical-tracking-data]] · [[expected-threat]] · [[gvdep]]
