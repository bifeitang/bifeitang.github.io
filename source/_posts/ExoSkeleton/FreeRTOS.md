---
title: Main Controller Architecture Design with FreeRTOS
date: 2020-10-25 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## FreeRTOS

> The core RTOS code is contained in three files, which are called called `tasks.c`, `queue.c` and `list.c`. These three files are in the `FreeRTOS/Source` directory. The same directory contains two optional files called `timers.c` and `croutine.c` which implement software timer and co-routine functionality respectively.



## Main Controller Task Design

| Tasks                             | Function                                                     | Priority | Progress |
| --------------------------------- | ------------------------------------------------------------ | -------- | -------- |
| **IdleTask** (State Machine Task) | Handle the state machine  transition                         | 0        | 20%      |
| **DbgPrintTask**                  | Write message to SCI so that logging message will be stored / displayed on UI | 1        | 60%      |
| **HeartBeatTask**                 | Ping each node receiver every 1s                             | 1        | 100%     |
| **CANReceiveTask**                | Polling commands from CAN receive message box and quickly copy into circular buffer | 2        | 100%     |
| **ExoCmdTask**                    | ==Provide following APIs==<br />-  **EnqueueCmd**: state machine can call this function to send a command<br />-  **IsCmdReceived** <br />==with following functions==<br />- Send command<br />- Resend command if no echo received before timeout <br />- Pull message from circular buffer see if there's command echo | 2        | 40%      |
| **EmergencyStopTask**             | Emergency Handling                                           | 4        | 0%       |



## Main Controller CAN Communication

### Phase 1 -- Basic test with FreeRTOS Timer

![image-20210916202838825](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210916202838825.png)

### Phase 2 -- Establish Basic CAN Communication  

![image-20210916203117639](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210916203117639.png)

### Phase 3 -- Command management 

At this stage, we create one layer abstraction to the command management, which is command queue to manage the command resend, timeout, etc. Following is the detailed design proposal

![image-20210916202940950](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210916202940950.png)

![image-20210916203357313](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210916203357313.png)



## Main Controller Communication to PC

Planning to have a UI with `PyQt`like following to catch logging information from COM port 

<img src="https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210208174357523.png" alt="image-20210208174357523" style="zoom:70%;" />

## Main Controller State Machine

![image-20210916203635160](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210916203635160.png)
