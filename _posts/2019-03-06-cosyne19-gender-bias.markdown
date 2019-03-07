---
layout: post
title:	"Tails You Win, One Tail You Lose"
date:	2019-03-06
category: stats
---

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">I presented the math for this at the <a href="https://twitter.com/hashtag/cosyne19?src=hash&amp;ref_src=twsrc%5Etfw">#cosyne19</a> diversity lunch today. <br><br>Success rates for first authors with known gender: <br>Female: 83/264 accepted = 31.4%<br>Male: 255/677 accepted = 37.7%<br><br>37.7/31.4 = a 20% higher success rate for men <a href="https://t.co/u2sF5WHHmy">https://t.co/u2sF5WHHmy</a></p>&mdash; Megan Carey (@meganinlisbon) <a href="https://twitter.com/meganinlisbon/status/1101870079858409478?ref_src=twsrc%5Etfw">March 2, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Controversy over hypothesis testing methodology encountered in the wild
[a second time]({{site.url}}/stats/2018/01/09/hypothesis-test-example.html)!
At this year's Computational and Systems Neuroscience conference, CoSyNE 2019,
there was disagreement over whether the acceptance rates indicated bias against women authors.
As it turns out, part of the disupute turned over which statistical test to run!
<!--exc-->

### Controversial Data

CoSyNe is
[an annual conference](http://cosyne.org/c/index.php?title=Cosyne_19)
where COmputational and SYstems NEuroscientists to get together.
As a conference in the intersection of two male-dominated fields,
concerns about gender bias abound.
Further, the conference uses single-blind review,
i.e. reviewers but not submitters are anonymous,
which could be expected to
[increase bias against women](https://doi.org/10.1177%2F1075547012472684),
though effects
[might be small](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5715744/).


During the welcome talk, the slide below was posted
(thanks to Twitter user
[@neuroecology](https://twitter.com/neuroecology) for sharing their image of the slide;
they have a nice
[write-up](https://neuroecology.wordpress.com/2019/02/27/cosyne19-by-the-numbers/)
data mining other CoSyNe author data)
to support the claim that bias was "not too bad",
since the ratio of male first authors to female first authors was about the same
between submitted and accepted posters.

![cosynegenderbias]
{: style="text-align: center"}

However, this method of viewing the data has some problems:
the real metric for bias isn't the final gender composition of the conference,
it's the difference in acceptance rate across genders.
A subtle effect there would be hard to see in data plotted as above.

And so Twitter user
[@meganinlisbon](https://twitter.com/meganinlisbon/)
got hold of the raw data and computed the acceptance rates and their ratio
in the following tweet:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">I presented the math for this at the <a href="https://twitter.com/hashtag/cosyne19?src=hash&amp;ref_src=twsrc%5Etfw">#cosyne19</a> diversity lunch today. <br><br>Success rates for first authors with known gender: <br>Female: 83/264 accepted = 31.4%<br>Male: 255/677 accepted = 37.7%<br><br>37.7/31.4 = a 20% higher success rate for men <a href="https://t.co/u2sF5WHHmy">https://t.co/u2sF5WHHmy</a></p>&mdash; Megan Carey (@meganinlisbon) <a href="https://twitter.com/meganinlisbon/status/1101870079858409478?ref_src=twsrc%5Etfw">March 2, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Phrased as "20% higher for men", the gender bias seems staggeringly high!

It seems like it's time for statistics to come and give us a definitive answer.
Surely math can clear everything up!

### Controversial Statistics

Shortly afterwards, several other Twitter users,
including
[@mjaztwit](https://twitter.com/mjaztwit/status/1101899788688257024)
and
[@alexpiet](https://twitter.com/alexpiet/status/1101882724581822465)
attempted to apply
[null hypothesis significance testing]({{site.url}}/stats/2018/06/09/hypothesis-testing.html)
to determine whether the observed gender bias was likely to be observed
in the case that there was, in fact, no bias.
Such a result is called _significant_,
and the degree of evidence for significance is quantified by a value $$p$$.
For historical reasons, a value of $$0.05$$ is taken as a threshold
for a binary choice about significance.

And they got different answers!
One found that the observation was _not significant_, with $$p \approx 0.07$$,
while the other found the the observation was _significant_, with $$p \approx 0.03$$.
What gives?

There were some slight differences in low-level, quantitative approach:
one was parametric, the other non-parametric.
But they weren't big enough to change the $$p$$ value.
The biggest difference was a choice made at a very high level:
namely, are we testing whether there was _any gender bias in CoSyNe acceptance_,
or are we testing whether there was more specifically _gender bias against women_.

The former is called a _two-tailed test_ and is more standard.
Especially in sciences like biology and psychology,
we don't know enough about our data to completely discount the possibility
that there's an effect opposite to what we might expect.

Because we consider extreme events "in both directions",
the typical effect of switching from a two to a one-tailed test
is to cut the $$p$$-value in half.
And indeed, we $$0.03$$ is approximately half of $$0.07$$.

But is it reasonable to run a two-tailed test for this question?
The claims and concerns of most of the individuals concerned about bias
was framed specifically in terms of female-identifying authors
(to my recollection, choices for gender identification were
_male_, _female_, and _prefer not to answer_, making it impossible to talk
about non-binary authors with this data).
And given the other evidence for misogynist bias in this field
(the undeniably lower rate of female submissions,
the near-absence of female PIs,
the still-greater sparsity of women among top PIs)
it would be a surprising result indeed if there were bias
that favored women in just this one aspect.
Suprising enough that only very strong evidence would be sufficient,
which is approximately what a two-tailed test does.

Even putting this question aside,
is putting this much stock in a single number like the $$p$$ value sensible?
After all, the $$p$$ value is calculated from our data,
and it can fluctuate from sample to sample.
If just two more female-led projects had been accepted or rejected,
the two tests would agree on which side of $$0.05$$ the $$p$$ value lay!

Indeed, the CoSyNe review process includes _a specific mechanism for randomness_,
namely that papers on the margin of acceptance due to a scoring criterion
have their acceptance or rejection determined by the output of a random number generator.

And the effect size expected by most is probably not
too much larger than what is reported,
since the presumption is that the effect is mostly implicit bias from many reviewers
or explicit bias from a small cohort.
In that case, adhering to a strict $$p$$ cutoff is electing to have your conclusions
from this test determined _almost entirely by an explicitly random mechanism_.
This is surely fool-hardy!

It would seem to me that the more reasonable conclusion is that
there is moderately strong evidence of a gender bias in the 2019 CoSyNe review process,
but that the number of submissions is insufficient to make a definitive determination possible
based off of a single year's data.
This data is unfortunately not available for previous years.

### Coda

At the end of the conference,
the Executive Committee announced that they had heard the complaints
of conference-goers around this bit of gender bias and others
and would be taking concrete steps to address them.
First, they would be adding chairs for Diversity and Inclusion to the committee.
Second, they would move to a system of double-blind review,
in which the authors of submissions are also anonymous to the reviewers.
Given the absence of any evidence that such a system is biased against men
and the
[evidence that such a system reduces biases in general](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5715744/),
this is an unambiguously good move,
regardless of the precise $$p$$ value of the data for gender bias this year.

[cosynegenderbias]: {{site.imgurl}}/cosynegenderbias.jpg
