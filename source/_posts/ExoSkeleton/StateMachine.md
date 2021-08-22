## FreeRTOS Main Controller 

### Design Principles

- Don't starve the tasks
  - Using `vTaskDelay` to block high priority tasks so that the lower priorities ones get chance to execute.
- Using global variable is not encouraged in FreeRTOS.  Prefer usage of `static` variable like private in C++.
- Idle Task (keep the counter)

### State Machine

```c
typedef void(*state_func_t)(void);
static uint8_t CUR_STATE_ELAPSED_MS = 0;
static state_func_t state_func = null;

void StateInit(void);
void StateWalk(void);
void StateStand(void);
void StateSit(void);
void StateFreeze(void);
void StateErr(void); // Each error with an error code in lookup table, e.g. timeout

void SetNxtState(state_func_t func);
state_func_t GetNxtState(void);

void StateWalk(void)
{
    // 1. set CUR_STATE_ELAPSED_MS by calling xTaskGetTickCount(), check if timeout
    // 2. send commands, then SetNxtState(StateStand)
    // 3. or gets interrrupted by emergency, then SetNxtState(StateFreeze)
}

void SetNxtState(state_func_t func)
{
    // 1. set CUR_STATE_ELAPSED_MS = 0
    // 2. state_func = func
}

state_func_t GetNxtState(void)
{
    return state_func;
}

void StateMachineTask(xParameter* params)
{
    SetNxtState(StateInit);
    for (;;)
    {
        (*GetNxtState())();
        xTaskDelay();
    }
}
```



#### Common Practices

- **Using switch statement**
  https://stackoverflow.com/questions/46685292/state-machine-program-design-in-freertos-vtaskstartscheduler-in-a-switch-state
- Without using switch statement
  - Each function only needs to know two things
    - What needs to do in this state
    - What is next state
  - Benefits comparing with `switch`
    - Separate the state transfer form a centralized to distributed functions
    - No matter how complex the state transfer is, each   



### Test Bench

- LabWindows/CVI
  https://www.ni.com/en-us/support/downloads/software-products/download.labwindows-cvi.html#353603
- 

