---
layout: post
title:	"Fréchet Derivatives 2: Linear Least Squares"
date:	2018-03-07
category: math
---

$$\begin{align}
\nabla_{W} L(W; x,y) = 2 (Wxx^\top - y x^\top)
\end{align}$$<!--_ -->
{: style="text-align: center"}

<!--exc-->

### Introduction

The Fréchet derivative makes the computation
of matrix- and vector-valued derivatives by hand
substantially less painful,
as discussed and demonstrated in
[the first blog post in this series]({{site.url}}/math/2018/03/06/frechet-derivative-introduction.html).

In this blog post,
we use the Fréchet approach to
compute derivatives for more interesting problems
than the simple cases considered in the first post:
two different flavors of least-squares regression.

In the first,
we consider single-output linear least squares,
also known as multiple linear regression,
and compute the derivative with respect to the parameter vector
across the entire dataset.
This is the typical derivative one computes
when trying to determine the solution
to a linear regression problem,
and results in the famed *normal equations*.

In the second,
we consider multiple-output linear least squares
and compute the derivative with respect to the parameter matrix
for a single data vector.
This serves as a warmup for
[the next blog post in this series]({{site.url}}/math/2019/01/18/frechet-linear-network.html),
which applies the Fréchet derivative to linear neural networks
with one hidden layer.


### Least Squares Linear Regression

If you are familiar with linear regression,
feel free to skip this part.
The notation used is fairly standard
and is re-introduced at the start of each derivation below.

Suppose we can measure two quantities:
height and weight,
input to a neuron and output of a neuron,
control signals to a robot and the resulting positions of the actuators.
We've captured a dataset of these quantities
and we'd like to determine how best to predict
one given the other (typically, something we cannot control or measure as easily,
called the _outputs_,
given something we can control or measure easily, the _inputs_).

Denoting our outputs as $$y$$ and our inputs as $$x$$,
we want a function $$f$$ that,
when applied each value of $$x$$ that we've collected,
produces an $$f(x)$$ that is "close",
in some sense, to the matching $$y$$.
This is known as *regression*.

Denote the function that tells us how close
a given $$f(x)$$ and $$y$$ are to each other with $$D$$,
for difference or distance.
If we write $$L$$ for the function that
takes in $$f$$, $$x$$, and $$y$$
and uses our function $$D$$
on $$y$$ and $$f(x)$$
to tell us how badly we've done
(pessimism is traditional in mathematics)
we can write the generic regression problem as

$$
L(f; x,y) = D\left(y, f(x)\right)
$$
{: style="text-align: center"}

where $$L$$ stands for *loss*.
We would like to make this number small.

When the notion of closeness is the squared difference
between the prediction and the true value,
this is known as *least squares regression*.
For least squares regression,
we use the notation $$\|\cdot\|^2$$
for "sum of squares"
to write our loss as

$$
L(f; x,y) = \|y-f(x)\|^2
$$
{: style="text-align: center"}

Without any more constraints, regression is easy and uninteresting:
for each $$x$$, define a function $$f(x)$$ that returns its $$y$$.
This passes muster for any sane choice for $$D$$.
While this works for any value of $$x$$ that is in our dataset,
it completely fails for any value not found in our data.

We make progress by restricting the possible kinds of functions $$f$$.
The simplest class of useful functions are the *linear* functions.
For scalar measurements and scalar outputs,
these functions are straight lines (hence the name).
Every linear function can be represented by
what a mathematician would call a *vector*,
i.e. an element of a vector space,
which corresponds to things other quantitative folks would call either
a scalar (in which case that scalar is known as the *slope*),
a vector, a matrix, a tensor, or a linear operator.
Denoting that abstract vector by $$\gamma$$,
we can compactly write
our prediction function $$f$$ as

$$
f(x) = \gamma x
$$
{: style="text-align: center"}

And write the problem of finding the best $$\gamma$$,
denoted $$\gamma^*$$ <!--* -->
for a dataset (x,y) as

$$
\gamma^* = \arg\min_\gamma L(\gamma; x, y) = \arg\min_\gamma \|y-\gamma x\|
$$<!--* -->
{: style="text-align: center"}

where $$\arg\min_v g(v)$$ means <!--_ -->
"the value of $$v$$ that minimizes $$g(v)$$".

$$\arg\min$$ is a function,
but it's a very weird function.
In general, computing $$\arg\min$$
can be made arbitarily hard.

The tried and true method for finding $$\arg\min g$$
for the vast majority of functions
is to look for places where the derivative
of the the function $$g$$ is $$0$$
(this fails if the function isn't differentiable
or has derivatives that are zero at places
with high values of $$g$$;
if that's the case, you're out of luck).

So to solve the problem of least squares linear regression,
which happens to be one without the troublesome properties
parenthetically mentioned above,
we need the derivative of the loss function.

In the following,
we will work out just what that derivative is
for two important special cases of least squares linear regression:
single-output multiple-input linear regression
and multiple-output multiple-input linear regression.

### Single-Output Linear Least Squares

In order to make our problem concrete,
we just need to pick shapes for $$x$$ and $$y$$.
The standard way to view least-squares problems
is to consider the loss over the entire dataset.
In this formulation,
we have $$n$$ column vectors from $$\mathbb{R}^k$$
representing our $$n$$ data points composed of $$k$$ dimensions,
which we call regressors,
which we collect into an $$n\times k$$ matrix
we call $$X$$, often called the *design matrix*
by statisticians
(we choose a capital $$X$$ to remind us this is a matrix).

In single-output linear regression,
our prediction targets $$Y_i$$ are one-dimensional entities (scalars).<!--_ -->
For consistency with the shape of our inputs,
we need to separate different data points across columns,
rather than across rows,
and so $$Y$$, the collection of all of our prediction targets,
is a $$1 \times N$$ row vector.

As a concrete example,
take $$X$$ to be a matrix where each column is the stimulus to a neuron
and $$Y$$ to be a row vector where each entry is the
activity level, or firing rate, in response to that stimulus.

We would like to use the same linear function on each
column of $$X$$ to predict the paired element in $$Y$$.
Linear functions that act on column vectors to produce scalars
are represented by transposed column vectors,
so we define our linear function as
the transpose of $$k$$ dimensional
column vector $$\beta$$ and apply it to $$X$$
to get our prediction

$$
\begin{align}
	\hat{Y} &= \beta^\top X
\end{align}
$$
{: style="text-align: center"}

Our squared error is then the sum of the squares
of the differences between the entries of $$\hat{Y}$$ and $$Y$$:

$$
\begin{align}
	R(\beta; X, Y) &= \|Y-\hat{Y}\| = \|Y - \beta^\top X\|
\end{align}
$$
{: style="text-align: center"}

Resulting in a loss function

$$
\begin{align}
	L(\beta; X, Y) &= \|Y-\hat{Y}\|^2 = \|Y - \beta^\top X\| \\
	&=\left(Y-\beta^\top X\right)\left(Y - \beta^\top X\right)^\top
\end{align}
$$
{: style="text-align: center"}

Note that this the sum of squares is computed as $$RR^\top$$,
rather than $$R^\top R$$,
because our residuals are row vectors
rather than column vectors.

Now that we have a concrete loss function,
we need a concrete expression for its derivative
with respect to $$\beta$$ so that we can
minimize it.
Enter the Fréchet derivative.

The Fréchet derivative of a scalar-valued function $$f$$
is a function $$\nabla f(x)$$ that satisfies

$$
\begin{align}
	f(x+\epsilon) &= f(x) + \langle \nabla f(x), \epsilon \rangle + o(\epsilon)
\end{align}
$$
{: style="text-align: center"}

That is, the derivative is some function of $$x$$
that can be used to build a linear approximation of $$f$$
at a point $$x+\epsilon$$
by means of an inner product.

For more on this expression
and why it is almost always equivalent to the expression
you learned in calculus but might be preferred,
see the
[the first blog post in this series]({{site.url}}/math/2018/03/06/frechet-derivative-introduction.html).
We make one small change from what is discussed there:
$$o(v)$$ for $$v$$ an (abstract) vector
is taken to be the same thing as $$o(\|v\|)$$,
a common substitution that lightens the burden of notation
without much risk of confusion.

Since we are looking for the derivative of $$L$$ with respect to our parameters $$\beta$$,
the quantity on the left-hand-side above looks, for our problem, like

$$
\begin{align}
	L(\beta+\epsilon; X, Y) &= \|Y-(\beta+\epsilon)^\top X\|^2 \\
	&= \left(Y-(\beta+\epsilon)^\top X\right)\left(Y-(\beta+\epsilon)^\top X\right)^\top \\
	&= \left(Y-(\beta+\epsilon)^\top X\right)\left(Y^\top-X^\top(\beta+\epsilon)\right)
\end{align}
$$
{: style="text-align: center"}

To make further progress,
we must expand out the product and then pattern match
on our definition:

$$
\begin{align}
	L(\beta+\epsilon; X, Y)
	&= \left(Y-(\beta+\epsilon)^\top X\right)\left(Y^\top-X^\top(\beta+\epsilon)\right) \\
	&= YY^\top + \beta^\top X X^\top \beta - \beta^\top X Y^\top - YX\beta^\top \\
	&\ \ \ \ \beta^\top X X^\top \epsilon + \epsilon^\top X X^\top \beta
	- \epsilon^\top X Y - Y X^\top \epsilon \\
	&\ \ \ \ + \epsilon^\top X X^\top \epsilon
\end{align}
$$
{: style="text-align: center"}

Consider the three lines in our expanded product.
The first is just $$L(\beta; X,Y)$$.
The final one is $$o(\epsilon)$$.
That means we need to massage the middle line until we have an inner product with $$\epsilon$$.

We focus in on the middle line and start by rearranging terms,
making use of the fact that the transpose of a scalar
is equal to that scalar,
and all of the terms in the equation above are scalar-valued:

$$
\begin{align}
	\beta^\top X X^\top \epsilon + \epsilon^\top X X^\top \beta
	- \epsilon^\top X Y - Y X^\top \epsilon
	&= 2\beta^\top X X^\top \epsilon - 2YX^\top\epsilon \\
	&= 2\left(\beta^\top X X^\top - YX^\top\right)\epsilon
\end{align}
$$
{: style="text-align: center"}

And in this final form,
the inner product is almost obvious:
we just need to take the transpose of the quantity
that is multiplied on the right by $$\epsilon$$:

$$
\begin{align}
	2\left(\beta^\top X X^\top - YX^\top\right)\epsilon &=
	\langle 2(XX^\top\beta - XY^\top) , \epsilon \rangle
\end{align}
$$
{: style="text-align: center"}

We then stuff this back into our original equation for
$$L(\beta+\epsilon; X, Y)$$ to obtain

$$
\begin{align}
	L(\beta+\epsilon; X, Y) &=
	L(\beta; X, Y)
	+ \langle 2(XX^\top\beta - XY^\top) , \epsilon \rangle
	+ o(\epsilon)
\end{align}
$$
{: style="text-align: center"}

and so we have that the derivative of $$L$$ with respect to $$\beta$$ is

$$
\begin{align}
	\nabla_\beta L &= 2\left(X X^\top \beta - XY^\top\right)
\end{align}
$$
{: style="text-align: center"}

If $$XX^\top$$ is invertible,
we can solve the above equation for $$\beta^*$$,<!--* -->
i.e. the best choice of $$\beta$$,
by setting the derivative equal to $$0$$:

$$
\begin{align}
	0 &= 2\left(X X^\top \beta^* - XY^\top\right) \\
	XY^\top &= XX^\top \beta^*\\
	\left(XX^\top\right)^{-1}XY^\top &= \beta^*
\end{align}
$$
{: style="text-align: center"}

Considered as a system of equations,
this last line is known as the *normal equations*.

### Multiple-Output Linear Least-Squares

Now we consider the case where,
instead of having a single value to predict,
we have an entire vector to predict.
In the list of examples that started this post,
this corresponds to the problem of predicting a robot's final state
(position/angle/velocity of each arm/leg/tentacle)
from the control parameters
(voltage to each servo/ray gun) we send it.

To demonstrate the utility of Fréchet derivative
in a new context and to help us connect better to the
upcoming neural network case,
we compute the derivative for the loss on a single pair $$x,y$$.
To get the derivative for the loss on the entire dataset,
we just sum (or average, to taste)
the derivatives for each pair.

Denoting our parameters by $$W$$,
for *weight matrix*,
the (neuroscience-inflected)
traditional term for the parameters
of a matrix-valued linear function,
we have that our predictions are

$$
f(W; x) = Wx
$$
{: style="text-align: center"}

The squared error $$L$$ of a matrix $$W \in \mathbb{R}^{j\times k}$$
applied to an input-output pair $$x,y$$ is

$$
L(W; x, y) = \|y - Wx \|^2
$$
{: style="text-align: center"}

where $$k$$ is the dimensionality of $$x$$
and $$j$$ is the dimensionality of $$y$$.

We can expand this out into

$$
\begin{align}
L(W; x, y) &= \|y - Wx \|^2 \\
&= \left(y - Wx \right)^\top \left(y - Wx \right) \\
&= y^\top y + x^\top W^\top W x
- 2 x^\top W^\top y
\end{align}
$$
{: style="text-align: center"}

We wish to compute the derivative
of $$L$$ with respect to $$W$$.
We therefore add a small matrix $$\epsilon$$
(how do we calculate the size of a matrix?
read
[this blog post]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html)
to see why it's just the sum of squares)
to $$W$$ and begin to compute:

$$
\begin{align}
L(W+\epsilon; x, y) &= \|y - (W+\epsilon)x \|^2 \\
&= y^\top y + x^\top (W+\epsilon) ^\top (W+\epsilon) x
- 2 x^\top (W+\epsilon) ^\top y \\
&= y^\top y + x^\top W^\top W x
- 2 x^\top W^\top y
+ 2 x^\top W^\top \epsilon x
- 2 x^\top \epsilon^\top y
+ x^\top \epsilon^\top \epsilon x \\
&= L(W; x, y)
+ 2 x^\top W^\top \epsilon x
- 2 x^\top \epsilon^\top y
+ o(\epsilon)
\end{align}
$$
{: style="text-align: center"}

Where the last line follows
by matching terms with the definition of the Fréchet derivative,
just as in the first derivation.
To determine the derivatives,
we need to rewrite the inner terms from the last line
as matrix inner products with $$\epsilon$$.

The inner product of two matrices $$M$$ and $$N$$,
is given by

$$
\langle M, N \rangle = \mathrm{tr}(M^\top N)
$$
{: style="text-align: center"}

where $$\mathrm{tr}$$ is the *trace*,
or sum of eigenvalues.
The same
[blog post about the size of matrices]({{site.url}}/math/2018/02/28/how-big-is-a-matrix.html)
also covers where this inner product comes from
and how it is connected to singular values.

Squinting at the middle terms from the
expanded loss function above,
we notice that there are no traces,
which prevents us from writing the matrix inner product anywhere.
Luckily,
we also notice that all of the terms in this equation are scalars,
and so are equal to their own traces.

We start with the first term,
dropping the extraneous factor of $$2$$.

$$
\begin{align}
x^\top W^\top \epsilon x &=
\mathrm{tr}\left(x^\top W^\top \epsilon x\right) \\
&= \mathrm{tr}\left(xx^\top W^\top \epsilon \right) \\
&= \mathrm{tr}\left(\left(Wxx^\top\right)^\top\epsilon\right)
\end{align}
$$
{: style="text-align: center"}

where the first equality follows from the fact that the trace
of a scalar is equal to that scalar,
the second follows from the fact that the trace
is
[invariant to cyclic permutations](https://math.stackexchange.com/questions/252272/is-trace-invariant-under-cyclic-permutation-with-rectangular-matrices),
and the last equality follows from the facts that the transpose
is self-inverting (an involution)
and that the transpose of a product
is the reverse of the product of the transposes.

The second term proceeds similarly:

$$
\begin{align}
x^\top \epsilon^\top y &= \mathrm{tr}\left(x^\top \epsilon^\top y \right) \\
&= \mathrm{tr}\left(\epsilon^\top y x^\top\right) \\
&= \mathrm{tr}\left(x y^\top \epsilon\right) \\
&= \mathrm{tr}\left((y x^\top)^\top \epsilon\right)
\end{align}
$$
{: style="text-align: center"}

And so the Fréchet derivative of $$L$$ is

$$
\begin{align}
\nabla_{W} L(W; x,y) = 2 (Wxx^\top - y x^\top)
\end{align}
$$<!--_ -->
{: style="text-align: center"}


This datapoint-by-datapoint expression is useful
for the case where the explicit solution is not easily computable,
e.g. when the number of columns in $$X$$ is in the billions,
as encountered in so-called "Big Data" problems.

### Closing

By means of the algebraic properties of the trace and tranpose,
we calculated the derivatives, aka gradients,
for two common flavors of linear regression:
the former common for cases where the explicit solution exists
and is computable
and the latter common for cases where it is not.

In the next blog post in this series,
we will apply what we learned in the course of this derivation
to a more interesting problem:
the deep linear neural network.
