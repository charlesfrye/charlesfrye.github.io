---
layout: post
title:	"AlphaFold-ed Proteins in W&B Tables"
date:	2021-07-29
category: external
---

[![alphafold_screenshot]](https://wandb.me/alphafold-report)
{: style="text-align: center"}

> As with tensors, so with proteins: it's all about the shapes. The genetic code specifies a sequence of chemicals, called amino acids, that are the building blocks of proteins, in turn the building blocks of biological systems. These linear sequences are transformed, millions of times per day per cell in your body, into complex 3D shapes, from molecular motors to molecular scissors, in a process known as protein folding.

<!--exc-->

Today I released a blog post
with my co-worker Stacey Svetlichnaya
detailing how to combine DeepMind's new AlphaFold
neural network for predicting protein structures
with W&B's new Tables tool for logging rich media
and metadata in structured form.

The post has interactive 3D molecular structure viewers,
a cool example of using AlphaFold on an important
protein from the visual system, rhodopsin,
and some nice prose (if I do say so myself!).

Read it [here](http://wandb.me/alphafold-report).

[alphafold_screenshot]: {{site.imgurl}}/alphafold_screenshot.png
