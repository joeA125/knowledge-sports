---
title: "Google Brain"
type: entity
tags: [organisation, ai-research, google, simulator, agent-based-simulation]
sources: [raw/papers/attention-is-all-you-need.md]
confidence: 0.85
provenance:
  extracted: 55%
  inferred: 35%
  generated: 5%
  ambiguous: 5%
lifecycle: draft
created: 2026-05-07
updated: 2026-08-14
---

# Google Brain

Deep learning research team at Google, later merged into Google DeepMind. Two of its outputs matter to this vault, and they arrive by very different routes.

- **The [[transformer]]** ([[attention-is-all-you-need|Attention Is All You Need]], 2017) — reaches football through [[nmstpp|NMSTPP]], a Transformer-based point process for match events.
- **[[google-research-football|GFootball]]** (Kurach et al., AAAI 2020) — the 11v11 RL simulator. **Cited, not held.** Used directly by [[ai-football-reinforcement-learning|Scott et al.]], borrowed for its action vocabulary by [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]], and ultimately replaced by [[nfootball|NFootball]].

The second is the more consequential here. GFootball's value to the field was never its physics but that it was **shared** — a fixed reference point that made simulated-versus-real comparison possible at all. See [[agent-based-simulation]].

> **Scope note.** The Transformer's architectural detail and its authors are covered in the general vault. This page holds only what bears on football analytics.

## See Also

- [[google-research-football]] · [[nfootball]] · [[agent-based-simulation]] · [[domain-adaptation]] · [[proximal-policy-optimization]]
- [[transformer]] · [[attention-mechanism]] · [[nmstpp]]
- [[google-research]] · [[atom-scott]] · [[masaki-onishi]]
- [[attention-is-all-you-need|Transformer Summary]] · [[ai-football-reinforcement-learning|Scott et al. Summary]]
