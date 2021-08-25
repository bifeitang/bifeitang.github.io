---
title: fMRI Data Processing Basics
date: 2021-08-24 23:52:20
tags: [fMRI, HRF]
math: true
---

# Resources

- Andy's Brain Book https://andysbrainbook.readthedocs.io/en/latest/index.html
- 

# First look on the data

## Key Concepts

- **TR** : time (in seconds) between scanning successive FMRI volumes.
- 

# Data Processing

## `HRF`Hemodynamic Response Function

1. plot $hrf=gammapdf(\alpha_1, \beta_1, t)$​;
   ![plot hrf](./image-20210709181542189.png "Common Statistic Display for HRF plot in Matlab")

2. Why convolve with `Gamma distribution`?
   - BOLD (Blood oxygen level dependent) signal looks like a typical Gamma Distribution, this is used to identify the neuron firing
   ![image-20210706005105094](./image-20210706005105094.png)

3. Proof 
   - $\sigma = 3$, $my_1 = 5$
   - $\alpha_1 = {my_1}^2/\sigma^2 = 25/9$
   - $\beta_1 = {my_1}/\sigma^2 = 5/9$ 
   - t = [0, 0.1, 0.2, 0.3, 0.4, ..., 20]
   - $resolution = 0.1$
   - `gammapdf`:
   $$
   \begin{aligned}
   &\exp(a*log(b) + (a-1)*log(x) - b*x - \gamma \ln(a)) \\
   &= e^{a \cdot log(b)} \cdot e^{(a-1)log(x)} \cdot e^{-bx} \cdot e^{- \gamma ln(a)} \\
   &= b^a *x^{(a-1)}* e^{-bx}*\gamma ln(a) \\
   &= b^a *x^{(a-1)}* e^{-bx}* \dfrac{1}{\Gamma(a)}
   \end{aligned}
   $$

   - Use $\alpha_1 = a, \beta_1 = b$, (Use $\gamma ln(a)$ to approximate $\Gamma(a) ^ {-1}$)

   $$
   Gamma \; Distribution = \frac{x^{(\alpha_1-1)}\beta_1^{\alpha_1}e^{-\beta_1x}}{\Gamma(\alpha_1)}
   $$

4. Supplementary 
   - ![image-20210709174054933](./image-20210709174054933.png)
   - ![image-20210709174149366](./image-20210709174149366.png)

5. Properties

  >Evaluates a gamma of parameters a,b for all values in vector x
  >
  >Will have mean m and variance v: $m = a/b$; $v = a/b^2$;
  >
  >so, $a = m^2/v$; $b = m/v$;
  
- mean = $\alpha_1/\beta_1$, variance = $\alpha_1/\beta_1^2$
- `PDF`(probability distribution function)
- for  $x \geqslant 0$: 
  ![image-20210709174439817](./image-20210709174439817.png)

## Design Matrix

- https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FEAT/UserGuide#Appendix_B:_Design_Matrix_Rules
- ![image-20210714004332208](./image-20210714004332208.png)

## T-Test





# ROI

How to find the Reigon of Interest
