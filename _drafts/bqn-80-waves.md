---
layout: post
title: "Some simple animations in bqn-80"
---

{% include bqn-80.html code="c‿d ← 8‿7‿6‿5 ⋈ 0‿60‿200‿100
combined←{𝕨+𝕩×0=𝕨}´⍉¨d⌽¨c×(≠c)/⋈⍉{(>⊏¨𝕩)>20×5.06+•math.Sin>π×120÷˜1⊏¨𝕩}↕136‿240
{𝕨⌽˘combined}" %}

<script src="assets/bqn-80-embed/bqn.js"></script>
<script src="assets/bqn-80-embed/bqn-80.js"></script>