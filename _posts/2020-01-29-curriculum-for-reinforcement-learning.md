---
layout: post
comments: true
title: "Curriculum for Reinforcement Learning"
date: 2020-01-29 18:00:00
tags: reinforcement-learning generative-model meta-learning
---


> A curriculum is an efficient tool for humans to progressively learn from simple concepts to hard problems. It breaks down complex knowledge by providing a sequence of learning steps of increasing difficulty. In this post, we will examine how the idea of curriculum can help reinforcement learning models learn to solve complicated tasks.
 

<!--more-->



It sounds like an impossible task if we want to teach integral or derivative to a 3-year-old who does not even know basic arithmetics. That's why education is important, as it provides a systematic way to break down complex knowledge and a nice curriculum for teaching concepts from simple to hard. A curriculum makes learning difficult things easier and approachable for us humans. But, how about machine learning models? Can we train our models more efficiently with a curriculum? Can we design a curriculum to speed up learning?
 
Back in 1993, Jeffrey Elman has proposed the idea of training neural networks with a curriculum. His early work on learning simple language grammar demonstrated the importance of such a strategy: starting with a restricted set of simple data and gradually increasing the complexity of training samples; otherwise the model was not able to learn at all.
 
Compared to training without a curriculum, we would expect the adoption of the curriculum to expedite the speed of convergence and may or may not improve the final model performance. To design an efficient and effective curriculum is not easy. Keep in mind that, a bad curriculum may even hamper learning. 

Next, we will look into several categories of curriculum learning, as illustrated in Fig. 1. Most cases are applied to Reinforcement Learning, with a few exceptions on Supervised Learning.


![Types of curriculum]({{ '/assets/images/types-of-curriculum-2.png' | relative_url }})
{: style="width: 100%;" class="center"}
*Fig. 1. Five types of curriculum for reinforcement learning.*


In "The importance of starting small" paper ([Elman 1993](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.128.4487&rep=rep1&type=pdf)), I especially like the starting sentences and find them both inspiring and affecting:
 
> "Humans differ from other species along many dimensions, but two are particularly noteworthy. Humans display an exceptional capacity to learn; and humans are remarkable for the unusually long time it takes to reach maturity. The adaptive advantage of learning is clear, and it may be argued that, through culture, learning has created the basis for a non-genetically based transmission of behaviors which may accelerate the evolution of our species."
 
Indeed, learning is probably the best superpower we humans have.
 


{: class="table-of-content"}
* TOC
{:toc}





