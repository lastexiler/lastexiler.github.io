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
 




## Task-Specific Curriculum

[Bengio, et al. (2009)](https://www.researchgate.net/profile/Y_Bengio/publication/221344862_Curriculum_learning/links/546cd2570cf2193b94c577ac/Curriculum-learning.pdf) provided a good overview of curriculum learning in the old days. The paper presented two ideas with toy experiments using a manually designed task-specific curriculum:
1. Cleaner Examples may yield better generalization faster.
2. Introducing gradually more difficult examples speeds up online training.

It is plausible that some curriculum strategies could be useless or even harmful. A good question to answer in the field is: *What could be the general principles that make some curriculum strategies work better than others?* The Bengio 2009 paper hypothesized it would be beneficial to make learning focus on "interesting" examples that are neither too hard or too easy.

If our naive curriculum is to train the model on samples with a gradually increasing level of complexity, we need a way to quantify the difficulty of a task first. One idea is to use its minimal loss with respect to another model while this model is pretrained on other tasks ([Weinshall, et al. 2018](https://arxiv.org/abs/1802.03796)). In this way, the knowledge of the pretrained model can be transferred to the new model by suggesting a rank of training samples. Fig. 2 shows the effectiveness of the `curriculum` group (green), compared to `control` (random order; yellow) and `anti` (reverse the order; red) groups.


![Curriculum by transfer learning]({{ '/assets/images/curriculum-by-transfer-learning.png' | relative_url }})
{: style="width: 100%;" class="center"}
*Fig. 2. Image classification accuracy on test image set (5 member classes of "small mammals" in CIFAR100). There are 4 experimental groups, (a) `curriculum`: sort the labels by the confidence of another trained classifier (e.g. the margin of an SVM); (b) `control-curriculum`: sort the labels randomly; (c) `anti-curriculum`: sort the labels reversely; (d) `None`: no curriculum. (Image source: [Weinshall, et al. 2018](https://arxiv.org/abs/1802.03796))*

[Zaremba & Sutskever (2014)](https://arxiv.org/abs/1410.4615) did an interesting experiment on training LSTM to predict the output of a short Python program for mathematical ops without actually executing the code. They found curriculum is necessary for learning. The program's complexity is controlled by two parameters, `length` ∈ [1, a] and `nesting`∈ [1, b]. Three strategies are considered:
1. Naive curriculum: increase `length` first until reaching `a`; then increase `nesting` and reset `length` to 1; repeat this process until both reach maximum.
2. Mix curriculum: sample `length` ~ [1, a] and `nesting` ~ [1, b]
3. Combined: naive + mix.

They noticed that combined strategy always outperformed the naive curriculum and would generally (but not always) outperform the mix strategy --- indicating that it is quite important to mix in easy tasks during training to *avoid forgetting*.
 

 
To follow the curriculum learning approaches described above, generally we need to figure out two problems in the training procedure:
1. Design a metric to quantify how hard a task is so that we can sort tasks accordingly.
2. Provide a sequence of tasks with an increasing level of difficulty to the model during training.

However, the order of tasks does not have to be sequential. In our Rubik's cube paper ([OpenAI et al, 2019](https://arxiv.org/abs/1910.07113.)), we depended on *Automatic domain randomization* (**ADR**) to generate a curriculum by growing a distribution of environments with increasing complexity. The difficulty of each task (i.e. solving a Rubik's cube in a set of environments) depends on the randomization ranges of various environmental parameters. Even with a simplified assumption that all the environmental parameters are uncorrelated, we were able to create a decent curriculum for our robot hand to learn the task.



## References

[1] Jeffrey L. Elman. ["Learning and development in neural networks: The importance of starting small."](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.128.4487&rep=rep1&type=pdf) Cognition 48.1 (1993): 71-99.

[2] Yoshua Bengio, et al. ["Curriculum learning."](https://www.researchgate.net/profile/Y_Bengio/publication/221344862_Curriculum_learning/links/546cd2570cf2193b94c577ac/Curriculum-learning.pdf) ICML 2009.

[3] Daphna Weinshall, Gad Cohen, and Dan Amir. ["Curriculum learning by transfer learning: Theory and experiments with deep networks."](https://arxiv.org/abs/1802.03796) ICML 2018.

[4] Wojciech Zaremba and Ilya Sutskever. ["Learning to execute."](https://arxiv.org/abs/1410.4615) arXiv preprint arXiv:1410.4615 (2014).

[5] Tambet Matiisen, et al. ["Teacher-student curriculum learning."](https://arxiv.org/abs/1707.00183) IEEE Trans. on neural networks and learning systems (2017).

[6] Alex Graves, et al. ["Automated curriculum learning for neural networks."](https://arxiv.org/abs/1704.03003) ICML 2017.

[7]  Remy Portelas, et al. [Teacher algorithms for curriculum learning of Deep RL in continuously parameterized environments](https://arxiv.org/abs/1910.07224). CoRL 2019.

[8] Sainbayar Sukhbaatar, et al. ["Intrinsic Motivation and Automatic Curricula via Asymmetric Self-Play."](https://arxiv.org/abs/1703.05407) ICLR 2018.

[9] Carlos Florensa, et al. ["Automatic Goal Generation for Reinforcement Learning Agents"](https://arxiv.org/abs/1705.06366) ICML 2019.

[10] Sebastien Racaniere & Andrew K. Lampinen, et al. ["Automated Curriculum through Setter-Solver Interactions"](https://arxiv.org/abs/1909.12892) ICLR 2020.

[11] Allan Jabri, et al. ["Unsupervised Curricula for Visual Meta-Reinforcement Learning"](https://arxiv.org/abs/1912.04226) NeuriPS 2019.

[12] Karol Hausman, et al. ["Learning an Embedding Space for Transferable Robot Skills "](https://openreview.net/forum?id=rk07ZXZRb) ICLR 2018.

[13] Josh Merel, et al. ["Reusable neural skill embeddings for vision-guided whole body movement and object manipulation"](https://arxiv.org/abs/1911.06636) arXiv preprint arXiv:1911.06636 (2019).

[14] OpenAI, et al. ["Solving Rubik's Cube with a Robot Hand."](https://arxiv.org/abs/1910.07113) arXiv preprint arXiv:1910.07113 (2019).

[15] Niels Justesen, et al. ["Illuminating Generalization in Deep Reinforcement Learning through Procedural Level Generation"](https://arxiv.org/abs/1806.10729) NeurIPS 2018 Deep RL Workshop.

[16] Karl Cobbe, et al. ["Quantifying Generalization in Reinforcement Learning"](https://arxiv.org/abs/1812.02341) arXiv preprint arXiv:1812.02341 (2018).

[17] Andrei A. Rusu et al. ["Progressive Neural Networks"](https://arxiv.org/abs/1606.04671) arXiv preprint arXiv:1606.04671 (2016).

[18] Andrei A. Rusu et al. ["Sim-to-Real Robot Learning from Pixels with Progressive Nets."](https://arxiv.org/abs/1610.04286) CoRL 2017.

[19] Wojciech Marian Czarnecki, et al. ["Mix & Match – Agent Curricula for Reinforcement Learning."](https://arxiv.org/abs/1806.01780) ICML 2018.





