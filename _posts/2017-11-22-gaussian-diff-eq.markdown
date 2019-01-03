---
layout: post
title:	"A Differential Equations View of the Gaussian Family"
date:	2017-11-22
category: stats
---

$$\begin{align}
\frac{d}{dx}p(x) = -xp(x)
\end{align}$$
{: style="text-align: center"}

<!--exc-->

## Summary

Distributions from the Gaussian family
appear in a wide variety of distinct settings:
as
[the sampling distributions]({{site.url}}/stats/2017/02/24/statistics-as-pushforward.html)
of many useful statistics,
thanks to the
[central limit theorem](https://www.khanacademy.org/math/statistics-probability/sampling-distributions-library/sample-means/v/central-limit-theorem),
as the
[maximum entropy distributions](http://www.math.uconn.edu/~kconrad/blurbs/analysis/entropypost.pdf)
for fixed mean and covariance,
and as the assumed error distributions
for optimization problems that use the
[mean squared error](https://en.wikipedia.org/wiki/Mean_squared_error)
in their cost function.

In the post below,
I will derive the key properties of the Gaussian family
that lead to its appearance in these settings,
all without ever writing down what the formula for a Gaussian even is.
All we need to get started is a single differential equation:

$$
\frac{d}{dx}p(x) = -x p(x)
$$
{: style="text-align: center"}

with the condition that $$p(x)$$ goes to $$0$$ at the boundaries,
$$-\infty$$ and $$\infty$$.

If, after performing an affine transformation on the inputs,
a function $$p$$ satisfies the above differential equation,
we will say that it is a member of the *Gaussian Family*
$$\mathcal{G}$$.

### The Gaussian Family has Quadratic Surprise Functions

The surprise function $$S$$ for a random variable
with distribution $$p$$ is

$$
S(x) = \log\frac{1}{p(x)}
$$
{: style="text-align: center"}

Roughly speaking, this is how "surprised"
I should be when an event that has probability $$p(x)$$
occurs.
This idea can be made rigorous by analyzing a
[game where players compete to be the least surprised]({{site.url}}/stats/2017/11/09/the-surprise-game.html).
The expected value of the surprise is equal to the
[entropy of the random variable]({{site.url}}/stats/2016/03/29/info-theory-surprise-entropy.html).
Furthermore, if we are using
[the maximum likelihood principle](https://www.youtube.com/watch?v=I_dhPETvll8)
to fit models to data,
we can reformulate our optimization problem as
minimizing the surprise instead,
often leading to simpler expressions.

What does our differential equation tell us about the surprise functions of the Gaussian family?
Let's begin by calculating the derivative of the surprise
for an arbitrary $$p$$:

$$
\begin{align}
\frac{d}{dx}S(x) &= \frac{d}{dx}\log\frac{1}{p(x)} \\
&= -\frac{d}{dx} \log p(x)\\
&= \frac{-\frac{d}{dx}p(x)}{p(x)}
\end{align}
$$
{: style="text-align: center"}

If we rearrange our differential equation, we see that

$$
\frac{\frac{d}{dx}p(x)}{p(x)} = -x
$$
{: style="text-align: center"}

and so combining our results, we get

$$
\frac{d}{dx}S(x) = x
$$
{: style="text-align: center"}

Meaning that the surprise function must necessarily be quadratic,
since the derivative of $$x^2/2$$ is $$x$$.

Quadratic functions other than $$x^2/2$$,
e.g. $$(x^2-\mu)/2$$ or $$(x^2-\mu)/2\sigma^2$$
for arbitrary $$\mu$$ and positive $$\sigma$$
will also have linear derivatives whose sign matches their inputs.
That is, we have confirmed that affine transformations of Gaussian random variables
are also Gaussian random variables.

This has several consequences:

1. If we are fitting a model and we measure our goodness-of-fit
by calculating the squared error,
we must be performing some kind of maximum likelihood estimation with a Gaussian distribution on the errors.
2. Extending this argument to vector-valued $$x$$, we see that the surprise is,
if we allow no transformations, simply proportional to the
squared Euclidean length of $$x$$,
or, if we allow translations, of the squared distance from $$x$$ to some $$\mu$$.
Therefore these members of the Gaussian family are *isotropic*, or dependent only on distance.
In fact,
[the Gaussian family contains all distributions](http://www-biba.inrialpes.fr/Jaynes/cc07s.pdf)
that are isotropic and have independent components.

This leads to our next key property.

### The Gaussian Family is Closed Under Pointwise Multiplication

Say I have a distribution $$r$$ that is arrived at by
multiplying two members of $$\mathcal{G}$$,
$$p_1$$ and $$p_2$$.
Is $$r$$ in $$\mathcal{G}$$?

Consider the following argument:

$$
\begin{align}
r(x) &= p_1(x) \cdot p_2(x) \\
\log(r(x)) &= \log\left(p_1(x) \cdot p_2(x)\right) \\
\log(r(x)) &= \log\left(p_1(x)\right) + \log\left(p_2(x)\right) \\
-\log(r(x)) &= -\log\left(p_1(x)\right) + -\log\left(p_2(x)\right) \\
\end{align}
$$
{: style="text-align: center"}

That is, the surprise function of $$r$$ is just the sum
of the surprise functions of $$p_1$$ and $$p_2$$.
But the family of quadratics,
like all families of polynomials,
is closed under addition,
and so $$r$$ also has a quadratic surprise function
and so must be a member of $$\mathcal{G}$$.
We will make use of this after we prove the next closure property of $$\mathcal{G}$$.

### The Gaussian Family is Closed Under the Fourier Transform

The Fourier transform $$\mathcal{P}$$ of a distribution $$p$$
is defined as

$$
\mathcal{P}(k) = \int p(x) \mathrm{e}^{-ikx} dx
$$
{: style="text-align: center"}

In a probability setting,
$$\mathcal{P}$$ is also called the
*characteristic function*
of a random variable with distribution $$p$$.
For $$\mathcal{G}$$ to be closed under the Fourier transform,
it should be the case that

$$
\frac{d}{dx}p(x) = -xp(x) \Rightarrow \frac{d}{dk}\mathcal{P}(k) = -k\mathcal{P}(k)
$$
{: style="text-align: center"}

So let's start from the lefthand-side of the second equation above
and try to work our way to the righthand-side.

We start by writing out the definition of the Fourier transform,
then exchanging the integration and differentiation operations,
serene in the expectation that we're not in one of the pathological cases
in which that's not allowed.

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k) &= \frac{d}{dk} \int p(x) \mathrm{e}^{-ikx} \\
&= \int \frac{d}{dk} p(x) \mathrm{e}^{-ikx}
\end{align}
$$
{: style="text-align: center"}

We then take that derivative inside the integral,
remembering our product rule
and then making use of the fact that $$p(x)$$ doesn't depend on $$k$$:

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k) &= \int \frac{d}{dk} p(x) \mathrm{e}^{-ikx} \\
&= \int -ix p(x) \mathrm{e}^{-ikx} + \frac{d}{dk}p(x) \cdot \mathrm{e}^{-ikx} \\
&= i \int -x p(x) \mathrm{e}^{-ikx}
\end{align}
$$
{: style="text-align: center"}

We recognize this new term of $$-xp(x)$$ from our differential equation:
it is the derivative of $$p(x)$$.

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k)
&= i \int -x p(x) \mathrm{e}^{-ikx} \\
&= i \int \frac{d}{dx} p(x) \mathrm{e}^{-ikx}
\end{align}
$$
{: style="text-align: center"}

If we really remember our introductory calculus,
we recognize this as a prime candidate for
[integration by parts](http://tutorial.math.lamar.edu/Classes/CalcII/IntegrationByParts.aspx).
We make the following substitution, using the usual notation:

$$
\begin{align}
	&u = \mathrm{e}^{-ikx}
	&v = p(x) \\
	&du = -ik\mathrm{e}^{-ikx}dx
	&dv = \frac{d p(x)}{dx} dx
\end{align}
$$
{: style="text-align: center"}

The result is

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k)
&= i \int \frac{d}{dx} p(x) \mathrm{e}^{-ikx} \\
&= i \left( \left[p(x)\mathrm{e}^{-ikx}\right] - \int -ik p(x) \mathrm{e}^{-ikx} \right)
\end{align}
$$
{: style="text-align: center"}

where the $$\left[f(x)\right]$$ means $$f(\infty) - f(-\infty)$$.
Since $$p(x)$$ is $$0$$ at both positive and negative infinity,
that term is $$0$$,
and we can simply write

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k)
&= i \left( \left[p(x)\mathrm{e}^{-ikx}\right] - \int -ik p(x) \mathrm{e}^{-ikx} \right) \\
&= -i \int -ik p(x) \mathrm{e}^{-ikx}
\end{align}
$$
{: style="text-align: center"}

Almost done!
We just need to pull the $$-i$$ and $$k$$ out,
since they are constants.

$$
\begin{align}
\frac{d}{dk}\mathcal{P}(k)
&= -i \int -ik p(x) \mathrm{e}^{-ikx} \\
&= -i\cdot-i\cdot k \int p(x) \mathrm{e}^{-ikx} \\
&= -k \mathcal{P}(k)
\end{align}
$$
{: style="text-align: center"}

Voila!
The Fourier transform of the function that satisfies
our differential equation ALSO satisfies the same equation.
If we allow translation and scaling,
the answer is the same,
[but requires more algebra](https://math.stackexchange.com/questions/270566/how-to-calculate-the-fourier-transform-of-a-gaussian-function).

### The Gaussian Family is Closed Under Convolution

We can obtain this result by combining the previous two results
via the
[convolution theorem](https://www.khanacademy.org/math/differential-equations/laplace-transform/convolution-integral/v/the-convolution-and-the-laplace-transform),
which states that
the convolution of two functions is equal to the pointwise multiplication of their Fourier transforms.
Mathematically, we can write this out,
using calligraphic letters for Fourier transforms and $$\circledast$$
for convolution, as

$$
p \circledast q = \mathcal{P} \cdot \mathcal{Q}
$$
{: style="text-align: center"}

Because the Gaussian family is closed under Fourier transforms
AND closed under pointwise multiplication,
it is ALSO closed under the composition of those operations,
which is the same as being closed under convolution.

Why is this important? It leads directly to the next two results.

### The Gaussian Family is an Attractor for Sums of Independent Random Variables

Say I have two independent random variables, $$X$$ and $$Y$$,
that both have Gaussian distributions.
Does their sum, $$Z = X+Y$$,
also have a Gaussian distribution?

The answer is *yes*,
because
[the distribution of the sum of two independent random variables
is the convolution of their distributions](http://colah.github.io/posts/2014-07-Understanding-Convolutions/).

Therefore, if I add two independent random variables with Gaussian distributions together,
the result also has a Gaussian distribution.

### Sums of Independent Random Variables Get Closer to the Gaussian Family

This last section is a bit looser than the other sections,
since the proofs become substantially more technical.

But speaking intuitively,
what we've proven thus far is that convolving with a Gaussian
won't take you *out* of the Gaussian family.
That is, the Gaussian family is *stable*
under the operation of repeated convolution.
Put another way,
if we take the space of all distributions,
and we imagine repeatedly convolving every distribution in that space with a Gaussian,
we'd see that members of the Gaussian family stay in the Gaussian family.
In fact, if we restrict ourselves to the set of distributions with finite variance,
the Gaussian family is the only subset with this property.

In terms of random variables,
this says that adding a bunch of independent Gaussian variables to some initial random variable
will always result in a variable that is Gaussian distributed.

In fact, something stronger can be shown:
convolution with any distribution with finite variance
is a *contracting map* on the space of distributions modulo location and scaling,
and so all iterated convolutions are drawn towards the fixed point of that map,
the Gaussian family.

This is one way of stating the central limit theorem,
which states that the distribution of a sum of independent random variables
tends towards a Gaussian distribution as more variables are added.
