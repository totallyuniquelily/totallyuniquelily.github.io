---
layout: post
title: "Some simple animations in bqn-80"
---

From BQN-80's [official site](https://dancek.github.io/bqn-80/):
> BQN-80 is an experimental fantasy sizecoding platform inspired by [TIC-80](https://tic80.com/). \
> This is a very early alpha version. 

Yeah it allows you to make animations in BQN. So I made some!
Here's the first one (you'll have to press RUN!):

## Waves/Mountains

{% include bqn-80.html code="c‿d ← 8‿7‿6‿5 ⋈ 0‿60‿200‿100
combined←{𝕨+𝕩×0=𝕨}´⍉¨d⌽¨c×(≠c)/⋈⍉{(>⊏¨𝕩)>20×5.06+•math.Sin>π×120÷˜1⊏¨𝕩}↕136‿240
{0‿𝕨⌽combined}" %}

If you run it on BQN-80 <!-- editor mention ---> you can configure it!:

- `c` colors for the waves, consisting of palette indices (0-15)
- `d` offsets for the waves

note that `c` and `d` must have the same length, and are separated by ⋈ in the assignment.

As you can see, this is kinda code-golfed.
I unfortunately do not have a fully spelled out version, as I changed the algorithm used to generate the rotated (`⌽`) image in the process of "minifying" my code.
This version runs `•math.Sin` on a whole 136‿240 sized matrix (240x136, the screen dimensions). The previous/original version would:

- run •math.Sin on the x coordinate
- generate a list of 1s (truths) with length equal to each result (producing a list of lists)
- extend each of these lists with 0s (falses) so their lengths equal display height
- create a matrix from those lists of lists

You can see that version [here](https://dancek.github.io/bqn-80/#c=YyDihpAgOOKAvzfigL824oC/NQpkIOKGkCAw4oC/NjDigL8yMDDigL8xMDAKd2F2ZSDihpAg4oyKKDIww5cyK+KAom1hdGguU2luIM+Aw5cxMjDDt8uc4oaVMjQwKQpEcmF3V2F2ZSDihpAge+KNiT4o4p+oMeKfqeKKuCgvy5wpwqjwnZWpKSDCq8KoIDzLmOKNiTEzNuKAvyjiiaDwnZWpKeKlijB9CmN3YXZlcyDihpAgY8OXKOKJoGMpL+KLiCDijYlEcmF3V2F2ZSB3YXZlCmNvbWJpbmVkIOKGkCB78J2VqCvwnZWpw5cwPfCdlah9wrQg4o2JwqggKGQpIOKMvcKoIGN3YXZlcwp78J2VqCDijL3LmCBjb21iaW5lZH0=).

(You may notice one difference in behavior from that version: the pixels at the top of the wave are gone.
This an (in part) intentional change.
The newer version would have a similar 1-pixel wave extremum but at the bottom, the `.06` in `×5.06+•math.Sin` is responsible for removing that (it nudges the waves down by a tiny amount).)

(side-note: the minified version originally ended with `{𝕨⌽˘combined}`, but as a minor, and useless, optimisation I changed it to `{0‿𝕨⌽combined}`. This way, `⌽` is ran once, rather than for every row in the matrix.
This did add one character of course.)

(I also made variants that randomize the waves' offsets, rather than hard-coding them and letting you choose. Links to both versions: [small](https://dancek.github.io/bqn-80/#c=Y+KGkDjigL834oC/NuKAvzUKZOKGkCjiiaBjKeKAonJhbmQuUmFuZ2UgMjQwCmNvbWJpbmVk4oaQe/Cdlagr8J2VqcOXMD3wnZWofcK04o2Jwqhk4oy9wqhjw5co4omgYykv4ouI4o2Jeyg+4oqPwqjwnZWpKT4yMMOXNS4wNivigKJtYXRoLlNpbj7PgMOXMTIww7fLnDHiio/CqPCdlal94oaVMTM24oC/MjQwCnsw4oC/8J2VqOKMvWNvbWJpbmVkfQ==) and [original](https://dancek.github.io/bqn-80/#c=YyDihpAgOOKAvzfigL824oC/NQpkIOKGkCAo4omgYykg4oCicmFuZC5SYW5nZSAyNDAKd2F2ZSDihpAg4oyKKDIww5cyK+KAom1hdGguU2luIM+Aw5cxMjDDt8uc4oaVMjQwKQpEcmF3V2F2ZSDihpAge+KNiT4o4p+oMeKfqeKKuCgvy5wpwqjwnZWpKSDCq8KoIDzLmOKNiTEzNuKAvyjiiaDwnZWpKeKlijB9CmN3YXZlcyDihpAgY8OXKOKJoGMpL+KLiCDijYlEcmF3V2F2ZSB3YXZlCmNvbWJpbmVkIOKGkCB78J2VqCvwnZWpw5cwPfCdlah9wrQg4o2JwqggKGQpIOKMvcKoIGN3YXZlcwp78J2VqCDijL3LmCBjb21iaW5lZH0=).)

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

(EPILEPSY WARNING: this animation start to flash a lot after you let it run for a while)
{% include bqn-80.html code="l‿h ← (-⋈⊢)60
base ← (⊢×15<|) (××10+|) -⟜68 ⊑¨↕136‿240
# 0¨⌾(l↓h↓⊢)
{|𝕨÷base}" %}

The "l‿h" in this one is used to only keep the bottom and top 60 pixels, since the (excluded) middle part flashed a lot.

<script src="assets/bqn-80-embed/bqn.js"></script>
<script src="assets/bqn-80-embed/bqn-80.js"></script>