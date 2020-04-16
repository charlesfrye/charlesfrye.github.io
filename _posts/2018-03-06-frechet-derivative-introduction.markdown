---
layout: post
title:	"Fréchet Derivatives 1: Introduction"
date:	2018-03-06
category: math
---

$$\begin{align}
f(x+\epsilon) = f(x) + \langle \nabla_x f(x), \epsilon \rangle+ o(\|\epsilon\|)
\end{align}$$<!--_ -->
{: style="text-align: center"}

<!--exc-->

## Summary

Derivatives with respect to vectors and matrices
are generally presented in a symbol-laden,
index- and coordinate-dependent manner.
The Fréchet derivative provides an alternative
notation that leads to simple proofs
for polynomial functions,
compositions and products of functions, and more.
Here, we work through the definition of the Fréchet derivative
and its application in a few fundamental examples.
In
[the next blog post in this series]({{site.url}}/math/2018/03/07/frechet-least-squares.html).
we will work out the derivative of least-squares linear regression
for multiple inputs and outputs (with respect to the parameter matrix),
then apply what we've learned to calculating
[the gradients of a fully linear deep neural network]({{site.url}}/math/2019/01/18/frechet-linear-network.html).

For a quick intro video on this topic,
check out
[this recording of a webinar](https://youtube.com/watch?v=QvJ544dBvRg)
I gave, hosted by
[Weights & Biases](https://wandb.com).

###  The Fréchet Derivative is an Alternative but Equivalent Definiton

In calculus class, the derivative is usually introduced as a limit:

$$
f^\prime(x) = \lim_{\epsilon\rightarrow0} \frac{f(x+\epsilon)-f(x)}{\lvert x+\epsilon-x\rvert}
$$<!--_ -->
{: style="text-align: center"}

which we interpret as the limit of the "rise over run"
of the line connecting the point $$(x,f(x))$$
to $$(x+\epsilon,f(x+\epsilon))$$.

This same expression can be re-written as

$$
f(x+\epsilon) = f(x) + f^\prime(x)\epsilon + o(\epsilon)
$$
{: style="text-align: center"}

where "$$o(\epsilon)$$" means
"something whose ratio with $$\epsilon$$ has limit $$0$$".
The $$o$$ is pronounced "little-o".
For example, $$x^2$$ is $$o(x)$$.
If you are familar with the so-called "Big-O" notation,
you can think of Big-O as meaning "less than or equal to, up to constants"
and little-o as meaning "strictly less than, up to constants".
See
[these notes](http://www.stat.cmu.edu/~cshalizi/uADA/13/lectures/app-b.pdf)
for more details.

Ignoring the subtleties of little-o,
we can read the above expression as saying:
"we can locally approximate the function $$f(x)$$ around $$x$$
as just a line with slope $$f^\prime(x)$$",
which should sound familiar.

This slight rearrangement makes our lives a bit easier
when we move from our calculus class to our multivariate calculus class.
There, we need to consider functions that have vectors and matrices as inputs,
resulting in derivative functions $$f^\prime$$
that return vectors and matrices, rather than just scalars.
These are frequently called *gradients* rather than derivatives.
For functions that take vectors as inputs and returns scalars,
we can calculate the gradient vector of $$f$$ at $$x$$,
usually denoted $$\nabla_xf(x)$$, <!--_ -->
as:

$$
f(x+\epsilon) = f(x) + \nabla_xf(x)^\top\epsilon + o(\|\epsilon\|)
$$ <!--_ -->
{: style="text-align: center"}

The motivation usually offered for this expression is that
the gradient vector is just a vector of partial derivatives:
just compute the derivative of $$f(x)$$ if we vary only one of the coordinates,
rather than all of them, and then collect the derivatives into a vector.
The total change is just the sum of the change caused by each derivative,
and so we get the expression above, where those changes and their sum
are calculated in the inner product.

This will end up giving us major headaches when we move to matrices,
and anyways does not generalize to infinite-dimensional vectors,
like functions.
Rather than relying on coordinates and indices,
we can instead motivate ourselves by the linear approximation argument.
If the derivative of a scalar function gives us a line,
determined by a linear function of one variable,
that approximates $$f(x)$$,
then the derivative of a scalar function should give us some kind of
line-equivalent object that approximates $$f(x)$$
by means of a linear function of multiple variables.

Linear functions of multiple variables correspond precisely with
transposed vectors (row vectors, in the mathematical convention).
The "line-equivalent object" is a *plane*
(in two dimensions; in higher dimensions the term of art is *hyperplane*).
That is, we locally approximate $$f(x)$$ around $$x$$ as a plane
by applying the linear function $$\nabla_xf(x)^\top\epsilon $$ <!--_ -->
to our small changes $$\epsilon$$.

But what if our input is a matrix?
Taking a matrix transposed times a matrix
does not generally give a scalar value.

If we re-examine the argument above,
we realize that we didn't need "something transpose times something",
we needed "a linear function on $$\epsilon$$ that outputs a scalar".
Such a linear function is called a  "linear functional".
For a vector, a linear functional is determined by some vector,
which we write transposed to remind ourselves it's a function,
not an input to a function.
For a generic element of a vector space,
which can be, e.g. a matrix or a function or a scalar,
linear functionals are given by the *inner product*
with a vector from that space
(at least, in the cases we are considering).
So we can write a form of the derivative
that works for all these cases at once:

$$
f(x+\epsilon) = f(x) + \langle \nabla_x f(x), \epsilon \rangle + o(\|\epsilon\|)
$$<!--_ -->
{: style="text-align: center"}

this is the *Fréchet derivative*
for functions of vectors that return scalars.
For the fully generic form,
which does not, in the end, require an inner product,
see
[this blog post](https://wj32.org/wp/2013/02/21/differentiation-done-correctly-1-the-derivative/)
for a mathier take,
[this blog post](http://michael.orlitzky.com/articles/the_derivative_of_a_quadratic_form.xhtml)
for a more careful comparison to what's usually taught,
and
[this blog post](https://alok.github.io/2017/08/02/matrix-calculus/)
for a polemic and a quick example.

You may be wondering what the inner product of two matrices is.
The answer turns out to be

$$
\langle X, Y \rangle = \mathrm{tr}\left(X^\top Y\right)
$$
{: style="text-align: center"}

for some very interesting reasons
connected to singular values and the vectorization operator that I explore in
[this blog post]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html).

### Simple Examples

#### Scalar Inputs

Consider the quadratic function

$$
f(x) = x^2
$$
{: style="text-align: center"}

Can we compute its derivative with our new expression?
We begin by writing out $$f(x+\epsilon)$$
and then trying to write it in terms that look like
our expression for the Fréchet derivative.

$$\begin{align}
f(x+\epsilon) &= (x+\epsilon)^2 \\
&= (x+\epsilon)(x+\epsilon) \\
&= x^2 + 2\epsilon x + \epsilon^2\\
&= f(x) + 2x\epsilon + o(\epsilon)\\
&= f(x) + \langle 2x, \epsilon\rangle + o(\epsilon)\\
\end{align}$$
{: style="text-align: center"}

where in the second to last line I used the fact that
$$\epsilon^2 = o(\epsilon)$$.
And so the derivative is $$2x$$, as hoped.
Note that this isn't the simplest function we could've chosen.
Try for yourself on the even simpler functions
$$f(x) = \lambda x$$
and
$$f(x) = c$$.
Once you've done that,
consider polynomials
$$f(x) = \sum_i x^i$$. <!--_ -->


#### Vector Inputs

So far, so done in Calc I.
Let's kick it up a notch to
the vector version of the quadratic function above:

$$
f(x) = x^\top Q x
$$
{: style="text-align: center"}

also known as the *quadratic form* $$Q$$.
The answer again proceeds by writing out
$$f(x+\epsilon)$$ and rearranging:

$$\begin{align}
f(x+\epsilon) &= (x+\epsilon)^\top Q (x+\epsilon) \\
&= x^\top Q x + \epsilon^\top Q x + x^\top Q \epsilon + \epsilon^\top Q \epsilon \\
&= f(x) + x^\top Q^\top \epsilon + x^\top Q \epsilon + o(\|\epsilon\|)
\end{align}$$
{: style="text-align: center"}

where in the last line, in addition to pattern matching
with the definition of the derivative,
we used the fact that all of the values are scalar and so equal to their own transpose.
We then pull out the factor of $$x^\top$$
and recognize a vector inner product:

$$\begin{align}
f(x+\epsilon) &= f(x) + x^\top Q^\top \epsilon + x^\top Q \epsilon + o(\|\epsilon\|) \\
&= f(x) + x^\top(Q + Q^\top)\epsilon + o(\|\epsilon\|) \\
&= f(x) + \langle (Q + Q^\top)x, \epsilon\rangle + o(\|\epsilon\|)
\end{align}$$
{: style="text-align: center"}

to establish that the derivative is
$$(Q+Q^\top)x$$.

#### Matrix Inputs

Now let's instead think of $$f$$
above as a function of $$Q$$,
rather than of $$x$$:

$$
f(Q) = x^\top Q x
$$
{: style="text-align: center"}

Computing this derivative coordinatewise is an exercise in the kind of
masochism and minutia-wrangling that turns off a multitude of bright young folks
from quantitative approaches every time a math class is taught badly.
Let's see what happens when we try it:

$$\begin{align}
f(Q+\epsilon) &= x^\top (Q+\epsilon) x \\
&= x^\top Q x+ x^\top \epsilon x \\
&= f(Q) + x^\top \epsilon x + o(\|\epsilon\|)
\end{align}$$
{: style="text-align: center"}

where in the last line we made use of the fact that
$$0$$ is $$o(\|\epsilon\|)$$,
and the norm $$\|\cdot\|$$ is the Frobenius norm,
aka the square root of the sum of squared entries,
as discussed in
[this blog post]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html).

We're almost at the end,
but before we complete our derivation,
I'd like to point out that we've already learned something
about this function:
our little-o term is *always $$0$$*,
meaning that our linear "approximation" is
*exact* -- nothing approximate about it.
This fact is usually somewhat obscured by the profusion of symbols
that accompanies the standard method.

What this method does require
is familiarity with the *algebra* part of linear algebra:
with transposes, traces, and the like.
For the symbolically minded, this is a huge relief.
In our case,
we need to recognize that standing in between
our current expression
and a matrix inner product
is the absence of a trace.
Realizing that the trace of a scalar is a scalar,
our quantities are scalars,
and traces are
[invariant to cyclic permutations](https://math.stackexchange.com/questions/252272/is-trace-invariant-under-cyclic-permutation-with-rectangular-matrices),
we discover

$$\begin{align}
f(Q + \epsilon) &= f(Q) + x^\top \epsilon x + o(\|\epsilon\|) \\
&= f(Q) + \mathrm{tr}(x^\top \epsilon x) + o(\|\epsilon\|) \\
&= f(Q) + \mathrm{tr}(x x^\top \epsilon) + o(\|\epsilon\|) \\
&= f(Q) + \langle x x^\top, \epsilon\rangle + o(\|\epsilon\|) \\
\end{align}$$
{: style="text-align: center"}

indicating that the derivative is $$xx^\top$$.

### Closing

If you are not yet convinced of the utility of the method of Fréchet derivatives
for computing gradients,
check out
[part two of this series]({{site.url}}/math/2018/03/07/frechet-least-squares.html),
which will apply the Fréchet derivative
to the problem of linear least squares
and produce the expected result with a minimum of fuss.
