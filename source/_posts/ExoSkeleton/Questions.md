---
title: On board Q&A with Luu to get Simulink work together with Hercules
date: 2020-10-21 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Enviornment 

> **Q1**: How fundimentally the two adds-on are different?

![image-20201214013454316](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201214013454316.png)

- Matlab

- HalCoGen `v04.07.01`

- Code Composer Studio Tool Chain: `C:\ti\ccs1011\ccs\tools\compiler\ti-cgt-arm_5.2.9`

> **Q2**: How Simulink code gets compiled and linked with tools provided above and then load to the Hercules board?

## Code

> **Q3**: Why this `No rule to make target C:/Program, needed by updown.obj` come up every first time open the simulink or switch from CCS to Matlab?

![image-20201214011201751](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201214011201751.png)



> **Q4**: Why the code working in CCS but not in Matlab Simulink? What can cause the problem?

### Code Composer Studio

- Code

![image-20201214014418829](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201214014418829.png)

- Running in CCS
  ![image-20201214014625695](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201214014625695.png)


  ![image-20201214014915756](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201214014915756.png)

  > I am not using `printf` here sinc I had some issue with configuration of section `.cio` for file `HL_sys_link.cmd`, it would  be good if there's any insight. 

- Same code running in Simulink

  - Simulink Model
    ![image-20201214015247520](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201214015247520.png)

  - Following is what's inside of the code block

    ```matlab
    function [canID, data, nBytes] = CANread()
    canID = uint32(nan(1,1));
    data = uint8(nan(1,8));
    nBytes = uint8(nan(1,1));
    
    if strcmp(coder.target,'rtw')    
        coder.ceval('HerculesCANRead',coder.wref(canID),coder.wref(data),coder.wref(nBytes));
        fprintf('canID %u', canID);
        fprintf('nBytes %u', nBytes);
    end
    ```

    ```c
    void HerculesCANRead(uint32_t* canID, uint8_t* data, uint8_t* nBytes)
    { 
        canInit();
    
        while (!canIsRxMessageArrived(canREG1, canMESSAGE_BOX2));
        canGetData(canREG1, canMESSAGE_BOX2, data);
    
        *canID = canGetID(canREG1, canMESSAGE_BOX2);
        *nBytes = canREG1->IF2MCTL & 0xFU;
    }
    ```



**Note that the CANWrite works with no problem like following.**

![image-20201214020304346](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201214020304346.png)

