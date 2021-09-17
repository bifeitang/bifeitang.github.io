---
title: Troubleshoot joint can't communicate with Hercules
date: 2021-06-06 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Joint can't communicate with Hercules

- ```c
      if (CANbus.available())
      {
        CANbus.read(jointCAN.frame);
        Serial.println("Receive CAN request");
        Serial.println((char*)jointCAN.frame.buf);
      }
  ```

- The `available()` function suppose to indicate receiving of CAN message, but nothing came up

- CAN bit timing may not good

  - https://embedclogic.com/can-protocol-protocol-to-broadcast-message-on-a-network/can-bit-timing-and-calculation/

  - Original 

    - ```c
          /** - Setup bit timing 
          *     - Setup baud rate prescaler extension
          *     - Setup TSeg2
          *     - Setup TSeg1
          *     - Setup sample jump width
          *     - Setup baud rate prescaler
          */
          canREG1->BTR = (uint32)((uint32)0U << 16U) |
                         (uint32)((uint32)(4U - 1U) << 12U) |
                         (uint32)((uint32)((3U + 4U) - 1U) << 8U) |
                         (uint32)((uint32)(4U - 1U) << 6U) |
                         (uint32)24U;
      ```

    - Confirmed the bit rate is 250kb/s

      - ![image-20210606214742664](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210606214742664.png)

    - Try change the heart beat frequency from 20ms to 2s 

      ```c
      /* The 20ms value is converted to ticks using the portTICK_PERIOD_MS constant. */
      #define mainHEARTBEAT_FREQUENCY_MS         ( 2000 / portTICK_PERIOD_MS )
      ```

    - Confirm for Teensy (FlexCAN.cpp)

      ```c
        } else if ( 250000 == baud ) {
          FLEXCAN0_CTRL1 = (FLEXCAN_CTRL_PROPSEG(2) | FLEXCAN_CTRL_RJW(1)
                            | FLEXCAN_CTRL_PSEG1(7) | FLEXCAN_CTRL_PSEG2(3) | FLEXCAN_CTRL_PRESDIV(3));
      ```

      

- [x] Issue with CAN transceiver? No

- [x] Issue with Baud rate? No

- [ ] Timing issue? Teensy needs to delay a certain period of time in order to get the corresponding message. 