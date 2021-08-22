---
title: Discussion Notes
date: 2020-10-25 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Summary

- Heartbeat with timeout to check the joint status periodically
- **Event based** interruptions in main controller
- Power distribution, voltage, temperature

## Raw Notes

- Enable can bus
- Setup the communications
- Set up the thing to test the loop
- Event base
- Interrupt
  - Setup command
  - Send a time out
  - Turn on the timer
- Master controller
- **Event based** interruptions
  - Set some interrupts in master controller
  - Emergency frame and turn on the flag
  - Turn on **LED/print out something when I receive CAN message**
  - What is ==event based==? Attach flag to the buffer to notify the interrupt
  - Check Luu's code, encode and decode the CAN frame
- How to do multithreading
  - On one thread control joint
- Pattern monitor send out the command as well as the timeout
- Master controller (functionality)
  - ==make one thread to check heartbeat==
    - Send out a confirmation for the configuration is set up correctly
    - To make sure the the communication is working correctly
    - Turn on the time out when command is sent out 
  - CAN BUS communication
    - 100: for the communication heart beat.
    - Every one seconds send out a CAN message
    - One second print a hello world
    - 2 thread running in parallels
  - One thread per joint? 
  - Then **finite state machine**
  - Power distribution, voltage, temperature another state of 
  - Another node to check the main controller?
    - Functional safety for duel controllers
- Command queue 
  - struct
  - timer when this enqueue
- OnStep periocally 1s,  command timeout 
- Github action ()
  - auto generated documentation for the function written
- Read the doc for auto generate comments for C code
  - Sphinx 



