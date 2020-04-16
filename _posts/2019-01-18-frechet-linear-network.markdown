---
layout: post
title:	"Fréchet Derivatives 3: Deep Linear Networks"
date:	2019-01-18
category: math
---

$$\begin{align}
\nabla_{W_k} l(W_1, \dots, W_L) = W_{k+1:}^\top \nabla L(W) W_{:k}^\top
\end{align}$$
{: style="text-align: center"}

<!--exc-->

### Introduction

The Fréchet derivative makes the computation
of certain matrix- and vector-valued derivatives by hand
substantially less painful.
[The first blog post in this series]({{site.url}}/math/2018/03/06/frechet-derivative-introduction.html)
defined the Fréchet derivative and applied it to some very simple functions,
while
[the second blog post]({{site.url}}/math/2018/03/07/frechet-least-squares.html)
applied it to the derivation of the normal equations
and gradient updates for linear least squares.

In this final blog post,
we will apply the Fréchet derivative to
computing the gradients for a somewhat peculiar version of linear regression:
the _deep linear network_.

### Neural Networks Without All That Pesky Nonlinear Stuff

Neural networks are currently pushing forward the state of the art
in machine learning and artificial intelligence.
If you're unfamiliar with them, check out
[this blogpost]({{site.url}}/external/2018/10/31/neural-nets-colab.html)
for a gentle introduction.

The most common form of neural network, a feed-forward neural network,
is a parameterized function whose parameters are matrices
and which looks like

$$
f\left(x; W_1, W_2, \dots W_L\right) = \sigma_L \left( W_L \dots \sigma_2\left(W_2 \sigma_1 W_1 x\right)\right)
$$
{: style="text-align: center"}

where each $$\sigma_i$$ is some nonlinear function.
In English, we take the input $$x$$ and multiply it by a series of matrices,
but in between we apply nonlinear transformations.

Neural networks can mimic
[just about any function you'd like](http://neuralnetworksanddeeplearning.com/chap4.html),
provided you carefully choose the $$W_i$$s.
This genericness is part of what makes them so powerful, but it also makes them hard to understand.

If we're interested in more deeply understanding things like how neural networks learn and why they
[behave strangely sometimes](https://ml.berkeley.edu/blog/2018/01/10/adversarial-examples/),
we need a model that's like a neural network in certain important ways, but which we can analyze.

Enter the _linear network_.
I wrote about single-layer linear networks in a blog post
[introducing linear algebra ideas to neuroscientists]({{site.url}}/math/2017/08/17/linear-algebra-for-neuroscientists.html).
A linear network chooses each $$\sigma_i$$ to be the identity function,
or a "pass-through" function  that doesn't touch its inputs.
The result is a network that looks like

$$
f\left(x; W_1, W_2, \dots W_{L-1} W_L\right) = W_L W_{L-1} \dots W_2 W_1 x
$$
{: style="text-align: center"}

Unlike in the nonlinear case, it's easy to write down what function the network is computing.
If we denote by $$W$$ the matrix that results from multiplying all of the $$L$$ matrices together,
we obtain

$$
F\left(x; W\right) = Wx
$$
{: style="text-align: center"}

and we know quite a bit about functions which can be represented by matrices.
These linear functions, in fact, are just about the only functions we really understand.

### Computing the Loss Gradients of a Deep Linear Network

This derivation is taken directly from
[this arXiV paper](https://arxiv.org/abs/1712.01473),
which goes on to prove some more cool stuff about linear networks.

When we train a neural network,
we're interested in adjusting the parameters so as to minimize some negative outcome,
which we call the loss, $$l$$.
In linear regression, this loss is often the squared prediction error.

For our purposes, we won't need to know what it is.
We just need the following fact:

$$
l\left(W_L, \cdots W_1\right) = L(W)
$$
{: style="text-align: center"}

that is, there exists a function $$L$$ that is just a function of $$W$$,
our product of matrices, and which is equal to the original loss $$l$$,
which is a function of all of the matrices.

A side note:
you might think of both of these as being functions also of the data $$x$$,
but we won't be referring to the data, so I've chosen to suppress that in the notation.

And before we take off, I'll introduce one last piece of notation.
We'll need to refer to abbreviated versions of our matrix product $$W$$,
e.g. the product of the first 5 or last 3 matrices.
Inspired by Python, we'll denote the products truncated at $$i$$
from the front and from the back as

$$
W_{:i}, W_{i+1:}
$$
{: style="text-align: center"}

respectively.
If $$i$$ is 1 or $$L$$,
the resulting matrices are taken to be the identity.

We now recall the Fréchet definition of the derivatve
$$\nabla g$$,
of a matrix-to-scalar function $$g$$:

$$
g(M + \epsilon) = g(M) + \langle\nabla g(M), \epsilon\rangle + o(\epsilon)
$$
{: style="text-align: center"}

see
[the first blog post in this series]({{site.url}}/math/2018/03/06/frechet-derivative-introduction.html)
for details.
Most importantly, the inner product $$\langle , \rangle$$ is in the
[Frobenius norm]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html),
meaning it applies to matrices and can also be written

$$
\langle A, B \rangle = \mathrm{tr}\left(A^\top B\right)
$$
{: style="text-align: center"}

where $$\mathrm{tr}$$ is the trace of the matrix.
This is equivalent to multiplying the matrices entrywise and summing up the results,
much like the more familiar inner product of vectors.

Let's take our derivative definition and plug in our linear network loss function,
computing the derivative with respect to the $$k$$th weight matrix:

$$
\begin{align}
&l\left(W_1, \dots W_{k-1}, W_k +\epsilon, W_{k+1}, \dots W_L\right)\\
&= L(W + W_{k+1:}\epsilon W_{:k}) \\
&= L(W) + \langle \nabla L(W), W_{k+1:}\epsilon W_{:k}\rangle + o(\epsilon)
\end{align}
$$
{: style="text-align: center"}

where the second line follows by multiplying out the matrix product and applying our definitions.

Next, we swap over to the trace version of the inner product and apply the cyclic property
which was so crucial in the
[the second blog post in this series]({{site.url}}/math/2018/03/07/frechet-least-squares.html).
That is, underneath the trace, we are allowed to permute matrices,
letting us write

$$
\begin{align}
&\langle \nabla L(W), W_{k+1:}\epsilon W_{:k}\rangle\\
&= \mathrm{tr}\left(\nabla L(W)^\top W_{k+1:}\epsilon W_{:k}\right)\\
&= \mathrm{tr}\left( W_{:k}\nabla L(W)^\top W_{k+1:}\epsilon\right)\\
&= \langle W_{k+1:}^\top\nabla L(W) W_{:k}^\top,\epsilon\rangle
\end{align}
$$
{: style="text-align: center"}

and thus, pattern-matching to the definition of the derivative, we have that

$$\begin{align}
\nabla_{W_k} l(W_1, \dots, W_L) = W_{k+1:}^\top \nabla L(W) W_{:k}^\top
\end{align}$$
{: style="text-align: center"}

That is, we take the derivative with respect to the whole product of matrices
and then propagate it through every other matrix, transposed.

### Closing

This proof may feel shockingly short and direct.
Where are all the indices and sums?
This is the advantage, which I hope this series has made clear,
of using Fréchet derivatives on polynomial and linear functions
that have scalar outputs but take in vectors or matrices.

For functions, like neural networks, where there is mixture of linear and nonlinear components,
the Fréchet derivative can be used to handle the linear component,
which is frequently the source of troubles, from shapes to indexing.
With that part handled, attention can go where it is most needed,
on the nonlinear piece.
