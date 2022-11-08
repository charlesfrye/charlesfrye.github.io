---
layout: post
title:	"Lecture on Troubleshooting & Testing"
date:	2022-08-22
category: external
---

<div align="center">
<iframe width="720" height="405" src="https://www.youtube-nocookie.com/embed/RLemHNAO5Lw?list=PL1T8fO7ArWleMMI8KPJ_5D5XSlovTW_Ur" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<!--exc-->

As part of the
[new and improved 2022 edition of Full Stack Deep Learning](https://fullstackdeeplearning.com/course/2022/),
I delivered the lecture on troubleshooting and testing.

You can find the lecture notes on the course website
[here](https://fullstackdeeplearning.com/course/2022/lecture-3-troubleshooting-and-testing/).

On this blog, I wanted to write a bit about why this topic --
which most people find a bit dull --
is so important to me.

My intellectual background is not in software engineering.

It's in the sciences of organic intelligences and neurons,
in
[psychology and biology]({{site.url}}/research).

That foundation in the soft, wet sciences has stuck with me.
When I wrote about understanding a PyTorch training step a year ago, I
[used the metaphor of a dissection]({{site.url}}/external/2021/08/24/trace-report.html).

One thing that drove me to leave those disciplines
for math and computer science was the constant feeling that
I was missing something:
that some apparatus was broken or some effect was not controlled
for and my findings were false or misleading.

It doesn't help that
[meta-analyses show](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1182327/)
my fears are in fact justified 😬

In that context,
the promise of rigor & technical correctness
in software engineering and mathematics was seductive.

My [first ever paper in ML](https://arxiv.org/abs/1901.10603)
was actually just on describing and running a "unit test"
for algorithms that find critical points of the loss, and
my dissertation work,
[summarized on Twitter here](https://twitter.com/charles_irl/status/1242294753787609088),
was on running an extension of that test against some
published algorithms and understanding why they failed.

So I love tests, I love rigor, and I think it's incredibly important.

But since leaving academia for the world of engineering --
even my teaching-centric corner of it --
I've also learned that making a piece of technology
that works isn't just about correctness and rigor, narrowly understood.

In building a new technology,
we don't often have the benefit of a well-specified problem,
on which we can bring rigor to bear.

So we marry what we can of rigor and correctness with
adaptability, measurement, iteration, and humility.

The chance to share that perspective,
alongside actionable and specific recommendations and techniques,
is why this talk is so exciting to me.

The key ideas are independent of the previous lectures and labs in the course,
and you can catch it
[here](https://youtu.be/RLemHNAO5Lw?list=PL1T8fO7ArWleMMI8KPJ_5D5XSlovTW_Ur).
