---
layout: post
title: "Draft: Error handling in BQN and Rust"
---
BQN has very few types, so returning an "error code" is impractical.
It is also fairly unfitting with common function design and primitives <!-- change to unidiomatic -->: e.g. Search funtions return *length* `≠` when an element is not found.
Not even -1, but *length*.

I get a feeling BQN primitives are designed with the assumption that if you can avoid exceptional/checked cases, you should. (i.e. redesign the function so it handles all cases as a result of the primitives chosen, rather than a result of checks/conditions)
Occasionally, obvious empty values can be available (like the empty list, `⟨⟩`), and you can always throw and catch errors.
Since errors can be any time, you *could* even build an exception system with them, if you'd like!<!-- add source-->
That said, the page [Problems with BQN]() states:
> However, catching errors shouldn't be common in typical code, in the sense that an application should have only a few instances of `⎊`. Ordinary testing and control flow should be preferred instead.
Which I believe is illustrative of the kind of code BQN was designed to handle, and it is the kind that doesn't put error management at it's focus.

If error handling is your main code<!--[citation needed]--> Rust and Haskell are probably going to be more suitable. I'm lumping them in together here, because their approaches are incredibly similar (I believe the only difference is that Rust has purpose-built syntax for rethrowing errors, while haskell uses monads, like it does for other types of control flow).

Rust/Haskell's solution is based around 2 algebraic datatypes:
- `Result<T, Err>`/`Either t err`[^1] - the value you want (`T`), or an error (`Err`)
- `Option<T>`/`Maybe t`[^1] - the value you want (`T`) or nothing.

[^1]: In Haskell these are called `Either a b` and `Maybe a`. Type parameters names changed here for consistency with Rust ~~to make it easier to conflate different languages~~.