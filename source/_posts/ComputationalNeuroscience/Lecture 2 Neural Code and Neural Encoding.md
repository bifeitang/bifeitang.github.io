---
title: Lecture 2 Neural Code and neural encoding
date: 2020-10-25 02:37:30
math: true
tags: Computational Neuroscience
categories:
- [Computational Neuroscience]

---

# Neural Code and neural encoding

## 2.1 What is the neural code

- Raster plot

  - ![1599366215170](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599366215170.png)
  - ![1599366242735](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599366242735.png)

- **Neural representation of information**

  - <img src="https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599366316184.png" alt="1599366316184" style="zoom:60%;" />
  - Neural response: average firing rate, the probability of generating a spike
  - Stimulus parameter -> s, vary according to conditions

- **Tuning curves**

  - ![1599366504795](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599366504795.png)

- **Functional map**

  - $$
    response = f(location)
    $$

  - 

  - ![1599366595178](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599366595178.png)

## 2.2 Neural Encoding: Basic coding models

- **Temporal filter**: filter based on time
  - **Firing rate** -> basic coding model

    - $$
      P(response|stimulus) \rightarrow r(t|s)
      $$

  - **Linear Response** (most basic)
    $$
    r(t) = \sum_{k = 0}^n s_{t-k}f_k
    $$

    - Start from time t, look back for n steps indexed by k, weight the stimulus ***s*** by function ***f*** 

    - integral form
      $$
      r(t) = \int_{-\infty}^t d\tau s(t-\tau)f(\tau)
      $$

    - Linear system can be thought of as a system that searches for portions of the signal that resembles its filter

- **Spatial Filtering**
  $$
  r(x,y) = \sum_{x'= -n, y'=-n}^n s_{x-x',y-y'}f_{x',y'}
  $$
  integral
  $$
  r(x,y) = \int_{-\infty}^\infty dx'dy's(x-x',y-y')f(x',y')
  $$
  E.g.

  - Given, which detects the global changes

    ![1599369979149](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599369979149.png)

    The f(x',y')  is similar to the following

    ![1599370065124](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599370065124.png)

    Thinking a point (x, y), use it as center, and integral over space, it will averaging to zero if no much change eventually, and become a large value if there is difference in the surrounding.

  - ![1599370239453](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599370239453.png)This model for a receptive field is often approximated as a difference of Gaussians. A narrow positive Gaussian blob in the middle. That's the excitatory center, and subtracted from it, a broader and shallower Gaussian to capture the, the suppressive surround. The effect of such a differencing filter is to detect local changes. Such a filter will respond strongly when there's, when there's a bright patch near to a dark patch.

- ***Spatiotemporal* filtering**

  - $$
    r(x,y,t) = \int\int\int dx'dy'd\tau s(x-x',y-y',t-\tau)
    $$

- Drawbacks of the filters discussed so far and solution

  - Drawbacks

    - No bound, the product *s***f*  can be large
    - May have negative firing rates (e.g. stimulus varies between large positive and negative number)

  - Solution

    - Imposes a nonlinear function *g*
      $$
      r(t) = g(\int S(t-\tau)f(\tau)d\tau)
      $$
      ![1599370918159](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599370918159.png)

### **2.3 Neural Encoding: Feature selection**

Goal:

- How to find the components of the basic coding model, including
  - **Linear filter**: extracts some component of the stimulus *f(t)*
  - **Non-Linear Input-output function** that maps the filtered stimulus onto the firing rate *g(t)*
- How to modify this model to incorporate other important neuronal properties

#### Reduce the dimension, find Linear filter

Eg. for a stimulus given by 3000 times maybe 100 time points, or in the order of 300,000 values. The dimension is huge

- Goal: 
  - Find one or two or a few meaningful components in the image that drives the stimulus then hopefully computing this response function.
  - This will help transfer
    [response depends on arbitrary characteristics of input] =?
    [response depends on key characteristics]
- Approach:
   Sample the response of the system to many stimuli, just enough response for us to learn what it is really drives the cell.
   
   - A model depends on arbitrary characteristics of the input => depends only on the key characteristics.
- How to describe arbitrary stimuli as a high dimensional vector?
   ![1599884870773](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599884870773.png)
- ![1599884898493](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\1599884898493.png)

- Using Gaussian white noise as stimuli 

  - Some property about the Gaussian. Some time we cut off the higher frequency. All frequency have equal power.

  - ![1599885531020](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599885531020.png)

  - Select Gaussian white noise as stimuli as shown on the left hand side, and plot it in the high dimensional space as a dot in the right side. We will find it follows a certain distribution.

    - The prior distribution is the distribution of the stimulus points. $P(r|s) = P(r) * P(s|r) / P(s)$
    - P(s) is independent from the neural system 
    - So take the entire distribution and project  it into any axis, it still Gaussian. 
    - Take all the spiked points and take average. Project the points to the average vector acquired. It will be Gaussian distribution very likely.

  - Intuitive explanation below. Every time the random noise input triggers a stimuli, take the past certain time of stimuli and add them together.

    ![1599885636847](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599885636847.png)

- Linear filtering

  - **Take spike triggered average as a unit vector** $f$

  - $f$ is our desired vector that captures the feature of the stimulus.

  - Recall what linear filter does
    ![1599885932821](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599885932821.png)

    This takes an arbitrary input stimulus and apply the linear filter $f$ to it. The operation is same with linear filtering/convolution/projection.

  - ![1599886100572](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599886100572.png)
    So the step one is clear: 
    give a lot of vectors of stimulus, check the response and take the average of stimulus that has triggered a response. Get a unit vector of the stimuli average which is $f_1$

#### Determine the nonlinear input/output function

Analyze

- Rewrite the stimulus into vector format, we have
  $P(spike|stimulus) => P(spike|s_1)$

- This can be found from data using Bayes' rule
  $$
  P(spike|s_1) =\frac{P(s_1|spike)P(spike)}{P(s_1)}
  $$
  We already knows about the prior $P(s_1)$. We want to find out $P(s_1|spike)$ as spike conditioned distribution which is $f_1$.

  ![1599886901086](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599886901086.png)

  We take a stimuli, collect the point where the spike happens. We project the stimuli into feature $f_1$, extracting component $s_1$. We use this long stimulus run to make a histogram of $s_1$. Then collect the point when the spikes are triggered and make a distribution of those as $P(s_1|spike)$. As we can see here, the spike happens when stimuli is large then it might look like with a higher Avg. Then we scale the $P(s_1|spike)$ with the probability of having spikes $P(spike)$

**Nonlinear input/output function**

![1599887426390](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599887426390.png)

Here the $P(s)$ and $P(s|spike)$, if selected stimuli points are following the same Gaussian distribution, then there's no relation between input and output. If  they appear very differently, we know the neuron pretend to fire when the stimulus project to the feature we selected.

 **What's missing?**

There might be multiple features fires the neuron. 
![1599887756852](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599887756852.png)

The function $g(f_1*s, f_2*s, ..., f_n*s)$ may be non-trivial. Means it may have some way to combine features  together somehow, only all the features $f_1, f_, ..., f_n$ appears at the same time, then it will fire.

![1599887948254](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599887948254.png)

In this picture, we can extract more information from this cloud of points. Eg. compute the next order moment, or its covariance. To do this, we apply a method something like **principle component analysis** or **PCA**.
![1599888582963](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599888582963.png)





### 2.4 Neural Encoding: Variability

- **Maximally information dimensions**

  Question statement:

  For a tuning curve
  $$
  P(spike|s_f) = P(s_f|spike)* P(spike)/P(s_f)
  $$
  In this case, the prior distribution $P(s_f)$ is predetermined Gaussian distribution. $P(spike)$ is a determined scalar. $P(s_f|spike)$ is the parameter we care about.  

  ![1599449886653](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599449886653.png)

  In order to make the s_f we pick be selective, we want to make the P(s_f|spike) and P(s_f) as different as possible => we use 
  ==Kullback-Leibler divergence== to evaluate the difference between two probability distribution
  $$
  D_{KL}(P(s),Q(s)) = \int dsP(s)log_2(P(s)/Q(s))
  $$
  Try to find a $f$ (filter) to maximize  the $D_{KL}$ between spike-conditional and prior distributions.

  ![1599450664421](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599450664421.png)

  ==This turns out to be maximizing the mutual information between two distribution==. It's try to find a stimulus component as informative as possible.

  **This technique also applies to nature stimuli**. However, the downsides is that the maximization step is not guaranteed to converge to a unique maximum. => Difficult optimization problem.

- **Summary**

  1. Single filter determined by the conditional average
  2. A family of filters derived using PCA
  3. Then this section, using $D_{KL}$, which can generalize to nature stimuli. we find **Information theoretic methods use the whole distribution of stimuli to compute the output distribution**. The last method removes the constraint to Gaussian distribution.


Last Step: Go from $r(t)$ to $spikes$

![1599889626883](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599889626883.png)

==Each spike is generated independently with the probability that scaled by the time variant r of t $t * r(t)$.==



- Calculate/Estimate $P(spike)$

  - **Binomial Spiking**:
    $$
    P_n[k] = {n \choose k} p^k(1-p)^{n-k}
    $$

    - Mean: $<k> = np$
    - Variance: $Var<k> = np(1-p)$

  - **Poisson Spiking**
    $$
    P_T[k] = \frac{(rT)^ke^{-rt}}{k!}
    $$

    - Mean: $<k> = rT$
    
    - Variance: $Var(k) = rT$
    
    - **Fano factor: $F = 1$**
    
    - **Interval distribution: $P(T) = re^{-rT}$**
    
    - The Fano factor can be used to determine if it is a Poisson distribution. Interval distribution is exponential distribution of times.
    
    - ![1599890220734](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599890220734.png)
    
      Here is a practical example. The Fano number, is close to 1. And the interval distribution is also exponential. So we can say the cell response looks Poisson.
    
  - **Where does the variability come from?**
  
    - It's likely that while the neuron is receiving a mean input that's proportional to the stimulus, it's also receiving a barrage of background input.
      ![1599890519555](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599890519555.png)
  
      The picture is plotted in a log of interspike interval against the time, which would be a straight line if it's Poisson distribution. It is the case except from the very short beginning of the time. This is because the neuron itself prevents firing immediately after a firing potential.  
  
      
  
  **Generalized Linear Model**
  
  - ![1599890964419](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599890964419.png)
  - ![1599890978284](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599890978284.png)
  - We can use the Poisson distribution to test against to see if we have collected all the information. This is called Time-rescaling theorem and is used to measure how well one has done in capturing all the influences on spiking with ones model.
    ![1599891063571](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/1599891063571.png)
  - 