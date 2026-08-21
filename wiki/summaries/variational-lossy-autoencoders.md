---
title: "Variational Lossy Autoencoder — Source Summary"
type: summary
tags: [deep-learning, generative-model, vae, autoregressive-model, representation-learning, density-estimation, counterfactual]
sources: [raw/papers/variational-lossy-autoencoders.md]
confidence: 0.9
provenance:
  extracted: 80%
  inferred: 12%
  generated: 6%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-10
updated: 2026-08-14
---

# Variational Lossy Autoencoder

**Chen, Kingma, Salimans et al.**, UC Berkeley and [[openai]], ICLR 2017; arXiv:1611.02731.

> **Held here because [[variational-autoencoder|the VAE]] is the substrate of [[c-obso|C-OBSO's]] trajectory predictor.** The density-estimation benchmarks and the autoregressive-flow prior live in the general vault. What follows is the argument football uses.

## The Information Preference Property

When a VAE's decoder is powerful enough — autoregressive, say — **the latent code gets ignored** and the model collapses to an unconditional autoregressive model.

Earlier work blamed optimisation difficulty. This paper shows it is structural: using the latent incurs an unavoidable cost $D_{KL}(q(\mathbf{z}\mid\mathbf{x}) \| p(\mathbf{z}\mid\mathbf{x}))$ from imperfect posterior approximation. **If the decoder can model the data without $\mathbf{z}$, the model avoids that cost by not using it.**

## The Move That Matters

Rather than treating this as a failure, VLAE turns it into a **design lever**.

Constrain the decoder's receptive field — a small-window PixelCNN — and information requiring long-range dependencies *cannot* be modelled locally. It must flow through $\mathbf{z}$:

- **Local statistics** (texture, noise) → the autoregressive decoder
- **Global structure** (shape, identity) → the latent code

**The type of information in the latent becomes controllable by architecture.** On MNIST, VLAE encodes an image in 19.2 bits against a standard VAE's 37.3, preserving digit identity while discarding local style. Lossiness becomes a specification rather than a defect.

## Why This Matters in Football

Two connections, and the second is the more interesting.

**The direct one.** [[c-obso|C-OBSO]] uses a GVRNN — a VAE composed with recurrence and graph message passing — to predict how an average player would have moved. The latent is what carries multimodality: a deterministic model asked for several plausible futures returns their *average*, and the midpoint between "runs left" and "runs right" is "stands still". See [[variational-autoencoder]] and [[trajectory-prediction]].

**The structural one.** VLAE's insight is that **a representation learns what it is not given for free.** That is the same mechanism [[scoutgpt|ScoutGPT]] exploits by masking position tokens during training, forcing role to be inferred from player identity and surrounding events — and the resulting embeddings separate by position *anyway*, with better cross-season retrieval. Two unrelated papers, same principle. See [[representation-learning]].

## The Cautionary Half

VLAE also demonstrates that **likelihood and sample quality can be traded against each other deliberately.** That undercuts likelihood as an evaluation criterion for generative models used as *means* rather than ends — which is every generative application in this vault.

[[scoutgpt|ScoutGPT's]] value depends on its counterfactuals being right, not its perplexity being low, and held-out likelihood cannot tell you whether substituting a player produces a realistic alternative world. See [[generative-model]] and [[counterfactual-simulation]].

## See Also

- [[variational-autoencoder]] · [[generative-model]] · [[autoregressive-model]] · [[representation-learning]] · [[kl-divergence]]
- [[c-obso]] · [[trajectory-prediction]] · [[scoutgpt]] · [[counterfactual-simulation]] · [[counterfactual-baseline]]
- [[openai]] · [[creating-scoring-opportunities-trajectory-prediction|C-OBSO Summary]] · [[scoutgpt-counterfactual-player-valuation|ScoutGPT Summary]]
