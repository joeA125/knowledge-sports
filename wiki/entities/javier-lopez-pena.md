---
title: "Javier López Peña"
type: entity
tags: [person, researcher, network-analysis, sports-analytics, tactical-analysis, single-source]
sources: [raw/papers/football_strategy_network_theory_analysis.md]
confidence: 0.7
provenance:
  extracted: 48%
  inferred: 30%
  generated: 12%
  imported: 8%
  ambiguous: 2%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Javier López Peña

First author of [[network-theory-football-strategies|A network theory analysis of football strategies]], with [[hugo-touchette]] — the vault's **only primary source on pass-network analysis**, and the work that closed its longest-standing acquisition gap.

> ⚠️ **The vault cited this author wrongly for three ingests.** Four pages carried the work as **"Peña & Hugo (2012)"**, treating Touchette's forename as a second surname and truncating "López Peña" to "Peña". The error came from [[ai-football-reinforcement-learning|Scott et al.]]'s reference list and was extracted faithfully. Corrected 2026-08-29 across [[social-network-analysis]], [[action-valuation-frameworks-compared]], [[ai-football-reinforcement-learning]] and the overview.
>
> **"López Peña" is a compound surname**, not a middle name followed by a surname. Alphabetising or shortening it to "Peña" is the error, and it is the one the citation chain made.

## What Is Actually Evidenced

Very little, and the page says so rather than filling the gap.

| Evidenced | From |
|---|---|
| First authorship | The paper's byline |
| **EU Marie-Curie fellowship** PIEF-GA-2008-221519 | Acknowledgements |
| **Spanish MCIM grant** MTM2010-20940-C02-01 | Acknowledgements |
| Mathematics background | The paper's framing — see below |

**No institutional affiliation is stated in the text held here.** The byline carries a footnote marker whose target is not present in the extracted file.

⚠️ **Not asserted:** any specific university. The conventions' worked example on this exact failure — an entity page stating an affiliation from background knowledge that a later primary source contradicted — applies directly, and the temptation here is strong because the grant numbers *feel* like enough to guess from. They are not.

The grant pairing does support one careful reading: a **Marie-Curie Intra-European Fellowship funds mobility between European countries**, and it is held alongside a Spanish national mathematics grant.^[imported: the funding scheme's purpose is general background, not stated in the paper] That is consistent with a Spanish-affiliated mathematician working abroad, and consistent with several other arrangements. Recorded as a lead for a future source, not a fact.

## An Outsider Paper, and That Shows

The mathematical framing is that of someone who came to football from **graph theory rather than from sports analytics**:

- The introduction opens on **Euler and the Königsberg bridges**, and on Erdős — not on football.
- The venue was the **Euromech Physics of Sports Conference**, reached via an invitation recorded in the acknowledgements, rather than a sports-analytics venue.
- Cited neighbours include *"Mean free path in soccer and gases"* — a physics-of-sport paper, not an analytics one.
- The paper describes football's data situation from outside it, noting that the sport had **lagged behind baseball and basketball** in released statistics until UEFA Euro 2008.

> ### `the-network-tradition-entered-football-from-outside`
> **Pass-network analysis arrived in football from graph theory and statistical physics rather than from the sports-analytics literature, and that origin explains its shape: rich in structural invariants, indifferent to validation against outcomes, and disconnected from the valuation tradition that dominates this vault.**
> ^[generated: no source states this; drawn from the paper's framing, venue and citation neighbourhood against the vault's valuation corpus. rests-on: source:lopez-pena-graph-theory-framing]

This matters beyond biography. The vault's central observation about its own corpus is that groups **do not cite each other** — and [[social-network-analysis]] is the sharpest case, because it is not merely a different group but a different *discipline*, with different standards for what counts as a result. The paper reports no statistical test anywhere and does not treat that as a gap, because a structural description does not require one.

**Which is why the claim that travelled downstream was inferential and the paper's own claims were not.** See [[network-theory-football-strategies]] on how "winning teams presented lower betweenness" appeared in the literature without appearing in this paper.

## The Date

The held file is stamped **November 27, 2024**, which is a recompilation artefact rather than a publication date.^[inferred: internal evidence dates the work to 2012 — Sage 5.0.1, references stopping at 2011, and grant numbers from 2008 and 2010. [[ai-football-reinforcement-learning|Scott et al.]] cite it as 2012.] The vault treats the work as **2012**.

That makes it, by some margin, the **oldest football source held here** — predating [[expected-threat|xT]], [[vaep|VAEP]] and the entire [[pitch-control]] line.

## See Also

- [[network-theory-football-strategies|A network theory analysis of football strategies]] — the source
- [[hugo-touchette]] — co-author
- [[social-network-analysis]] · [[network-cohesion]] · [[tactical-analysis]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[atom-scott]] — the downstream user, and the origin of the citation error
- [[action-valuation-frameworks-compared]] · [[free-parameters-load-bearing]]
