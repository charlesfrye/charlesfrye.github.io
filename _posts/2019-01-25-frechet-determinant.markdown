---
layout: post
title:	"Fréchet Derivatives 4: The Determinant"
date:	2019-01-25
category: math
---

$$\begin{align}
\nabla \det M &= \det M \cdot \left(M^{-1}\right)^\top
\end{align}$$
{: style="text-align: center"}

<!--exc-->

### Introduction


In this series of blog posts,
we've seen that writing derivatives in the Fréchet style
makes the computation of derivatives of functions that have
matrix- or vector-valued inputs and scalar outputs
substantially easier.
[The first blog post in this series]({{site.url}}/math/2018/03/06/frechet-derivative-introduction.html)
defined the Fréchet derivative and applied it to some very simple functions.
[The second blog post]({{site.url}}/math/2018/03/07/frechet-least-squares.html)
applied it to the derivation of the normal equations
and gradient updates for linear least squares.
[The third blog post]({{site.url}}/math/2019/01/18/frechet-linear-network.html)
applied the Fréchet derivative to deep linear networks,
a form of neural network with no nonlinearity.

That was intended to be the final blog post,
but then I came across
[this post by Terry Tao](https://terrytao.wordpress.com/2013/01/13/matrix-identities-as-derivatives-of-determinant-identities/)
on the interesting relationships between derivatives of determinants and famous matrix identities.

That post is excellent, and I highly recommend you read it.
I will focus on one of the claims, which appears in the pull-out for this post:
the derivative of the determinant is the determinant times the transpose of the inverse.

### The Determinant

The determinant measures, for real-valued symmetric matrices,
the effect that the matrix has on volumes.
If the determinant is $$2$$, then the matrix doubles volumes.
That is, if a region has volume $$x = 10$$ before applying the matrix,
then it will have volume $$2x = 20$$ after applying it.
Check out
[this video by master explainer 3blue1brown](https://www.youtube.com/watch?v=Ip3X9LOh2dk)
for a nice intuitive look at the determinant.
It's an important function that appears all over the place in linear algebra.

Most important for us, though, is that it takes in a matrix $$M$$
and returns a scalar.
That means we can write a relation for the derivative, $$\nabla \det$$, easily and concretely in the Fréchet style:

$$
\det(M + \varepsilon) =
\det M + \mathrm{tr}\left(\nabla \det (M)^\top \varepsilon\right) + o(\varepsilon)
$$
{: style="text-align: center"}


See
[this previous post]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html)
for a bit on why the $$\mathrm{tr}(X^\top Y)$$ is showing up
and the previous posts in this series for context and examples on why
we write the derivative this way.

For the mathier folks, I'd like to additionally note that the determinant is a
[multilinear functional](https://en.wikipedia.org/wiki/Multilinear_map),
that is, it is a linear functional on a tensor product space,
where the tensor product is over vectors (viewed as rows or columns of the matrix).
This is a fancy way of saying that the determinant changes linearly when a column or row is changed linearly.
Given how smoothly the Fréchet derivative handles linear and polynomial functionals,
we might hope that it smoothly handles the determinant.
And our faith is duly rewarded.

### The Derivative of the Determinant

We begin by taking the expression on the left side and trying to find a way to expand it
so that terms that look like the right side begin to appear.

We don't have a ton of options, but a sufficiently clever individual might try the following:

$$\begin{align}
\det(M + \varepsilon) &= \det\left(M\left(I + M^{-1}\varepsilon\right)\right)\\
&= \det(M) \cdot \det\left(I + M^{-1}\varepsilon\right)
\end{align}$$
{: style="text-align: center"}

First, we "pulled the $$M$$ out", incurring an $$M^{-1}$$ for our trouble.
Then, we recognized that the determinant of a product of matrices
is the product of the matrices' determinants.
Consider: if the matrix $$A$$ scales volumes by $$2$$, and the matrix $$B$$ scales them by $$5$$,
then the matrix $$AB$$, which first applies the transformation $$B$$, then $$A$$, must scale volumes by $$10$$.

Now, we've traded the original question,
which was "what does the determinant function look like at a small perturbation around the matrix $$M$$?",
for an easier question:
"what does the determinant function look like at a small perturbation around the identity matrix $$I$$?"

Let's write this perturbed identity matrix out, ignoring the $$M^{-1}$$ for now:

$$
I + \varepsilon =
\begin{bmatrix}
1 + \varepsilon & \varepsilon & \dots & \varepsilon \\
\varepsilon & 1 + \varepsilon & \dots & \varepsilon \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\varepsilon & \varepsilon & \dots & 1 + \varepsilon
\end{bmatrix}
$$
{: style="text-align: center"}

What does a matrix like this do to volumes?
It's _almost_ a matrix with only diagonal elements,
since every value off of the diagonal is $$\varepsilon$$.

A matrix with only diagonal elements is sometimes called a _scaling matrix_.
Along each dimension, it simply scales by the diagonal element:
dimension 1 is scaled by the element in row 1 column 1,
dimension 2 by the element in row 2 column 2,
and so on.
Because each dimension is scaled independently,
it's easy to say what happens to the volume:
it's scaled by the product of all of the scalings.

Therefore the determinant of the matrix $$I +\varepsilon$$
is equal to the $$n$$-fold product of $$1 + \varepsilon$$,
plus any effects due to the off-diagonal terms,
which we will presume to be $$o(\varepsilon)$$ (they are).
We write this fact down and then expand the $$n$$-fold product,
immediately noting that all of the terms after the first two
are $$o(\varepsilon)$$:

$$\begin{align}
\det \left(I + \varepsilon\right) &= (1 + \varepsilon)^n + o(\varepsilon)\\
&= 1 + n\varepsilon + o(\varepsilon) + o(\varepsilon) \\
&= 1 + n\varepsilon + o(\varepsilon)
\end{align}$$
{: style="text-align: center"}

where the last line took advantage of the fact that two small things
(our $$o$$ terms) added together are still a small thing.

How do we generalize this to the case where our matrix has different values on the diagonal?
In that case, our $$n$$-fold product is a product of $$n$$ terms that look like

$$
(1 + \alpha\varepsilon)(1 + \beta\varepsilon) \dots
$$
{: style="text-align: center"}

and so there will be $$n$$ terms that are larger than $$o(\varepsilon)$$,
one for each element of the diagonal,
each scaled by the corresponding element of the diagonal.

So the total $$\varepsilon$$ term is _the sum of the diagonal values of the perturbation_.
Or, succinctly, it is the trace, $$\mathrm{tr}$$, of the perturbation.
This insight lets us write

$$\begin{align}
\det \left(I + M^{-1}\varepsilon\right)
&= 1 + \mathrm{tr}\left(M^{-1}\varepsilon\right) + o(\varepsilon)
\end{align}$$
{: style="text-align: center"}

This is very exciting!
The Fréchet strategy for finding expressions for derivatives of functions on matrices
is all about massaging until we get a trace-of-something-times-$$\varepsilon$$ term.
We're in the home stretch!

First, let's remind ourselves of where we were,
then slap in our expression above:

$$\begin{align}
\det(M + \varepsilon)
&= \det(M) \cdot \det\left(I + M^{-1}\varepsilon\right) \\
&= \det(M) \cdot \left(1 + \mathrm{tr}\left(M^{-1}\varepsilon\right) + o(\varepsilon)\right)
\end{align}$$
{: style="text-align: center"}

We now distribute that $$\det(M)$$ term across the remainder of the terms.
The first one is simple, it's a $$1$$.
In the second, we pull the determinant inside the trace,
making use of the fact that the trace is a linear functional (see for yourself!).
In the third, we recall that any concretely-sized thing times a small thing is small thing
($$\lambda o(\varepsilon) = o(\varepsilon)$$)
to simply disappear the determinant, leaving us with:

$$\begin{align}
\det(M + \varepsilon)
&= \det(M) + \mathrm{tr}\left(\det(M) M^{-1}\varepsilon\right) + o(\varepsilon)
\end{align}$$
{: style="text-align: center"}

Now, we're ready to pattern-match onto the definition of the Fréchet derivative:

$$
\det(M + \varepsilon) =
\det M + \mathrm{tr}\left(\nabla \det (M)^\top \varepsilon\right) + o(\varepsilon)
$$
{: style="text-align: center"}

On the left hand sides, we have the function evaluated at a point perturbed away
from $$M$$ by $$\varepsilon$$.
On the right hand sides, we have three terms.
The first term is equal to the function, evaluated at the point $$M$$.
The third term is $$o(\varepsilon)$$.
The second term is the trace of a matrix times $$\varepsilon$$.
Therefore, this second term contains the derivative.
We just need to transpose the matrix to get the final answer:

$$\begin{align}
\nabla \det M &= \det M \cdot \left(M^{-1}\right)^\top
\end{align}$$
{: style="text-align: center"}
