---
title: fMRI Data Processing Basics
date: 2021-08-24 23:52:20
tags: [fMRI, HRF]
math: true
categories:
- [fMRI]
---

# Resources

- [Andy's Brain Book ](https://andysbrainbook.readthedocs.io/en/latest/index.html)
- [The Brain Imaging Data Structure](https://bids-specification.readthedocs.io/en/stable/)

# First look at fMRI data

## Key Concepts

For a full list, use [A checklist for fMRI acquisition methods reporting in the literature](https://thewinnower.com/papers/977-a-checklist-for-fmri-acquisition-methods-reporting-in-the-literature)

- **BOLD** :  blood oxygenation-level dependent (contrast)
- **TR** : time (in seconds) between scanning successive FMRI volumes.
- **TE**: Echo time
- **Matrix**=64x64
- **FOV**: Field-of-view (e.g. 192 mm)
- **Acquisition Voxel Size** 3x3x4mms

## Data Format

### Motion Regressor

- For a design matrics created with `FSL` following format

  > FSL is a comprehensive library of analysis tools for FMRI, MRI and DTI brain imaging data. [Link](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki)

  ![image-20210714014330573](./image-20210714014330573.png)

  - The number of rows is the number of subjects
  - The number of columns is the number of linear variables
  - The PPheights are the maximum values in the columns
  - The means of the columns have been subtracted (first column is age, second is score)

- Depending on the way of design, in this case, the last 6 columns are used for motion regressors

- References

  - https://web.stanford.edu/group/vista/cgi-bin/wiki/index.php/MrVista_TBSS
  - https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=fsl;94689ebe.1805

  - http://mriquestions.com/dti-tensor-imaging.html

### Events Data

- E.g. for data found from [OpenNeuro ](https://openneuro.org/datasets/ds000102/versions/00001)

  ![image-20210825022741854](./image-20210825022741854.png)

- | **Column name**   | **Requirement level** | **Data type**    | **Description**                                              |
  | ----------------- | --------------------- | ---------------- | ------------------------------------------------------------ |
  | **onset**         | REQUIRED              | number           | Onset (in seconds) of the event measured from the beginning of the acquisition of the first volume in the corresponding task imaging data file. If any acquired scans have been discarded before forming the imaging data file, ensure that a time of 0 corresponds to the first image stored. In other words negative numbers in "onset" are allowed5. |
  | **duration**      | REQUIRED              | number           | Duration of the event (measured from onset) in seconds. Must always be either zero or positive. A "duration" value of zero implies that the delta function or event is so short as to be effectively modeled as an impulse. |
  | **sample**        | OPTIONAL              | number           | Onset of the event according to the sampling scheme of the recorded modality (that is, referring to the raw data file that the events.tsv file accompanies). |
  | **trial_type**    | OPTIONAL              | string           | Primary categorisation of each trial to identify them as instances of the experimental conditions. For example: for a response inhibition task, it could take on values "go" and "no-go" to refer to response initiation and response inhibition experimental conditions. |
  | **response_time** | OPTIONAL              | number           | Response time measured in seconds. A negative response time can be used to represent preemptive responses and "n/a" denotes a missed response. |
  | **value**         | OPTIONAL              | string or number | Marker value associated with the event (for example, the value of a TTL trigger that was recorded at the onset of the event). |
  | **HED**           | OPTIONAL              | string           | Hierarchical Event Descriptor (HED) Tag. See Appendix III for details. |

### fMRI Data

- E.g.
  ![image-20210714015725940](./image-20210714015725940.png)
- After processing to get the Region of Interest (ROI)
  ![image-20210714015611531.png](./image-20210714015611531.png)



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
- ![image-20210714004446402](./image-20210714004446402.png)

## T-Test





# ROI

How to find the Reigon of Interest
