---
layout: post
title: "The theory of deep learning: how"
mathjax: true
---

*This is part two. [Part one]({% post_url 2026-08-12-deep-learning-theory-why %}) argued that deep learning needs a mathematical theory.*

We shall now explore some of the most interesting and promising directions in learning mechanics today. It's fun, I promise.

## Linearization

Like with any field of mechanics, a reliable way to build scientific understanding is to study simpler representative systems that are amenable to analysis, which then serve as sources of intuition for much broader classes of systems.

The simplest way to make a neural net $$f$$ simple is to omit the nonlinear functions altogether; $$f(x)$$ reduces to the form complicated-matrix-of-parameters (call it $$W$$) times $$x$$. One can prove that gradient descent on the parameters to train the network to map each $$x_i$$ to $$y_i$$ yields a very interesting property: the network exhibits what is called a greedy low-rank bias, which is a fancy way of saying "start with the simplest parameter choices, and then add complexity as needed". A bit of mathematical formalism to make this precise: Suppose for each $$i$$

$$y_i = \begin{bmatrix} 5 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & 1 \end{bmatrix} x_i.$$

Then look at what happens when we train a linear network $$f(x) = W x$$ to minimize the loss $$\sum_i \|f(x_i) - y_i\|^2$$. The network notices that learning the coefficient $$5$$ is top priority, since matching that reduces the loss more than matching the coefficients $$3$$ or $$1$$ will; the training progress looks like this:

$$W_1 \approx \begin{bmatrix} 5 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} \rightarrow W_2 \approx \begin{bmatrix} 5 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & 0 \end{bmatrix} \rightarrow W_3 \approx \begin{bmatrix} 5 & 0 & 0 \\ 0 & 3 & 0 \\ 0 & 0 & 1 \end{bmatrix}.$$

In general, the network will learn the best matrix's singular value decomposition in order of decreasing singular values. This intuition, though we have not yet proved it, carries over to the nonlinear case; simpler (nonlinear) functions are learned first in a way that minimizes the loss as quickly as possible, followed by more complex functions to finetune the model. Why might neural networks have this behavior? The key is that gradient descent is a greedy algorithm itself! The direction of steepest immediate descent is the negative of the gradient, and so the network greedily reduces the loss maximally at each step.

## Infinitely large networks

Back to physics for a sec. Constructing a microscopic theory tracking individual particles in a physical system, say a gas, is hopelessly complicated. However, it has often proven to be extremely fruitful to take the limit of infinitely many particles, which allows one to track the system as a single distribution over the particles.

![Taking the number of particles to infinity turns a histogram of individual particle speeds into a single smooth speed distribution.]({{ site.baseurl }}/public/images/infinite_v2.png)

A concrete example of this is the Maxwell-Boltzmann distribution to describe the distribution of particle velocities in a gas, and it is very accurate even for finite-particle gases. Same goes for the ideal gas law $$PV = nRT$$.

Is the same true of deep learning? Does taking the limit of an infinite number of neurons in a layer, or an infinite number of layers, or an infinite amount of data, et cetera simplify analytical calculation? One moves again into the distribution world, where an infinite layer is replaced by a single distribution modeling neuron activation. The answer is, of course, yes, since this section exists. Lots of very good progress here.

Let's briefly look at the most well-studied limit: the infinite width limit, where there are a finite number of infinitely wide layers. In this limit, one of two interesting things always happens: depending on the initialization (as a distribution) of the weights of the last layer of the network, the network is either *lazy* and learns very little — its weights hardly shift at all from their initial values — or is *rich* and its weights shift by a very substantial amount throughout training, making it learn well from data.

Perhaps a useful analogy is the stress-strain curve of a material: if the strain is very small, the material does not deform much from its original shape; however, once the strain is large enough, the material deforms substantially and learns a new shape. Indeed, the last layer's initialization in the lazy case is such that the output is already the right order needed to simply fit training data without changing weights much. In the rich case, the last layer's initialization is uniformly zero, forcing it to change substantially if the output is to be nonzero and close to the training labels.

The rich regime is the one we must seek to operate in. Yang and Hu (2021) set up the *Tensor Programs* framework to reliably construct hyperparameter and learning rate choices that land us in the rich regime for infinite-width networks. It is then empirically observed that the same choices work well for large finite-width networks too! This parameterization, called the *maximal update parameterization* or $$\mu$$P, is starting to see adoption in practice. Limits can be useful even for finite systems!

## Empirical laws

For hundreds of years, we have described gases and chemical reactions by simple relationships between macroscopic measures: pressure, volume, temperature, concentration. These laws came from careful experiment, after the fact. They continue to be extremely predictive, and they are the basis of most of us non-chemist folks' understanding of chemistry. Since pretty much everything about the training process is super easy to measure experimentally, such empirical laws are natural to look out for in deep learning too. Indeed, a family of such laws called *scaling laws* exists today and turns out to be very predictive of the behavior of large networks. What are these laws, though?

Let's fix a model architecture and a dataset, and denote test loss at convergence by $$L$$, number of parameters by $$N$$, compute in floating-point operation (FLOP) count by $$C$$ and number of training tokens by $$D$$. The latter three variables are related approximately as $$C = 6ND$$: a forward and backward pass through a typical transformer model costs about $$6$$ FLOPs per parameter per token. Then it turns out $$\log L$$ is linear in $$\log N$$, $$\log C$$ and $$\log D$$ (i.e. a power-law dependence) when the other two are not bottlenecking. Such laws are very similar to the thermodynamic laws of adiabatic expansion and compression, where the exponents of the power law are dependent on the structure of the gas and were theoretically understood and estimated well after they were measured.

![The loss surface over parameters and tokens, with the Chinchilla-optimal point marked along the fixed-compute curve 6ND = C.]({{ site.baseurl }}/public/images/chincilla.png)

Let's quickly see an example of how scaling laws help in practice. This is called *Chinchilla* optimality in the literature. Consider a fixed training compute budget $$C$$. We must tune $$N$$ and $$D$$ to minimize the loss, respecting $$C = 6ND$$, to obtain the best model possible. So we set up a joint scaling law for $$L$$ as a function of both $$N$$ and $$D$$:

$$L(N, D) = E + \frac{A}{N^{\alpha}} + \frac{B}{D^{\beta}}.$$

One can think of $$E$$ as the irreducible loss, $$A/N^{\alpha}$$ as the penalty for a finite-sized model and $$B/D^{\beta}$$ as the penalty for a finite dataset. Fitting parameters to many different much-smaller-scale experiments yields rough values of $$E, A, B, \alpha$$ and $$\beta$$ for a given architecture and dataset. One finishes by optimizing $$L(N, D)$$ subject to the constraint $$C = 6ND$$ to find the "*Chinchilla*-optimal" model size $$N^{\ast}$$ and dataset size $$D^{\ast}$$. Hoffman's experiments give $$D/N = 20$$ tokens/parameter at Chinchilla optimality. And now one can simply train a much bigger model on a much bigger dataset and expect to get a model with as small a test loss as possible at that compute budget, for free.

A quick note: Over the last couple of years, inference costs have skyrocketed, so one usually replaces the cost with $$C = 6ND + \lambda N$$ to account for inference, forcing $$N$$ to be much smaller than Chinchilla $$N^{\ast}$$, pushing upwards of $$10$$k tokens/parameter. This means that one actually trains much longer (many more tokens) than Chinchilla optimality prescribes.

## Neural collapse

It is also often fruitful to look at the behavior of the network at convergence (i.e. after training is complete) and come up with empirical laws describing the structure of its parameters at this time. One really cool phenomenon in this regard is called *neural collapse*. Consider a neural network trained to classify images into $$k=4$$ classes. So we throw in a bunch of layers, and at the end we have four outputs corresponding to the four classes, and we say the network predicts the class with the highest output value. Let's train it till we get near-perfect accuracy on the training set. To find out what the function learns to do, consider the last set of weights of the network and denote by $$\tilde x$$ the result of passing an image $$x$$ until the penultimate layer, just before this last set of weights. This last set consists of four vectors $$[w_1, w_2, w_3, w_4]$$, and the four-dimensional output of the network is the dot product with $$\tilde x$$, i.e. $$[w_1 \cdot \tilde x, w_2 \cdot \tilde x, w_3 \cdot \tilde x, w_4 \cdot \tilde x]$$. Now the dot product measures alignment between two vectors, with a larger value indicating that the two vectors are more aligned.

![At initialization the class weight vectors and datapoints are scattered; at convergence they collapse onto the vertices of a regular tetrahedron with each class clustered around its weight vector.]({{ site.baseurl }}/public/images/neural-collapse.png)

Apologies for the long-winded setup. Here's the cool part: it turns out that the vectors $$[w_1, w_2, w_3, w_4]$$ are arranged in a very specific way after training: they form the vertices of a regular tetrahedron in three-dimensional space! And the $$\tilde x$$ vectors corresponding to datapoints of each class $$i$$ are clustered very close to $$w_i$$, so the network pretty much perfectly classifies them. This is about as good as classification can ever get: a tetrahedron is the unique arrangement of four points in 3D space that maximizes the distance between them, so the network has learned to represent each class as far away from the others as possible. So for a new image $$x$$, the network computes $$\tilde x$$ and then simply finds the closest $$w_i$$ to $$\tilde x$$ to classify it. In other words, keeping different classes as far as possible accounts for the network's ability to generalize very well even when the data is slightly corrupted or off-distribution, since the closest $$w_i$$ is still likely to be the correct class. This very simple structure of the network at convergence is one of the many examples of the prioritization of simplicity in neural networks, and is part of why they generalize so well to unseen data.

## Conclusion

The samples above are simply hand-picked nuggets of the current state of learning mechanics. It's a very new field, since deep learning only started working about ten years ago, and there is much, much more to unearth, especially in LLM mechanics given its importance today. Nevertheless, empirical laws have already started to guide practice, and the community is working hard towards more predictive theory. It is a hard problem and will take a long time yet to resolve, but it is going to happen. Come one, come all, and join the quest: you could be the next Isaac Newton, Charles Darwin or Albert Einstein of learning mechanics.

A substantial portion of this article is based on the excellent survey *There Will Be a Scientific Theory of Deep Learning* (2026), a collaborative effort from the leaders in the field. For the reader interested to know more (thank you!), I highly recommend reading it and the references therein.

Peace out.