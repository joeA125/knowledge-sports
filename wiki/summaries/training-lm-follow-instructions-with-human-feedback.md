---
title: "Training Language Models to Follow Instructions with Human Feedback — Source Summary"
type: summary
tags: [deep-learning, reinforcement-learning, alignment, policy-gradient, imitation-learning, auxiliary-loss, evaluation]
sources: [raw/papers/training-lm-follow-instructions-with-human-feedback.md]
confidence: 0.85
provenance:
  extracted: 72%
  inferred: 16%
  generated: 10%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-06-10
updated: 2026-08-14
---

# Training Language Models to Follow Instructions with Human Feedback (InstructGPT)

**Ouyang et al.**, [[openai]], NeurIPS 2022.

> **Held here for two reasons, neither of them language modelling.** It is the origin of [[proximal-policy-optimization|PPO]] as used by [[ai-football-reinforcement-learning|Scott et al.]] in GFootball, and it is the vault's **best-behaved instance of the [[imitation-reward-tradeoff|imitation/reward trade-off]]**. The alignment literature lives in the general vault.

## The Pipeline

1. **Supervised fine-tuning** on ~13K human-written prompt–demonstration pairs.
2. **Reward model** trained on ~33K human preference comparisons.
3. **Policy optimisation** with PPO against that reward model, plus a **KL penalty** anchoring the policy to the SFT model.

**InstructGPT 1.3B was preferred by labellers over GPT-3 175B** — 100× smaller. Alignment substituted for scale.

## Step 3 Is Why This Paper Is Here

Every RL framework in this vault balances **reproducing observed behaviour** against **maximising reward**, and each anchors differently:

| | Anchor | Coefficient reported? | Swept? |
|---|---|---|---|
| **InstructGPT** | KL to the SFT policy | **Yes** | **Yes** |
| [[action-supervision\|Nakahara et al.]] | Cross-entropy on softmax-$Q$ | $\lambda_1 = 0.01$ | Two points |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | DTW-adaptive supervision | **No value given** | No |

That anchoring strength governs how much apparent suboptimality survives into the results — which is the whole content of `optimality-gap-is-tunable` on [[action-supervision]]. **InstructGPT demonstrates that reporting and sweeping it is normal practice**, which makes its absence in the football literature a choice rather than a convention. See [[rlhf]] and [[free-parameters-load-bearing]].

## Two Further Transfers

**Reward models can be gamed.** A policy may score highly without satisfying intent. The football parallel is direct: [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] add a shot reward because goals are sparse, and nothing checks whether agents then optimise for shots rather than goals. See [[rare-event-proxy-targets]].

**The alignment tax.** Small regressions on unrelated benchmarks, mitigated by mixing pretraining data back in. The analogue is untested here — no football source asks what a metric optimised for one objective costs on others, which is the same gap [[capability-profiling]] identifies from the evaluation side.

## See Also

- [[rlhf]] · [[proximal-policy-optimization]] · [[imitation-reward-tradeoff]] · [[action-supervision]] · [[reinforcement-learning]] · [[imitation-learning]]
- [[kl-divergence]] · [[free-parameters-load-bearing]] · [[observed-versus-optimal-decisions]] · [[rare-event-proxy-targets]] · [[capability-profiling]]
- [[openai]] · [[ai-football-reinforcement-learning|Scott et al. Summary]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al. Summary]]
