Commutativity of $$\star$$ is easy to see; let's quickly show that it is also associative. For each $$n \geq 1$$, we have

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

yay! Please do notice the very similar structure of the proofs of associativity for the different convolutions. Many different convolutions go through the same proofs and have similar properties, and after a while the mathematician treats such new proofs as "pencil-pushing" exercises, referring to the mechanical re-writing of the same steps over and over again. Other mathematicians filter out exactly what properties are used about the convolution to make the proof work, and then generalize the proof to any convolution that satisfies those properties in one go -- you see, abstraction in mathematics is quite similar to the "if you use it more than once, make it a function" rule in programming.

Apologies for the digression.