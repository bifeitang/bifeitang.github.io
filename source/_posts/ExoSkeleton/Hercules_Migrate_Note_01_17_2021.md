---
title: Debugging FreeRTOS tasks running in parallel issue
date: 2021-01-17 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

> **Error**
>
> Tried to run 3 FreeRTOS tasks in parallels, but tasks hang. It is not the case if I just run one task.
>
> Following is the place the project stops when I pause
>
> ![image-20210117224646232](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210117224646232.png)

- Reference:
  - https://e2e.ti.com/support/microcontrollers/hercules/f/312/t/566100?RM57L843-How-to-get-debug-information-CCS-RM57-LwIP
  - https://e2e.ti.com/support/microcontrollers/hercules/f/312/t/501906?Problem-with-simple-program-using-SCI1-and-RM57-program-enters-prefetchEntry
- **The PC hanging at the` prefetchEntry` or the `dataEntry` means there was a memory violation. If at the `prefetchEntry`, it was caused by an instruction fetch, if at the `dataEntry`, it was caused by a data fetch. Check the CP15 registers for more information.** **They are explained in the ARM Cortex R5** [TRM](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiTjZzEvLXRAhVIsVQKHdCcDiIQFggcMAA&url=http%3A%2F%2Finfocenter.arm.com%2Fhelp%2Ftopic%2Fcom.arm.doc.ddi0460c%2FDDI0460C_cortexr5_trm.pdf&usg=AFQjCNFlXLUip94vHK5xi19_jxgcP__jhA&sig2=flTSlmiRgSE0RuvUTuweWA) **section 4.3.20**.

- Register that logged the fault
  ![image-20210117225631652](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210117225631652.png)
