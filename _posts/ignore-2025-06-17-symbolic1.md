---
layout: post
title: An invitation to generating function and the symbolic method
mathjax: true
---

Welcome to the first post in a sequence on enumeration via the symbolic method and complex analytic techniques, dubbed [Analytic Combinatorics](https://en.wikipedia.org/wiki/Analytic_combinatorics). The method was popularized by Philippe Flajolet and Robert Sedgewick in their seminal book, *Analytic Combinatorics* (2009). The method is an extremely powerful tool for counting, and it has applications in math (duh), computer science, physics, and basically every field that requires counting. The area is quickly blowing up, with new results and techniques being developed and long-standing problems getting sledge-hammered away.

The plan for today is to present a quick introduction to generating functions and establish a few basic properties of symbolic constructions. In the next few posts, we will explore the symbolic method in more detail, including how to use it to count combinatorial structures like trees, graphs, and permutations that satisfy certain constraints.

Before we get started, perhaps I should hype it up just a little bit to improve the scroll rate on this post. The following example is what first got me into this subject. The Bell numbers, denoted \(B_n\), count the number of ways to partition a set of \(n\) elements into non-empty subsets. For example, there are 5 ways to partition a set of 3 elements \(\{1, 2, 3\}\):

\[
    \begin{align*}
    & \{1, 2, 3\} \\
    & \{1\}, \{2, 3\} \\
    & \{2\}, \{1, 3\} \\
    & \{3\}, \{1, 2\} \\
    & \{1\}, \{2\}, \{3\}
    \end{align*}
\]

and so \(B_3 = 5\). One can compute the Bell numbers recursively using the following 




---

That's a wrap. Until next time on Saturday the fourteenth. Do ping me if you've got comments or suggestions. I'm very new to this business of blogging and could certainly use some help. You can reach me at the usual email address: ```kagaram@cse.iitb.ac.in```.