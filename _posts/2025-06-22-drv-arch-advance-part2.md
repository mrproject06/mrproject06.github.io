---
title: "Linux Driver Model - Advance - Part II"
date: 2025-06-22 00:00:00  +0500
categories: [Linux]
tags: [Driver]
---

## Driver Architecture - Peripheral Devices/Slave

```
┌───────────────────────┐       ┌───────────────────────┐
│          APP          │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────▼───────────┐       │                       │
│ Kernel Abstraction    │       ┼───────────────────────┤
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│  High Level Drv.      │       │         CPU           │
│  ──────────────────── │◄──────┼───────────────────────┤
│   Slave Controller    │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│   Mid Level Drv.      │       │                       │
│  ──────────────────── │◄──────┼───────────────────────┤
│     Bus Manager       │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│   Low Level Drv.      │       │                       │
│  ──────────────────── │◄──────┼───────────────────────┤
│    Bus Controller     │       │                       │
└───────────┬───────────┘       └───────────┬───────────┘
            ⇅                               ⇅
┌───────────┴───────────────────────────────┴───────────┐
│                                                       │
│                     Bus Controller                    │
│                                                       │
└───────────┬───────────────────────────────┬───────────┘
            ⇅                               ⇅
    ┌───────▼───────┐               ┌───────▼───────┐
    │   Slave #1    │               │   Slave #2    │
    └───────────────┘               └───────────────┘
            ⇅
    ┌───────▼───────┐
    │   Slave #3    │
    └───────────────┘
```

From CPU perspective Bus controller can be reached first as it can be reached first, it should be
name high level from CPU perspective. 
Then peripheral driver should be consider low level. 

It is the application which is going to consume the kernel service. 

Application is interested with peripheral device hence from application point of view this topology
or naming convention is correct. 


What is rationale behind this 3 layere architecture instead of 2 ? 
Bus manager can be made library or service which high level driver can make use of.

Bus manager provided better management it is acting as a abstraction layer between high level and low
level. High level driver can be implemented without worrying about underlying implementation of low
level. 

For example same driver for eMMC slave chip can work on 2 different SoC without much change.  


