## CAN communication between Teensy and Hercules

- [CAN Prototyping with Teensy 3.1/3.2](https://copperhilltech.com/blog/controller-area-network-can-prototyping-with-teensy-3132/)

  - In case of working with the CAN port, you will need a CAN transceiver in order to connect the board to a real CAN bus network. The best choice for adding a CAN transceiver, in terms of size and electrical compatibility, is the [CAN Bus Mini Breakout Board](http://copperhilltech.com/can-bus-mini-breakout-board/) we offer through this website.
  - ![image-20201114174628529](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201114174628529.png)
  - ![image-20201114174729789](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201114174729789.png)

- Teensy: 

- Able to read data from Teensy Can bus

  - ![image-20201114182837644](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201114182837644.png)
  - ![image-20201114183009690](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201114183009690.png)

  - ![image-20201114184659352](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201114184659352.png)

- Connect Hercules to the CAN bus

  - Install [HalCoGen](https://www.ti.com/tool/HALCOGEN)
  
  - Install Code Composer Studio
  
  - ![image-20201115005749530](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201115005749530.png)
  
  - Configure DCAN_TX, DCAN_RX
  
    ![image-20201115233805328](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201115233805328.png)
  
  - Follow the tutorial and write code 
  
- Figure out the DCAN1 Pin

  - https://www.ti.com/lit/df/sprr397/sprr397.pdf?ts=1605496271087
  - ![image-20201115234025988](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201115234025988.png)
    - I was thinking maybe the pin is multiplexed to here
    - But wasn't able to find corresponding CAN1
  - So looks like J10 44/45 are the pin I should use. And I have solder pin connecter to it.
    ![image-20201115235233243](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201115235233243.png)

- Set up CAN TX

  - Code Modified in `HL_sys_man.c`

  - ```c
    /* USER CODE BEGIN (0) */
    #include "HL_can.h"
    #include "HL_system.h"
    /* USER CODE END */
    
    /* Include Files */
    
    #include "HL_sys_common.h"
    
    /* USER CODE BEGIN (1) */
    /* USER CODE END */
    
    /** @fn void main(void)
    *   @brief Application main function
    *   @note This function is empty by default.
    *
    *   This function is called after startup.
    *   The user can use this function to implement the application.
    */
    
    /* USER CODE BEGIN (2) */
    #include "HL_can.h"
    #include "HL_system.h"
    
    uint8 tx_data[D_SIZE] = {1, 1, 2, 0, 2, 0, 2, 0, 'E', 'X', 'O', '\0'};
    
    uint32 checkPackets(uint8 *src_pkg, uint8 *dst_pkg, uint32 psize);
    
    /* USER CODE END */
    
    int main(void)
    {
    /* USER CODE BEGIN (3) */
    
        canInit();
        canTransmit(canREG1, canMESSAGE_BOX1, tx_data);
    
    /* USER CODE END */
    
        return 0;
    }
    
    
    /* USER CODE BEGIN (4) */
    uint32 checkPackages(uint8 *src_pkg, uint8 *dst_pkg, uint32 psize)
    {
        uint32 err = 0;
        uint32 cnt = psize;
    
        while(cnt--)
        {
            if((*src_pkg++) != (*dst_pkg++))
            {
                err++;
            }
        }
        return err;
    }
    
    
    void canMessageNotification(canBASE_t *node, uint32 messageBox)
    {
        return;
    }
    
    void canErrorNotification(canBASE_t *node, uint32 notification)
    {
        return;
    }
    
    void canStatusChangeNotification(canBASE_t *node, uint32 notification)
    {
        return;
    }
    /* USER CODE END */
    
  ```
    
  - ![image-20201120224019913](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120224019913.png)

  - ![image-20201120224129579](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120224129579.png)

  - ![image-20201120224157852](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120224157852.png)

- Set up CAN RX

  - ![image-20201120234019661](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120234019661.png)

  - ![image-20201120234038976](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120234038976.png)

  - Code

    ```c
    while(!canIsRxMessageArrived(canREG1, canMESSAGE_BOX2));
    canGetData(canREG1, canMESSAGE_BOX2, rx_data);
    
    sciSend(sciREG1, D_SIZE, rx_data);
    uint32 i;
    
    for(i = 0; i < D_SIZE; ++i)
    {
        printf("Pos[%u] received data [%u]\n", i, rx_data[i]);
    }
    ```

- Both

  ```C
  int main(void)
  {
      canInit();
      sciInit();
  
      while(1)
      {
          tx_data[0]++;
          canTransmit(canREG1, canMESSAGE_BOX1, tx_data);
  
          while(!canIsRxMessageArrived(canREG1, canMESSAGE_BOX2));
          canGetData(canREG1, canMESSAGE_BOX2, rx_data);
  
          sciSend(sciREG1, D_SIZE, rx_data);
          uint32 i;
  
          for(i = 0; i < D_SIZE; ++i)
          {
              printf("Pos[%u] received data [%u]\n", i, rx_data[i]);
          }
      }
  
      return 0;
  }
  ```

  ![image-20201120234921668](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201120234921668.png)

- Set up the test coverage and add a unit test badge
- Next Steps
  - Communicate
  - State machine for joints
  - Matlab Simulink real-times
    - Receive and send out the message
    - Focus on the can receive
    - CAN_Not_available
  - ![image-20201121111506761](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201121111506761.png)
    







## Use MATLAB Simulink with Hercules RM57x

### Set Up

- **Pre-requisite**

  - 

- Install Cortex-R adds-on
  ![image-20201128150811124](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128150811124.png)

  - The package toolchain may be outdated, the latest code composer version is v10, but only v5 and v6 are present.
    ![image-20201128151129927](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128151129927.png)

  - Make sure the version matches
    ![image-20201128151213973](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128151213973.png)

  - Looks like ccsv6 has different file structure comparing with ccsv10. So installed ccsv6. 

    ![image-20201128152234251](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128152234251.png)
    ![image-20201128161503273](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161503273.png)

    This works![image-20201128161532729](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161532729.png)
    ![image-20201128161816355](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161816355.png)
    ![image-20201128161825513](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161825513.png)

    ![image-20201128161857315](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161857315.png)
    ![image-20201128161914373](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128161914373.png)

    

  - 



### Run Demo Code

- Remember to switch the directory

<img src="C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128174747736.png" alt="image-20201128174747736" style="zoom:100%;" />

- Change the directory from ***C:\Program Files\Polyspace\R2020a\bin*** to your own drive
  ![image-20201128175033477](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128175033477.png)



- Second bug is I used `&` in my folder name which confused the compiler.

- Eventually I am able to deploy and build
  ![image-20201128183655265](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201128183655265.png)

  But there's no command window pop up as stated in the example

  > After the model launches on the hardware, a command window opens and shows that the executable is running on in the TI Hercules RM57Lx LaunchPad Development Kit. To stop the model running on the TI Hercules RM57Lx LaunchPad Development Kit, close the command window.

  I suspect it's due to the warning 

  >All blocks in block diagram '[arm_cortex_r_gettingstarted](matlab:open_system ('arm_cortex_r_gettingstarted'))' are either virtual or have been removed by block reduction optimization or they are inactive variants, therefore there is nothing to simulate. Note, for code generation, block reduction optimization removes all diagram branches terminating in sink blocks that do not participate in code generation. For example, To Workspace blocks and potentially their sources are removed when the [MAT-file logging](matlab:configset.internal.open('arm_cortex_r_gettingstarted','MatFileLogging');) is off.
  >
  >Component:Simulink | Category:Block diagram warning
  - **Can't connect to hardware board**
    ![image-20201129140320119](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20201129140320119.png)
    - Followed the question here https://www.mathworks.com/matlabcentral/answers/622373-embedded-coder-for-arm-cortex-r-processors-hercules-tms570lc43x
  - 





## Check List

- [ ] Be able to compile and upload code to the Teensy
  - Clean, blink LED test
  - [https://www.arduino.cc/en/Tutorial/BuiltInExamples/Blink](https://hangouts.google.com/_/elUi/chat-redirect?authuser=1&dest=https%3A%2F%2Fwww.arduino.cc%2Fen%2FTutorial%2FBuiltInExamples%2FBlink)
  - \#define LED 13

- [ ] Luu will send the CAN send/receive code

- [ ] CAN Analyzer, microchip

- [ ] After the communication between Tessny-> Hercules Data 
  - Logging 

  - Timestamp, sampling rate, every signgle frame

  - Will log the data into as Restful API call, Json format

    - 200 Hz, every frame, log into the Json file 0.005