---
layout: post
title:	"A Simple DNN for Identifying Mouse Sleep Stages"
date:	2019-12-13
category: external
---

![sleepscore]
{: style="text-align: center"}
<!--exc-->

Almost all animals sleep,
and sleep is deranged in a number of diseases.

When scientists want to do fundamental research on sleep,
they often study it in mice, which are easier, cheaper, and faster
to do experiments on than people or other primates.

One tricky bit of sleep research is knowing when the animal is asleep
and which phase of sleep it is in.

I consulted with some sleep biologists,
Yang Dan and Zeke Barger here at Berkeley,
on the design and testing of a simple convolutional network
that can be used to accurately identify
the sleep phase of mice from commonly-recorded electrical signals
(brain waves and neck muscle activity).

Check out
[the paper here](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0224642)
for details.

[sleepscore]: {{site.imgurl}}/sleepscore.png
