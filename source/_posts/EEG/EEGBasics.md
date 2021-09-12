---
title: EEG Data Preprocessing
date: 2021-09-06 19:00:14
math: true
tags: EEG
categories:
- [EEG]
---


![](https://eeglab.org/assets/images/tutorial_image.jpg)

## Basics about EEG data

EEG Waveforms display a rhythmic pattern. According to different types of activity human perform, the waveform falls into distinct rhythms of frequency range as following.

| Type      | Frequency      | Activity                 | Location               |
| --------- | -------------- | ------------------------ | ---------------------- |
| **Gamma** | 20 - 60 Hz     | Visual Attention         | Occipital              |
| **Beta**  | 14 - 20 Hz     | Mental Activity          | Parietal and frontal   |
| **Alpha** | 8 - 13 Hz      | Sensory Stimulation      | Occipital and parietal |
| **Theta** | 4 - 8 Hz       | Emotional Stress         | Temporal and parietal  |
| **Delta** | Less than 4 Hz | Occur during sleep, coma | Everywhere             |

==Why the naming of frequency ranges dose not follow the order of alphabetic order?==

## EEG Data Preprocessing 

To make sense of EEG data from its raw value, we need to extract meaningful information from it. 

Following are commonly used tools

- [EEGLAB](https://sccn.ucsd.edu/eeglab/index.php) 
- [Python-EEG: MNE-Python](https://mne.tools/stable/index.html)

The order for processing steps changes differently from article to article. 

### 1. **Import event markers and channel locations**

When load EEG data with EEGLAB, it will display name and value for each channel. But it doesn't know the scalp locations of the recording electrodes. EEGLAB can read channel labels and correlate with channels locations in a default database of 385 defined channels labels, which is `standard-10-5-cap385`.

### 2. **Remove irrelevant channels** 

Remove channels that are not needed at later stage, like *bilateral mastoid* electrodes (TP9, TP10), or  *electro*-*optical* data. 

### 3. **Filtering** 

Following are basic filter types

- **Hight pass filter**
- **Low Pass Filter**
- **Band Pass Filter**
- **Band Stop (Notch) Filter**

![](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/P80ysEnyG0UKqeGNRgLc09Q-IMGmKBjmnqlHgV7VJcrmlE4rrl3088wKu2cmzfXf3CLbsynqi2iIhBB3CEIb-g0-Rw2SxnlEdcwVRGo0aQYl-lyhvw9xSAP19phglEnIB9xdXCYr)

[^Pic Source]: https://dvic.devinci.fr/imported/tutorial/eeg-preprocess

As picture shown above, the green line is cut of frequency. Y-axis is normalized output power. 

### 4. Extract Epochs 

Subdivide EEG data into different segments (epochs). 

The data are recorded from the beginning to the end of the experiment, but we are only interested in the signal of the event when the subject receives a stimulus or makes a response. So we have to slice and dice the data into segments according to the marks we put on them, and drop the data outside of the range.

One common ERP (Event-Related-Potential) takes a time span which is 200ms before the event and 1000ms after the event, as almost all ERP components are generated within 1 second. However, if time-frequency analysis is required at a later stage, the segmentation time needs to be extended to cover the time period from 1 second before to 2 seconds after the event, as the algorithm for time-frequency analysis requires the data to be of a certain length.

### 5. Baseline Correction

There's two reason for baseline correction. 

- As a time-resolving signal, EEG may have temporal upward **drifts** that are unrelated to the experiment due to various reasons. After the segmentation, the start of each segment will not be in the same place due to the upward drift, which will also make the absolute amplitude of the segment higher. Baseline correction corrects for this drift, so that each segment has a similar starting point.
- In ERP experiments, we are interested in what kind of changes the stimulus event brings to the subject and therefore need to have a comparison. Before the event, we consider the issue to be in a relatively calm state. So we take the EEG activity during this time as a baseline and compare it against the after-event data to discern the actual reaction to the stimulus.

Traditional way for baseline correction is subtracting the mean of a baseline period from every time point of the baseline and post-stimulus interval. 

### 6. Re-referencing

EEG recordings are collected by measuring the potential differences between current electrode and the reference electrode. The recording has following characters

- `Reference voltage` can be a combination of electrodes.     
- Other electrodes will reflect the change at reference electrodes.
- The chosen of EEG reference will greatly impact the data
  e.g. *Picture taken from* *http://martinos.org/mne/stable/auto_examples/preprocessing/plot_rereference_eeg.html#sphx-glr-auto-examples-preprocessing-plot-rereference-eeg-py [(15)](http://learn.neurotechedu.com/preprocessing/#references)*
  ![](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/different-references.png)

The re-referencing formula can be 
$$
V_{curchan} = V_{prevchan} + V_{prevref} - V_{curref}
$$

### 7. Downsampling

Downsampling is used to reduce the amount of data while maintaining the information needed. 

There's two points to pay attention 

1. **Downsampling should happens after filtering**, since it will cause the loss or distortion of high frequency information. To ensure the interested signal is not distorted, it is better to filter it out before downsampling. 
2.  **Nyquist–Shannon sampling theorem** Sampling rate $f_s$​ should at least be twice of the bandwidth we are interested. $f_s > 2B$​ E.g. The samples of two sine waves can be identical when at least one of them is at a frequency above half the sample rate. The suggested $f_s$​ is 3rd or 4th times of the bandwidth interested. 

### 8. Interpolation

Interpolation fills the bad channels' missing data basing on the other good channels' data. The most common way for interpolation is called `spherical splines`, which consists of following steps

>1. Project the channel locations onto a unit sphere (representing the head)
>2. Compute a matrix that describes the relationship between the good and bad electrodes
>3. Use the result from (2) to interpolate the data for bad electrodes
>
>***Source: http://learn.neurotechedu.com/preprocessing/#interpolation*** 

### 9. Artifacts Correction

Artifacts correction removes the neural signals that are not useful for the analysis. 

- **Type of artifacts**:
  - *Environmental Artifacts:* like power lines, electrodes losing contact or other people’s movement during the experiment
  - *Biological Artifacts*: like blinks, eye movements, head movements, heart beats and muscular noise
- **Source Separation**: picks apart different contributions to a measured signal
  - **ICA**: Independent Component Analysis
  - **PCA**: Principal Component Analysis 
  - **SSP**: Signal Space Projection
- **Source Localization**: estimate the signal's localization in the brain 
  - **Diploe Fit**: model the brain's behavior as oscillating dipoles in specific positions

### 10. Reject extreme values

As last step, one can remove extreme values like trials value $> 100 \mu V$ or $< 100 \mu V$. Since such large variance can't be triggered by common cognitive activity. 



## Reference 

- https://eeglab.org/tutorials/
- http://learn.neurotechedu.com/preprocessing/
- https://neuro.inf.unibe.ch/AlgorithmsNeuroscience/Tutorial_files/Introduction.html
- https://pressrelease.brainproducts.com/referencing/

