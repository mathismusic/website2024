---
layout: post
title: "The theory of deep learning: why"
mathjax: true
---

<!-- *This is part one of two. Part one asks whether deep learning needs a mathematical theory at all; [part two]({% post_url 2026-08-13-deep-learning-theory-how %}) surveys what that theory looks like so far.* -->

I will unfortunately start with a cliché. Artificial Intelligence (AI) has been transformative for pretty much everyone in the world, greatly affecting our perspective on what is considered a normal part of daily life. We are however much more primitive in our understanding of *why* these techniques work so well in practice and what a potential ceiling for their capabilities might look like. We do keep improving our models, e.g. in data and compute efficiency or in how often they confidently make things up, but these improvements are primarily directed by accumulated intuition, careful data collection and tuning and not really by a mathematical framework.

The question we ask is then: Do we really need a mathematical framework to understand artificial intelligence? Or is it enough to keep improving the techniques and their applications?

In the next section, I shall attempt to convince the reader of an affirmative answer to this question by drawing parallels between the history of the development of the natural sciences and that of the man-made science we call AI. And then we shall survey some interesting tidbits of the emerging field of (machine) learning mechanics.

## Do we need the math?

Let's review a time in human history similar to today. The people of the late eighteenth century were fortunate to receive a similar gift as us in the form of the steam engine which made mechanical automation possible for the first time. This led to the industrial revolutions for over a century until the world went to war in 1914. We ask the question: did one need a mathematical framework to understand the steam engine? Certainly not immediately; the world went nearly eighty years into the industrial revolution before we had a fairly solid understanding of thermodynamics and the efficiency of engines. Do we need it today? Honesty compels me to admit that most progress has still been driven by engineering, e.g. better alloys, blade cooling and aerodynamics, and not theory. But it was thermodynamics that told us a performance ceiling exists that is independent of the specific implementation, that entire directions like perpetual motion or fancier working fluids were not worth pursuing, that very high compression improves efficiency, and so on; essentially, theory still tells us which building to lean the ladder against, and the engineer scales the building one step at a time.

Beyond pragmatic considerations, there is a deeper, more fundamental reason for a mathematical framework. Recall again the steam engine and thermodynamics. The formal study of the steam engine led to a much better understanding of energy usage and drove cryo tech, heat pump tech, industrial chemistry, and more. In other words, a mathematical understanding of the principles governing a system usually gives a much more generalizable understanding of other things that work on similar ideas. For AI, this is especially important: clearly, our systems exploit deep principles of learning and representation that we do not yet understand, and a general understanding of learning and intelligence can shed light on how biological intelligence works with potentially important implications for neuroscience and psychology. We are possibly en route to solving the last great scientific problem of our time: understanding the human mind. A solid study of existing artificial intelligence together with progress in neuroscience is likely to be a fruitful path forward for both fields.

There is one more point to get out of the way. Even if useful, is it *possible* to formalize AI anytime soon? It has evaded our mathematics and has sprung surprise after surprise for over ten years. However, this is not a reason to give up. The history of science is full of examples of phenomena that were not understood for decades or centuries, and yet eventually gave way to accurate and predictive mathematical frameworks: classical mechanics, chemistry, thermodynamics, electromagnetism, et cetera. It is not unreasonable to expect that the same will happen *eventually* for learning, which is simply another man-made science, and indeed we have started to see some of the first glimpses of such a theory in the last few years.

Welcome to the theory of AI: *learning mechanics*.

## Setting up

As any good mathematician will tell you, one must always begin by setting up the precise problem to attack in front of oneself.

A deep learning system is specified by four things.

- The *architecture* is the network $$f(x;\theta)$$, built as a composition of simple linear and nonlinear functions. $$\theta$$ is the set of parameters of the network, e.g. weights and biases.
- The *data* is a set $$\mathcal{D} = \{(x_i, y_i)\}_{i=1}^n$$ drawn from some unknown distribution $$P_{\mathrm{data}}$$.
- The *task* is an objective $$\mathcal{L}(\theta)$$ scoring how well $$f(x;\theta)$$ does on $$\mathcal{D}$$.
- The *learning rule* is the update we apply, e.g. $$\theta^{(t+1)} = \theta^{(t)} - \eta \nabla \mathcal{L}(\theta^{(t)})$$, along with an initialization $$\theta^{(0)}$$ and hyperparameters such as the learning rate $$\eta$$.

We shall, for the sake of this article, restrict ourselves to the setting of fully-connected neural networks; but note that the theory of transformers and convolutions has started to emerge as well.

## The current state of affairs

Here is a diagram of (my current knowledge of) the current state of learning mechanics:

![Today's map of learning mechanics: model size against model complexity, with simple systems and linear/NTK theory at the top, today's systems and scaling laws in the middle, and infinite limits and approximation at the edges.]({{ site.baseurl }}/public/images/learning-mech-overview.png)

Why is learning mechanics hard? The four ingredients above are all visible and measurable, and (nowadays) experiments are fast and almost free. The obstacle is complexity: the four ingredients interact to give dynamics that are nonlinear, coupled and enormously high-dimensional. Just like physical systems in the real world or the Avogadro number of particles in a slow organic chemical reaction. The bet of learning mechanics is to find macroscopic regularities that emerge from a deep learning system at hand, and use them to predict the system's behavior.

We would like to mention here that learning mechanics is very different from universality: yes, neural networks are fully expressive and they can approximately model any function; however, the problem of deep learning lies in showing that gradient descent does actually help them converge to said functions. This is very nontrivial because of nonconvexity and dimensionality; learning mechanics is concerned with understanding the training process. It's just like how showing the existence of a solution to a differential equation and showing that a numerical method finds said solution are very different claims.

---

*Continue to [part two: the theory of deep learning, how]({% post_url 2026-08-13-deep-learning-theory-how %}), where we look at linearization, infinite limits, scaling laws and neural collapse.*
