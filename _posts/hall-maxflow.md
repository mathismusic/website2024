---
layout: post
title: Hall's theorem from max-flow
mathjax: true
---

- Consider the standard network. s -> S -> S' -> t, all weights 1. Pick a min s-t cut, let $X = S \cap A, X' = S \cap B, Y = S' \cap A, Y' = S' \cap B$. Cut value is $|X'| + |Y| + |X \rightarrow Y'| < n$ (we need the harder i.e. mincut <= maxflow part here). For each $y \in N(X)$, if $y \in Y'$ then move it in, we do not increase the cut value. For each $y \not\in N(X)$, if $y \in Y$ then move it out, we reduce the cut value. Thus wlg $Y = N(X)$. The cut equation is then $|X'| + |N(X)| < n$ or $|N(X)| < |X|$.

\<In the context of TA allocation\>

Morally, the following is happening. Think of an $s-t$ flow as the following problem: the HoD $s$ hands out $n$ tokens to the $n$ willing TAs on the left side of the room. The right side has the set of $m$ course professors, with their hand open to receive a token of TAship. When $s$ says GO!, the TAs race to the right side of the room, and drop their token, if possible, into the *empty* hand of a professor they can TA for. As soon as a token enters a Professor's hand - assume that two TAs don't put their token at the same time - the hand is not empty, and no TA can put their token there now. Once no more token-placing is possible, the game ends, and the admin counts the number of Professors who have a token in their hand. This is the final value of the flow.

A flow function is simply a particular execution of this game. For any set $X \subseteq S$ of TAs, $s$, $X$, and the set of Professors $N(X)$ they can TA for forms the $s$-part of an $s-t$ cut. Let us now think of the following simple proposition in the language of $s-t$ cuts.

**Proposition.** Suppose that for some set $X$ of TAs, we have $|X| > |N(X)|$, then there is a Professor who is not TA'd for by any TA in $X$.

This is quite trivial to note, but why exactly did we agree with it? The reason is that total < n. Now we use the reverse to show that there is a set $|X|$ with $|X| > |N(X)|$.