---
title: "Autoregressive Model"
type: concept
tags: [deep-learning, generative-model, autoregressive-model, sequence-modelling, point-process, density-estimation, event-prediction, model-selection, sports-analytics]
sources: [raw/papers/variational-lossy-autoencoders.md, raw/papers/transformer-point-process-football-event-modelling.md]
confidence: 0.85
provenance:
  extracted: 55%
  inferred: 35%
  generated: 7%
  imported: 0%
  ambiguous: 3%
lifecycle: reviewed
created: 2026-05-10
updated: 2026-08-14
---

# Autoregressive Model

Factorises a joint distribution by the chain rule — $p(\mathbf{x}) = \prod_i p(x_i \mid x_{<i})$ — modelling each element conditioned on all previous ones. The decomposition is **assumption-free** and, with a sufficiently powerful model, can represent arbitrary distributions.

> **Scope note.** The general treatment — language models, PixelCNN, WaveNet, exposure bias — lives in the general vault. This page keeps the two things football event modelling actually exploits.

## Factorising *Within* an Element, Not Just Across a Sequence

The chain rule applies to any set of variables, not only sequence positions. [[nmstpp|NMSTPP]] uses it to decompose the *components of a single event* — its inter-event time, zone, and action type:

$$f(t_i, z_i, m_i \mid H_i) = f_t(t_i \mid H_i) \; f_z(z_i \mid t_i, H_i) \; f_m(m_i \mid t_i, z_i, H_i)$$

Each component gets its own network, but each conditions on the components already predicted. **Autoregression across an event's attributes, nested inside autoregression across the event sequence.**

The payoff is measurable: severing the within-event links so each network sees only $H_i$ raises total loss from 4.40 to 4.44, with the entire degradation falling on the last component in the chain.

This is also how NMSTPP pays for its [[encoder-decoder-bottleneck|single shared history vector]] — components recover dependence through the chain rather than through separately attended history.

## Ordering Is Free in Theory, Not in Practice

Any ordering of the chain rule gives a valid factorisation of the same joint distribution. With unlimited capacity and data, ordering would be irrelevant.

It is not. [[transformer-point-process-football-event-modelling|Yeung et al.]] grid-search all six orderings of $(t, z, m)$ and find a **0.18 spread in total loss** — $(t, z, m)$ best, $(z, t, m)$ worst — with the difference concentrated almost entirely in the final component's loss.

> ### `factorisation-order-is-an-unswept-parameter`
> **Where a model factorises a multi-component prediction, the ordering is a free parameter that is exact in theory and consequential in practice. Yeung et al. are the only held source to search it; every other multi-component football model asserts one.**
> ^[generated: the source reports its own grid search; the observation that it is unique in doing so is drawn here. rests-on: source:yeung-ordering-gridsearch, absence:no-sensitivity-analysis-on-horizon-parameters]

The practical reading: **ordering determines which conditionals are easy** — put the components that best predict the others first. See [[free-parameters-load-bearing]], where this joins sixteen other asserted parameters, and note that it is one of only two cases in the vault where such a parameter was actually searched.

## Relation to Point Processes

A temporal [[point-process]] shares exactly this structure — $f(t_1, t_2, \dots) = \prod_i f(t_i \mid H_i)$ — but over continuous time rather than a fixed sequence index. **The extra work a point process does is modelling *when* the next element occurs, not merely what it is.** That is the whole content of the step from a sequence model like [[seq2event]] to an [[neural-temporal-point-process|NTPP]] like NMSTPP.

## Relation to VAEs

Used as a VAE decoder, an autoregressive model can reconstruct data without using the latent code at all — the information-preference problem. [[variational-autoencoder|VLAE]] turns that failure into a design lever by restricting the decoder's receptive field, forcing global structure into the latent. See [[variational-autoencoder]].

## See Also

- [[nmstpp]] · [[point-process]] · [[neural-temporal-point-process]] · [[encoder-decoder-bottleneck]] · [[event-prediction]] · [[seq2event]]
- [[generative-model]] · [[variational-autoencoder]] · [[transformer]] · [[lstm]] · [[attention-mechanism]]
- [[free-parameters-load-bearing]] · [[model-selection]] · [[hpus]] · [[scoutgpt]] · [[large-event-model]]
- [[transformer-point-process-football-event-modelling|NMSTPP Summary]] · [[variational-lossy-autoencoders|VLAE Summary]]
