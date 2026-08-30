---
title: "The Permutation Problem (Unordered Players)"
type: concept
tags: [set-modelling, ordering, multi-agent, sports-analytics, trajectory-prediction, graph-neural-network, dynamical-systems, model-selection, combinatorial-optimisation, interpretability]
sources: [raw/papers/team_sports_data_analysis.md, raw/papers/sequence-to-sequence-sets.md]
confidence: 0.8
provenance:
  extracted: 48%
  inferred: 22%
  generated: 18%
  imported: 11%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# The Permutation Problem (Unordered Players)

**Twenty-two players have no canonical ordering, and every model that takes them as input has to choose one anyway.** A neural network's input layer, a distance function between two passages of play, and a matrix of inter-agent relations all require an index — and nothing in football supplies it.

The problem is easy to state and easy to miss, because a model that silently sorts by shirt number will train, converge, and report a number.

## The Vault Now Holds a Source That Names It

> **Created 2026-08-29.** [[data-driven-team-sports-behaviors|Fujii's 2021 survey]] states the problem directly — comparing agent-to-agent trajectories "encounters permutation problems among the players" — and catalogues the responses.
>
> ⚠️ **This partly falsifies a premise the vault was carrying.** [[sequence-to-sequence-sets]] recorded that football frameworks solve this problem "mostly without acknowledging it is a known one". A held football source, by the vault's most central author, **does** acknowledge it as a known one and names it as such. The claim survives about individual *method* papers and fails about the field. See below.

## Four Responses, Not Three

The vault held three. The survey supplies a fourth, and it is the one with the longest track record.

| Route | Mechanism | Property | Where |
|---|---|---|---|
| **Sort by a key** | Impose a canonical order — e.g. distance to ball | None; a *choice* | [[vdep]], [[gvdep]] |
| **Symmetric aggregation** | Message passing over a graph | **Equivariant** | [[graph-neural-network\|GNN]] behind [[c-obso]] |
| **Attention over a set** | Learned order-free weighting | Equivariant | [[transformer]] via [[nmstpp]] |
| **Role assignment** ⭐ | **Solve a linear assignment to map players onto roles** | Canonical *by optimisation* | Basketball and soccer work described in the survey; **not held** |
| **Dynamical invariants** ⭐ | Extract dynamics, which do not carry player identity | **Invariant** | [[koopman-mode-decomposition]]; **not held** |

⭐ = added on this ingest.

### Role assignment is the interesting addition

Rather than sorting by a fixed key or engineering the model to ignore order, **role assignment computes the ordering as the solution to an optimisation** — the Hungarian algorithm for linear assignment, mapping players onto tactical roles (guard, forward, centre in basketball; positional roles in soccer).^[imported: described in Fujii's survey; the primary works are not held]

This sits between the vault's existing routes in a way none of them occupies:

- **Sorting by ball distance** is cheap and arbitrary — the key is asserted.
- **Equivariance** refuses the question and pays in interpretability, because there is no longer a "third player" to point at.
- **Role assignment** produces a canonical order *and* keeps it meaningful, at the cost of needing a role model and an $O(n^3)$ solve per frame.

> ### `permutation-handling-trades-order-against-interpretability`
> **The four routes past the permutation problem are not interchangeable: they differ in whether the resulting player index means anything. Sorting and role assignment yield an index a coach can read; equivariance and dynamical invariance yield a model whose per-player outputs cannot be attributed without extra machinery. A framework that needs to name a player has effectively already chosen its route.**
> ^[generated: no source frames the routes as a trade-off; the survey lists them and the vault's pages treat them separately. rests-on: source:fujii-survey-permutation-routes, source:vdep-proximity-sort]

This connects a problem the vault treated as an architectural detail to one it treats as central. [[off-ball-value]] records that of six off-ball mechanisms, some report **team-only** figures and cannot individuate. **At least part of that is the permutation route, chosen upstream for unrelated reasons.**^[inferred: the survey does not link permutation handling to reporting granularity; drawn here against [[off-ball-value]]]

## What Actually Falsified, and What Survived

The claim on [[sequence-to-sequence-sets]] had two parts. They come apart.

| Part | Status |
|---|---|
| *The ordering is a free parameter with measurable effect* | **Survives.** Unchanged, and still untested |
| *No held source compares VDEP's sort against alternatives* | **Survives.** The survey lists alternatives; it compares nothing |
| *Football frameworks mostly do not acknowledge this is a known problem* | ⚠️ **Falsified at field level.** Fujii's survey names it explicitly and catalogues four responses |

**Narrowed rather than deleted**, per the conventions. The surviving form: *individual football method papers mostly do not acknowledge the permutation problem, though the field's own survey does — so the choice is made in awareness that alternatives exist, which makes an unexamined sort harder to excuse rather than easier.*

That is a **stronger** claim than the original, not a weaker one. An unmarked arbitrary choice is a blind spot; the same choice made in a field that has catalogued the alternatives is a decision nobody wrote down.

## The Test, Restated

Unchanged from [[sequence-to-sequence-sets]], and now with a fourth arm:

Recompute [[vdep|VDEP]]'s classifier under **ball-distance sort** (the current choice), **role assignment**, **shirt number** (a deliberate null), and **randomised order averaged over draws**. Report Spearman's $\rho$ between the resulting team rankings.

- If shirt number and ball distance agree, the sort is not load-bearing and [[gvdep|GVDEP]]'s finding that ball-gain prediction saturates at three or four sorted players is the whole story.
- If role assignment beats both, the ordering is carrying tactical information the current key discards.

The null arm matters. Without it, two sensible orderings agreeing is uninformative — they may agree because the model ignores order entirely.^[generated: the null arm is added here; not proposed by any source]

## Why It Is Easy to Miss

Ordering has no error signal. A wrong horizon parameter produces wrong values; a wrong gate produces the wrong event set; **a wrong ordering produces a model that trains normally and reports a plausible number.** Nothing fails.

That places it alongside the entries on [[free-parameters-load-bearing]] — asserted, consequential, unswept — while being harder to see than any of them, because it is not written as a parameter at all. It is written as an array index.

## See Also

- [[sequence-to-sequence-sets|Order Matters]] — the general-ML source, and the claim this page resolves
- [[data-driven-team-sports-behaviors|Fujii's survey]] — where the problem is named in a football context
- [[koopman-mode-decomposition]] — the permutation-invariant route
- [[graph-neural-network]] · [[message-passing]] · [[transformer]] · [[attention-mechanism]] · [[trajectory-prediction]]
- [[vdep]] · [[gvdep]] · [[c-obso]] · [[nmstpp]] · [[off-ball-value]]
- [[free-parameters-load-bearing]] · [[model-selection]] · [[action-space-design]] · [[interpretability]]
