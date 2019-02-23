---
layout: post
title:	"Multiplication Made Convoluted, Part I: Math"
date:	2019-02-20
category: math
---

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Well, actually, this is more right than you think: <br>A multiplication *is* a convolution of one multi-digit number by another one over the digit dimension.<br>Think about it.</p>&mdash; Yann LeCun (@ylecun) <a href="https://twitter.com/ylecun/status/1053719869005447168?ref_src=twsrc%5Etfw">October 20, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
{: style="text-align: center"}

<!--exc-->

### Introduction

Convolutions show up in many places:
in signal processing,
in probability,
and of course in the marriage of the two,
machine learning.
Check out
[this convolution tutorial]({{site.url}}/external/2016/03/27/convolutions.html)
for more details.
They are just as intimately related to such deep and powerful mathematics as
[the Central Limit Theorem]({{site.url}}/stats/2017/11/22/gaussian-diff-eq.html)
and
[the Fourier transform](https://www.khanacademy.org/math/differential-equations/laplace-transform/convolution-integral/v/the-convolution-and-the-laplace-transform)
as they are to
[understanding what happens when you shuffle cards](www.colah.github.io/posts/2014-12-Groups-Convolution).

The above tweets,
by satirical convolution fanatic
[@boredyannlecun](https://twitter.com/boredyannlecun)
and actual convolution fanatic
[actual Yann Le Cun](https://twitter.com/ylecn),
reveal an unexpected connection between convolutions
and the humble mutliplication operation.
I decided to work it out thoroughly and write it up.

In this blog post,
we'll work together through the _Think about it._ phase
by first thinking about how we normally do multiplication,
then generalizing it, and then deriving the convolutional form.

In a
[follow-up blog post]({{site.url}}/programming/2019/02/22/multiplication-convoluted-part-two.html),
we'll work through implementing a number type in Python
that actually makes use of this relationship to do multiplication.
Along the way, we'll learn how to hook into Python's built-in operators
with our own objects using what are called, almost without hyperbole,
_magic methods_,
and we'll see some benefits of thinking about our programs
using ideas from abstract algebra.

### The Multiplication Algorithm

Briefly, let's review the multiplication algorithm
we learned in school, taking care to emphasize some points
that will be salient later.

Imagine we wish to multiply two multi-digit numbers together,
e.g. $$123 \times 45$$:

$$\begin{array}{ccccc}
  &  & 1 & 2 & 3 \\
\times & & & 4 & 5 \\
\hline
  & & & & \text{?} \\
\end{array}$$
{: style="text-align: center"}

We proceed by multiplying 3 by 5, obtaining 15,
then writing a 5 in "the ones place" and "carrying the 1"
over to the next column, where we add it to the result of 2 times 5,
and so on.
When we have finished, we have multipled the number on top, 123,
by the first number on the bottom, 5, and obtained 615.

$$\begin{array}{ccccc}
  &  & 1 & 2 & 3 \\
\times & & & 4 & 5 \\
\hline
  &  & 6 & 1 & 5 \\
  &  &   &   & ? \\
\end{array}$$
{: style="text-align: center"}

Then we begin again:
we start writing the second row, but this time we put a 0 at the front.
Why do we do this?
Once again, we wish to mutiply the number on top by the number on the bottom,
but now "the number on the bottom" isn't just 4,
it's _40_.
Multiplying by 40 is the same as multiplying by 10 and then multiplying by 4,
and writing the 0 is our way of doing that first step.

$$\begin{array}{ccccc}
  &  & 1 & 2 & 3 \\
\times & & & 4 & 5 \\
\hline
  &  & 6 & 1 & 5 \\
+ & 4 & 9 & 2 & 0 \\
\hline
& 5 & 5 & 3 & 5
\end{array}$$
{: style="text-align: center"}

To get the final result, we add our intermediate results, columnwise.

With the benefit of several more years of math education,
we can write this algorithm compactly and concretely
using sums.

First we note that it makes use of the fact that we can think of a single number,
say $$v$$, as the sum of a sequence of smaller numbers, or digits,
multiplied by powers of 10:

$$\begin{align}
v &= \sum_k {\mathbf{v}_k \cdot 10 ^ k}
\end{align}$$
{: style="text-align: center"}

where an italic letter like $$v$$ always means a number, while a bold-faced letter like
$$\mathbf{v}$$ means the _sequence of digits_ that we use to represent the number.
To refer to a digit in that number, we use a subscript, as in $$\mathbf{v}_k$$.
Though this way of thinking feels so natural to us as to be unquestionable,
the idea of representing numbers this way had to be invented.
Indeed, Romans had to use a
[much more complicated algorithm](www.phy6.org/outreach/edu/roman.htm)
to multiply their numerals.

This notation in hand, we can write our multiplication algorithm
for pairs of multi-digit numbers as:

$$\begin{align}
z &= x \cdot y \\
&= \sum_j {x \cdot \mathbf{y}_j \cdot 10 ^ j}
\end{align}$$
{: style="text-align: center"}

notice that there are still multiplications inside the sum,
but they are now between a multi-digit number and a single-digit number
and between a multi-digit number and a power of ten,
which we have separate algorithms for.

### Rethinking the Multiplication Algorithm

In order to obtain our "convolution-style"
multiplication algorithm,
we need to reorganize our expression in terms of a different set of sums
and multiplications that result in the same final value.

First, we recognize that, for the sum over $$j$$, the value of $$x$$ is fixed,
so we can pull it out of the sum.
We then rewrite $$x$$ the same way we rewrote $$y$$,
i.e. as a sum over its digits:

$$\begin{align}
z &= x \cdot y \\
&= \sum_j x \cdot \mathbf{y}_j \cdot 10 ^ j \\
&= x\cdot \sum_j \mathbf{y}_j \cdot 10 ^ j \\
&= \sum_i \mathbf{x}_i \cdot 10 ^ i \cdot \sum_j \mathbf{y}_j \cdot 10 ^ j
\end{align}$$
{: style="text-align: center"}

Let's write these sums out for a pair of short numbers:
e.g. 123 and 45 as (100 + 20 + 3)(40 + 5).
It should be clear that instead of adding first and then multiplying,
we can just as well multiply everything first, then add
(FOIL style, if that acronym was used in your math education).
And if we do so, we'll have to multiply each value $$\mathbf{x}_i \cdot 10^i$$
by each value $$\mathbf{y}_j \cdot 10 ^ j$$.

That means we're free to write down any order of addition we want,
just so long as, once all of those additions are done, we've managed to include
all of the combinations $$\mathbf{x}_i$$ and $$\mathbf{y}_j$$.
Each order will correspond to a different choice of multiplication algorithm,
and the one we described first, corresponding to the one we were taught in school,
is just a particular, convenient choice for order of additions.

### Viva la Convolucion

To get "multiplication as convolution in the digit domain",
we choose the following ordering
(and simplify it with algebra in the second step):

$$\begin{align}
z &= \sum_i \mathbf{x}_i \cdot 10 ^ i \cdot \sum_j \mathbf{y}_j \cdot 10 ^ j\\
&= \sum_k \sum_{i+j=k} \mathbf{x}_i \cdot 10^i \cdot \mathbf{y}_j \cdot 10^j \\
&= \sum_k \sum_{i+j=k} \mathbf{x}_i \mathbf{y}_j \cdot 10^k
\end{align}$$
{: style="text-align: center"}

That is, we first split our pairs of $$i$$ and $$j$$ values according to
what $$i+j$$ equals (and we call that value $$k$$),
then we add up all of the products for each pair whose indices add to $$k$$,
and then finally we add up across all choices of $$k$$.
Mathematically, this is expressed by the notation $$i+j=k$$ at the bottom of the sum,
which means "over values of $$i$$ and $$j$$ such that they add together to equal $$k$$".

If the connection to convolutions isn't clear yet,
first take a look at the final line from above.
We've equated $$z$$ with a sum over an index of something
times powers of 10 to the index.
That's the same as our definition of the entries of $$\mathbf{z}$$!

Indeed, we can write

$$\begin{align}
\mathbf{z}_k &= \sum_{i+j=k} \mathbf{x}_i \mathbf{y}_j
\end{align}$$
{: style="text-align: center"}

which is one way of expressing
"$$\mathbf{z}$$ is the convolution of $$\mathbf{x}$$ and $$\mathbf{y}$$",
thought of as vectors.
This particular notation is non-standard, but
[more intuitive for me](www.colah.github.io/posts/2014-07-Understanding-Convolutions).
We can obtain something a bit more standard if we substitute $$j = k-i$$:

$$\begin{align}
\mathbf{z}_k &= \sum_{i} \mathbf{x}_i \mathbf{y}_{k-i}
\end{align}$$
{: style="text-align: center"}

which should look familiar!

### Convolution in the Digit Domain

Let's look at how this algorithm pans out in our example.

The convolution operation on a pair $$a,b$$ is sometimes described as
"reverse $$b$$, then slide it along $$a$$,
multiplying the values that align and adding up the results".

Below, I've drawn out this process for the "convolutional multiplication"
of 123 and 45.
A single line separates the two values being multiplied
from the running total of the result.
At each step, we multiply any numbers that line up
and add the results.
Double lines separate out iterations of the process:
when we cross a double line, 45 is "slid along" 123 by one increment.

$$\begin{array}{ccccc}
  & 1 & 2 & 3 &  \\
  &   &   & 5 & 4 \\
\hline
  &   &   & 15 & \\
\hline\hline
  & 1 & 2 & 3 &  \\
  &   & 5 & 4 &\\
\hline
  &   & 22 & 15 & \\
\hline\hline
  & 1 & 2 & 3 &  \\
  & 5 & 4 &   &\\
\hline
  & 13 & 22 & 15 & \\
\hline\hline
  & 1 & 2 & 3 &  \\
5 & 4 &   &   &\\
\hline
4 & 13 & 22 & 15 & \\
\end{array}$$
{: style="text-align: center"}

Notice that there's one tiny snag:
if $$\sum_{i+j=k} \mathbf{x}_i \cdot \mathbf{y}_j$$ is greater than 10 for a given $$k$$,
then $$\mathbf{z}_k$$ won't be a "digit", as we normally think of them
(we write 5535 for the answer to $$123 \times 45$$, not 4,13,22,15).
In fact, for something like $$x=5$$, $$y=3$$,
we end up with $$\mathbf{z}_0=15$$,
rather than $$\mathbf{z}_0=5, \mathbf{z}_1=1$$, as we'd like.
In
[the follow-up to this blog post]({{site.url}}/programming/2019/02/22/multiplication-convoluted-part-two.html),
where we implement a $$\texttt{DecimalSequence}$$ type in Python
that uses convolution to do multiplication,
we'll see a simple way to fix this problem
by decomposing the necessary "simplification" operation
into a pair of maps to and from the integers.
