---
title: Explore Event Driven Programming for State Machine
date: 2021-02-28 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

## Review State Machine Event Driven Programming

https://www.state-machine.com/event-driven-programming/

- [x] Object oriented flavor with `wnd` object

  ```c
  wnd.cbSize = sizeof(wnd)
  ```

- [ ] `EventLoop`, with message loop and message pump

  - [ ] 1. use a special `object` to record all the events potentially interesting to an application, this object is only used for communication and can be conveniently stored in the event queue, and later retrieved for processing. Can provide `asynchronous` processing
  - [ ] 2. `DispatchMessage` must complete and return to the event-loop before the loop goes to the next event. (Event processing proceeds in Run-to-Completion (RTC)). Realize `inversion of control` for the event driven.
  - [ ] `GetMessage` unblock, take the message from the queue, place it to the object

- [ ] `Event-driven` programming should be separated from `sequential` programming

  - [ ] E.g. instead of using `sleep`, use `SetTimer()`
  - [ ] This is a perfect way to delegate a command to sleep and wake up later