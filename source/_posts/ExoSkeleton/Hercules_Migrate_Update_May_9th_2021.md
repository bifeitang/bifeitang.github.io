## May 9th

#### Can't compile `exoJoint` 

- Can't find `sourceID` and `targetID`![image-20210509183446878](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210509183446878.png)
  - ​	**Solution**
    - Added to `ExoCAN` class
      ![image-20210509183558145](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210509183558145.png)

- Can't find `encodeMsgID`
  ![image-20210509183756498](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210509183756498.png)
  -  Solution: comment that line out



#### Can't run PyQt (UI, Linux/Windows/Mac)

- `canGUI` folder does not contain any file which import `PyQt4`
  - Send command to main controller 
    - API exposed to PyQt UI to accept any outside command 

## Able to get angle recording

- Teensy

  ![AngleRecord](C:\Users\kydn8\Desktop\AngleRecord.gif)

- Try write to CAN bus with error, stops at waiting for CAN msg request
  ![image-20210509234646787](C:\Users\kydn8\AppData\Roaming\Typora\typora-user-images\image-20210509234646787.png)

- Send angle information via CAN bus

  ![AngleCANRecorder](C:\Users\kydn8\Desktop\AngleCANRecorder.gif)

- Hercules basic test with value receiver

  ![AngleMainControllerCommunication](C:\Users\kydn8\Desktop\AngleMainControllerCommunication.gif)





- Uplink works: teensy -> main controller (Task)
- Teensy 