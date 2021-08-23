---
layout: post
title:	"Selectivity and Robustness of Sparse Coding Networks"
date:	2020-11-02
category: external
---

![robustness]
{: style="text-align: center"}

Adversarial attacks on neural networks
allow "hacking" of contemporary AI systems:
they can be easily convinced that a stop sign
is actually a toaster with the right (tiny!)
changes to the input.

Humans aren't so easily fooled,
and, as it turns out,
neither are some more biologically-plausible
but less popular approaches to neural networks,
like locally-competitive sparse coding networks.

I worked with
Dylan Paiton, Sheng Lundquist, Joel Bowen, Ryan Zarcone, and Bruno Olshausen
on understanding why.
There seem to be two basic, interrelated ingredients:
population non-linearities give more complex response functions
and generative models are harder to hack.

Check out
[the paper here](https://jov.arvojournals.org/article.aspx?articleid=2772000)
for details.

[robustness]: {{site.imgurl}}/robustness_banner.png
<!--exc-->
