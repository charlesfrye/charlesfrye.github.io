---
layout: post
title:	"Hypothesis Testing in the Wild"
date:	2018-01-09
category: stats
---

>Apart from being an interesting exercise in the real-life uses of probability, this example, with its massive gap between the true negative rate and the negative predictive value, highlights the importance of thinking critically (and Bayesian-ly) about statistical evidence.

<!--exc-->

## Lies, Damn Lies, and Hypothesis Tests

Someone I know had a medical test done
in an attempt to confirm a diagnosis
strongly suggested by an earlier test.
This test came back negative,
out of alignmnent with both the other test
and with the clinical evidence.
In light of this,
the physician recommended
more expensive and invasive testing
to rule out alternatives.

I decided to look up this second test
to get a sense for its
[negative predictive value](https://en.wikipedia.org/wiki/Positive_and_negative_predictive_values).
The negative predictive value is the probability that an individual
who tests negative is a true negative, rather than a false negative.
If we use $$D$$ and $$N$$ to represent
the statements "the patient has the _D_isease"
and "the patient does _N_ot have the disease"
and $$+$$ and $$-$$ to represent the statements
"the patient's test is positive"
and "the patient's test is negative",
then the negative predictive value is the conditional probability
$$p(N\lvert -)$$.

If you want a refresher or introduction
to probabilities with a focus
on the ideas needed here, check out
[this post on conditional probability]({{site.url}}/stats/2016/02/04/bayes-rule.html)

Though I was able to track down
a paper on the second test,
I wasn't able to find the NPV directly.
Instead, the authors of the paper reported
two numbers called the *sensitivity*
and the *specificity*.
The sensitivity
is the conditional probability $$p(+\lvert D)$$,
the probability that a patient with the disease tests positive.
It is also known as the true positive rate.
The specificity is the conditional probability $$p(-\lvert N)$$,
and is therefore also known as the true negative rate.

The reported numbers for the sensitivity and specificity
looked quite good:
the sensitivity was around 70%
and the specificity was around 90%.
When you hear someone say
"90% of folks without the disease tested negative",
the immediate gut reaction is to infer
"someone with a negative result probably doesn't have the disease".

However, it is a grievous error to take $$p(-\lvert N)$$,
the specificity, and use it as though it were the same as
$$p(N\lvert -)$$, the negative predictive value.
The specificity is useful to the designers of tests,
who can calculate and control how the test performs
in cases where we know the underlying truth.
The users of tests, from physicians and their patients
to scientists and their peer revieweers, are instead interested in
the probability that a given individual has or does not have
the disease, given that their test was negative.

As an example of how these numbers can be different,
consider waking up in the morning to find that your room is dark.
Even though the probability that your room will be dark,
given that the sun has failed to rise in the morning,
is quite high (and so we have a high sensitivity),
there are many other, likelier causes for awakening in darkness,
like closed blinds,
and so the negative predictive value of awakening in darkness
for predicting whether the sun is shining is quite low.

As indicated by the above example,
the probability that the hypothesis being tested is true,
prior to the running of the test,
plays a crucial role in determining negative predictive value.
This is another reason why it is often not calculated during test design,
since the founders of the discipline were frequentists,
while the concept of a prior probability is Bayesian.
[See this blog post for more on that ideological difference in statistics]({{site.url}}/stats/2016/02/04/bayes-rule.html).

There are (at least) two methods for determining
what these prior probabilities should be.

1. We can combine the subjective information of the diagnosis
with the output of the previous test and
domain-specific knowledge to try to quantify
how strongly we believe, before running this second test,
that the patient has the disease.
2. We can collect data from similar cases
where we have access to the true status of the patient.
Then, we tally up the frequency that patients
similar to ours turned out to have the disease,
ignoring the output of this second test,
and use that ratio as the prior probability
for this patient.

Since the first approach is somewhat subjective,
we'll take the second for now.
Luckily, the paper describing the test
included enough information:
the true prevalence of the disease
in the population used to design the test,
all of whom had tested positive on the first test.

The numbers were extremely lopsided:
over 90% of individuals who tested positive on the first test
turned out to, in fact, have the disease.
This was perfectly in accordance with a mechanistic
understanding of the relationship between the first test
and the disease.

Using Bayes' Rule, we can combine this prior
with the likelihood given by the sensitivity and specificity
to compute the posterior distribution for positive and for negative results.
These posterior distributions then tell us the chance that the patient
does or does not have the disease,
given their results from the test.

The result was striking:
even if the patient tests negative on the second test,
there is still just under a 90% chance that they have the disease.
Put another way, the negative predictive value is (well) below 50%,
and so, if someone were to put a gun to your head
and demand you give an answer as to whether
the patient had the disease,
even a negative result on this test wouldn't
mean you should say they did not.
Going against a negative test result would only
result in your (fatal) loss in this game 10% of the time.
A positive result simply raises the probability
of disease presence from roughly 93% to about 97%
(perhaps reassuring in the gunman scenario).
This of course raises the question of just why this test
was run in the first place,
since its utility seems minimal.

Apart from being an interesting exercise
in the real-life uses of probability,
this example,
with its massive gap between the true negative rate
and the negative predictive value,
highlights the importance
of thinking critically (and Bayesian-ly)
about statistical evidence.

I should note that this second test is algorithmic,
while the first test was based on good-old-fashioned biology.
If we want our medicine to be evidence-based, data-driven,
computational, and empirical,
we have to take care to adhere to the rigors of correct inference.
