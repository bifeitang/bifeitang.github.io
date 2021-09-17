---
title: HalCoGen FreeRTOS Configuration and CAN Code
date: 2020-12-09 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

According to the instruction https://www.ti.com/lit/an/spna237/spna237.pdf?ts=1607497080672&ref_url=https%253A%252F%252Fwww.google.com%252F



![image-20201209010536090](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201209010536090.png)



![image-20201209010625937](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201209010625937.png)

![image-20201209011013043](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201209011013043.png)





| msgBoxArbitVal | Representation                                               |
| -------------- | ------------------------------------------------------------ |
| Bit 31         | Not used                                                     |
| Bit 30         | 0 - The 11-bit ("standard") identifier is used for this message object.<br/>1 - The 29-bit ("extended") identifier is used for this message object. |
| Bit 29         | 0 - Direction = Receive<br />1 - Direction = Transmit        |
| Bit 28:0       | Message ID                                                   |

E.g.

- ID = 123, Direction = transmit, + 1 >> 29, 0 >> 



- [ ] Write a function to convert the ID to the msgArbitVal





```C
#include "HL_can.h"
#include "HL_system.h"

#include "HL_sys_common.h"

#define D_SIZE 12

uint32 getMsgBoxArbitVal(uint32 msgID, bool isTransmit, bool isExtended);

uint8 tx_data[D_SIZE] = {1, 1, 2, 0, 2, 0, 2, 0, 'E', 'X', 'O', '\0'};


int main(void)
{
    canInit();
    uint32 newMsgBoxVal = getMsgBoxArbitVal(128, 1, 1);
    canUpdateID(canREG1, canMESSAGE_BOX1, newMsgBoxVal);
    canTransmit(canREG1, canMESSAGE_BOX1, tx_data);

    return 0;
}

uint32 getMsgBoxArbitVal(uint32 msgID, bool isTransmit, bool isExtended)
{
    uint32 direction = isTransmit ? (1 << 29) : 0;
    uint32 extended = isExtended ? (1 << 30) : 0;
    return (uint32) msgID | direction | extended;
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

```





