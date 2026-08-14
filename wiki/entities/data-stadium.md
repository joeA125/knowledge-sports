---
title: "Data Stadium Inc."
type: entity
tags: [organisation, data-provider, sports-analytics, event-stream-data, optical-tracking-data, off-ball, construct-validity, selection-bias, action-space, reinforcement-learning, network-analysis]
sources: [raw/papers/football_defence_evaluation.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md, raw/papers/action_valuation_football_agentic_reinforcement_learning.md, raw/papers/adaptive_action_supervision_multi_agent_reinforcement.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.85
provenance:
  extracted: 62%
  inferred: 31%
  generated: 5%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-07-27
updated: 2026-08-08
---

# Data Stadium Inc.

Japanese sports data provider, supplier of the event and tracking data behind **five held sources**, all from the [[nagoya-university|Nagoya]] group and all on the 2019 Meiji Yasuda J1 League. Events at 30 Hz, tracking of all players and the ball at 25 Hz.

| Source | Coverage | Unit extracted |
|---|---|---|
| [[football-defence-evaluation-vdep\|VDEP]] | 45 matches | All events |
| [[creating-scoring-opportunities-trajectory-prediction\|C-OBSO]] | 34 Yokohama F. Marinos matches | **412 shot-ending sequences** |
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 54 matches, incl. all 34 Yokohama | **~2,900 attacking-third possessions** |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | The same 54 matches | **1,583 last-pass sequences** |
| **[[ai-football-reinforcement-learning\|Scott et al.]]** | **5 matches**, 3 teams | **Passes and shots only** |

VDEP's data was distributed jointly with the Research Center for Medical and Health Data Science at the Institute of Statistical Mathematics via a research competition. Scott et al. state theirs was **purchased**.

## Why It Is Worth Recording

**It is the vault's only non-Western data provider.** Everything else runs on [[stats-perform|STATS LLC / Opta]], Wyscout, or StatsBomb, covering European and South American football. A J-League source is a meaningful widening — the vault's findings otherwise describe elite European football and generalise elsewhere only by assumption, the data-availability instance of [[selection-bias]].

**The access routes differ and both are unusual.** VDEP's data arrived through an academic competition; Scott et al. bought theirs outright. Both are materially different from the club-embedded access behind [[expected-value-possession-framework|the Barcelona EPV work]] or the vendor licensing behind [[vaep]], and between them they represent the two realistic routes for academics without a club affiliation.

**Provider conventions are load-bearing.** VDEP's definitions of *ball recovery* and *effective attack* are built on this provider's 19-type event taxonomy. As with [[duel-skill-rating|duel-winner definitions]] and the vendor differences that motivated [[spadl]], a metric defined over an annotation scheme inherits that scheme's judgements.

## Five Extractions From One Dataset

> **Extended 2026-08-08.** The fifth source makes the pattern unmistakable.

Every one of the five **subsets and reshapes this data differently**, and each choice is a modelling decision nobody defends. C-OBSO takes shot-ending sequences; Fujii et al. take last-pass sequences whether or not a goal followed; Nakahara et al. take attacking-third possessions regardless of ending; VDEP takes everything; **Scott et al. flatten to passes and shots only, in a simplified [[spadl|SPADL]] format**, discarding everything else because their comparison target is a simulator that produces nothing else.

That is a partial explanation for the vault's most striking number — C-OBSO and Nakahara's Q-values correlating at $\rho = 0.182$. **Metrics computed over different populations of moments need not agree, and the shared name conceals the difference.** See [[construct-validity]].

The action vocabulary diverges too: Nakahara et al. discard dribbling and trapping labels; Fujii et al. split passing into high and short and drop sprint states; Scott et al. keep only passes and shots. **The provider's taxonomy sets the ceiling and each paper sets its own floor.** See [[action-space-design]].

## The Provider That Made the Vault's Comparisons Possible

Two held metrics — [[c-obso|C-OBSO]] and Nakahara et al.'s Q-values — are computed on **the same 34 Yokohama F. Marinos matches**, which is what allows them to be correlated at all. That $\rho = 0.182$ is the vault's only head-to-head between two off-ball metrics. See [[off-ball-value]].

**Scott et al. add a second kind of comparison this data enabled**: real J-League pass networks as the reference against which simulated agents were measured. Yokohama appear there too, alongside Kashima Antlers and FC Tokyo. So this provider underwrites both the vault's only metric-versus-metric comparison **and** its only simulation-versus-reality comparison. See [[social-network-analysis]] and [[domain-adaptation]].

> ### `shared-data-not-shared-method-enables-comparison`
> **Cross-framework comparison requires two methods on one dataset, which licensing usually prevents. Where a single provider serves a single group repeatedly, comparison becomes available almost by accident — and is still mostly not done.**
> ^[generated: drawn across the five sources; none states it. rests-on: absence:no-held-source-benchmarks-across-frameworks, source:nakahara-obso-cobso-correlations, source:scott-jleague-comparison]

**Five papers now share this dataset and only two comparisons have been drawn.** The remaining pairings are equally computable and unrun. Availability is necessary and plainly not sufficient.

That also bounds what the comparisons show: one league, one season, one provider's conventions, and — per the table above — not even the same subset of moments.

## See Also

- [[event-stream-data]] · [[optical-tracking-data]] · [[spadl]] · [[action-space-design]] · [[selection-bias]]
- [[stats-perform]] · [[vdep]] · [[c-obso]] · [[off-ball-value]] · [[construct-validity]] · [[social-network-analysis]] · [[domain-adaptation]] · [[action-valuation-frameworks-compared]]
- [[nagoya-university]] · [[aist]] · [[keisuke-fujii]] · [[atom-scott]] · [[hiroshi-nakahara]] · [[masakiyo-teranishi]] · [[kazushi-tsutsui]]
- [[football-defence-evaluation-vdep|VDEP]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] · [[ai-football-reinforcement-learning|Scott et al.]]
