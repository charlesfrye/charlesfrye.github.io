---
layout: post
title:	Linear Algebra for Neuroscientists
date:	2017-08-17
category: math
---

![matrixmultneuralcircuit]
{: style="text-align: center"}

<!--exc-->
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

### The Importance of Linear Algebra

Linear algebra
is one of the most important workhorses
of applied mathematics.
Linear algebra is critical in
statistics,
optimization,
geometry,
and more.
It shows up whenever
we need to consider a collection of numbers
as a single object, lying in some space.

This is no less true of neuroscience
than of any other scientific field that uses math.
Even though neurons and neural circuits
have complex, non-linear behavior,
we need the tools of linear algebra
to describe that behavior.

Unfortunately,
despite its critical importance,
linear algebra just isn't *sexy*,
so it is often taught perfunctorily and
by someone who'd rather be teaching something else
to students who'd rather be learning something else.
The result is that a lot of folks
dislike linear algebra
and find it more of a confusing stumbling block
than a useful tool.

These notes are present a few core concepts
of linear algebra
-- vectors,
matrices, and
dot products --
in a manner that neuroscientists will find intuitive,
in an effort to clear away some of that confusion.

My hope is that this will encourage
folks to dive deeper into other resources
for learning linear algebra,
whether in a neuroscience context, like in
[Kenneth Miller's short textbook](https://www.neurotheory.columbia.edu/Ken/math-notes/)
or in a more general context, as in
[Khan Academy's online course](https://www.khanacademy.org/math/linear-algebra).
Readers interested in an intuitive presentation of the core ideas of linear algebra
should check out
[Grant Sanderson's YouTube Lectue Series](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab),
which pairs lucid explanations with slick animations and graphics.

### A Simple Neural Circuit

As our neural circuit model,
let's take the
[parallel fiber and Purkinje cell](http://charlesfrye.github.io/FoundationalNeuroscience/16/)
circuit of the cerebellum.
In order to get a clean, cartoon picture,
we focus on a collection of
three parallel fibers
(below, in shades of blue,
labeled A, B, and C)
that are all connected to each of
three Purkinje cells
(below, in shades of orange,
labeled 1, 2, and 3).

The dendritic inputs for the parallel fibers
enter from the right,
while the inputs of the Purkinje cells
enter from the left.
The axons of the parallel fibers turn
and pass over the dendrites of the Purkinje cells,
synapsing with each of them in turn
(indicated by colored circles).

![matrixmultneuralcircuit]
{: style="text-align: center"}

We will assume that these neurons form a
*linear system*.
What this means is that
if the input to a neuron is doubled,
or multiplied by any number,
the output of the neuron doubles,
or is multiplied by the same number.
Also,
if we measure the outputs of a neuron
for two inputs separately,
then we know the output of the neuron
for those two inputs together:
it is the sum of the outputs to the separate inputs.

The first property is called *scaling*
and the second property is called *superposition*.
Real neurons don't have either of these properties --
as a very rough example,
multiplying the input current to a neuron by seven million
won't cause the output to be seven million times greater,
it'll just fry the neuron --
but our cartoon neurons do.
This will make it possible for us to write down
precisely how our neurons behave.

### Experiment #1: Measuring the Behavior of a Single Neuron

Let's perform some thought experiments on our cartoon neurons.
First, let's place our imaginary
[recording electrode](http://charlesfrye.github.io/FoundationalNeuroscience/80/)
(indicated by two almost-intersecting lines, representing our pipette tip)
on Neuron 1 and shine our imaginary laser
(indicated by the green lightning bolts)
on Neurons A, B, and C,
which we've modified using
[optogenetics](https://en.wikipedia.org/wiki/Optogenetics)
so that they respond to light by increasing and decreasing their activity.
The experiment is depicted below.

![experiment1]
{: style="text-align: center"}

If we stimulate Neuron A to fire some unit amount,
we will measure some rate of firing from Neuron 1.
That is,
when Neuron A is firing at a rate of 1,
we measure some firing rate from Neuron 1.
Loosely speaking,
we can think of that value as a
*synaptic weight*
for the synapse between Neuron A and Neuron 1.
Our assumption that scaling inputs
causes outputs to scale by the same amount
(the *scaling* assumption)
lets us take that synaptic weight and use
it to predict the response of Neuron 1.

Indicating the firing rate of Neuron 1 as
$$fr_1$$, <!---_--->
the firing rate of Neuron A as
$$fr_A$$, <!---_--->
synaptic weight between
Neuron 1 and Neuron A
$$A_1$$, <!---_--->
we can write a simple equation
for the output of Neuron 1
as a function of the output of Neuron A:

$$
fr_1 = A_1 \cdot fr_A
$$ <!---_--->
{: style="text-align: center"}

Plugging in a few numbers will verify
that this equation is correct:
when Neuron A is not firing,
Neuron 1 won't fire,
and when Neuron A is firing at a rate of 1,
the firing rate of Neuron 1 is
$$A_1$$. <!---_--->

Notice the similarity of this equation
to the equation for a line of slope $$m$$
and intercept of 0:

$$
y = mx
$$

this is why our neurons are *linear* neurons:
if we graph their outputs as a function of their input,
we get a line!

We can repeat these measurements
for Neurons B and C,
and we will get three equations
that will let us predict
the output of Neuron 1
in response to separate
stimulation of each input neuron.

$$
fr_1 = A_1 \cdot fr_A \\
fr_1 = B_1 \cdot fr_B \\
fr_1 = C_1 \cdot fr_C \\
$$ <!---_--->
{: style="text-align: center"}

With these three equations,
we can now predict the output
of Neuron 1 in response to any input!
We just need to use our other assumption,
the *superpositioning* assumption.
That assumption told us
that the response of a neuron
to the combination of two inputs
is just the sum of the outputs
to the two inputs individually.
Written out mathematically,
in our case, that means:

$$
fr_1 = A_1 \cdot fr_A + B_1 \cdot fr_B + C_1 \cdot fr_C
$$ <!---_--->
{: style="text-align: center"}

We can shorten this equation substantially by writing

$$
fr_1 = \sum_i \text{i}_1 \cdot fr_i
$$

where $$i$$ goes from $$A$$ to $$C$$.

While these ways of writing
the behavior of our linear neuron work,
they have their flaws:
the first is too long,
while the second is too compressed.
We'd like a notation
that reminds us of which numbers are multiplied
with which,
but without all the extra $$+$$'s and $$\cdot$$'s.

If we write the seven relevant numbers
down on our picture of the circuit
(as seen below),
a geometric notation suggests itself:

$$
\left[\begin{array}{c}
A_1 & B_1 & C_1
\end{array}\right]
\left[\begin{array}{ccc}
fr_A \\ fr_B \\ fr_C
\end{array}\right] = fr_1
$$ <!---_--->
{: style="text-align: center"}

To compute the output,
we take a pair of elements in turn,
one from each of the boxes,
and multiply them together.
Once that is done,
we add the three results up
to get our output.
The result of each multiplication
is the component of the firing rate
of Neuron 1 that is due to the activity
of a single neuron,
and the superposition principle lets us add them all up.

The two boxes are called
*vectors*.
For obvious reasons,
the one on the left is a *row vector*
and the one on the right is a *column vector*.
We might call them
$$\vec{w}_1$$ ($$w$$ for "weight")
and $$\vec{fr}$$<!---_--->
The process of multiplying two vectors together
is variously known as

- the *dot product*, because it can be written
$$\vec{w}_1\cdot\vec{fr}$$<!---_--->
(notice the dot)
- the *scalar product*, since the result is a single number,
also known as a "scalar" because multiplying by a number "scales" things
- a *weighted sum*, since we multiply each element of $$\vec{fr}$$
by a weight from $$\vec{w}_1$$<!---_--->

These operations are ubiquitous in mathematics.
For example,
computing the average of a collection of $$N$$ numbers
is a multiplication of vectors:
one vector containing all of the numbers
and the other containing $$1/N$$ in each position.
In the greatest generality,
even derivatives and integrals are a form of
vector multiplication called a
[linear operator](https://www.encyclopediaofmath.org/index.php/Linear_operator).

Rather than diving deeper into those ideas,
we'll return to our neural circuit to see
how recording the outputs
of more than one neuron at a time
lead us to matrices.

### Experiment #2: Measuring the Behavior of Multiple Neurons

Under Construction!

[matrixmultneuralcircuit]: _img/matrixmultneuralcircuit.png
[experiment1]: _img/experiment1.png
[vectornotation]: _img/vectornotation.png
