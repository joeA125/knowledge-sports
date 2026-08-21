---
title: "Multi-Scale Context Aggregation by Dilated Convolutions — Source Summary"
type: summary
tags: [deep-learning, computer-vision, semantic-segmentation, architecture, probability-surface, sports-analytics]
sources: [raw/papers/context-aggregation-dilated-convolutions.md]
confidence: 0.9
provenance:
  extracted: 80%
  inferred: 12%
  generated: 6%
  imported: 0%
  ambiguous: 2%
lifecycle: reviewed
created: 2026-05-08
updated: 2026-08-14
---

# Multi-Scale Context Aggregation by Dilated Convolutions

**Yu & Koltun**, Princeton and Intel Labs, ICLR 2016; arXiv:1511.07122.

> **Held here because [[soccermap|SoccerMap]] faces exactly the problem this paper solves**, and takes the other route. The segmentation benchmarks and the wider dense-prediction literature live in the general vault.

## The Problem

Dense prediction needs **both** pixel-level precision and multi-scale context. Classification networks get global context through successive pooling, which destroys resolution. Prior fixes either recovered lost resolution with up-convolutions or processed multiple rescaled copies. Neither is clean.

## Dilated Convolution

Apply the filter kernel at spaced-out positions:

$$(F *_l k)(\mathbf{p}) = \sum_{\mathbf{s}+l\mathbf{t}=\mathbf{p}} F(\mathbf{s}) k(\mathbf{t})$$

Standard convolution is $l = 1$. With exponentially increasing dilation ($l = 1, 2, 4, 8, \dots$), **the receptive field grows exponentially while parameters grow linearly, and no resolution is lost.**

The plug-in context module — seven layers at dilations 1, 1, 2, 4, 8, 16, 1 — achieves a $67 \times 67$ receptive field with roughly $64C^2$ parameters, and is initialised to the identity so backpropagation learns aggregation from a working starting point.

Results improved state of the art across VOC 2012 (75.3% mIoU), CamVid, KITTI and Cityscapes.

## The Football Connection

[[expected-value-possession-framework|Fernández et al.'s]] [[soccermap|SoccerMap]] predicts a value at **every location on a $104 \times 68$ pitch** from tracking data. That is dense prediction with the same tension: a defender's relevance depends on both local geometry and the wider configuration.

**SoccerMap takes the other route** — an encoder–decoder with predictions at 1×, ½× and ¼× resolution, upsampled and merged through 1×1 fusion layers. This paper's approach would have preserved resolution throughout instead.

| | Mechanism | Cost |
|---|---|---|
| **SoccerMap** | Multi-scale pyramid, fuse | Downsampling loses detail, recovered by fusion |
| **Dilated** | Expand receptive field in place | More memory; gridding artefacts at high dilation |

Neither has been compared against the other on football data. See [[fully-convolutional-network]], where the three families of solution are set out, and [[single-pixel-supervision]] for what makes learning such a surface possible at all.

## Why the Comparison Would Be Cheap

The pitch is small — $104 \times 68$ against Cityscapes' megapixel images — so a dilated variant of SoccerMap is a modest experiment, and the architectures are otherwise interchangeable. It would also bear on [[probability-surface]] more broadly, since every surface-based framework here inherits SoccerMap's resolution choice without examining it.

## See Also

- [[fully-convolutional-network]] · [[soccermap]] · [[probability-surface]] · [[single-pixel-supervision]] · [[semantic-segmentation]]
- [[pitch-value-model]] · [[expected-possession-value]] · [[pitch-control]] · [[game-state-reconstruction]]
- [[expected-value-possession-framework|EPV Framework Summary]]
