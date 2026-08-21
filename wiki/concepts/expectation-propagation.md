---
title: "Expectation Propagation"
type: concept
tags: [bayesian, statistics, inference, approximation, ranking-system]
sources: [raw/papers/bayesian-true-skill-rating.md]
confidence: 0.8
provenance:
  extracted: 50%
  inferred: 42%
  generated: 5%
  imported: 0%
  ambiguous: 3%
lifecycle: draft
created: 2026-05-08
updated: 2026-08-14
---

# Expectation Propagation

An approximate inference algorithm (Minka, 2001). It replaces intractable factors with members of an exponential family — typically Gaussians — by **moment matching**, which minimises $\text{KL}(p \| q)$.

The mechanism, briefly: remove a factor's current approximation from the posterior to get a *cavity* distribution, multiply the cavity by the true factor, project the result back into the exponential family by matching moments, and set the approximate factor to the ratio of the two.

## Why It Is Held Here

[[trueskill]] needs it. Skill beliefs are Gaussian, but the factors encoding *who beat whom* are not — a comparison outcome truncates the distribution rather than shifting it. Exact messages are therefore intractable, and EP supplies the Gaussian approximation via truncated-Gaussian integrals.

**Without EP there is no TrueSkill**, and without TrueSkill [[ai-football-reinforcement-learning|Scott et al.]] have no principled way to rank fifteen RL agents on a common scale. That is the whole chain by which an approximate-inference algorithm from 2001 ends up in a football vault.

## What It Buys Over the Alternative

The comparison worth holding onto is with the moment-matching-free approach: Elo updates a point estimate by a fixed step. EP-based systems carry a **distribution** and update it by an amount that depends on how uncertain both parties are — which is what makes [[glicko-rating-system|Glicko's]] rating deviation and TrueSkill's $\sigma$ behave sensibly for new or returning competitors.

See [[uncertainty-quantification]], where the vault's habit of computing uncertainty and then discarding it downstream is set out.

> **Scope note.** EP's wider role in approximate inference — the general algorithm, its relatives, and the surrounding factor-graph machinery — lives in the general vault. This page keeps the TrueSkill chain.

## See Also

- [[trueskill]] · [[bayesian-inference]] · [[message-passing]] · [[uncertainty-quantification]]
- [[glicko-rating-system]] · [[elo-rating-system]] · [[bradley-terry-model]] · [[league-strength-rating]]
- [[kl-divergence]] · [[bayes-theorem]] · [[microsoft-research]]
- [[bayesian-true-skill-rating|TrueSkill Summary]] · [[ai-football-reinforcement-learning|Scott et al. Summary]]
