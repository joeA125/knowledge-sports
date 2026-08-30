---
title: "Champdas Master System"
type: entity
tags: [data-provider, event-stream-data, reliability, sports-analytics, evaluation, single-source]
sources: [raw/papers/stats_reliability_football_champdas.md]
confidence: 0.8
provenance:
  extracted: 74%
  inferred: 14%
  generated: 10%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Champdas Master System

A **semi-automatic match analysis system** built by Champion Technology Co., Ltd., a Chinese sports-data company founded in 2004. Operators code live matches with keyboard shortcuts plus mouse marking on a miniaturised on-screen pitch.

**The only data provider in this vault whose reliability has been independently measured** — see [[champdas-validity-reliability|Gong et al. (2019)]] and [[operator-reliability]].

## Coverage

Chinese Football Super League (first division), Chinese Football Association China League (second division), China National Youth Super League (U13–U19), the China men's national team, and clubs in the Korean K-League. Also supplies data to PPTV for online broadcast of the Korean, Spanish and English first divisions.

⚠️ **No held football source uses Champdas data.** The vault's corpus runs on StatsBomb, Wyscout, Opta and [[data-stadium]]. Champdas matters here as the **subject of a reliability study**, not as a source of anything the vault analyses.

## How It Works

Two input modes combine: **hot-keys** for event types, and **on-screen positioning** by mouse on a scaled-down pitch.

Two categories of data are then generated automatically:

1. **Location-inferred** — corners, free kicks, throw-ins, identified from where the marking falls
2. **Logic-inferred** — long ball, successful pass, consecutive pass, attacking shift, derived from the relationship between players and pitch zones

Event timestamps are automatic. Marked locations are later integrated to produce tactical summaries.

> **Roughly half the event vocabulary is derived rather than observed.** An operator marks a location and presses a key; the system decides whether that was a long ball, a lateral pass or an attacking shift. The operator never enters those labels.
> ^[inferred: the paper describes the two automatic categories without characterising their share; the reading is drawn here from the variable list]

This is why the paper's **pass-direction errors originate in marking, not in judgement**. The system classifies a pass as lateral when the angle to the sideline parallel falls under 15°, so a small mouse-placement error silently changes the category. The operator makes no wrong decision and the output is still wrong.

**That failure mode is specific to derived attributes and invisible to agreement statistics computed on event *types*** — two operators can agree perfectly that a pass occurred and disagree on its direction band.^[generated: the paper reports the mechanism and the disagreement separately. rests-on: source:gong-15-degree-threshold]

## Distinguishing Feature

The system encodes **passing direction** — forward, lateral, diagonal, backward, plus through balls, long/short, crosses, key passes and consecutive passes — computed from the angle between a pass and the following event relative to the sideline and attacking direction.

The authors present this as its main advantage over peers, which they characterise as focused on success/failure outcomes of discrete events. It is also, by their own results, **the least reliable part of the system**.

> The feature that distinguishes the system is the feature that fails most. Both facts have the same cause: direction is *derived from marked geometry* rather than notated directly, so it buys tactical richness with positional precision the interface cannot deliver.
> ^[generated: the paper does not connect its distinguishing claim to its error analysis. rests-on: source:gong-direction-errors, source:gong-champdas-passing-advantage]

## Measured Reliability

Four operators, one La Liga match (Real Madrid vs Villarreal, 13 January 2018), coded twice two weeks apart from TV coverage. Full figures on [[operator-reliability]]; in summary, intra-operator ICC 0.98–1.00 and inter-operator ICC 0.93–1.00, weakest on defensive and goalkeeping actions.

Content validity of its 31 variables was separately rated by 20 licensed coaches at Aiken's V ≈ 0.84–0.85 against a 0.52 threshold.

## Comparison Against OPTA

The authors benchmark their operators against OPTA on the same match. Agreement is acceptable across variables **except short passes**, where a differing length threshold produces divergence despite similar total pass counts.

**This is the vault's only direct provider-versus-provider event comparison.** It is one match and two systems, and it establishes that provider disagreement is real, definitional, and located in attributes rather than event types. See [[spadl]].

## See Also

- [[champdas-validity-reliability|Gong et al. (2019)]] — the source
- [[operator-reliability]] — the concept · [[split-half-reliability]] — the layer above
- [[data-stadium]] · [[stats-perform]] · [[event-stream-data]] · [[spadl]]
- [[action-valuation]] · [[vaep]] · [[expected-threat]] · [[tactical-analysis]]
