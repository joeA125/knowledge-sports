---
title: "Reinforcement Learning from Human Feedback"
type: concept
tags: [deep-learning, reinforcement-learning, alignment, policy-gradient, imitation-learning, auxiliary-loss, evaluation]
sources: [raw/papers/training-lm-follow-instructions-with-human-feedback.md]
confidence: 0.9
provenance:
  extracted: 70%
  inferred: 20%
  generated: 8%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-06-13
updated: 2026-08-14
---

# Reinforcement Learning from Human Feedback

An alignment technique that trains a model to follow human intent by optimising against a **reward model learned from human preference judgments** rather than a hand-specified reward.

> **Scope note.** RLHF's role in language modelling is covered in the general vault. This page holds what bears on football analytics: RLHF is the vault's **best-behaved instance of the [[imitation-reward-tradeoff|imitation/reward trade-off]]**, and the standard against which the football RL papers fall short.

## The Pipeline, Briefly

[[training-lm-follow-instructions-with-human-feedback|Ouyang et al. (2022)]]:

1. **Supervised fine-tuning** on human demonstrations
2. **Reward model** trained on human preference rankings
3. **Policy optimisation** with [[proximal-policy-optimization|PPO]], plus a **[[kl-divergence|KL]] penalty** anchoring the policy to the SFT model

Step 3 is the one that matters here.

## Why This Page Is Football-Side

Every RL framework in this vault must balance **reproducing observed behaviour** against **maximising reward**. [[action-supervision|Nakahara et al.]] weight an imitation loss by $\lambda_1$; [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] measure the trade-off against DTW distance. RLHF does the same thing with a KL penalty — and does it better in one specific, checkable respect:

| | Anchor to observed behaviour | Coefficient reported? | Swept? |
|---|---|---|---|
| **RLHF** | KL to the SFT policy | **Yes** | **Yes** |
| [[action-supervision\|Nakahara et al.]] | Cross-entropy on softmax-$Q$ | $\lambda_1 = 0.01$ | Two points only |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | DTW-adaptive supervision | **No value given** | No |

> ### `anchoring-strength-should-be-reported`
> **Where a policy is regularised toward observed behaviour, the anchoring coefficient determines how much apparent suboptimality is the model's finding and how much is the modeller's choice. RLHF reports and sweeps it as standard; the football RL literature does neither.**
> ^[generated: no source draws this comparison. Bears directly on `optimality-gap-is-tunable`, declared on [[action-supervision]]. rests-on: source:ouyang-kl-penalty, source:nakahara-lambda-tradeoff, source:fujii-lambda-unreported]

That is the whole reason this page is retained football-side. It is the existence proof that reporting the coefficient is normal practice elsewhere — which makes its absence in [[free-parameters-load-bearing|the football literature]] a choice rather than a convention.

## Two Further Points That Transfer

**Alignment can substitute for scale.** InstructGPT 1.3B was preferred by labellers over a 175B model. The football analogue is on [[gated-recurrent-unit]] and [[handcrafted-features-rule]] — at football data scale, capacity is rarely the binding constraint.

**Reward models can be gamed.** A policy may find outputs scoring highly without satisfying intent. The direct parallel is [[rare-event-proxy-targets|proxy substitution]]: [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] add a shot reward because goals are sparse, and nothing checks whether agents then optimise for shots rather than goals.

## Limitations

- Expensive: large-scale human annotation for both demonstrations and comparisons.
- Reward hacking, above.
- Imperfect labeller agreement introduces noise into the reward signal — an uncertainty the football frameworks do not have an equivalent of, since their reward is a scoreline.

## See Also

- [[imitation-reward-tradeoff]] · [[action-supervision]] · [[proximal-policy-optimization]] · [[reinforcement-learning]] · [[imitation-learning]]
- [[kl-divergence]] · [[free-parameters-load-bearing]] · [[observed-versus-optimal-decisions]] · [[rare-event-proxy-targets]]
- [[openai]] · [[training-lm-follow-instructions-with-human-feedback|InstructGPT Summary]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al. Summary]]
