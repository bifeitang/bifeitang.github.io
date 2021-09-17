---
title: Heartbeat from Hercules (Main Controller) to Teensy (Joint)
date: 2021-01-17 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Heartbeat from Hercules to Teensy

- Heartbeat Task design

  - Hercules -> Teensy: send heartbeat to all teensy nodes every 200ms
  - Teensy corresponding node reply with it's current angle

- Major code from Hercules

  ```c
  static void HeartBeatTask(void *pvParameters)
  {
      TickType_t xNextWakeTime;
      xNextWakeTime = xTaskGetTickCount();
  
      for( ;; )
      {
          vTaskDelayUntil( &xNextWakeTime, mainHEARTBEAT_FREQUENCY_MS );
          int i;
          for(i = 2; i <= 7 ; ++i)
          {
              uint32_t canID = i;
              char data = '1';
              HerculesCANWrite(canID, &data, 1);
  
              DbgPrint("Send HeartBeat Message from CAN bus");
          }
      }
  }
  
  ```

- Major code from Teensy

  ```c++
  if (CANbus.available()) {
          CANbus.read(rxCANmsg);
          if (rxCANmsg.id == 2 && rxCANmsg.buf[0] == '1')
          {
            Serial.println((String) "Receive Hear Beat CAN message");
            Serial.println((String) "Reply with Joint Angle" + jointCAN.frame.buf[0] + "-" + jointCAN.frame.buf[1]);
            CANbus.write(jointCAN.frame);
          }
      }
  
  ```

- Screenshot

  ![image-20210723010627617](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210723010627617.png)

- Please find gif from attachment

![HerculesHearBeatResponseSingleNode](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/HerculesHearBeatResponseSingleNode.gif)



