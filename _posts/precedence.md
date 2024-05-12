---
layout: post
title: context-free parsing: handling precendence and associativity
mathjax: true
---

The set of acceptable expressions using digits and operators is typically easily described by a context-free grammar, for example like so:

$$expr ::= expr + expr | expr * expr.$$

Perfectly fine to describe the *syntax* of acceptable expressions. Unfortunately, the meaning (the order of reducing operations till we are left with a single value, the value of the expression) of an acceptable expression may be ambiguous; the problem stems from the simple observation that operands (numbers here) can be combined with the operator to their left, or their right.

Consider the acceptable expression $3 * 4 + 5 * 6$. One can reduce the three operators left-to-right - $(((3 * 4) + 5) * 6) = 102$, middle operator last - $((3 * 4) + (5 * 6)) = 42$, right-to-left - $(3 * (4 + (5 * 6))) = 102$ (the equality of value with the first ordering is a devious coincidence), middle operator first - well, you get the idea. All $n!$ permutations of the $n$ operators serve as possible orderings of the expression. How does one then describe the ordering one wants the expression to carry?

The simple solution - bracketing provides a natural order of reducing operators. Of course, bracketing is terribly messy to read. A far lazier way is to simply use some reduction rules that are only dependent on the type of operator ($+$, $*$, etc) and not on the context around it. If we wish for an ordering on a sub-expression different from what the rules give us, we explicitly specify the order required by bracketing.

The rules typically take the form of *precedence*, specifying something like the following: reduce all <top level operators> first, then reduce all <next level operators>, and so on. This is obviously not sufficient, since it tells us nothing about the order in which to reduce identical operators. Sometimes it does not matter, for example with $3 * 4 + 5 * 6$ - the multiplication operators are disjoint - but when identical operators are on either side of an operand, the resulting ordering is not unique - $3 * 4 * 5 + 6$ can be expanded $(((3 * 4) * 5) + 6)$ or $((3 * (4 * 5)) + 6)$. We are again lazy, and typically allot each operator an *associativity* - if associativity is *left*, then all instances of the operator are reduced from the leftmost occurrence to the right most occurrence in order, and vice versa if the associativity is *right*.

It is natural to ask if associativity is really required, since the arithmetic operators are associative, so that the order of reduction of adjacent occurrences of the same operator is irrelevant to the result. There are two reasons here: first, a set of operators is typically assigned the same precedence - for example $+$ and $-$ - and in this case, this "set" is not associative - $((9-5)+2) \neq (9-(5+2))$, which neccessitates a specification of associativity. The second reason is simply that there are binary operators that are not associative - the exponentiation operator $**$ is not associative.

Precedence and associativity rules, when specified for all operators, induce a unique ordering for operator reduction. In context-free-grammar lingo, this means that the expression grammar above along with these rules has a unique *parse tree*. This begs the question; the current approach has an *ambiguous* (more than one parse tree/ordering for an acceptable expression) grammar where precedence and associativity rules choose a particular parse tree from the different possible ones.

