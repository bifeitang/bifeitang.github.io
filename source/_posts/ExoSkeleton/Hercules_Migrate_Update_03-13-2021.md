---
title: Migration Note 03_13_2021
date: 2021-03-13 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Hercules_Migrate_Update_03-13-2021

#### Task creation in another task

- it is perfectly ok to create tasks from inside other tasks.  
- Any call to `xTaskCreate()` will allocate a TCB structure and stack for that task. No matter where the task gets created (from  a  function or another task), they will be in the separated memory. Say `TaskX` is created, without deleting `TaskX`, `TaskY` is also created. Then two tasks won't use the same memory because both calls to `xTaskCreate()` will allocate separated RAM. 



#### Passing Parameters to Task

- Ref
  - https://www.hackster.io/Niket/tasks-parametertotasks-freertos-tutorial-5-b8a7b7



#### TaskDelayUntil

```c
/* The rate at which data is sent to the queue.  The 200ms value is converted
to ticks using the portTICK_PERIOD_MS constant. */
#define mainQUEUE_SEND_FREQUENCY_MS			( 200 / portTICK_PERIOD_MS )

xNextWakeTime = xTaskGetTickCount();

/* Place this task in the blocked state until it is time to run again. The block time is specified in ticks, the constant used converts ticks to ms.  While in the Blocked state this task will not consume any CPU time. */
vTaskDelayUntil( &xNextWakeTime, mainQUEUE_SEND_FREQUENCY_MS );
```



#### FreeRTOS Task Name

1



I'm a newbie of FreeRTOS. But I don't think it's well documented. As in [xTaskCreate()](https://www.freertos.org/a00125.html) :

> `pcName` A descriptive name for the task. This is mainly used to facilitate debugging, but can also be used to obtain a task handle. The maximum length of a task’s name is set using the `configMAX_TASK_NAME_LEN` parameter in `FreeRTOSConfig.h`.

1. Is a task must be associated with a name?
   - No it does not need a name
2. What happened if `pcName` is NULL?
   - If `pcName` is `NULL` it simply does not have a name. Nothing happens.
3. What happened if I created multiple tasks with the same name?
   - You will get several tasks with the same name so you cannot identify them by name. The behavior of `xTaskGetHandle` is then undefined (as per the documentation).
4. Should I maintain the mapping between task's handle and name? Or FreeRTOS maintains this relation?
   - FreeRTOS handles this.





To Do

- [ ] Make debug message take parameter, use vector convention
- [ ] Write a circular buffer to record the mapping information between hashcode and command sending state
- [ ] Explore other simpler data structure since our commands to won't be too much
  - [ ] Command we send are usually in sequence, FIFO
- [ ] Read emergency, we can start a highest priority task, to handle all the emergency 
- [ ] Put all the priority define into one dedicated file
- [ ]  

