---
title: "SPADL (Soccer Player Action Description Language)"
type: concept
tags: [sports-analytics, data-engineering, action-space, event-stream-data, simulator, evaluation, action-valuation]
sources: [raw/papers/evaluating-football-player-actions.md, raw/papers/ai_football_reinforcement_learning.md]
confidence: 0.9
provenance:
  extracted: 80%
  inferred: 13%
  generated: 5%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-07-20
updated: 2026-08-08
---

# SPADL (Soccer Player Action Description Language)

SPADL ([[evaluating-football-player-actions|Decroos et al., 2019]]) is a unified language for representing soccer event stream data, designed to address five data science challenges with vendor-specific formats (Opta, Wyscout, StatsBomb): conflicting terminology, backward compatibility bloat, mixed objectives, optional information snippets, and variable-length features.

## Representation

SPADL represents a game as a sequence of on-ball actions $[a_1, \ldots, a_m]$, where each action is a fixed tuple of 9 attributes:

| Attribute | Description |
|---|---|
| StartTime / EndTime | Action's start and end time |
| StartLoc / EndLoc | $(x, y)$ start and end location |
| Player / Team | Who performed it |
| ActionType | One of 21 types |
| BodyPart | foot, head, other, none |
| Result | success, fail, offside, own goal, yellow card, red card |

The key design principle is **fixed-length representation**: every action has exactly the same 9 attributes, making actions directly amenable to ML algorithms requiring fixed-length feature vectors.

## Actions vs Events

SPADL distinguishes actions (requiring a player — passes, shots, tackles) from events (game start/end, referee decisions). Only actions are represented.

## Action Types

21 types: pass, cross, throw-in, crossed corner, short corner, crossed free-kick, short free-kick, take-on, foul, tackle, interception, shot, penalty shot, free-kick shot, keeper save, keeper claim, keeper punch, keeper pick-up, clearance, bad touch, dribble. Passes dominate (64.6%), then dribbles (8.7%) and interceptions (5.0%).

## Adoption, and a Practitioner's Complaint

SPADL provides the data foundation for the [[vaep]] framework and has been adopted as a common interchange format, with a Python package converting Opta, Wyscout and StatsBomb streams automatically.

> **Added 2026-08-08.** Adoption is not the same as sufficiency, and [[ai-football-reinforcement-learning|Scott, Fujii & Onishi]] supply an author-side statement of where it falls short.

They needed to compare **simulated** football against **real** football, and found no representation that spanned both:

> we noticed that it was difficult to use state of the art football analysis methods due to different representations of the underlying data

Their workaround was to convert both [[google-research-football|GFootball]] output and [[data-stadium|Data Stadium]] J-League event data into a **simplified SPADL** — passes and shots only, dropping the other 19 action types — and they close by asking the community to build on such representations so existing methods can be reused.

Two things follow.

**The barrier to cross-framework comparison is named as representational rather than social.** The vault records `no-cross-framework-benchmarking` largely as a matter of groups not comparing themselves to competitors. Here a practitioner says the obstacle is more basic: the methods *cannot be applied* across datasets because the data does not share a shape. See [[action-valuation-frameworks-compared]].

**SPADL's coverage stops at the simulator boundary.** It was designed to unify *commercial event vendors*, and does that well. A simulator emits a different kind of thing — a state–action trace over an [[action-space-design|action space]] chosen for control, not annotation — and the intersection turns out to be two action types.

> ### `interchange-formats-unify-within-a-modality-not-across-one`
> **A representation standard resolves disagreement among sources of the same kind and offers little where the kinds differ. SPADL unifies event vendors and reduces to a two-type subset the moment a simulator is on the other side of the comparison.**
> ^[generated: no source states this; drawn from Scott et al.'s workaround. rests-on: source:scott-simplified-spadl, source:decroos-spadl-design-goals]

## Two Vocabularies, Opposite Directions

SPADL and GFootball's action set do structurally similar work — both impose a shared answer to *what counts as an action* — from opposite ends:

| | **SPADL** | **[[google-research-football\|GFootball]]** |
|---|---|---|
| Purpose | **Describe** what happened | **Control** what happens |
| Types | 21 | 19 |
| Includes | tackles, fouls, clearances, keeper actions, take-ons | 8 movement directions, sprint and dribble *state* |
| Excludes | **movement** — it is on-ball only | most defensive and set-piece actions |
| Result field | Yes — success/fail | No — outcomes emerge from dynamics |

**Neither is a superset.** SPADL cannot express "the player ran north-east"; GFootball cannot express "the tackle succeeded". The overlap is passes and shots, which is precisely what Scott et al. were reduced to.

That also explains a choice the vault flagged as odd on [[action-space-design]]: [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] adopt GFootball's vocabulary over SPADL despite working on real tracking data, and consequently discard the dribbling and trapping labels their provider supplies. **The movement-first vocabulary was the one that matched their question**, and the cost was on-ball granularity SPADL would have preserved.

## See Also

- [[vaep]] · [[action-valuation]] · [[event-stream-data]] · [[optical-tracking-data]] · [[action-space-design]]
- [[google-research-football]] · [[nfootball]] · [[social-network-analysis]] · [[data-stadium]] · [[stats-perform]]
- [[game-state-reconstruction]] · [[camera-calibration-benchmarking]] · [[action-valuation-frameworks-compared]]
- [[tom-decroos]] · [[jesse-davis]] · [[atom-scott]]
- [[evaluating-football-player-actions|Decroos et al. Summary]] · [[ai-football-reinforcement-learning|Scott et al. Summary]]
