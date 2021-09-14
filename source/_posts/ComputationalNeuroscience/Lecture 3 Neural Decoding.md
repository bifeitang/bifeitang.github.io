---
title: Lecture 3 Neural Decoding and Signal Detection Theory
date: 2020-10-25 02:37:30
tags: Computational Neuroscience
categories:
- [Computational Neuroscience]

---

## Neural Decoding and Signal Detection Theory

**Decoding**: how well we can learn what stimulus is by looking at the neural responses?

Try different set of dots moving direction on monkey

![1599955268055](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\1599955268055.png)

How should one decode that firing rate in order to get the best guess about whether the stimulus was moving upward or downward?

Use probability and taking false positive into consideration as well.



> The prior probability gives us a way of quantifying the distribution of firing rates of the cell in an average case, which we can think of as simply the noisy case (the cell is receiving any random input). By adjusting for that normal firing rate, we can pick out instances when the firing rate is significantly different from what we consider normal.

## Questions Checklist (double check when finish the course)

#### Video 1

- [x] ==How well can we learn what the stimulus is by looking at neural responses?==

- [x] Solve this by giving a Tiger Vs Rustle example. Think about how the neuron is able to decide stay or go with accumulated information. What does that look like mathematically?

- [x] In the previous section, coherence is introduced. When coherence is zero, how should one seeing the firing rate from neuron and decide what the stimulus is? Upward/Downward?

- [ ] ==How does one go from that distribution of firing rates that we saw in the last step to this measure of performance? So this requires decoding step==

- [x] Decoding means

  > we would like a policy that tells us if we see some value r, we can map the stimulus onto either an upper going or downward going stimulus. 

- [ ] It turns out the `likelihood ratio test` is the most efficient statistic we can use to analyze our data. This is called `Neyman-Pearson lemma`

- [ ] ==Classification of noisy data. Nonlinear separation of signal and noise.==

- [ ] The firing rate wrap up until they reach some threshold of confidence, at which point the monkey is willing to make a move to signal his decision.

- [ ] ==How do we additionally take these costs into account in our decision?==



#### V2

- [ ] ==Why then do we need all these neurons, especially in cortex when many of them seem to be doing approximately the same thing?==
- [ ] Consider some decoding algorithms that allow for the influences of many neurons in interpreting the stimulus. Example ==Cricket cercal system, How did its neurons as a group communicate wind velocity to the animal and aid its escape?==
- [ ] Use statistical way to sum up all the input and maximize the likelihood of predicting correctly. ==What is the limitation of these approaches?==



#### V3

- [x] Topic: ==Reconstruction==. In the pervious lecture, we talked about methods to find an estimate of the stimulus using Bayesian decoding. ==Now we'd like to extend our decoding procedures to the case of the Reponses and the stimuli==




