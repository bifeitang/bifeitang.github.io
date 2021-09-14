---
title: Lecture 1 Neural Encoding I - Firing Rates and Spike Statistics
date: 2020-10-25 02:37:30
tags: Computational Neuroscience
math: true
categories:
- [Computational Neuroscience]

---

# Neural Encoding I: Firing Rates and Spike Statistics

##  1.1 Introduction

- **Action potentials**: characteristic electrical pulses
- Nerve fibers
- Study of Neural coding: **measuring and characterizing how stimulus attributes** *such as light or sound intensity, or motor actions, such as the direction of an arm movement* **are represented by action potentials**.- - 

- Aspects:

  - Neural **encoding**: stimulus -> response

    - Study wide variety of stimulus, predict how neurons responses with constructed model.

    - $$
      P(response|stimulus)
      $$

  - Neural **decoding**: response -> stimulus

    - How the information represented by action potential can be quantified and maximized.

    - $$
      P(stimulus|reponse)
      $$

- Terminologies introduction

  - axons, dendrites
  - ion channel
  - membrane potential
  - hyperpolarization and depolarization
  - refractory period
  - synapse
  - sharp and patch electrodes
  - extracellular electrodes

- **Characterizing stimulus to responses**

  - Same stimulus, response will always wary
  - Seek model can account for the probabilities that different spike sequences are evoked by a specific stimulus.
  - Population coding: one stimulus -> a large neural populations activated -> study the relationship of firing patterns across the population of responding cells

- **Firing Rate** and **Spike-Train** correlation functions

  - basic measures of spiking probability and statistics

- **Spike-triggered averaging**: relating **actions potentials** to the **stimulus** that evoked them. 

- Stochastic descriptions of spike generation

  - Homogeneous and inhomogeneous Poisson models

- Reverse-correlation methods -> construct estimates of firing rates

## 1.2 Spike Trains and Firing Rates

- Representation of action potentials: 

  - Characterize action potential by a list of times when spikes occurred

  - Neural response function

    - Infinitesimally narrow, idealized spikes in the form of Dirac rho function
      $$
      \rho(x) = \sum_{i=1}^{n}\delta(t-t_{i})
      $$

    - Re-express sums over spikes as integrals over time

    - eg.
      $$
      \sum_{i=1}^{n}h(t-t_i) = \int_0^T d\tau h(\tau)\rho(t-\tau)
      $$

- Average firing rate

  - Probability of generating a spike