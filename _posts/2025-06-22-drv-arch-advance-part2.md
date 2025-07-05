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

Latentcy of operation because of this 3 layered arch hence linux cannot be real time. 


### 3 Layer Driver Architecture Role in Linux Kernel

- Low Level Driver (Board specific bus-controller drivers)
    These drivers are initialized during kernel boot and are responsible for following:

    1. Bus Probe
    2. Bus/Device Enumeration
        Enumeration of bus and device information through linux specific data structures .. (struct device struct bus)
        Note : Every bus does not support this enumeration or bus scan such as static bus like I2C & SPI.
        In such cases BSP code directly enumerate the data strutcure and present this info to subsystem.
    3. Implementing operations for Physical bus I/O
    4. Interrupt handling

- Mid Level Driver (Kernel provided core layers)
    These drivers are initialized during kernel boot and are responsible for following:

    1. Interface for low-level drivers to register
    2. Interface for high-level drivers to register
    3. Maintaining bus/device/driver data structures
    4. init high-level drivers when matching device is found.

    Utilities:

    5. Enumerating bus/device/driver information to kernel's hot-plug subsystem
    6. Power management on bus
    7. Execption Handling

    API's interaction

    8. Abstract function interfaces (for high-level driver's use) for communication with slave devices

    Examples : usb-core, i2c-cire, spi-core, pcicore, platform-core, gpio-lib
    linux/drivers/spi/spi.c -- mid level driver
    linux/drivers/spi/spi-imx.c -- low level chipset driver
    linux/drivers/i2c/i2c-core.c
    linux/drivers/<bus-folder>/ will be hosting low level as well as mid level driver code

- High Level Driver (Peripheral Specific)
    These drivers will need to register with mid-layer with details of peripheral device they are
    looking for, and mid layer would initialize them when matching device is found

    1. Register with core layer.
    2. Implements functions to perform ops on slae device as per kernel driver model

    
