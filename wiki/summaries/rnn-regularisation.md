---
title: "Recurrent Neural Network Regularization — Source Summary"
type: summary
tags: [deep-learning, rnn, lstm, regularization, training-technique, model-selection]
sources: [raw/papers/rnn-regularisation.md]
confidence: 0.85
provenance:
  extracted: 78%
  inferred: 12%
  generated: 8%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Recurrent Neural Network Regularization

**Zaremba, Sutskever & Vinyals**, [[google-brain]], ICLR 2015; arXiv:1409.2329.

> **Held on forward-looking grounds (D2).** No football source cites it. It is retained because **small-data overfitting is the binding constraint across the football RL corpus**, and this is the vault's only source addressing regularisation of recurrent models directly. The dropout mechanics live in the general vault.

## The Contribution

Dropout applied naïvely to recurrent connections **amplifies noise across timesteps and destroys the LSTM's long-term memory.** The fix is placement: apply it to non-recurrent connections only — between layers, not within a layer across time.

Information is then corrupted exactly $L + 1$ times regardless of sequence length, rather than once per timestep.

The consequence was practical and large. Before this, practitioners used **smaller RNNs that underfitted**, because larger ones overfitted with no effective regulariser available.

| Model | Test perplexity (PTB) |
|---|---|
| Non-regularised LSTM | 114.5 |
| Medium regularised (650 units, 50% dropout) | 82.7 |
| Large regularised (1500 units, 65% dropout) | **78.4** |

Gains replicated on speech recognition, translation and captioning.

## Why It Bears on Football

The finding that matters is not the technique but **the trap it describes**: without a working regulariser, model size gets capped by overfitting rather than chosen on merit.

That is precisely the position the football RL papers are in:

| Framework | Training set | Response |
|---|---|---|
| [[action-valuation-multi-agent-reinforcement-learning\|Nakahara et al.]] | 1,669 sequences | $L_1$ penalty **and** a 64-unit [[gated-recurrent-unit\|GRU]] |
| [[adaptive-action-supervision-multi-agent-rl\|Fujii et al.]] | 1,121 sequences | $L_2$ penalty, no recurrence |

**Both cap capacity as well as penalising weights** — the Zaremba trap, handled by shrinking the model rather than by regularising a larger one. Whether a larger recurrent network with correctly-placed dropout would beat a small GRU on football tracking data is untested, and this paper is the reason to think it might be worth trying.

Neither paper reports its penalty weight. See [[regularization]] and [[free-parameters-load-bearing]].

> ### `capacity-capping-substitutes-for-regularisation`
> **Where no effective regulariser is available, model size is chosen by what will not overfit rather than by what the task needs. The football RL papers cap capacity and penalise weights simultaneously, and neither reports whether the cap is binding.**
> ^[generated: connects Zaremba's stated motivation to the football papers' architecture choices; none of the three addresses this. rests-on: source:zaremba-underfitting-motivation, source:nakahara-l1-justification, source:fujii-l2-justification]

## See Also

- [[regularization]] · [[lstm]] · [[gated-recurrent-unit]] · [[deep-q-network]] · [[free-parameters-load-bearing]]
- [[handcrafted-features-rule]] · [[theory-based-modelling]] · [[representation-learning]] · [[model-selection]]
- [[google-brain]] · [[action-valuation-multi-agent-reinforcement-learning|Nakahara et al.]] · [[adaptive-action-supervision-multi-agent-rl|Fujii et al.]]
