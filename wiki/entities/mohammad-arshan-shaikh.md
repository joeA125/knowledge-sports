---
title: "Mohammad Arshan Shaikh"
type: entity
tags: [person, researcher, university, reliability, evaluation, statistics, sports-analytics, single-source]
sources: [raw/papers/metric-stability-elite-football.md]
confidence: 0.7
provenance:
  extracted: 62%
  inferred: 18%
  generated: 18%
  imported: 1%
  ambiguous: 1%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Mohammad Arshan Shaikh

Sole author of [[year-to-year-metric-stability-football|Year-to-Year Metric Stability in Elite Football]], the vault's **first and only source reporting [[metric-stability|stability]] figures for football metrics.**

Khoury College of Computer Science, Northeastern University, Miami. ORCID 0009-0004-6962-3633. Preprint submitted to the *Journal of Sports Analytics*.

## Why This Page Exists

Not for the author's standing, which a single preprint cannot establish. For **what the paper's existence says about where the vault's top priority will be closed.**

The overview has argued across several ingests that a closing source would come from outside the machine-learning football literature. This paper supports that and refines it: it is **sole-authored, uses free public FBref data via Kaggle, requires no tracking access, no club partnership and no funding** — the declarations state no grant was received.

> **The measurement the vault has wanted for thirteen ingests is not blocked by difficulty.** Stability estimation needs multiple seasons of a metric over a shared player set and nothing else. It was done here by one person on public data. **What blocks it for tracking-derived metrics is data access, not method, funding or expertise.**
> ^[generated: drawn from the paper's resource profile against the vault's acquisition problem. rests-on: source:shaikh-data-availability, source:shaikh-no-funding]

That is a useful narrowing. It means the vault should stop looking for a *methodologically sophisticated* source and start looking for anyone with **multi-season tracking access** — a club, a provider, or a paper using a multi-season commercial dataset. See [[metric-stability]] on why the vault's own held sources cannot supply it.

## What the Paper Does Well, and Where It Is Weak

Recorded because a single-source entity page should not read as endorsement.

**Well:** genuine sample scale (8,207 pairs, 2,866 players, five leagues, eight seasons); confidence intervals throughout; two sensitivity analyses; formal Fisher $z$ tests rather than eyeballed differences; public code and data. The **age-group** and **two-year decay** analyses are the parts with no precedent in the vault's corpus — see [[stability-decay]].

⚠️ **Weak:** two verifiable mis-citations, including of [[meta-analytics-sports-metrics|Franks et al.]], which the vault holds and can check — described as an ice-hockey paper when it is NBA and NHL, with the basketball half the larger. A soccer paper by Brooks et al. is twice described as baseball pitch-type work.

⚠️ And an unresolved methodological question about the **headline result**: pass completion percentage is multiplied by 90-minute equivalents to form a "seasonal total", which is dimensionally odd, and the Pearson and ICC estimates then agree to three decimals across nearly every row despite being computed on deliberately different scalings. See [[year-to-year-metric-stability-football]].

**Not peer-reviewed at time of ingest.** Both issues are the kind review should catch.

## The Franks Lineage

The paper explicitly adopts [[meta-analytics-sports-metrics|Franks et al. (2016)]] as its conceptual foundation — *"their decomposition of variance into signal and noise components provides the conceptual foundation adopted here"*.

**So the vault now holds the framework and an application of it**, acquired one ingest apart. That is the first framework-plus-application pair in the reliability material, and it is what makes the mis-citation checkable at all.

⚠️ It also inherits a limit the vault recorded on ingesting the framework: stability measures **internal** quality. Shaikh acknowledges this directly in §4.5 — a high stability coefficient does not mean a metric measures individual skill, since pass completion may reflect tactical role and team style. **That is `discrimination-rewards-measuring-position` in its stability form**, and the author reaches it independently, citing Franks. See [[metric-discrimination]] and [[construct-validity]].

## See Also

- [[year-to-year-metric-stability-football]] — the source
- [[metric-stability]] · [[stability-decay]] · [[metric-discrimination]] · [[metric-variance-components]] · [[reliability-layers]]
- [[meta-analytics-sports-metrics]] · [[alexander-franks]] · [[split-half-reliability]]
- [[recruitment]] · [[player-development-curve]] · [[event-stream-data]] · [[expected-goals]]
