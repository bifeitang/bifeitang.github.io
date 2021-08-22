---
title: FreeRTOS
date: 2020-10-25 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Legacy Code

```c
/** @file HL_sys_main.c 
*   @brief Application main file
*   @date 11-Dec-2018
*   @version 04.07.01
*
*   This file contains an empty main function,
*   which can be used for the application.
*/

/* 
* Copyright (C) 2009-2018 Texas Instruments Incorporated - www.ti.com  
* 
* 
*  Redistribution and use in source and binary forms, with or without 
*  modification, are permitted provided that the following conditions 
*  are met:
*
*    Redistributions of source code must retain the above copyright 
*    notice, this list of conditions and the following disclaimer.
*
*    Redistributions in binary form must reproduce the above copyright
*    notice, this list of conditions and the following disclaimer in the 
*    documentation and/or other materials provided with the   
*    distribution.
*
*    Neither the name of Texas Instruments Incorporated nor the names of
*    its contributors may be used to endorse or promote products derived
*    from this software without specific prior written permission.
*
*  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS 
*  "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT 
*  LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
*  A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT 
*  OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, 
*  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT 
*  LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
*  DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
*  THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT 
*  (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE 
*  OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
*
*/


/* USER CODE BEGIN (0) */
/* USER CODE END */

/* Include Files */

#include "HL_system.h"
#include "HL_can.h"
#include "HL_sys_common.h"

#include <stdio.h>

#define D_SIZE 8

void HerculesCanRead(uint32_t* canID, uint8_t* data, uint8_t* nBytes);

int main(void)
{
    uint32_t id;
    uint8_t data[8] = {0};
    uint8_t bytes;

    HerculesCanRead(&id, data, &bytes);

    return 0;
}

void HerculesCanRead(uint32_t* canID, uint8_t* data, uint8_t* nBytes)
{
    canInit();

    while (!canIsRxMessageArrived(canREG1, canMESSAGE_BOX2));
    canGetData(canREG1, canMESSAGE_BOX2, data);

    *canID = canGetID(canREG1, canMESSAGE_BOX2);
    *nBytes = canREG1->IF2MCTL & 0xFU;
}


/* USER CODE BEGIN (4) */
/* USER CODE END */

```





## FreeRTOS

> The core RTOS code is contained in three files, which are called called `tasks.c`, `queue.c` and `list.c`. These three files are in the `FreeRTOS/Source` directory. The same directory contains two optional files called `timers.c` and `croutine.c` which implement software timer and co-routine functionality respectively.



### Made Blink LED work on target RM57Lx board

**Tasks**, **queue**, **scheduler** are practiced.

![image-20210103205607047](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210103205607047.png)

