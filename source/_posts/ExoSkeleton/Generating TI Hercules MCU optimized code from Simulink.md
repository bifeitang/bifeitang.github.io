---
title: Generating TI Hercules MCU optimized code from Simulink
date: 2020-10-25 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Generating TI Hercules MCU optimized code from Simulink

- TI **`processor-in-the-loop`** (PIL) testing with Simulink and TI Hercules MCUs.



### CMSIS

- Replace equivalent sin/cos with CMSIS library
  - The CMSIS library is free to download as `Cortex-R4 CMSIS DSP Library`
    - `CMSIS` stands for `ARM's Cortex Microcontroller Software Interface Standard` 
    - 60+ functions covering 
      - vector operations
      - matrix computing
      - complex arithmetic
      - filter functions
      - control functions
      - PID controller
      - Fourier transforms 
      - many other frequently used DSP algorithms
    - ARM DSP SIMD (Single Instruction Multiple Data) 





### Hardware Configuration

> **ANSI C**, **ISO C** and **Standard C** are successive standards for the [C programming language](https://en.wikipedia.org/wiki/C_(programming_language)) published by the [American National Standards Institute](https://en.wikipedia.org/wiki/American_National_Standards_Institute) (ANSI) and the [International Organization for Standardization](https://en.wikipedia.org/wiki/International_Organization_for_Standardization) (ISO). Historically, the names referred specifically to the original and best-supported version of the standard (known as **C89** or **C90**). Software developers writing in C are encouraged to conform to the standards, as doing so helps [portability](https://en.wikipedia.org/wiki/Porting) between compilers.



### Processor in the loop

> In **processor-in-the-loop (PIL)** simulation, code generated from a Simulink® model runs directly on the target hardware, which means you can test models on the hardware using the same test cases as on the host. PIL tests are designed to expose problems with execution in the embedded environment.



- Simulink Model based design

  - Benefits

- Simulink support for certification standards

  ### 



#### Demo - Processor in the loop (PIL) testing with Simulink and TI Hercules MCUs

- Tolerant Fuel System Model![image-20201204153215321](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204153215321.png)

- Set up for PIL
  - <img src="C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204153317091.png" alt="image-20201204153317091" style="zoom:50%;" />
  - ![image-20201204153346768](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204153346768.png)
  - ![image-20201204153354599](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204153354599.png)
- About the board
  - USB connection contains emulation connection for launching simulation and a UR there for USB virtual COM port provides serial port for communication with Simulink
  - `loadti`: launcher
- Purpose: verify the numerical equivalence the embedded target that running your algorithm and the algorithm in Simulink

#### Profiling report

`executionProfile.report`

- Cycle time in ticks

### Use of SIMSIS Library with code replacement table for optimization

- **CRL**
  - Generate **Code Replacement Library** **(CRL)**
    - `CMSIS.m` 
      ![image-20201204154423996](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204154423996.png)
    - Replace sin/cos with corresponding algorithm in the SIMSIS library
  - Add `CMSIS.m` to the matlab path and do a `sl_refresh_customization` which register the CRL
  - Select the CRL for the model 
    ![image-20201204154722269](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204154722269.png)
  - 

## Setup for RM48

![image-20201204154938511](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204154938511.png)

- This will make help available

  - Including the required packages 

  - **Making customizations** for different board in Hercules family

    - Support RM48 as chips 
    - Customize this to match my particular Hercules that I am using  
      - ![image-20201204155320875](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204155320875.png)
      - It contains
        - Builder
        - Launcher 
        - Communicator 
        - Timer (for Profiling)
      - ![image-20201204155437717](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204155437717.png)

  - **Builder**

    - The way that matlab will generate a file for you

      ![image-20201204155518332](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204155518332.png)

      All the make utility, register the tool chain that I am using with Matlab, which is CCS 

    - `TI_HERCULES_RM48_toolchain.m`
      ![image-20201204155713302](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204155713302.png)

      These used to be the options in the make file

    - Instead of creating a make file or a template makefile, now you create this tool chain registration. And the actual make file is created on the flag. But it uses the option here. Eg. if I want to change big/little endian, I change compiler assemble options in this file

    - See more detail in `making customization help` page.

    - Framework

      - Second part of the builder 
        ![image-20201204160054864](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204160054864.png)
      - Add HalCoGen sources and RTIO stream sources to this build 
      - What is target application framework, I can't directly run main function on the target without the code to initialize the target so I have to set the clock frequency, interrupt etc. These all has to be configured before main function. And create a run time environment. And also set up the C runtime environment. `halcogen` will generate the startup code for any of the hercules product. 
      - The serial port needs to be configured 
      - Also I need to enable the interrupts in the VIM 
      - Serial IO stream communication. We needs to provide c function for openning, closing the serial ports, sending and receiving

  - **Launcher**

    -  ![image-20201204161233789](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204161233789.png)
    - Here use a utility called `loadti`, which comes as a debug server scripting. And I need to know what target configuration file to use 
    - ![image-20201204161516253](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204161516253.png)
    - target configuration file -> I have to generate a CCXML file `TCONF` needs to be changed. 
    - ![image-20201204161617736](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204161617736.png)

  - **Communicator**

    - ![image-20201204161749891](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204161749891.png)
    - 

  - Timer for Profiling

    - Provide a function to read the timestamp
    - `cr4pmu_crl`: code replacement library that replace the `code_profile_read_timer` with `_pmuGetCycleCount_`

  - Get following example to run
    ![image-20201204155201386](C:\Users\YHu15\AppData\Roaming\Typora\typora-user-images\image-20201204155201386.png)

- 