---
layout: post
title: "Some simple animations in bqn-80"
---

<div class="bqn-80">
<canvas class="canvas" width="240" height="136" style="display:block; width: 100%; image-rendering: pixelated; margin-bottom:4px;"></canvas>
<div class="frametime" style="float:right"></div>
<button class="button-reload">RUN! (shift-enter)</button>
<button class="button-stop">STOP!</button>
<pre class="error" style="color: brown"></pre>
<pre><code class="source" style="width: 100%; height: 8em;">
c‿d ← 8‿7‿6‿5 ⋈ 0‿60‿200‿100
combined←{𝕨+𝕩×0=𝕨}´⍉¨d⌽¨c×(≠c)/⋈⍉{(>⊏¨𝕩)>20×5.06+•math.Sin>π×120÷˜1⊏¨𝕩}↕136‿240
{𝕨⌽˘combined}
</code></pre>
<div class="charcount" style="float: right"></div>
</div>

<script src="assets/bqn-80-embed/bqn.js"></script>
<script src="assets/bqn-80-embed/bqn-80.js"></script>