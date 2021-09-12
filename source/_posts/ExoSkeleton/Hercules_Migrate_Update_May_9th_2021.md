---
title: Migration Note 05_09_2021
date: 2021-05-09 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---


#### Can't compile `exoJoint` 

- Can't find `sourceID` and `targetID`![image-20210509183446878](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210509183446878.png)
  - ​	**Solution**
    - Added to `ExoCAN` class
      ![image-20210509183558145](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210509183558145.png)

- Can't find `encodeMsgID`
  ![image-20210509183756498](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210509183756498.png)
  -  Solution: comment that line out



#### Can't run PyQt (UI, Linux/Windows/Mac)

- `canGUI` folder does not contain any file which import `PyQt4`
  - Send command to main controller 
    - API exposed to PyQt UI to accept any outside command 

## Able to get angle recording

- Teensy

  ![AngleRecord](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/AngleRecord.gif)

- Try write to CAN bus with error, stops at waiting for CAN msg request
  ![image-20210509234646787](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20210509234646787.png)

- Send angle information via CAN bus

  ![AngleCANRecorder](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/AngleCANRecorder.gif)

- Hercules basic test with value receiver

  ![AngleMainControllerCommunication](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/AngleMainControllerCommunication.gif)





- Uplink works: teensy -> main controller (Task)
- Teensy 