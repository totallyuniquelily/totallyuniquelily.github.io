---
layout: post
title: "Some simple animations in bqn-80"
---

From BQN-80's [official site](https://dancek.github.io/bqn-80/):
> BQN-80 is an experimental fantasy sizecoding platform inspired by [TIC-80](https://tic80.com/). \
> This is a very early alpha version. 

Yeah it allows you to make animations in BQN. So I made ~~one~~ some!
Here's the first one (you'll have to press RUN!):

## Waves/Mountains

{% include bqn-80.html code="c‿d ← 8‿7‿6‿5 ⋈ 0‿60‿200‿100
combined←{𝕨+𝕩×0=𝕨}´⍉¨d⌽¨c×(≠c)/⋈⍉{(>⊏¨𝕩)>20×5.06+•math.Sin>π×120÷˜1⊏¨𝕩}↕136‿240
{𝕨⌽˘combined}" %}

If you run it on BQN-80 <!-- editor mention ---> you can configure it!:

- `c` colors for the waves, consisting of palette indices (0-15)
- `d` offsets for the waves

note that `c` and `d` must have the same length.

As you can see, this is kinda code-golfed.
I unfortunately do not have a fully spelled out version, as I changed the algorithm used to generate the rotated (`⌽`) image in the process of "minifying" my code.
This version runs `•math.Sin` on a whole 136‿240 sized matrix (240x136, the screen dimensions). The previous/original version would:

- run •math.Sin on the x coordinate
- generate a list of 1s (truths) with length equal to each result (producing a list of lists)
- extend each of these lists with 0s (falses) so their lengths equal display height
- create a matrix from those lists of lists

You can see that version [here](https://dancek.github.io/bqn-80/#c=YyDihpAgOOKAvzfigL824oC/NQpkIOKGkCAw4oC/NjDigL8yMDDigL8xMDAKd2F2ZSDihpAg4oyKKDIww5cyK+KAom1hdGguU2luIM+Aw5cxMjDDt8uc4oaVMjQwKQpEcmF3V2F2ZSDihpAge+KNiT4o4p+oMeKfqeKKuCgvy5wpwqjwnZWpKSDCq8KoIDzLmOKNiTEzNuKAvyjiiaDwnZWpKeKlijB9CmN3YXZlcyDihpAgY8OXKOKJoGMpL+KLiCDijYlEcmF3V2F2ZSB3YXZlCmNvbWJpbmVkIOKGkCB78J2VqCvwnZWpw5cwPfCdlah9wrQg4o2JwqggKGQpIOKMvcKoIGN3YXZlcwp78J2VqCDijL3LmCBjb21iaW5lZH0=).

<!-- writeup more here -->

## Lines

Here's another animation.
You can also configure it in BQN-80's editor, <!-- editor mention --> but the variables are documented in the code this time :3.

{% include bqn-80.html code="# !!WARNING: small values of t or ri cause flashing lights!!
t ← 7 # can be anything but 0. (lower t = faster)
ri ← 5 # 0-7. size, but also impacts speed (smaller = faster)
diag ← 1 # (-1)to1. 0-vertical columns, 1-diagonal, (-1)-diagonal (reverse).
# technically diag can be outside -1,1 range, causing a shallower angle

# we try every value of r up to 30, and check which ones
# create vertical columns correctly (columns are later rotated)
# then we get the nth value, where n = ri (set above)
valid_rs ← 1+/ {∧´(1⊸⊏=⊏) 136‿240⥊𝕩/↕8}¨ 1+↕30
r ← ri⊏valid_rs # one of: ⟨ 1 2 3 5 6 10 15 30 ⟩

{p ← ⌊𝕨÷(t×r) ⋄ m ← r|⌊𝕨÷t
(diag×↕136)⌽˘ 136‿240⥊p+m⌽r/↕8}" %}

Originally I intended `diag` to be a boolean (0 or 1), but it *turned out* to work with different values too, so I noted that in the comment, and changed the way diag is implemented, from:

  repeat "shift each row by one" `diag` times `((↕136)⌽˘⍟diag)`
  (intended to be 0 or 1 times)
 
to:

  shift each row by diag × row number `((diag×↕136)⌽˘)`

## Forwards

(EPILEPSY WARNING: this animation can still get pretty "flashy" after you let it run for a while)
{% include bqn-80.html code="l‿h ← (-⋈⊢)60
base ← (⊢×15<|) (××10+|) -⟜68 ⊑¨↕136‿240
# 0¨⌾(l↓h↓⊢)
{|𝕨÷base}" %}

The "l‿h" in this one is used to only keep the bottom and top 60 pixels.
The middle part was really flashy so I remove it.

<!--
original waves:

{% include bqn-80.html code="c ← 8‿7‿6‿5
d ← 0‿60‿200‿100
wave ← ⌊(20×2+•math.Sin π×120÷˜↕240)
DrawWave ← {⍉>(⟨1⟩⊸(/˜)¨𝕩) «¨ <˘⍉136‿(≠𝕩)⥊0}
cwaves ← c×(≠c)/⋈ ⍉DrawWave wave
combined ← {𝕨+𝕩×0=𝕨}´ ⍉¨ (d) ⌽¨ cwaves
{𝕨 ⌽˘ combined}"%}
-->

<script src="assets/bqn-80-embed/bqn.js"></script>
<script src="assets/bqn-80-embed/bqn-80.js"></script>