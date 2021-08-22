

## Main Controller Tasks

| Tasks                             | Function                                                     | Priority | Progress |
| --------------------------------- | ------------------------------------------------------------ | -------- | -------- |
| **IdleTask** (State Machine Task) | Handle the state machine  transition                         | 0        | 20%      |
| **DbgPrintTask**                  | Write message to SCI so that logging message will be stored / displayed on UI | 1        | 60%      |
| **HeartBeatTask**                 | Ping each node receiver every 1s                             | 1        | 100%     |
| **CANReceiveTask**                | Polling commands from CAN receive message box and quickly copy into circular buffer | 2        | 100%     |
| **ExoCmdTask**                    | ==Provide following APIs==<br />-  **EnqueueCmd**: state machine can call this function to send a command<br />-  **IsCmdReceived** <br />==with following functions==<br />- Send command<br />- Resend command if no echo received before timeout <br />- Pull message from circular buffer see if there's command echo | 2        | 40%      |
| **EmergencyStopTask**             | Emergency Handling                                           | 4        | 0%       |

## Main Controller UI

<img src="C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210208174357523.png" alt="image-20210208174357523" style="zoom:70%;" />

- Send command to main controller
  - More exploration (USB) 
  - Upload initial configuration to different joint
- Listening to COM port 
  - COM5 e.g. 
  - save message to computer 
- State Machine 
  - Initialization (Power up)
    - LED indication on the mode of main controller (Individual test by Lujayna )

Next Step

- Send joint max/min range at start up