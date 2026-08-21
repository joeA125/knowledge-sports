---
title: "OpenAI"
type: entity
tags: [organisation, ai-research]
sources: [raw/papers/variational-lossy-autoencoders.md, raw/papers/training-lm-follow-instructions-with-human-feedback.md]
confidence: 0.85
provenance:
  extracted: 50%
  inferred: 40%
  generated: 5%
  ambiguous: 5%
lifecycle: draft
created: 2026-05-10
updated: 2026-08-14
---

# OpenAI

AI research organisation. Two of its papers are held here, both because a football framework depends on them rather than for their own sake.

- [[training-lm-follow-instructions-with-human-feedback|InstructGPT]] (Ouyang et al., 2022) — the [[rlhf]] alignment pipeline, and the vault's cleanest instance of the [[imitation-reward-tradeoff|imitation/reward trade-off]]. Reached here via [[proximal-policy-optimization|PPO]], which Scott et al. use in GFootball.
- [[variational-lossy-autoencoders|Variational Lossy Autoencoder]] (Chen et al., 2017) — underpins [[variational-autoencoder]], and through it the GVRNN trajectory predictor in [[c-obso|C-OBSO]].

> **Scope note.** OpenAI's wider output — GPT, scaling laws, the pre-training paradigm — is covered in the general vault, along with the people associated with it. This page holds only what bears on football analytics.

## See Also

- [[rlhf]] · [[proximal-policy-optimization]] · [[imitation-reward-tradeoff]] · [[reinforcement-learning]]
- [[variational-autoencoder]] · [[generative-model]] · [[c-obso]]
- [[training-lm-follow-instructions-with-human-feedback|InstructGPT Summary]] · [[variational-lossy-autoencoders|VLAE Summary]]
