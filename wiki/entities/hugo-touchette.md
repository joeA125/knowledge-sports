---
title: "Hugo Touchette"
type: entity
tags: [person, researcher, network-analysis, sports-analytics, tactical-analysis, single-source]
sources: [raw/papers/football_strategy_network_theory_analysis.md]
confidence: 0.7
provenance:
  extracted: 46%
  inferred: 32%
  generated: 10%
  imported: 10%
  ambiguous: 2%
lifecycle: draft
created: 2026-08-29
updated: 2026-08-29
---

# Hugo Touchette

Second author of [[network-theory-football-strategies|A network theory analysis of football strategies]], with [[javier-lopez-pena]].

> ⚠️ **This is the author the vault's citation error was made of.** Four pages cited the work as **"Peña & Hugo (2012)"** — **"Hugo" is this author's forename**, mistaken for a surname by [[ai-football-reinforcement-learning|Scott et al.]] and inherited from there. The correct short form is **López Peña & Touchette**. Corrected 2026-08-29.
>
> The error is a specific and recognisable failure mode: a Spanish compound surname in first position and a forename-surname pair in second, collapsed into one plausible-looking pair of surnames.

## What Is Actually Evidenced

| Evidenced | From |
|---|---|
| Second authorship | The paper's byline |
| Maintained the project's results website | Footnote 1: `maths.qmul.ac.uk/~ht/footballgraphs/` |
| **Mathematics department affiliation** | The same URL — `maths` path, personal directory `~ht` |

The URL is the only affiliation evidence in the held text, and it is reasonably strong: a departmental personal page under `~ht` at a mathematics faculty, hosting this paper's supplementary results.^[inferred: the initials match and the directory hosts this paper's per-team networks; the paper does not state an affiliation in the extracted text]

⚠️ **Recorded as inference, not fact**, per the conventions' worked example on entity affiliations. The stronger version — naming the institution as his employer at the time of writing — is not supported by anything in the file, only by the hostname.

**No footnote target for the byline dagger is present in the extracted file**, so the formal affiliation line the paper almost certainly carried did not survive extraction. Worth flagging as an *extraction* gap rather than a source gap: the information probably exists in the original PDF.

## The Physics Connection

The paper's venue and framing place both authors outside sports analytics — see [[javier-lopez-pena]] for the argument, which applies to the pair rather than to either individually. Two details attach specifically here:

- The acknowledgements credit **C. Clanet** with inviting the work to the **Euromech Physics of Sports Conference** and encouraging its write-up. The paper exists in published form because a physics-of-sport community asked for it.
- **L. Mahadevan** is credited with suggesting the study of communities within football teams — the one part of the paper that **failed**. See [[network-cohesion]].

That second credit is unusually informative. The paper's reported negative result was not a stray idea the authors dropped; it was a **suggestion from outside that they took seriously, attempted, and reported as unsuccessful**. Reporting a suggested-and-failed line rather than quietly omitting it is rarer in this vault's corpus than it should be.

## Why This Page Exists at All

A two-author paper with one held source would ordinarily justify one entity page and a mention. This one gets two because **the citation error is the finding**, and it is specifically an error about *this* author's name.

> ### `citation-errors-propagate-further-than-the-claims-they-carry`
> **A mis-citation travels with every claim extracted from the citing source and is invisible to every check the vault runs — dead-link detection reads wikilinks, `verify_sources` reads file paths, and neither reads bibliographies. It surfaces only when the primary is acquired, which may be never.**
> ^[generated: no source states this; drawn from this error's four-page propagation and the coverage gaps in the vault's own tooling. rests-on: source:scott-pena-hugo-miscitation, claim:an-extracted-claim-inherits-its-source's-errors]

The practical consequence for the vault is a gap with no tool behind it. `find_mentioned_but_missing` and `verify_sources` between them cover wikilinks and `sources:` frontmatter; **nothing covers the accuracy of a bibliographic reference to a work that is not held.** Acquisition is the only check, and it arrived here three ingests late.

## See Also

- [[network-theory-football-strategies|A network theory analysis of football strategies]] — the source
- [[javier-lopez-pena]] — co-author, and the discipline-origin argument
- [[network-cohesion]] — including the community-detection failure credited to Mahadevan's suggestion
- [[social-network-analysis]] · [[tactical-analysis]]
- [[ai-football-reinforcement-learning|Scott et al.]] · [[atom-scott]] — origin of the citation error
