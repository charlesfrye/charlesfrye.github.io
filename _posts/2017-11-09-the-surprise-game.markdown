---
layout: post
title:	"The Surprise Game"
date:	2017-11-09
category: stats
---

$$\begin{align}
\mathbb{E}\left[ S(x) \right]
&= H(p) + D_{KL}\left(p \lvert\rvert q \right) + \log \frac{1}{\sum_{x \in \mathcal{X}} 2^{-S(x)}}
\end{align}$$ <!--_ -->
{: style="text-align: center"}

<!--exc-->

## Summary

Consider playing the following game:
you and I compete to be the least surprised
by some event whose outcome is uncertain.
Examples include
the next presidential election,
the images returned by a random English word input to Google,
the data produced by a scientific experiment,
the arrival time of a bus,
or the future prices of commodities.
For each possible outcome,
we both specify how surprised we'd be
if that outcome occurred,
then we "roll the dice",
as it were,
and the person who is less surprised
is the winner.

A number of questions immediately arise:
Who do we expect to win?
How do we pick an optimal strategy?
What is the performance of that strategy?
In the above examples,
the answers to these questions have immediate consequences for
e.g., election forecasting,
scientific modeling,
and optimal portfolio design.

In this post,
I'll provide a simple analysis of this game,
resulting in a surprising (ha!) answer:
resolving these questions
puts us on the trail of a central object
in probability theory,
statistics,
information theory,
physics, and
machine learning:
the Kullback-Leibler divergence.

## Detailing the Setup

Let's call the set of all possible outcomes $$\mathcal{X}$$.
Because outcomes are random,
this set comes with a function $$p$$,
called a $$p$$robability distribution,
that we can use to answer questions about
how likely certain outcomes are.
Neither player necessarily knows
the value of $$p$$.

Each player in the game must also provide a function,
which we'll call $$S$$,
for $$S$$urprise,
that indicates how surprising each outcome is.

A player seeking an easy way to win the game
might say
"I am not surprised by anything"
and choose the surprise function
$$S(x) = 0$$.
That game would be no fun at all,
so we add a rule to prevent it.
A player's surprise function must satisfy the following:

$$
\sum_{x \in \mathcal{X}} 2^{-S(x)} \leq 1 
$$ <!--_ -->
{: style="text-align: center"}

The choice of the number $$2$$ here isn't special;
it can be any positive number bigger than $$1$$.
Other folks might prefer $$10$$ or $$\mathrm{e}$$
or even $$\pi$$.

To see why this solves our problem
with folks claiming to never be surprised,
notice that if any $$S(x) = 0$$,
then $$2^{-S(x)} = 1$$ already,
so then $$2^{-S(x)} = 0$$ for
every other $$x$$.
Put another way:
if one outcome is totally unsuprising,
then every other outcome is infinitely surprising!
This condition helps enforce that our surprise function
has something to do with our belief in the chance
that a given outcome occurs.

In fact,
we can go even further.
We can use the sum from above
to normalize the values of $$2^{-S(x)}$$
so that they add up to one.
That is,
we can define a function $$q$$ by

$$
q(x) = \frac{2^{-S(x)}}{
\sum_{x \in \mathcal{X}} 2^{-S(x)}}
$$ <!--_ -->
{: style="text-align: center"}

and this function $$q$$
will sum to $$1$$ over the domain $$\mathcal{X}$$
and all of its entries will be greater than $$0$$,
since $$2^{-z}$$ is positive for all $$z$$.

Any function that adds up to $$1$$
over its entire domain
and has no negative values
can be interpreted as a probability distribution.
Below, we will see how this $$q$$
can be interpreted as the probability distribution
that the player believes governs the random outcomes.

## Analyzing the Game

Since the players don't directly interact,
the winner is determined simply by who has a lower score,
which we can analyze for just a single player.

If this game is to be played over multiple rounds,
(e.g. the scientific experiment takes more than one data point,
the bus arrival times are measured more than once)
then the score of each player,
which is a random number,
will quickly converge towards its average value times the number of rounds.
Therefore to determine the likely winner,
we just have to look at the expected value
of their score:

$$
\mathbb{E}\left[ S(x) \right] = \sum_{x \in \mathcal{X}} p(x) S(x)
$$ <!--_ -->
{: style="text-align: center"}

The expected value captures what the average "should" be:
if we're asked to guess what the average of a dataset
will turn out to be, the expected value gives the best guess,
in the sense that the distance from the measured value
to the expected value will, on average, be lower than
that from the measured value
to any other value.

The discussion above about $$q$$
involved negative exponentiated
surprise values,
and so if we want to make progress in that direction,
we'll need negative exponentiated surprises.
Our equation, though,
is in terms of surprises.
To get negative exponentiated surprises in there,
we just need to apply the inverse transformation:
a negative logarithm,
or logarithm of one over the value.
The negative logarithm of the negative exponentiated surprise
is just the surprise,
and so the equation still holds.

In math terms, we write:

$$\begin{align}
\mathbb{E}\left[ S(x) \right] &= \sum_{x \in \mathcal{X}} p(x) S(x) \\
&= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{1}{2^{-S(x)}} \right) \\
\end{align}$$ <!--_ -->
{: style="text-align: center"}

We want to be comparing these negative exponentiated surprise
values to probabilities,
in particular those given by the probability distribution $$p$$.
So we pull an old trick from the mathematicians' toolbox:
we multiply and divide by the same value,
which is the same as multiplying by $$1$$.
A judicious choice of value here can make all the difference.
For us, we choose to multiply the inside of the logarithm by
$$1 = \frac{p(x)}{p(x)}$$.

$$\begin{align}
\mathbb{E}\left[ S(x) \right] &= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{1}{2^{-S(x)}} \right) \\
&= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{p(x)}{p(x) 2^{-S(x)}} \right) \\
\end{align}$$ <!--_ -->
{: style="text-align: center"}


The nice thing about logarithms
is that we can take things that are multiplied together
or divided inside a logarithm and turn the expression
into a sum or difference of logarithms.
Now that our logarithm has some stuff in it,
we can contemplate splitting it apart.
We could either do the obvious thing,
splitting it over the fraction,
or we could do something slightly less obvious,
and split it over the product in the denominator.
Since we're clever, we choose the road less traveled.

$$\begin{align}
\mathbb{E}\left[ S(x) \right]
&= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{p(x)}{p(x) 2^{-S(x)}} \right) \\
&= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{1}{p(x)} \right) +
\sum_{x \in \mathcal{X}} p(x)\left(\log \frac{p(x)}{2^{-S(x)}} \right) \\
\end{align}$$ <!--_ -->
{: style="text-align: center"}


The term on the left is familiar:
it is the
[*entropy*]({{site.url}}/stats/2016/03/29/info-theory-surprise-entropy.html)
of the distribution $$p$$,
which we denote $$H(p)$$.

The term on the right is *almost* familiar:
if $$2^{-S(x)}$$ were a probability distribution,
then it'd be a Kullback-Leibler divergence
between $$p$$ and that distribution.

Nothing daunted,
we proceed to make $$2^{-S(X)}$$
into a distribution using the trick we came up with above:
we divide each element by the sum.
(Notice that this is the same trick,
the *softmax* that is pulled
in logistic regression or
in the final layer of a neural network performing classification.
This is not a coincidence.)
Dividing something so that it sums to a particular value
is called *normalization*,
so this is a *normalization factor*.
It is also called the *partition function*,
due to a connection to statistical physics.

Of course, dividing by the sum means
that we also need to multiply by the sum,
so that our equation still holds.
If we do this,
we get another logarithm,
which we can split apart again,
as below:

$$\begin{align}
\mathbb{E}\left[ S(x) \right]
&= \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{1}{p(x)} \right) +
\sum_{x \in \mathcal{X}} p(x)\left(\log \frac{p(x)}{2^{-S(x)}} \right) \\
&= H(p) + \sum_{x \in \mathcal{X}} p(x)\left(\log
\frac{p(x)}{\frac{2^{-S(x)}}{\sum_{x \in \mathcal{X}} 2^{-S(x)}}} \right) +
\sum_{x \in \mathcal{X}} p(x) \log\frac{1}{\sum_{x \in \mathcal{X}} 2^{-S(x)}}
\end{align}$$ <!--_ -->
{: style="text-align: center"}

We are now home free,
and just need to make a few small adjustments.
We introduce the (standard) notation
$$D_{KL}\left( p \lvert\rvert q\right)$$ <!--_ -->
for the second term, the Kullback-Leibler Divergence.
Further more,
we notice that the terms in the final sum,
don't actually depend on $$x$$: they are all the same, and are
equal to the logarithm of the normalizing factor.
Therefore we can pull them out of the sum,
which, because $$p$$ is a probabilty distribution,
is just equal to $$1$$.

$$\begin{align}
\mathbb{E}\left[ S(x) \right]
&= H(p) + \sum_{x \in \mathcal{X}} p(x)\left(\log \frac{p(x)}{q(x)} \right) +
\sum_{x \in \mathcal{X}} p(x) \log \frac{1}{\sum_{x \in \mathcal{X}} 2^{-S(x)}} \\
&= H(p) + D_{KL}\left(p \lvert\rvert q \right) +
\log \frac{1}{\sum_{x \in \mathcal{X}} 2^{-S(x)}} \sum_{x \in \mathcal{X}} p(x)  \\
&= H(p) + D_{KL}\left(p \lvert\rvert q \right) +
\log \frac{1}{\sum_{x \in \mathcal{X}} 2^{-S(x)}}\\
\end{align}$$ <!--_ -->
{: style="text-align: center"}

This is our final expression for the expected score
of a player with surprise function $$S$$.

## Breaking Down the Result

The KL divergence is never negative
and neither is the last term
(the sum inside the log is never greater than $$1$$).
Therefore, the best average performance that a player can have
is equal to $$H(p)$$.

This is one way of operationally defining the entropy.
For example, if we build a model of scientific data
by trying to minimize the surprise that the model reports
when presented with the data
(also known as *maximum likelihood modeling*),
then the entropy gives us a bound on how well we can perform.

The third term essentially penalizes us for having surprises
that don't exactly sum up,
once they are negatively exponentiated,
to $$1$$.
This is equivalent to saying that,
if we want to win this game,
the surprise should look like the negative logarithm
of something that sums to $$1$$,
aka a probability distribution.
This provides a mathematical characterization
of the surprise as the logarithm of one over the probability.
For more on this view, see
[this blog post]({{site.url}}/stats//2016/03/29/info-theory-surprise-entropy.html).

Finally, we have the divergence term.
The KL divergence is a measure of difference
between probability distributions.
How does it measure that difference?
From our equation above,
we can see that it quantifies how much worse
than optimal we perform in the surprise game
if we believe $$q$$ is true when $$p$$
is actually true.
This connects to its definition in terms of
[coding theory](http://colah.github.io/posts/2015-09-Visual-Information/),
where the "surprise" is measured in bits,
and our game is a competition to represent random objects
with the fewest bits.
Alternatively, it measures the speed at which we become certain
that samples from a distribution $$p$$
are from that distribution,
rather than a proposed alternative $$q$$.

The Kullback-Leibler Divergence is well-known for a
variety of reasons,
including, but not limited to

1. It appears, as it does here, in the objective function for
[maximum likelihood modeling](https://wiseodd.github.io/techblog/2017/01/26/kl-mle/)
1. It quantifies the
[highest rate at which a statistical test can distinguish hypotheses](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence#Discrimination_information)
1. Theorems involving its derivatives include
[the 2nd Law of Thermodynamics](http://www.mdpi.com/1099-4300/18/2/46/pdf)
and
[optimality of Bayesian inference](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5392554)
1. It appears in [Sanov's Theorem](https://blogs.princeton.edu/sas/2013/10/10/lecture-3-sanovs-theorem/),
which provides tight upper and lower bounds on the probabilities of [large deviations](https://en.wikipedia.org/wiki/Large_deviations_theory)
1. Its Hessian matrix is the [Fisher Information matrix](https://web.stanford.edu/class/stats311/Lectures/lec-09.pdf)
1. It quantifies one of the strictest forms of convergence,
making it
[an upper bound](https://en.wikipedia.org/wiki/Pinsker%27s_inequality)
on looser notions of convergence,
like convergence in
[Earth-Mover Distance](https://www.datadoghq.com/blog/engineering/robust-statistical-distances-for-machine-learning/),
[total variation](http://www.stat.yale.edu/~pollard/Courses/607.spring05/handouts/Totalvar.pdf), and
[distribution](https://en.wikipedia.org/wiki/Convergence_of_random_variables#Convergence_in_distribution)
1. It is the (essentially)
[unique divergence](http://ieeexplore.ieee.org/abstract/document/5290302/)
that is both a
[Bregman](http://mark.reid.name/blog/meet-the-bregman-divergences.html)
and an [$$f$$](https://en.wikipedia.org/wiki/F-divergence) divergence

I leave a full explication of these connections
to future blog posts.
