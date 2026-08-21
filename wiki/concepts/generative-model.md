---
title: "Generative Model"
type: concept
tags: [generative-model, machine-learning, deep-learning, density-estimation, vae, autoregressive-model, counterfactual, representation-learning, sports-analytics, evaluation]
sources: [raw/papers/variational-lossy-autoencoders.md, raw/papers/scoutgpt-generative-transformer-football-player-valuation.md, raw/papers/evaluation_creating_scoring_opportunities_trajectory_prediction.md]
confidence: 0.85
provenance:
  extracted: 40%
  inferred: 52%
  generated: 5%
  imported: 0%
  ambiguous: 3%
lifecycle: draft
created: 2026-07-27
updated: 2026-08-14
---

# Generative Model

A model of the data distribution $p(x)$ itself, rather than only a conditional $p(y \mid x)$. It can be **sampled from**, which is what distinguishes it from a discriminative model and what makes a class of downstream uses possible.

## The Families

| Family | Mechanism | Likelihood | In this vault |
|---|---|---|---|
| **[[autoregressive-model\|Autoregressive]]** | Chain rule: $p(x) = \prod_t p(x_t \mid x_{<t})$ | Exact | [[large-event-model]], [[eventgpt]], [[scoutgpt]], [[nmstpp]] |
| **[[variational-autoencoder\|Latent variable / VAE]]** | Latent $z$, optimise a lower bound | Bounded | VRNN, GVRNN in [[trajectory-prediction]] |
| **Adversarial (GAN)** | Generator versus discriminator | Implicit | Synthetic-data camera calibration |

Autoregressive models give exact likelihoods but generate sequentially, so errors compound over long rollouts — the exposure-bias problem, covered in the general vault. VAEs give a usable latent space and fast sampling but blur detail. GANs give sharp samples and no likelihood at all.

> **Scope note.** The general theory of generative modelling lives in the general vault. This page keeps what football applications actually use it *for*, which is not sampling.

## What Generativity Buys Here

Being able to sample is rarely the point in this vault. Three derived capabilities are.

**Counterfactuals.** A model conditioned on an intervenable entity can be asked what *would* have happened. [[scoutgpt]] substitutes a player into a lineup and regenerates; [[eventgpt]] substitutes and re-scores. Neither is possible discriminatively. See [[counterfactual-simulation]].

**Reference behaviour.** A trajectory model trained on league-wide data produces what a *typical* player would have done — a baseline to measure deviation against. This is [[c-obso|C-OBSO's]] mechanism, and it inverts the usual objective: the model is wanted for its notion of *normal* rather than its accuracy, and the metric is identically zero under a perfect model. See [[counterfactual-baseline]] and [[imitation-learning]].

**Multimodality.** A deterministic model asked for several plausible futures returns their average, which is often physically implausible — the midpoint between "runs left" and "runs right" is "stands still". A stochastic latent lets the modes be represented separately. This is the specific failure motivating VRNN over RNN in [[trajectory-prediction]], where the endpoint-error gap is nearly an order of magnitude.

## The Evaluation Problem

Likelihood is available for autoregressive models, bounded for latent-variable ones, absent for GANs. But **high likelihood does not imply good samples and good samples do not imply high likelihood** — the two can be traded, which [[variational-lossy-autoencoders|the VLAE work]] exploits deliberately by pushing local detail into the decoder.

For football applications this bites hard, because the model is a *means*. [[scoutgpt|ScoutGPT's]] value depends on its counterfactuals being right, not its perplexity being low, and held-out likelihood cannot tell you whether substituting a player produces a realistic alternative world.

**And the substitute checks scale badly with representation fineness.** ScoutGPT and EventGPT regenerate *event tokens*, where a generated sequence cannot be physically impossible; [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]] regenerate *continuous movement* and fail to reproduce demonstrated football at all. See `regeneration-fidelity-scales-with-representation-coarseness` on [[counterfactual-simulation]].

## The Causal Caveat

Worth stating plainly because the language invites the error: a generative model trained on observational data learns the **observational** distribution. Intervening and regenerating gives the correct causal answer only if the model captured the right dependency structure and nothing important is unmeasured.

In football, observed performance is confounded with team quality, tactics and opposition. Conditioning on a lineup absorbs some of that; nothing guarantees the learned association is a causal effect. **"Generative" is not "causal"**, and treating them as equivalent is the most common error in this literature.

## See Also

- [[autoregressive-model]] · [[variational-autoencoder]] · [[trajectory-prediction]] · [[representation-learning]] · [[tokenization]]
- [[counterfactual-simulation]] · [[counterfactual-baseline]] · [[imitation-learning]] · [[domain-adaptation]] · [[agent-based-simulation]]
- [[scoutgpt]] · [[eventgpt]] · [[large-event-model]] · [[c-obso]] · [[nmstpp]] · [[event-prediction]] · [[space-creation]]
- [[variational-lossy-autoencoders|VLAE Summary]] · [[scoutgpt-counterfactual-player-valuation|ScoutGPT Summary]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO Summary]]
