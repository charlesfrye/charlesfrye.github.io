---
layout: post
title:	Statistics in One Sentence
date:	2017-02-24
category: stats
---

> Statistics is the study of pushforward probability measures from a probability space of datasets to a measurable space of statistics under maps that we call statistical procedures.

<!--exc-->

## Statistics as the Study of Pushforward Measures

Statistics is frequently presented as a hodge-podge of
techniques and tools, to the consternation of mathematicians,
who prefer the austere simplicity of measure-theoretic probability.

There is, however, a mathematician-friendly
(short, abstract, and incomprehensible until unpacked)
definition of the business of frequentist statistics:
it is the study of
*pushforward probability measures*
from a
*probability space of datasets*
to a
*measurable space of statistics*
under maps that we call
*statistical procedures*.

The probability measure
over the datasets is called a *hypothesis*.
When the pushforward measure
determines a distribution,
we call that distribution the
*sampling distribution of the statistic*.

Below, I define the pushforward measure
and give an example of this definition in practice.
I append, for completeness,
a collection of other definitions,
primarily to remind folks who have been
exposed to measures but are rusty;
a more leisurely introduction is recommended for the unfamiliar.
I suggest Pivato's
*Probability, Analysis, and Measure: A Visual Introduction*.

### Pushforward Probability Measures

Pushforward probability measures tell us what the
distribution of outcomes of
a deterministic function will be
when applied to random inputs.

Given a measurable map between
a probability space $$\mathcal{X}$$
with probability measure $$\mu$$
and a measurable space $$\mathcal{S}$$,
we can define the
pushforward probability measure $$f_*\mu$$
on $$\mathcal{S}$$ as

$$
    f_*\mu(S) = \mu \circ f^{-1}(A) = \mu\left(f^{-1}(A)\right)
$$

for sets $$S$$ in the $$\sigma$$-algebra of $$S$$.
Here $$f^{-1}(A)$$ means
"the preimage of the set $$A$$",
not
"the inverse function of $$f$$ applied to $$A$$".

### The Setup for Statistics

We begin with a dataset -- often, but not always,
a vector in $$\mathbb{R}^n$$.
We assume the existence of a probability measure
on some suitable $$\sigma$$-algebra over the space
in which our dataset lives.
For example, we might assume a Gaussian measure
with mean $$0$$ and unit, spherical variance.

We then apply a transformation, $$f$$, to this dataset.
This transformation might map us to the real line,
as when we compute the average of real numbers,
or it might map us to the unit interval,
as when we produce a "p value".

The transformation $$f$$ is called a *statistical procedure*
and the output of the transformation is called a *statistic*.

The result of combining the hypothesized or assumed
probability measure over datasets
with the statistical procedure $$f$$
is a pushforward measure over the space of statistics.
This measure defines, in almost all cases, a distribution,
which we call the *sampling distribution*.

### An Example

One idea from statistics that becomes easier to describe
in this framework is the *confidence interval*,
which is notorious for its
tendency to be mis-interpreted and mis-defined.

We call a statistic a confidence interval
for confidence level $$\alpha$$ when

- the probability measure over datasets is parametrized by a value $$\theta$$
- the statistical procedure maps from
the space of datasets to
the space of intervals "suited" to $$\theta$$
- the pushforward measure of the set of intervals that contain $$\theta$$
is at least $$\alpha$$ for all settings of $$\theta$$
Frequently, $$\theta$$ is real, and so the space of intervals
is the space of all real intervals.

### Closing Remarks

In statistics,
we are deeply concerned with the performance of
our statistical procedures.
I posit that any procedure can be derived
by applying (possibly constrained)
variational optimization principles to the
space of all measurable functions
between datasets and statistics.
Perhaps this re-casting would make it easier
for traditional hypothesis testing ideas
to make contact with the burgeoning applied field of
variational inference,
which defines statistical inference
as variational optimization.

## Appendix: Measure Theory Definitions

#### $$\sigma$$-algebra

$$\sigma$$-algebras are, for our purposes here,
simply collections of subsets of some set
that are "rich enough"
to support measures (to be defined shortly).
They are closed under the operations of
complementation and
countable union and intersection, and
they include the set itself and the empty set $$\varnothing$$.

Two key examples of $$\sigma$$-algebras over
a set $$X$$ are
the collection
$$\{\varnothing,X\}$$
and the set of all subsets of $$X$$,
also known as the *power set* of $$X$$.
They are the smallest and largest
$$\sigma$$-algebras you can define,
and are often too small and too big,
respectively, to be useful.

Another specific example would be, for the real line $$\mathbb{R}$$,
the set of all intervals plus their (countable) unions and intersections,
also known as the *Borel $$\sigma$$-algebra*,
$$\mathcal{B}$$.
It contains just about every subset of the real line that
a sane person would attempt to construct,
but misses out on the subsets, e.g., that lead to the
Banach-Tarski paradox.
If you promise not to attempt to construct such sets,
you can mentally replace all occurrences of "$$\sigma$$-algebra"
with "power set".

#### Measure

Measures capture our intuitive notions
of size, like
length, area, volume, and mass,
in such a way that we can
talk about them rigorously and extend them to
strange spaces, like spaces of functions.

A measure is a function, $$\mu$$,
from a $$\sigma$$-algebra
to the non-negative real numbers
that assigns measure $$0$$ to the empty set
and for which the following property holds
for all countable collections of disjoint sets
$$\{X_i\}$$:

$$
    \mu\left(\bigcup X_i\right)
    	= \sum \mu\left(X_i\right)
$$

That is, if we take two sets that don't overlap,
the "size", as measured by $$\mu$$,
of their union is just the sum of their sizes.

#### Probability Measure

If the measure assigns a value of $$1$$ to the entire set,
then we call it a *probability measure*.

In measure-theoretic probability, the set on which we define
a $$\sigma$$-algebra is the set of all possible universes.
We take as subsets the subsets on which, for example,
the die I am about to roll comes up $$1$$,
or not a $$1$$, or even, or so on.
The measure of a subset is called the
*probability*
of the event that defines it.

#### Probability Space

A probability space is the triple of
a set, $$X$$,
a $$\sigma$$-algebra over it, $$\mathcal{X}$$,
and a probability measure, $$\mu$$.

#### Measurable Space

A measurable space is the pair of a set $$X$$
along with a $$\sigma$$-algebra of its subsets
$$\mathcal{X}$$.

#### Measure Space

A measure space is the triple of a set $$X$$,
a $$\sigma$$-algebra of its subsets $$\mathcal{X}$$,
and a measure, $$\mu$$.
