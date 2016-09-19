---
layout: article
title: Why Should I Normalize My Data?
modified:
categories: devlogs
excerpt:
tags: [machine learning, data preparation, 0-1, normalization, explanations]
image:
  feature: north.jpg
  teaser: binary-code.jpg
  thumb: binary-code.jpg
date: 2016-09-19T13:41:54-07:00
---

Let's start at the beginning. 

## What is data normalization?

To begin with, we have a potential for confusion. Mathematicians, ever so good at naming things, use this word to describe three very different unrelated operations. This has caused me confusion in the past, so I wanted to lay it all down. 

### 1: Vector Normalization

Vector normalization is my favorite of the three because it has a regular name that is used to distinguish it from the other two, so I will dwell on it the least. Lots of computer programming applications will require a handling of vectors, one-dimensional matrices of values referring to a single element. This is visualized in the context of a physics vector, a thing in the real world that has motion in some number of dimensions, but it could be anything. You might have a vector with the number of people in your company in one dimension, and the size of the front door on another. You probably won't learn from those values, but you can. 

If a correllation exists between values on two dimensions, it is usually fair to assume that it does not depend on magnitude. Let's say that there exists a correllatin betwen door size and number of employees. You don't care about the particular size of each data point, that's just noise and a waste of resources. What you want is the relationship between them, which is encapsulated in the *direction* that your vector is pointing. Not on its size. 

![Vector diagram]({{ site.url }}/images/devlogs/cosines.gif)

In this diagram, we don't care about u. What we want to learn about are α, β, and γ. Working with angles is literally hell, so we use *unit vectors*, vectors with a magnitude of 1 as a proxy. Normalization is transforming a vector to have magnitude 1 while maintaining its original directionality. 

When you want to capture relationships between dimensions, rather than between data points, this is the way to go. But it's not what we're talking about here. 


### 2. Proportional Normalization

Some machine learning methods and algorithms require inputs or (more often) initial parameters to have a probability distribution. i.e., when taken together, they must sum to 1. Why this is necessary may be algorithm or implementation specific. 

For example, at every point in its life, every transition vector and every emission vector in a Hidden Markov Model must sum to 1, because it is a probabilistic machine. 

![My thanks to Evan Wallace]({{ site.url }}/images/devlogs/hmm.png)

<div>
$$ x = 24 $$
</div>