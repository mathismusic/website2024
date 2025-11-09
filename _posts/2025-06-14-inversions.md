---
layout: post
title: When math turns to magic: Self-inverse transforms and convolutions
mathjax: true
---

There are some things in mathematics that are simply too magical to be true. One such thing, in my opinion, are self-inverse and unitary transforms. In this post, I'd like to talk about some of the most beautiful self-inverse transforms that I have come across so far. I'm *very* sure I've missed some gems, but ah well one learns something new everyday. 

### What is a self-inverse transform?

A transform $$T$$ takes an input object $$x$$ and outputs another object which we call $$T(x)$$ or $$Tx$$. We say $$T$$ *is applied to* $$x$$. Real functions are the most common example of transforms, taking one real number to another.

A transform becomes self-inverse if it can be applied to its own output to get back the original input. That is, to *undo* $$T$$, we simply have to *redo* it. This is amazing news for engineers, because for many applications, coming up with an input that produces a desired output is much harder (e.g. finding a function that produces a known signal or satisfies a known differential equation) than verifying that a given input produces a desired output (e.g. checking that a function satisfies a differential equation). That is, re-doing a transform is often much, much easier than undoing it.

To be cheeky, if construction was self-inverse, then all we would have to do to rebuild something is to destroy it again. Too bad this isn't true in practice.

<!-- ### Functions on $$\mathbb R$$

Examples of self-inverse transforms are functions $$f: \mathbb R \to \mathbb R$$ that satisfy $$f(f(x)) = x$$ for each $$x$$ -- to invert them, one simply *re-applies* them. 

Well, please don't go $$f(x) = 1/x$$ on me haha, because these things get a lot more interesting than that. Or $$x/(x-a)$$, which is just the re-centering of the previous hyperbola to $$(a,a)$$.

In this  -->

### A familiar starter: the Discrete Fourier transform

Much of the engineering world is familiar with the ubiquitous fourier transform, which revolutionized signal processing and reading this article via the internet would hardly be possible without it.

Here, in true computer science fashion, we will discretize and consider the Discrete Fourier Transform (DFT). Let $$n$$ be a positive integer. The DFT is a linear transform $$\mathcal F$$ taking an $$n$$-length sequence $$a = (a_0, a_1, \ldots, a_{n-1})$$ to another sequence $$\mathcal F(a) = (b_0, b_1, \ldots, b_{n-1})$$ defined by

$$
b_k = \frac{1}{\sqrt{n}} \sum_{j=0}^{n-1} a_j \left(e^{-2\pi \iota k/n}\right)^j \quad \text{for } k = 0, 1, \ldots, n-1.
$$

As a matrix operation, consider the $$n\times n$$ matrix $$F$$ with entries

$$
F_{jk} = \frac{1}{\sqrt{n}} e^{-2\pi \iota jk/n} \quad \text{for } j, k = 0, 1, \ldots, n-1.
$$

Then the DFT is simply the matrix multiplication $$\mathcal F(a) = F a$$.

So the DFT is what's called *unitary*, which is almost as good as self-inverse: to undo the DFT, we need to do something *as easy as* apply the DFT again. In particular, let's define the *adjoint* of the DFT, denoted $$\mathcal F^\dagger$$ via the matrix $$F^\dagger$$ with elements $$F^\dagger_{jk} = \frac{1}{\sqrt{n}} e^{2\pi \iota jk/n}$$ -- so just a change from $$\iota$$ to $$-\iota$$. Since mathematics is symmetric in $$\iota$$ and $$-\iota$$, any algorithm to apply $$\mathcal F$$ yields one with the same complexity to apply $$\mathcal F^\dagger$$. The DFT satisfies the property $$F^\dagger F = I$$, where $$I$$ is the identity matrix. This means that for every sequence $$a$$, we have $$\mathcal F^\dagger \mathcal F(a) = a$$ (the unitary property).

Take a moment to appreciate the beauty of the transform: it's very easy using something called the Fast Fourier Transform to apply the DFT to a sequence. And yes, when we receive an "impure" signal, we often apply the DFT to it. Purifying the result of the DFT happens to be super-easy: the true signal shows up in the result as spikes (Grant Sanderson [does a great job](https://www.youtube.com/watch?v=spUNpyF58BY) (as usual) explaining this, by the way). Filtering the result by setting the noise to zero and leaving the spikes is easy too. The main thing now is to find a signal that produces this filtered result directly -- un-doing the fourier transform on the filtered result. The mathemagic of the unitarity of the DFT means that this is just as easy as applying the DFT. And boom: signal processing works!

If you're interested to learn more, I highly recommend [this amazing set of notes](https://www.cs.cmu.edu/afs/andrew/scs/cs/15-463/2001/pub/www/notes/fourier/fourier.pdf) and of course, [Grant Sanderson's video](https://www.youtube.com/watch?v=spUNpyF58BY) on the subject.

### Interlude: Convolutions

Central to many self-inverse transforms is the idea of a convolution. A convolution (etymology: entwine, combine) is a way of combining two sequences to produce a third sequence, and it is defined as follows: given two infinite sequences $$a = (a_0, a_1, \ldots)$$ and $$b = (b_0, b_1, \ldots)$$, their convolution $$c = a * b$$ is another infinite sequence defined by

$$
c_i = \sum_{j=0}^{i} a_j b_{i-j} \quad \text{for } i = 0, 1, \ldots.
$$

It arises naturally in many settings; most students encounter it first in the context of polyonomial multiplication: if $$a(x) = a_0 + a_1 x + \ldots + a_n x^n$$ (represented by the sequence $$(a_0, a_1, \ldots, a_n, 0, 0, \ldots)$$) and $$b(x) = b_0 + b_1 x + \ldots + b_m x^m$$ (represented similarly) are two polynomials, then the coefficients of the product polynomial $$c(x) = a(x)b(x)$$ are given by the convolution: $$c_i = \sum_{j=0}^{i} a_j b_{i-j}$$ for $$i = 0, 1, \ldots$$.

The convolution may appear convoluted, but it finds use more often than one might expect: think enumerative combinatorics, fourier analysis, number theory, etc. (The thing in "convolutional" neural networks is actually a *cross-correlation* and is slightly different but can be presented as an evaluation of a convolution too.) The essential motivation behind convolutions is that often one wants to pick some of one and some of another, in a way that the total quantity picked is a constant. And the number of different ways we can do that for each value of the total amount picked is described by the result of the convolution. For example, in combinatorics, the convolution tells you how many ways you can select items from two groups so that the total number of items selected equals a given number.

A key property of the convolution is that it is associative: that is, for any three sequences $$a, b, c$$, we have

$$
(a * b) * c = a * (b * c).
$$

Indeed, notice that for any $$i \geq 0$$,

$$
\begin{align*}
    [(a * b) * c]_i &= \sum_{j=0}^{i} (a * b)_j c_{i-j} \\ &= \sum_{j=0}^{i} \left(\sum_{k=0}^{j} a_k b_{j-k}\right) c_{i-j} \\
    &= \sum_{k=0}^{i} a_k \left(\sum_{j=k}^{i} b_{j-k} c_{i-j}\right) \\
    &= \sum_{k=0}^{i} a_k \left(\sum_{j'=0}^{i-k} b_{j'} c_{i-k-j'}\right) \\
    &= \sum_{k=0}^{i} a_k (b * c)_{i-k} \\ &= [a * (b * c)]_i.
\end{align*}
$$

Notice how $$i-k = i-j + j-k$$ helps us re-write the summation over $$j$$ as another convolution evaluated at $$i-k$$.

The convolution is also commutative, i.e. $$a * b = b * a$$ for any two sequences $$a$$ and $$b$$ (left as an easy exercise for readers with pen and paper). Associativity and commutativity help us manipulate convolutions easily without having to re-write them as summations (more on this later), treating sequence convolution like integer multiplication.

Convolutions go much, much further than the standard convolution presented here. We're going to see one more in this post, and perhaps one or two more in the sequel post.

---

The *binomial convolution* is defined by

$$
[a \circ b]_n := \sum_{j=0}^{n} \binom{n}{j} a_j b_{n-j} \quad \text{for } n = 0, 1, \ldots.
$$

For readers familiar with generating functions, this is the convolution involved in the product of two exponential generating functions.

Surprisingly, the binomial convolution is also associative! For each $$n \geq 0$$, we have

$$
\begin{align*}
    [(a \circ b) \circ c]_n &= \sum_{j=0}^{n} \binom{n}{j} (a \circ b)_j c_{n-j} \\
    &= \sum_{j=0}^{n} \binom{n}{j} \left(\sum_{k=0}^{j} \binom{j}{k} a_k b_{j-k}\right) c_{n-j} \\
    &= \sum_{k=0}^{n} a_k \left(\sum_{j=k}^{n} \binom{n}{j} \binom{j}{k} b_{j-k} c_{n-j}\right).
\end{align*}
$$

We now call for some sorcery with the binomial coefficients: it turns out that

$$
\begin{align*}
    \binom{n}{j} \binom{j}{k} &= \frac{n!}{j!(n-j)!} \cdot \frac{j!}{k!(j-k)!} \\
    &= \frac{n!}{k!(n-j)!(j-k)!}\cdot\frac{(n-k)!}{(n-k)!} = \binom{n}{k} \binom{n-k}{j-k}.
\end{align*}
$$

This helps us re-write the inner summation as a convolution:

$$
\begin{align*}
    \sum_{j=k}^{n} \binom{n}{j} \binom{j}{k} b_{j-k} c_{n-j} &= \sum_{j'=0}^{n-k} \binom{n}{k} \binom{n-k}{j'} b_{j'} c_{n-k-j'} = \binom{n}{k} (b \circ c)_{n-k},
\end{align*}
$$

which allows us to conclude that
$$
[(a \circ b) \circ c]_n = \sum_{k=0}^{n} \binom{n}{k} a_k (b \circ c)_{n-k} = [a \circ (b \circ c)]_n,
$$

so the $\circ$ operation is associative. Since $$\binom{n}{k} = \binom{n}{n-k}$$, the binomial convolution is also commutative.

A similar convolution is the *Dirichlet convolution*, defined by

$$
[a \star b]_n := \sum_{d|n} a_d b_{\frac{n}{d}} \quad \text{for } n \geq 1,
$$

where the notation $$d \mid n$$ means that $$d$$ divides $$n$$ (so the sum is over all divisors $$d$$ of $$n$$). The Dirichlet convolution is also associative (hell yes) and commutative, and it arises naturally in number theory, especially in the context of multiplicative functions. Commutativity of $$\star$$ is easy to see; let's quickly show that it is also associative. For each $$n \geq 1$$, we have

$$
\begin{align*}
    [(a \star b) \star c]_n &= \sum_{d|n} (a \star b)_d c_{\frac{n}{d}} \\
    &= \sum_{d|n} \left(\sum_{e|d} a_{e} b_{\frac{d}{e}}\right) c_{\frac{n}{d}} \\
    &= \sum_{e|n} a_{e} \left(\sum_{d: e|d, d|n} b_{\frac{d}{e}} c_{\frac{n}{d}}\right).    
\end{align*}
$$

The inner summation is over all $$d$$ such that $$e\mid d$$ and $$d\mid n$$, which is equivalent to (setting $$d = d'e$$ for positive integer $$d'$$) summing over all $$d'$$ such that $$d'\mid \frac ne$$. So we can re-write the inner summation as

$$
\sum_{d'|\frac ne} b_{d'} c_{\frac{n}{d'e}} = (b \star c)_{\frac{n}{e}}.
$$

This yields

$$
[(a \star b) \star c]_n = \sum_{e|n} a_e (b \star c)_{\frac{n}{e}} = [a \star (b \star c)]_n,
$$

and done. Yay! 

Please do notice the very similar structure of the proofs of associativity for the different convolutions. Many different convolutions go through the same proofs and have similar properties, and after a while the mathematician treats such new proofs as "pencil-pushing" exercises, referring to the mechanical re-writing of the same steps over and over again. Other mathematicians filter out exactly what properties are used about the convolution to make the proof work, and then generalize the proof to any convolution that satisfies those properties in one go -- you see, abstraction in mathematics is quite similar to the "if you use it more than once, make it a function" rule in programming.

Why care about -- indeed, focus on -- convolutions in a post on self-inverse transforms? Here's the big idea: *invertible* convolutions *with identity* lead to a natural invertible transform. And often, this transform is (almost) self-inverse. Thus, in a sense, convolutions can *generate* self-inverse transforms.

The *identity* for a convolution operation is a sequence $$e$$ such that for any sequence $$a$$, we have $$a * e = a$$. For the usual convolution $$*$$, notice that the identity is the sequence $$e = (1, 0, 0, \ldots)$$. The same sequence is also the identity for the binomial convolution $$\circ$$ (and the Dirichlet convolution, and more). We say that sequence $$b$$ *inverts* the sequence $$a$$ if $$a * b = e (= b * a)$$. By commutativity and associativity, this means that if $$d = a * c$$, then $$b * d = c$$, in accordance with rules that treat $$b$$ as "$$1/a$$".

Consider the following toy example. Define the **partial-sum** transform $$S$$ taking one sequence to another by $$S(a) := a * \mathbf 1$$, where $$\mathbf 1$$ is the sequence $$(1, 1, 1, \ldots)$$. Notice that for each $$i \geq 0$$, we have

$$
S(a)_i = \sum_{j=0}^{i} a_j,
$$

hence the name *partial-sum*. Now suppose for a moment that we could find a sequence $$\lambda$$ that inverts the sequence $$\mathbf 1$$, i.e. $$\mathbf 1 * \lambda = e$$. This would mean

$$
S(a) * \lambda = a * \mathbf 1 * \lambda = a * e = a,
$$
which means that convolution with $$\lambda$$ amounts to un-doing $$S$$! In general, if $$\mathbf 1$$ can be inverted, then a transform generated by convolution with $$\mathbf 1$$ can be inverted just as easily, by convolving with the inverse of $$\mathbf 1$$. Choosing a particular convolution and computing $$\lambda$$ is all that's enough to conclude what's called an *inversion formula* for the transform.

How does one find such an inverse $$\lambda$$? Just write out the constraints that must be met so that $$\lambda * \mathbf 1 = e = (1, 0, \ldots)$$.

$$
\begin{align*}
    \lambda_0 &= 1, \\
    \lambda_0 + \lambda_1 &= 0, \\
    \lambda_0 + \lambda_1 + \lambda_2 &= 0, \\
    &\;\;\vdots
\end{align*}
$$

which yields the unique solution $$\lambda = (1, -1, 0, 0, \ldots)$$. Alright, so what does this give us? $$a = S(a) * \lambda$$, so for each $$i \geq 1$$,

$$
a_i = [\lambda * S(a)]_i = \lambda_0 S(a)_i + \lambda_1 S(a)_{i-1} = S(a)_i - S(a)_{i-1}.
$$

But this is obvious: each element of the sequence $$a$$ is indeed the difference between adjacent prefix-sums!

Of course, like most things in mathematics, this is far from the end of the story. Much more powerful transforms and the associated inversion formulas can be generated with different convolutions. For example, doing the same thing as above with the binomial convolution, for example, leads to the [*binomial transform*](https://en.wikipedia.org/wiki/Binomial_transform) $$B$$ and the nontrivial Pascal's inversion formula (Pascal really did go crazy with binomial coefficients!). And with the Dirichlet convolution, the celebrated [Möbius inversion formula](https://en.wikipedia.org/wiki/M%C3%B6bius_inversion_formula). Note, of course, that all the nontriviality of a theorem thus generated lies in the proof of the associativity and commutativity of the convolution involved and in the proof that the sequence $$\mathbf 1$$ is indeed inverted by $$\lambda$$.

Anyways, we must now generate $$\lambda$$ for the binomial convolution. The constraints are

$$
\begin{align*}
    \lambda_0 &= 1, \\
    \binom{1}{0} \lambda_0 + \binom{1}{1} \lambda_1 &= 0, \\
    \binom{2}{0} \lambda_0 + \binom{2}{1} \lambda_1 + \binom{2}{2} \lambda_2 &= 0, \\
    &\;\;\vdots
\end{align*}
$$

which yields the pleasant solution $$\lambda = (1, -1, 1, -1, \ldots)$$ (remember $$\sum_{k=0}^n (-1)^k \binom{n}{k} = 0$$ for $$n \geq 1$$?). The associated transform is

$$
B(a)_n = \sum_{k=0}^{n} \binom{n}{k} a_k,
$$

and so our computation of the inverse $$\lambda$$ yields the inversion formula

$$
a_n = \sum_{k=0}^{n} \binom{n}{k} B(a)_k (-1)^{n-k}.
$$

so $$B$$ is also "almost self-inverse", except for the alternating sign. (Note: alternating signs can sometimes prove to be a nuisance in efficient computation, see [permanent vs determinant](https://www.wisdom.weizmann.ac.il/~feige/algs/permanent.pdf))



<!-- We move away from complex numbers all the way to the other end: natural numbers. Ah, the joy of natural-number arithmetic. -->




<!-- Note to JEE: A lot of high-schoolers will have studied the famous self-inverse function

$$
f(x) = \frac{x}{x-1}
$$

which JEE loves to trick students with; this is just the translated version of the previous function: $$f(x) - 1 = 1/(x-1)$$. -->

That's a wrap. For the reader interested in a further application of convolutions, let's go to combinatorics: I recommend checking out generating functions, compositional constructions and the use of convolutions as the coefficients of products of generating functions. Hopefully I'll get to writing about this at some point.
<!-- There are many more famous self-inverse (or slightly different, self-dual) transforms out there. Try the Möbius transform first -->
<!-- Until next time on Saturday the fourteenth. Do ping me if you've got comments or suggestions. I'm very new to this business of blogging and could certainly use some help. You can reach me at the usual email address: ```kagaram@cse.iitb.ac.in```. -->